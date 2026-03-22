# Research: Git Worktree Cleanup in Parallel Agent Systems

> Question: What are the best practices for git worktree cleanup in parallel agent systems?
> Date: 2026-03-21
> Confidence: medium

## Findings

### 1. Use `git worktree remove` over manual deletion; `prune` as safety net
**What:** The official Git documentation specifies that `git worktree remove <path>` is the correct way to tear down a worktree. It cleans both the working directory and the administrative files under `$GIT_DIR/worktrees/`. If a worktree directory is deleted manually (e.g., `rm -rf`), the admin metadata persists as orphaned entries. `git worktree prune` scavenges these orphans. The `--expire <time>` flag (e.g., `--expire 2.weeks`) limits pruning to entries older than a threshold, and `--dry-run` previews what would be removed. The config key `gc.worktreePruneExpire` controls automatic pruning during `git gc`.
**Source:** https://git-scm.com/docs/git-worktree
**Confidence:** high — official Git documentation, canonical reference
**Action:** Agent cleanup scripts should call `git worktree remove <path> --force` on exit. Run `git worktree prune --verbose` as a periodic sweep to catch any orphans left by crashed agents.

### 2. Lock worktrees during active use to prevent premature pruning
**What:** `git worktree lock <path> --reason <msg>` prevents a worktree from being pruned or removed by another process. This is critical in parallel agent systems where one agent might run `prune` while another agent's worktree is temporarily unreachable (e.g., on a network mount, or during a container restart). Unlocking requires an explicit `git worktree unlock`. Removing a locked worktree requires `--force --force` (double force).
**Source:** https://git-scm.com/docs/git-worktree
**Confidence:** high — official Git documentation
**Action:** Agents should lock their worktree on creation and unlock + remove on exit. This prevents race conditions where a central cleanup process prunes a still-active worktree.

### 3. Disk space accumulation is a major operational risk
**What:** Multiple practitioners report that parallel worktree systems consume disk rapidly. One account documented ~10 GB consumed in 20 minutes with a ~2 GB codebase, driven by duplicated `node_modules`, build caches (Bazel, Nx), and build artifacts. Without proactive cleanup, forgotten worktrees accumulate — one developer reported 15 stale worktrees consuming gigabytes before noticing.
**Source:** https://devcenter.upsun.com/posts/git-worktrees-for-parallel-ai-coding-agents/
**Confidence:** medium — practitioner report with specific metrics, corroborated by multiple community sources
**Action:** Implement mandatory cleanup on agent exit. Consider shared dependency caches (e.g., Worktrunk's build-cache copy feature) rather than per-worktree installs. Set `gc.worktreePruneExpire` to a short interval (e.g., 1 day) for agent repos.

### 4. Merge conflicts between parallel worktrees require coordination
**What:** Worktrees provide filesystem isolation but not logical isolation. Multiple agents editing overlapping code regions create merge conflicts that are only discovered at merge time. The recommended mitigation is: (a) assign agents to non-overlapping tasks, (b) use separate branches per agent, (c) perform pre-execution dependency analysis to detect overlapping file scopes, and (d) merge frequently to surface conflicts early.
**Source:** https://devcenter.upsun.com/posts/git-worktrees-for-parallel-ai-coding-agents/
**Confidence:** medium — community consensus across multiple tools (Worktrunk, parallel-cc, agent-orchestrator)
**Action:** Informational. The Citadel Fleet system already uses per-agent branches. Consider adding file-scope overlap detection during task assignment.

### 5. Consolidate cleanup into a single atomic operation
**What:** Worktrunk's `wt merge` command demonstrates a best-practice pattern: merge + delete worktree + delete branch in one atomic step. This prevents the common failure mode where an agent merges successfully but crashes before cleaning up its worktree/branch, leaving orphaned resources. The tool also uses hooks (post-start, pre-merge, post-merge) to automate setup/teardown tasks like installing dependencies or running tests.
**Source:** https://github.com/max-sixty/worktrunk
**Confidence:** medium — well-starred tool encoding community patterns, but single-project reference
**Action:** Fleet's worktree exit protocol should combine merge + worktree remove + branch delete into a single scripted sequence with error handling, rather than separate steps that can partially fail.

### 6. Runtime environment isolation goes beyond filesystem separation
**What:** Worktrees isolate code but not the runtime environment. Port collisions (multiple agents defaulting to ports 3000, 5432, 8080), shared database instances causing race conditions, and missing `.env` files requiring per-worktree setup are documented failure modes. Mitigations include per-worktree port assignment, worktree-indexed Docker volume names, and templated `.env` generation.
**Source:** https://devcenter.upsun.com/posts/git-worktrees-for-parallel-ai-coding-agents/
**Confidence:** medium — practitioner report with specific examples
**Action:** If Fleet agents run dev servers or databases, assign unique ports per worktree. This is not currently relevant for Citadel's build-only agents but should be considered if agents gain runtime testing capabilities.

## Summary

Git worktree cleanup in parallel agent systems requires a three-layer approach: (1) proper use of `git worktree remove --force` as the primary cleanup mechanism, with `lock`/`unlock` to prevent race conditions between agents, (2) periodic `git worktree prune` sweeps to catch orphans from crashed agents, and (3) disk-space-aware policies including shared dependency caches and short `gc.worktreePruneExpire` intervals. The most robust pattern is atomic cleanup — combining merge, worktree removal, and branch deletion into a single scripted sequence so partial failures don't leave orphaned resources.

## Open Questions

- What is the optimal `gc.worktreePruneExpire` value for an agent system that creates/destroys worktrees every few minutes? Too short risks pruning active worktrees; too long accumulates orphans. Locking likely makes this moot, but needs testing.
- Should a central "janitor" process periodically scan for unlocked, stale worktrees, or is per-agent cleanup sufficient? Depends on agent crash frequency in practice.
- How does `git worktree repair` interact with concurrent operations? The docs don't specify thread-safety guarantees for repair vs. prune running simultaneously.
