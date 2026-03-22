# Research Fleet Report: Agent Orchestration Onboarding & First-Run Setup

**Date:** 2026-03-21
**Scouts:** 3 (competitor quickstarts, README best practices, user friction points)
**Research question:** How do other open-source agent orchestration tools handle onboarding and first-run setup?

---

## Consensus (All 3 scouts agree)

1. **3 steps or fewer from install to first use.** Every successful tool achieves this. Aider: install, cd, run. Cursor: download, open. OpenHands: visit URL. No tool that requires manual config file creation before first use has strong adoption.

2. **The README is a landing page, not a manual.** One-liner + visual + quickstart code block in the first screenful. Detailed docs live elsewhere. READMEs over ~300 lines lose readers.

3. **"Now what?" is a bigger problem than "how do I install?"** Users install fine but don't know what to try first. Every tool that handles this well provides 2-3 concrete example commands, not abstract capability descriptions.

4. **Error messages must be actionable.** The #1 rage-inducer across all tools is cryptic errors during setup with no fix suggestion. Auto-detection and auto-recovery beat documentation.

---

## Conflicts (Scouts disagree or tension exists)

1. **Wizard vs. zero-config.** mini-SWE-agent uses a first-run wizard; Aider uses inline CLI flags. Both work. The conflict: wizards feel guided but add friction; inline flags feel fast but require knowing the flags. **Resolution:** Support both — auto-detect config, but offer `--setup` for guided mode.

2. **Terminal-native vs. accessible.** Aider is criticized for "assuming terminal comfort." But Citadel's target audience IS terminal-native (Claude Code users). **Resolution:** This is fine for Citadel. Just state the prerequisite explicitly.

---

## Gaps (Questions the research didn't fully answer)

1. **How do orchestration-layer tools (not just coding assistants) handle onboarding?** Most research focused on direct coding tools (Aider, Cursor). Tools closer to Citadel's layer (campaign orchestration, multi-agent coordination) are newer and have less user feedback.

2. **What does "first success" look like for an orchestration harness?** For Aider, it's "AI edited my file." For Citadel, the equivalent moment is less obvious — is it running a campaign? Using `/do`? Having Fleet execute parallel tasks? The "aha moment" needs definition.

3. **Retention after first use.** All research focused on install-to-first-use. No data on what makes users come back for session 2.

---

## Surprises

1. **Config file creation is a bigger barrier than expected.** Even one `touch config.json && edit it` step causes measurable drop-off. The best tools generate config automatically or don't need it at all.

2. **Visual proof in README matters more than feature lists.** A single GIF of the tool working outperforms a 20-bullet feature list for conversion. Terminal tools use asciinema recordings.

3. **API key setup is STILL the #1 friction point in 2025-2026** despite being a known problem for years. OAuth/built-in auth (like Cursor's freemium model) is the only real solution. Citadel sidesteps this by inheriting Claude Code's auth — this is a significant advantage worth highlighting.

4. **Users want "what to try first," not "what this can do."** The distinction: capability lists are seller-oriented; example commands are buyer-oriented. Top repos lead with examples.

---

## Recommended Actions for Citadel

### Immediate (README/Quickstart rewrite)
- [ ] Write a one-liner: "Run autonomous coding campaigns with Claude Code"
- [ ] Create a 3-step quickstart: install Claude Code, clone/cd, run `/do setup`
- [ ] Add 3 concrete "try this first" examples with exact commands
- [ ] Record a terminal GIF showing `/do` end-to-end
- [ ] Keep README under 150 lines; link to QUICKSTART.md and CLAUDE.md for depth

### Short-term (First-run experience)
- [ ] Make `/do setup` auto-detect existing config and skip unnecessary steps
- [ ] Add actionable error messages: every "missing X" error should say how to fix it
- [ ] Auto-generate `harness.json` with sensible defaults on first `/do` invocation if missing
- [ ] Print "Try these next:" suggestions after setup completes

### Medium-term (Onboarding polish)
- [ ] Add badges to README (license, Node version, Claude Code compatibility)
- [ ] Create CONTRIBUTING.md for the harness itself
- [ ] Define the "aha moment" — what is Citadel's equivalent of "AI edited my file"?
- [ ] Consider a `--demo` flag that runs a safe, self-contained example campaign

---

## Sources

- [Aider Installation Docs](https://aider.chat/docs/install.html)
- [Cursor Official Site](https://cursor.com/)
- [OpenHands Docs](https://docs.openhands.dev/sdk)
- [mini-SWE-agent Quickstart](https://mini-swe-agent.com/latest/quickstart/)
- [How to Write a 4000-Stars README](https://www.daytona.io/dotfiles/how-to-write-4000-stars-github-readme-for-your-project)
- [README Rules: Structure, Style, Pro Tips](https://medium.com/@fulton_shaun/readme-rules-structure-style-and-pro-tips-faea5eb5d252)
- [freeCodeCamp: How to Write a Good README](https://www.freecodecamp.org/news/how-to-write-a-good-readme-file/)
- [Reddit Developer Sentiment on AI Tools](https://medium.com/@anoopm75/the-uncomfortable-truth-about-ai-coding-tools-what-reddit-developers-are-really-saying-f04539af1e12)
- [Roo-Code Onboarding Issue #11676](https://github.com/RooCodeInc/Roo-Code/issues/11676)
- [Cline AGENTS.md Proposal #5033](https://github.com/cline/cline/issues/5033)
- [Cursor Forum: GitHub Connection Issues](https://forum.cursor.com/t/stuck-at-connect-to-github-despite-full-authorization-cursor-0-50-background-agent-setup-bug/94041)
- [Best AI Coding Agents 2026 (Faros)](https://www.faros.ai/blog/best-ai-coding-agents-2026)
