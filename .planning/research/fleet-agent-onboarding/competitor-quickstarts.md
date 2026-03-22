# Scout 1: Competitor Quickstart Flows

## Finding 1: Aider — Gold Standard CLI Onboarding (3 steps)

**What:** Aider achieves install-to-first-use in exactly 3 steps:
1. `pip install aider-install && aider-install` (auto-provisions Python 3.12 + isolated env)
2. `cd /to/your/project`
3. `aider --model sonnet --api-key anthropic=<key>`

No config files required. No setup wizard. The API key is passed inline on first run.
Auto-commits with descriptive messages out of the box.

**Source:** https://aider.chat/docs/install.html
**Confidence:** High — verified from official docs.
**Action:** Citadel should target 3 steps or fewer. Aider's inline `--api-key` pattern eliminates the "create a config file" step that kills momentum.

---

## Finding 2: Cursor — Familiar IDE Wrapper (2 steps, but heavy)

**What:** Cursor's onboarding is:
1. Download installer from cursor.com
2. Open it — VS Code settings import automatically

First-run experience leans on VS Code familiarity. AI features "just work" with built-in model access (no API key needed for basic tier). Power users then configure `.cursor/rules/` and MCPs.

The low step count is deceptive — the download is ~300MB and the "real" setup (rules, MCPs, memories) is a separate learning curve.

**Source:** https://daily.dev/blog/setup-cursor-first-time, https://cursor.com/
**Confidence:** High.
**Action:** Two-phase onboarding is a valid pattern: instant gratification first, power-user config later. Citadel could separate "run your first task" from "configure your harness."

---

## Finding 3: OpenHands — Multiple Entry Points

**What:** OpenHands offers three onboarding paths:
- **Web UI** (hosted at openhands.dev — zero install)
- **CLI** via SDK install
- **Docker** for local deployment

The hosted option means true zero-step onboarding. The self-hosted path requires Docker, which adds friction but is well-documented.

**Source:** https://docs.openhands.dev/sdk, https://openhands.dev/
**Confidence:** Medium — details inferred from search snippets, not full walkthrough.
**Action:** A hosted/cloud option eliminates all install friction. For CLI tools like Citadel, the lesson is: offer a "try it now" path that skips config.

---

## Finding 4: SWE-agent / mini-SWE-agent — Setup Wizard Pattern

**What:** mini-SWE-agent (the recommended successor) uses:
1. `uvx mini-swe-agent` (or pipx)
2. First run triggers an interactive setup wizard for model config
3. Start using

The setup wizard on first run is notable — it means zero config files needed before install, but the tool still captures preferences. `mini-extra config setup` re-runs the wizard if missed.

**Source:** https://mini-swe-agent.com/latest/quickstart/
**Confidence:** Medium — from docs snippets.
**Action:** A first-run wizard that detects missing config and prompts interactively is a good pattern. Better than "error: missing config file."

---

## Finding 5: Continue.dev — IDE Extension Model

**What:** Continue.dev installs as a VS Code/JetBrains extension. Onboarding is:
1. Install extension from marketplace
2. Extension UI guides model selection

No terminal interaction needed. Configuration lives in the IDE.

**Source:** Search results (indirect mentions).
**Confidence:** Low — no direct docs fetched.
**Action:** IDE-native tools have inherently simpler onboarding. CLI tools must compensate with excellent error messages and guided setup.

---

## Summary

| Tool | Steps to First Use | Config Required Before Start | Key Pattern |
|------|-------------------|------------------------------|-------------|
| Aider | 3 | API key inline | Zero-config CLI |
| Cursor | 2 | None (freemium) | IDE familiarity |
| OpenHands | 1-3 | Varies by path | Multiple entry points |
| mini-SWE-agent | 2-3 | Wizard on first run | Interactive setup |
| Continue.dev | 2 | Extension UI guides | IDE-native |

**Consensus:** The best tools get to first use in 2-3 steps. None require manual config file creation before first run.
