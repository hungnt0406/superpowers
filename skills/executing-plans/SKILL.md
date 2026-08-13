---
name: executing-plans
description: Use when you have a written implementation plan to execute in a separate session with review checkpoints
---

# Executing Plans

## Overview

Load plan, review critically, execute all tasks, report when complete.

**Announce at start:** "I'm using the executing-plans skill to implement this plan."

**Note:** Tell your human partner that Superpowers works much better with access to subagents (Claude Code, Codex CLI, Codex App, Copilot CLI, and Gemini CLI all qualify; see the per-platform tool refs in `../using-superpowers/references/`). If subagents are available, use superpowers:subagent-driven-development instead of this skill.

## The Process

### Step 1: Load and Review Plan
1. Ensure an isolated workspace: use superpowers:using-git-worktrees to create one or verify the existing one
2. Read plan file
3. Review critically - identify any questions or concerns about the plan
4. If concerns: Raise them with your human partner before starting
5. If no concerns: Create todos for the plan items and proceed

### Step 2: Execute Tasks

For each task:
1. Mark as in_progress
2. Follow each step exactly (plan has bite-sized steps)
3. Run verifications as specified
4. Mark as completed

### Step 3: Complete Development

After all tasks complete and verified:
- Announce: "I'm using the finishing-a-development-branch skill to complete this work."
- **REQUIRED SUB-SKILL:** Use superpowers:finishing-a-development-branch
- Follow that skill to verify tests, present options, execute choice

## Decision Records

When you make a non-trivial call during execution, record it as a dated markdown
file under `docs/superpowers/`, slugged like plans/specs:
`docs/superpowers/<folder>/YYYY-MM-DD-<slug>.md`. One file per decision.

- **`decisions/`** — calls you made on your own (filled a small spec gap, picked
  a name/structure, chose between equivalent approaches).
- **`need-review/`** — a plan or scope change the human approved mid-execution
  (log it so they can re-review later), or a call you're unsure about. A verbal
  "go ahead" on a real change is a `need-review/` record, not something to leave
  only in the chat log.

**Every decision file uses this format:**

- **Decision** — State exactly what you decided.
- **Context** — Briefly explain the situation or problem.
- **Options considered** — Mention the main alternatives.
- **Reasoning** — Explain why you chose this option.
- **Impact** — What the decision means: benefits, risks, cost, timing, etc.
- **Next steps** — What happens next and who needs to do what.

**`need-review/` files add, after Next steps:**

- **Why this needs review** — why you're flagging it: uncertainty, scope/cost,
  hard to reverse, security/data risk, or "approved verbally, logged for re-review."
- **Trade-off** — the core tension being balanced (e.g. speed vs. flexibility,
  cost vs. correctness).
- **Alternatives — pros & cons** — for each main alternative, list its pros and
  cons side by side, so the human can weigh them without reconstructing the space.

## When to Stop and Ask for Help

**STOP executing immediately when:**
- Hit a blocker (missing dependency, test fails, instruction unclear)
- Plan has critical gaps preventing starting
- You don't understand an instruction
- Verification fails repeatedly

**Ask for clarification rather than guessing.**

## When to Revisit Earlier Steps

**Return to Review (Step 1) when:**
- Partner updates the plan based on your feedback
- Fundamental approach needs rethinking

**Don't force through blockers** - stop and ask.

## Remember
- Review plan critically first
- Follow plan steps exactly
- Don't skip verifications
- Reference skills when plan says to
- Stop when blocked, don't guess
- Never start implementation on main/master branch without explicit user consent
