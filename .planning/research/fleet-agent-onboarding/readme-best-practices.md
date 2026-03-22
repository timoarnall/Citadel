# Scout 2: README Best Practices in Popular Dev Tools

## Finding 1: The "Inverted Pyramid" Structure

**What:** Highly-starred repos follow a consistent information hierarchy:

1. **Hero section** (above the fold): Logo + one-liner + badges + GIF/screenshot
2. **Quickstart** (3-5 lines of code, copy-pasteable)
3. **Feature list** (bullet points, not paragraphs)
4. **Detailed install/usage** (expandable or linked to docs)
5. **Architecture/structure** (for contributors)
6. **Contributing + License + Support**

The key insight: the first screenful must answer "What is this?" and "How do I try it?" simultaneously. Everything else is secondary.

**Source:** https://www.daytona.io/dotfiles/how-to-write-4000-stars-github-readme-for-your-project
**Confidence:** High — pattern confirmed across multiple sources.
**Action:** Citadel's README should have a copy-pasteable quickstart within the first 20 lines of rendered markdown.

---

## Finding 2: The One-Liner Rule

**What:** The project description must be a single sentence that a non-user can understand. Examples from top repos:
- Aider: "AI pair programming in your terminal"
- OpenHands: "AI-Driven Development"
- SWE-agent: "Takes a GitHub issue and tries to automatically fix it"

The pattern: `[Tool name]: [verb phrase describing what it does for the user]`. No jargon, no architecture details, no "powered by X" qualifiers.

**Source:** https://medium.com/@fulton_shaun/readme-rules-structure-style-and-pro-tips-faea5eb5d252, various repo descriptions
**Confidence:** High.
**Action:** Citadel needs a one-liner. Current description ("An agent orchestration system for Claude Code") is close but could be more action-oriented: e.g., "Run autonomous coding campaigns with Claude Code."

---

## Finding 3: Visual Proof > Text Claims

**What:** READMEs with GIFs or screenshots of the tool in action get significantly more engagement. The visual serves as:
- Proof the tool works
- Implicit documentation of the UX
- A "taste" that motivates reading further

Terminal tools use animated GIFs (asciinema recordings). GUI tools use screenshots with annotations. The visual appears immediately after the one-liner, before any install instructions.

**Source:** https://www.daytona.io/dotfiles/how-to-write-4000-stars-github-readme-for-your-project, https://dev.to/documatic/how-to-write-an-awesome-readme-cfl
**Confidence:** High.
**Action:** Record a terminal GIF showing `/do "fix the login bug"` end-to-end. Place it at the top of the README.

---

## Finding 4: Quickstart Must Be Copy-Pasteable

**What:** The best quickstarts are literally: copy the code block, paste into terminal, see results. No "first create a config file" or "set up your environment" preamble.

Pattern from top repos:
```
pip install tool-name
cd your-project
tool-name "do something"
```

Three lines. If setup requires more, the quickstart shows the happy path and links to "Full Installation Guide" for edge cases.

**Source:** https://www.freecodecamp.org/news/how-to-write-a-good-readme-file/, Aider docs
**Confidence:** High.
**Action:** Citadel's quickstart should be the absolute minimum commands. Config generation should happen automatically on first run, not as a prerequisite.

---

## Finding 5: Separate README from Docs

**What:** READMEs that exceed ~300 lines lose readers. The pattern in successful projects:
- README: overview + quickstart + feature highlights + links
- `/docs/` or external docs site: full reference, tutorials, architecture
- CONTRIBUTING.md: contributor-specific setup
- ARCHITECTURE.md: codebase navigation

The README is a landing page, not a manual.

**Source:** https://www.archbee.com/blog/readme-creating-tips, https://www.tilburgsciencehub.com/topics/collaborate-share/share-your-work/content-creation/readme-best-practices/
**Confidence:** High.
**Action:** Citadel already has QUICKSTART.md and CLAUDE.md. The main README should be a concise landing page that links to these.

---

## Finding 6: Badges Signal Trust

**What:** Badges at the top of a README signal project health: CI status, test coverage, latest version, license. For dev tools specifically, useful badges include:
- Build/CI status
- npm/PyPI version
- License type
- "Works with X" compatibility badges

They're not decorative — they answer "Is this maintained?" and "Can I use this?" at a glance.

**Source:** https://www.freecodecamp.org/news/how-to-structure-your-readme-file/
**Confidence:** High.
**Action:** Add relevant badges to Citadel's README (license, Node version compatibility, etc.).

---

## Structural Template (Consensus)

```
# Project Name
> One-liner description

[Badges]

[GIF/Screenshot]

## Quickstart
[3-5 copy-pasteable lines]

## Features
[Bullet list]

## Documentation
[Links to detailed docs]

## Contributing
[Link to CONTRIBUTING.md]

## License
[Type + link]
```
