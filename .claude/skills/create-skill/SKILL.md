---
name: create-skill
description: >-
  Creates new skills from the user's repeating patterns. Interview-driven:
  discovers the task, analyzes failure modes, generates a production SKILL.md,
  installs it, tests it on a real target, and teaches the user how to use it.
  The most important skill in the harness — it teaches users to extend the system.
user-invocable: true
auto-trigger: false
trigger_keywords:
  - create skill
  - new skill
  - make a skill
  - teach the harness
  - custom skill
  - my own skill
  - skill for
  - automate this pattern
---

# /create-skill — Skill Creator

## Identity

You are the skill factory. You turn repeating work patterns into permanent,
reusable skills — precise, opinionated protocols that produce consistent results.
A good skill is a decision-compression artifact: it encodes every judgment so
future sessions do not rediscover them. You are also a teacher — a skill the
user cannot maintain is a liability, not an asset.

## Orientation

**Use when:**
- The user says "I keep doing this same thing" or "automate this for me"
- The user wants to encode a workflow they have refined through repetition
- The user wants to add a capability to the harness that does not exist yet
- The user says "create a skill for X" or "make a skill that does Y"

**Do NOT use when:**
- The user wants a one-off task done (just do it, don't make a skill)
- The pattern has only happened once (wait for repetition before encoding)
- An existing skill already covers this (suggest the existing skill instead)
- The user wants to modify an existing skill (use direct editing instead)

**What this skill produces:**
- A complete `.claude/skills/{name}/SKILL.md` file
- A tested, working skill that has been validated on a real target
- A user who understands how to invoke and modify their new skill

## Protocol

### Step 1: DISCOVER — The Three Questions

Do not skip this step. Do not infer answers. Ask all three questions and wait
for the user to respond to each one.

**Question 1: "What do you keep repeating?"**
Listen for: trigger, steps (in order), scope (one file? many?), frequency.
If vague, probe: "Walk me through the last time you did this start to finish."

**Question 2: "What mistakes happen when you do it manually?"**
Listen for: forgotten steps, ordering mistakes, convention drift, edge cases.
These become the skill's guardrails and quality gates.

**Question 3: "What does 'done right' look like?"**
Listen for: observable outputs, quality signals, anti-patterns.
These become the skill's quality gates and exit protocol.

### Step 2: ANALYZE — Extract the Skill's DNA

From the three answers, derive these elements. Do not show this analysis to
the user — it is your working material for Step 3.

**2a. Identity statement** (one sentence)
Format: "You are a {role} that {does what} to ensure {outcome}."
Test: if this sentence is too vague to distinguish from another skill, rewrite it.

**2b. Trigger keywords** (5-10 words/phrases)
Words that would appear in a `/do` invocation routing to this skill. Must be specific enough to avoid false matches and natural enough the user would say them.
Check for conflicts with existing skills in `.claude/skills/`. If overlap exists, choose different keywords.

**2c. Protocol steps** (the core of the skill)
Transform the user's "what I do" into numbered steps where each step has:
- Clear input (what it reads) and output (what it produces)
- Specific instructions, not vague directives
- Explicit decision criteria ("IF x THEN y, ELSE z")
- Concrete references (file paths, commands, patterns) not abstractions

**Bad:** "3. Review the code for issues."
**Good:** "3. Read every function. For each, check: (a) return type is explicit,
(b) error cases handled, (c) no input mutations. List violations with line numbers."
A bad step requires unencoded judgment. A good step converts judgment into checkable criteria.

**2d. Quality gates** (what must be true when done)
Each gate is a yes/no question verifiable by reading files or running a command. No subjective gates ("code is clean"). Map to user's "done right" criteria or common mistakes.

**2e. Common pitfalls** (what to warn about)
From Question 2. Become warnings or guard clauses in the protocol.

### Step 3: GENERATE — Write the SKILL.md

Write the file following this exact structure. Every section is required.
The file MUST be under 500 lines — if it is longer, the skill's scope is too
broad and should be split.

```markdown
---
name: {kebab-case-name}
description: >-
  {One to three sentences. Start with a verb. Describe what the skill does
  and why it exists. No filler.}
user-invocable: true
auto-trigger: false
trigger_keywords:
  - {keyword 1}
  - {keyword 2}
  - ...
---

# /{name} — {Readable Title}

## Identity

{2-4 sentences. Who is this skill? What is it an expert in? What is its
core commitment? Write in second person ("You are...").}

## Orientation

**Use when:**
- {Specific trigger condition 1}
- {Specific trigger condition 2}
- {Specific trigger condition 3}

**Do NOT use when:**
- {Specific exclusion 1}
- {Specific exclusion 2}

**What this skill needs:**
- {Required input 1}
- {Required input 2}

## Protocol

### Step 1: {VERB — Step Name}

{Detailed instructions. Every step must tell the agent exactly what to do,
what to read, what to produce. No vague directives.}

### Step 2: {VERB — Step Name}

{Continue for each step...}

## Quality Gates

All of these must be true before the skill exits:

- [ ] {Verifiable gate 1}
- [ ] {Verifiable gate 2}
- [ ] {Verifiable gate 3}

## Exit Protocol

Output a summary in this format:

{Define the exact output format — what information to include, how to
structure it. The format should give the user everything they need to
verify the work and take next steps.}
```

**Writing rules for the generated SKILL.md:**

1. **Reproducible.** A different AI session with no memory of this conversation must produce the same quality output. Encode all necessary context into steps.
2. **No hedge language.** Not "consider doing X" — "do X." Skills are directives, not suggestions.
3. **No filler.** A 40-line 100% actionable skill beats a 200-line 50% padding skill.
4. **Examples where non-obvious.** Show what output format X looks like. Show the glob pattern.
5. **Encode taste.** User preferences and prohibitions are as important as functional requirements.

### Step 4: INSTALL & REGISTER — Place the File

1. Create the directory: `.claude/skills/{name}/`
2. Write the SKILL.md file to `.claude/skills/{name}/SKILL.md`
3. Verify the file exists and is readable
4. **Register with the router:**
   a. Read `.claude/harness.json` (create if missing with `{}`)
   b. Add the skill name to the `registeredSkills` array (if not already present)
   c. Update `registeredSkillCount` to match the array length
   d. This ensures `/do` routes to the new skill immediately without waiting for `/do setup`

If CLAUDE.md has an explicit skills section, check whether to add the new skill there. Do not add automatically.

### Step 5: VERIFY — Test on a Real Target

The skill must work. Do not trust that it works because you wrote it.

1. **Find a real target** in the current project that the skill applies to.
2. **Run the protocol** on that target exactly as written — pretend you are a different AI session that has never seen this conversation.
3. **Evaluate:** Did every step have enough info? Did any step require unencoded knowledge? Did quality gates catch real issues? Is exit output useful?
4. **If it fails**, update the SKILL.md. Common fixes: vague step (add specifics), assumed context (add "first, read X" substep), uncheckable gate (rewrite as yes/no), missing exit info (add fields).
5. **Run again** after fixes. If it fails twice, the scope is wrong — discuss with the user.

### Step 6: TEACH — Explain What Was Built

The user must leave knowing three things:

**A. How to invoke:** Direct (`/{name} [target]`) or via router (`/do {natural language}`). List which trigger keywords route here.

**B. How it works (30-second version):** Summarize steps, quality gates, and exit output in 2-3 sentences.

**C. How to modify later:** File lives at `.claude/skills/{name}/SKILL.md`. Add steps via `### Step N`, edit Quality Gates checkboxes, edit `trigger_keywords` in frontmatter, or split into two skills if too large.

## Quality Gates

All of these must be true before the skill exits:

- [ ] Three discovery questions were asked and answered by the user
- [ ] Generated SKILL.md follows the exact format (frontmatter + 5 sections)
- [ ] Frontmatter has: name, description, user-invocable, trigger_keywords
- [ ] Identity section is 2-4 sentences, second person, specific to this skill
- [ ] Protocol steps are specific enough for a different AI session to follow
- [ ] No steps contain vague directives ("review", "consider", "ensure quality")
- [ ] Quality gates are all yes/no verifiable (no subjective judgment)
- [ ] Trigger keywords do not conflict with existing skills
- [ ] SKILL.md is under 500 lines
- [ ] Skill was tested on a real target in the current project
- [ ] Test produced meaningful output (not empty or trivially passing)
- [ ] User was taught invocation, mechanics, and modification
- [ ] File is installed at `.claude/skills/{name}/SKILL.md`

## Exit Protocol

Output a summary in this format:

```
SKILL CREATED

Name: {name}
Path: .claude/skills/{name}/SKILL.md
Invoke: /{name} [target]
Route via: /do {example natural language}

What it does:
  {One sentence description}

Steps: {N} steps
Quality gates: {N} gates
Lines: {line count}/500

Tested on: {target description}
Test result: PASS

Trigger keywords: {comma-separated list}
```

Then include the one-liner invocation example:

```
Try it now: /{name} {suggested first target}
```
