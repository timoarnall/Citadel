# Citadel

An agent orchestration harness for Claude Code by Seth Gammon. Provides intent routing, multi-session campaigns, parallel fleet execution, and 24 built-in skills for autonomous coding at scale.

## Key features
- **Intent router (`/do`)** — Classifies requests and dispatches to the cheapest viable tool
- **Orchestration ladder** — Skills → Marshal (single-session) → Archon (multi-session campaigns) → Fleet (parallel agents in isolated worktrees)
- **24 skills** — App creation pipeline, code review, testing, research, debugging, QA, documentation
- **8 lifecycle hooks** — Quality gates, circuit breaker, file protection, context compression
- **Campaign persistence** — Markdown-based state files survive context resets

## Usage
Copy into your project directory. Run `/do setup` to configure. Then use `/do <your request>` to route work automatically.

See README.md for full documentation, QUICKSTART.md for setup, and docs/ for architecture details.
