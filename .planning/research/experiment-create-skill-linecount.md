# Experiment: Reduce create-skill SKILL.md line count

> Metric: `wc -l < .claude/skills/create-skill/SKILL.md`
> Direction: lower is better
> Scope: `.claude/skills/create-skill/SKILL.md`
> Budget: 3 iterations
> Date: 2026-03-21

## Results

| Iteration | Value | Delta | Verdict | Change |
|-----------|-------|-------|---------|--------|
| baseline  | 339   | —     | —       | —      |
| 1         | 284   | -55   | KEEP    | Condensed discovery questions (removed verbose "Listen for" lists), tightened bad/good step example, compressed writing rules explanations, condensed Step 5 VERIFY section |
| 2         | 264   | -20   | KEEP    | Tightened trigger keywords section, compressed quality gates explanation, condensed Step 6 TEACH from bullet lists to inline descriptions |
| 3         | 253   | -11   | KEEP    | Compressed Identity section from 3 paragraphs to 1, shortened Step 1 intro, condensed CLAUDE.md check in Step 4 |

## Outcome
- **Start**: 339
- **End**: 253
- **Improvement**: 25.4% reduction (86 lines removed)
- **Iterations**: 3/3 kept
- **Stop reason**: budget exhausted

## Kept Changes
- Iteration 1: Condensed verbose explanatory prose in discovery, examples, writing rules, and verify sections
- Iteration 2: Tightened analysis sub-sections and teach section
- Iteration 3: Final pass on identity, step intros, and install notes

## Notes
- All protocol steps, quality gates, and essential instructions preserved
- File remains valid markdown with complete protocol structure
- No sections were deleted or merged — only prose was tightened
- Simulated protocol (no worktree isolation — edits made directly to file)
