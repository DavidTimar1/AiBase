# Hooks on a second tool

The primary tool runs hooks as separate commands: each receives an event on standard input and answers with an allow, a refusal or extra context. Pi, a second coding tool I use, works differently. It loads extensions into its own process and exposes events to them, and some events are notify-only: the extension can show the user a message but cannot change what happens.

## Why carry the guards across

A guard that exists on one tool and not the other is a hole in the system. The agent works the same way on both, so it can cause the same harm on both. The aim is that the same rule gives the same answer wherever the agent runs.

## How it is done

- **One decision core, two wirings.** The logic that decides allow, ask or refuse is shared code. Each tool has a thin adapter that turns its own event into the core's input and the core's answer into its own response.
- **Where the tool can block, the guard blocks.** Tool-call events accept a refusal, so the delete guard, identity gate, permission guard and the rest refuse there too. Where the primary tool would show a prompt, Pi uses its own confirmation dialog, and refuses outright when there is no interface to ask in.
- **Where the tool can only notify, the guard degrades honestly.** The end-of-turn review cannot send a draft back for rework on Pi. It leaves a notice for the human instead. The documentation says which guards block on which tool, so nobody assumes more protection than exists.
- **Chat-start checks run once.** They run at session start only, as on the first tool.
- **Subagents are covered explicitly.** A subagent in Pi is a separate session that would not inherit the parent's extensions, so the extension registers itself as required for each child. A child has no interface, so a guard that would ask the parent refuses instead.
- **Behavior differences are written down**, in the code and the docs, rather than hidden. A missing capability is a known limit, not a silent gap.
