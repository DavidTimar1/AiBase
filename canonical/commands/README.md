# Commands

A command is a short prompt the human can trigger by name. It is only run when asked; the model never invokes it by itself.

## The rules-reload command

The one command here re-reads the standing rules into the current session and tells the agent to follow them from now on, overriding anything already under way that conflicts.

### Why it exists

In a long session, early instructions lose weight. The agent has not forgotten them, but newer text outranks them in practice. When the human sees drift (a skipped check, a role ignored), the cheapest fix is to put the rules back in front of the model rather than to restart the chat and lose the context.

Two design choices:

- **Human-triggered only.** An agent that could reload its own rules could also decide not to. The human decides when the rules need restating.
- **Rules win over work in progress.** The command says so in plain words, so a half-finished plan does not outrank the rule it breaks.
