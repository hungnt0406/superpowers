# Route Reviews to GPT Reviewer

## Decision

Route Superpowers task reviews, scoped re-reviews, and final branch reviews to the custom `reviewer` agent. Set its default to `gpt-5.6-sol` with `effort: high`, and keep the agent file as the single source of truth for reviewer model and effort.

## Context

Superpowers previously dispatched review work to `general-purpose` and selected models per dispatch. The local Claude Code setup exposes GPT model IDs through CLIProxyAPI and needs one command to change the reviewer model without editing each skill template.

## Options considered

- Keep `general-purpose` and select the reviewer model in every dispatch.
- Add one custom `reviewer` agent with a shared model toggle.
- Split task review and final review into separate agents.

## Reasoning

One custom agent gives every review path the same read-only constraints and makes model changes immediate through one file. `gpt-5.6-sol` at high effort matches the requested quality level without adding a second agent or duplicate scripts.

## Impact

Reviews use a consistent GPT-backed reviewer and no longer inherit or override the controller session model. GPT model IDs work only when Claude Code is launched through the configured local proxy; plain Anthropic sessions cannot resolve them.

## Next steps

Review this decision and change `~/.claude/agents/reviewer.md` or run `~/.claude/agents/review-model.sh <model-id>` if a different reviewer model is preferred. Publish the fork and update the installed plugin only after reviewing the diff.

## Why this needs review

This is a cross-cutting model-routing choice approved verbally and logged for re-review. It also creates a runtime dependency on the local proxy for all Superpowers reviews.

## Trade-off

Consistent, high-quality GPT reviews versus portability to Claude Code sessions that bypass the local proxy.

## Alternatives — pros & cons

### Keep `general-purpose` with per-dispatch model selection

- **Pros:** Works without a custom agent; each review can use a different model.
- **Cons:** Repeats routing logic, makes model toggling inconsistent, and risks accidental session-model inheritance.

### One custom `reviewer` agent

- **Pros:** One source of truth, one toggle command, consistent review constraints and effort.
- **Cons:** A GPT default requires the local proxy for every review dispatch.

### Separate task and final reviewers

- **Pros:** Independent cost and effort tuning for small reviews and final branch review.
- **Cons:** More files and routing rules than the current need justifies.
