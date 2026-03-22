# Campaign: System Verification

Status: complete
Started: 2026-03-21T16:00:00Z
Completed: 2026-03-21
Direction: Full system verification of Wave 1+2 additions. Three phases: smoke test every new skill, verify Archon self-correction, audit first-run onboarding experience.

## Claimed Scope
- .planning/research/
- .planning/screenshots/
- .planning/campaigns/system-verification.md
- docs/
- README.md
- QUICKSTART.md

## Phases
1. [complete] Verify: Smoke test every new skill (/research, /experiment, /systematic-debugging, /live-preview, /research-fleet)
2. [complete] Verify: Archon self-correction protocol fires correctly
3. [complete] Verify: First-run experience audit + fixes

## Feature Ledger
| Feature | Status | Phase | Notes |
|---------|--------|-------|-------|
| /research smoke test | PASS | 1 | 6 findings, document in .planning/research/, confidence justified |
| /experiment smoke test | PASS | 1 | 339→253 lines (25.4%), 3/3 kept, convergence detection works |
| /systematic-debugging smoke test | PASS | 1 | Found real bug (broken regex in protect-files.js), 4-phase protocol executed |
| /live-preview smoke test | PASS | 1 | Graceful exit on non-UI repo. Fixed: added explicit early-exit clause |
| /research-fleet smoke test | PASS | 1 | v2 completed with search-only approach. 3 scouts, unified report with consensus/conflicts/gaps |
| protect-files.js regex fix | DONE | 1 | path.sep.join replaces broken regex from heredoc write |
| live-preview early-exit clause | DONE | 1 | Added guard for repos with no view-layer files |
| Archon self-correction verification | PASS | 2 | All 4 mechanisms fire: direction alignment, quality spot-check, regression guard, anti-pattern scan |
| README rewrite | DONE | 3 | 369→131 lines. Landing page format. Updated skill count (18). Added "Try These First" section |
| QUICKSTART.md rewrite | DONE | 3 | Comprehensive guide. No longer duplicates README. Includes project structure, telemetry, Superpowers comparison |
| Skill table updated | DONE | 3 | All 18 skills documented in 4 categories: Core, Research & Debugging, Orchestration, Utilities |
| File protection hook updated in docs | DONE | 3 | Hook table now mentions .env read protection |

## Phase 3 Audit: Friction Points Found & Fixed

| # | Friction Point | Severity | Fix Applied |
|---|---------------|----------|-------------|
| 1 | README 369 lines (manual, not landing page) | High | Rewritten to 131 lines. Detailed content moved to QUICKSTART.md |
| 2 | Skill count wrong ("13" tagline, "6 built-in" table) | High | Updated to 18. All skills listed in 4 categories |
| 3 | QUICKSTART.md duplicated README verbatim | Medium | QUICKSTART.md is now the comprehensive guide, README links to it |
| 4 | No "try this first" moment | High | Added "Try These First" section with 4 concrete commands |
| 5 | 3 OS-specific install blocks bloated first screen | Medium | README shows single bash snippet, QUICKSTART.md has all platforms |
| 6 | Wave 1+2 skills undocumented | High | All new skills in tables: research, experiment, systematic-debugging, research-fleet, live-preview |
| 7 | .env protection not mentioned in hooks table | Low | Added "blocks reads on .env secrets" to file protection hook description |

## Decision Log
- 2026-03-21: Replaced regex `replace(/\\/g, '/')` with `split(path.sep).join('/')` in protect-files.js
  Reason: Bash heredocs corrupt JS regex backslash escaping. path.sep approach is more robust.
- 2026-03-21: Added early-exit clause to /live-preview Step 1 for repos with no view files
  Reason: Smoke test revealed the protocol assumed view files always exist.
- 2026-03-21: Killed hung /research-fleet agent and restarted with search-only approach
  Reason: WebFetch on full GitHub repo pages hangs indefinitely. No orchestrator-level timeout exists.
- 2026-03-21: README rewritten as landing page, QUICKSTART.md as comprehensive guide
  Reason: Research-fleet consensus: README should be under 150 lines, concrete examples beat feature lists, 3 steps to first use.

## Review Queue
- [x] Architecture: Sub-agent hang detection — added Agent Timeouts section to Fleet and Marshal SKILL.md with configurable timeouts (10/15/30 min defaults) and timeout protocol
- [x] UX: /research-fleet WebFetch guard — added explicit WebFetch Restrictions section with raw.githubusercontent.com alternative

## Circuit Breakers
- Any skill crashes or hangs instead of exiting gracefully
- Archon self-correction mechanisms don't fire at all
- README is fundamentally broken (can't get from clone to working harness)
