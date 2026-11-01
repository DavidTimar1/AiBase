# Role agents

Each agent is a short instruction file that defines one role: what it owns, how it works, and what "done" means for it. The main agent dispatches them; they see only the brief they are given.

## The roles

- **Analyst.** Reads, never builds. Answers how the code behaves today, what a vendor's documentation really says, and whether a claim holds. It reports findings with their source and marks what it could not check.
- **Engineer.** The only role that builds above the small-work line. It works to an agreed design and written acceptance criteria, runs the tests that cover the change, and cleans up after itself.
- **Tester.** Proves a change works and looks for what it breaks. On a production release it is also the one that merges and deploys, because it is the one that checked. It is never the agent that wrote the change.
- **Architect.** Designs systems, data and infrastructure before the build, and produces a design, not code. It is used where a wrong choice costs a rebuild.
- **Designer.** Owns what a person sees: flows, layout, states, copy. Its screen spec becomes part of the acceptance criteria, and it checks the built screen.
- **Security.** An adversarial reviewer for anything touching authentication, permissions, untrusted input, personal data, secrets or system access.

## Why split the work this way

- **Separation of duties.** The agent that builds cannot also be the one that certifies. A builder reading its own code finds what it expected to find.
- **Small contexts.** Every step an agent takes re-sends its whole conversation. A narrow role with a short brief is cheaper and stays accurate longer than one agent holding the whole project.
- **Pinned models.** Each role names the model it usually needs. The main agent can override it per job when the work is harder or simpler.
- **No decisions below the CEO.** Agents report; the main agent decides. This keeps the acceptance criteria in one hand.
- **Analyst first.** Criteria are written after the analyst has read the code, so they rest on findings rather than on memory.

## How the main agent checks a report

Everything an agent says is treated as a claim.

- It looks at what the agent actually changed and where, not at the summary.
- It reads each commit message and asks whether it describes what the commit did.
- It confirms the work landed only in the agent's own copy of the project, and that the agent cleaned up: worktree merged or removed, scratch deleted or declared, task files current.
- It re-runs or re-reads the evidence. A report of "tested" is accepted when the test was run, not when it was described.
- If the agent got its own job wrong, it goes back to that same agent. A new job gets a fresh agent with a short brief, so a long context does not carry over.
- Two agents that could write at once get separate copies of the project. One writer per copy.
