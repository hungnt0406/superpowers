# Agent Model Source

## Decision

Custom `implementer` and `reviewer` subagents take their model and effort from their agent frontmatter files. Their dispatch templates must not include per-call model or effort fields. Intentional escalation changes the relevant agent file before redispatching.

## Context

A live `implementer` dispatch showed Sonnet even though `~/.claude/agents/implementer.md` specified `deepseek-v4-flash`. The dispatch template required an explicit model field, allowing the controller's `model: sonnet` override to bypass the configured model selector.

## Options considered

- Keep model fields in every dispatch template and rely on the controller to choose correctly.
- Make custom agent frontmatter the source of truth while retaining explicit models for generic subagents.
- Remove model selection entirely and let every subagent inherit the session model.

## Reasoning

The repository already provides `impl-model.sh` and `review-model.sh` to change custom-agent models. Explicit dispatch overrides defeat those controls and caused the observed mismatch. Generic subagents still need explicit models because they have no custom agent frontmatter.

## Impact

Normal implementation dispatches now use DeepSeek and review dispatches use the configured reviewer model (`gpt-5.6-sol` in the current agent file). Model changes remain hot-reloadable through the selector scripts. Stuck-task escalation is explicit and changes the agent file rather than silently overriding one dispatch. The currently installed marketplace copy must be refreshed before the change affects a new session.

## Next steps

Refresh the Superpowers plugin from the fork, start a new Claude Code session, and verify that an implementer dispatch reports `deepseek-v4-flash` and a reviewer dispatch reports `gpt-5.6-sol`.
