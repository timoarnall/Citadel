# Scout 3: Common Friction Points New Users Report

## Finding 1: API Key Setup Is the #1 Drop-off Point

**What:** Across AI coding tools, the most commonly reported friction point is API key configuration. Users must:
1. Create an account on the LLM provider
2. Generate an API key
3. Figure out where to put it (env var? config file? CLI flag?)
4. Handle multiple providers with different env var names

Roo-Code's GitHub issue #11676 explicitly calls out that "OpenAI setup currently relies on manual API key handling, causing drop-off during onboarding." The proposed fix is OAuth sign-in so users never handle raw keys.

**Source:** https://github.com/RooCodeInc/Roo-Code/issues/11676, https://community.openai.com/t/login-with-openai-api-key-environment-variable/1371740
**Confidence:** High — corroborated across multiple tools and forums.
**Action:** Citadel inherits Claude Code's auth, so this is partially solved. But any additional config (harness.json) adds friction. Auto-detect what's available, don't ask users to declare it.

---

## Finding 2: "It Takes 1-2 Weeks to Learn How to Use It"

**What:** Reddit developers consistently report that AI coding tools have a hidden learning curve. The tool installs fine, but users don't know:
- What to ask for (task framing)
- How much context to provide
- What the tool can vs. cannot do
- How to recover when it goes wrong

This isn't a setup problem — it's a "first 10 minutes after setup" problem. The quickstart gets them running, but they don't know what to do next.

**Source:** https://medium.com/@anoopm75/the-uncomfortable-truth-about-ai-coding-tools-what-reddit-developers-are-really-saying-f04539af1e12
**Confidence:** High — consistent sentiment across Reddit roundups.
**Action:** Citadel should include a "What to try first" section with 3 concrete example commands. Not abstract capabilities — literal commands they can paste.

---

## Finding 3: Configuration Fragmentation Across Tools

**What:** Developers who use multiple AI tools (Cursor + Aider + Claude Code) complain about maintaining separate config files for each: `.cursor/rules/`, `.aider*`, `CLAUDE.md`, `AGENTS.md`, etc. Cline issue #5033 proposes a standard `AGENTS.md` format to reduce this.

The broader complaint: every tool invents its own configuration language and file structure. Users can't transfer knowledge between tools.

**Source:** https://github.com/cline/cline/issues/5033, Cursor forum discussions
**Confidence:** High.
**Action:** Citadel's harness.json and skill/agent structure is unique. Document the "why" clearly so users understand the value proposition rather than seeing it as "yet another config format."

---

## Finding 4: GitHub/Auth Integration Failures Are Rage-Inducing

**What:** Cursor's community forum has multiple threads about GitHub connection failures for background agents:
- "Connect to GitHub" button spins forever
- Access denied after granting permissions
- Can only see 1 of N repos after auth

These are integration bugs, but the meta-lesson is: any setup step that involves third-party auth is a high-risk failure point. When it fails, users have zero ability to debug it.

**Source:** https://forum.cursor.com/t/stuck-at-connect-to-github-despite-full-authorization-cursor-0-50-background-agent-setup-bug/94041, https://forum.cursor.com/t/github-connection-for-background-agent-is-stuck/103616
**Confidence:** High — multiple independent reports.
**Action:** Citadel should minimize third-party auth dependencies in the critical path. If auth is needed, provide clear error messages with actionable fixes, not generic "connection failed."

---

## Finding 5: Terminal-Only Tools Exclude Non-CLI Users

**What:** Aider's "approachability downside" is explicitly called out: it "assumes comfort with the terminal and deliberate task framing, which turns some developers away." Tools that only work in the terminal have a ceiling on their addressable audience.

**Source:** https://www.faros.ai/blog/best-ai-coding-agents-2026
**Confidence:** Medium — single source but matches general sentiment.
**Action:** Citadel is terminal-native by design (Claude Code). This is a feature, not a bug, for its target audience. But the README/docs should explicitly state the prerequisite: "You should be comfortable with the terminal."

---

## Finding 6: Unclear Error Messages on Missing Dependencies

**What:** When setup fails, the most common complaint pattern is:
- Cryptic error message
- No suggestion for how to fix it
- User has to Google the error, find a GitHub issue, read 30 comments to find the fix

The tools that handle this well (like Aider's auto-provisioning of Python 3.12) eliminate entire categories of "missing dependency" errors.

**Source:** Multiple forum threads, general developer sentiment
**Confidence:** Medium — pattern observed but not from a single authoritative source.
**Action:** Every error in Citadel's setup flow should include: what went wrong, why, and what to do about it. `harness.json not found` should say `Run "/do setup" to generate it.`

---

## Top Friction Points (Ranked by Frequency)

1. **API key / auth setup** — universal complaint
2. **"Now what?" after install** — no guided first experience
3. **Config file creation** — manual step before first use
4. **Cryptic error messages** — no actionable guidance
5. **Multiple config formats** — tool fatigue
6. **Third-party integration failures** — undebuggable by user
