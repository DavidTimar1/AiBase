# The canonical

The canonical is the one place where the rules, agents, skills and hooks are written. Every AI tool on the machine gets its configuration from it. Nothing else is a source.

## Why one master copy

I use several AI coding tools. Each wants its own folder, its own file formats and its own settings. Maintained by hand, they diverge within a week: a rule fixed in one tool stays broken in another, and nobody can say which copy is right.

So there is a single canonical, authored in one format, and a deploy step that writes each tool's copy. The rules that follow from that:

- **Edit the canonical only.** A deployed copy is output, never input. A hand edit there is overwritten by the next deploy, and is a bug.
- **The canonical is source-only.** No tool is ever pointed at the repository itself; tools read only the deployed copies.
- **Machine settings live outside it.** What differs between machines (which tools are installed, which guards are on) is in a separate settings file. The repository ships only a worked example.
- **Every change says why.** The reason goes in the commit message, and a version file is bumped with each content change, so a deployed copy can be traced to a revision.

## What it holds

- **Standing rules**, a `CLAUDE.md` read at the start of every session.
- **Agents**: the role definitions. See [agents](agents/README.md).
- **Commands**: small user-invoked prompts. See [commands](commands/README.md).
- **Skills**: playbooks loaded on demand. See [skills](skills/README.md).
- **Hooks**: the runtime guardrails. See [hooks](hooks/README.md).

## The standing rules as a concept

The standing rules are a long instruction file with a clear structure. They do not repeat what the code says; they set the behavior that code cannot. The topics, and the reason for each:

- **Role of the main agent.** It is the manager, not the builder. This keeps the human talking to one accountable party, and keeps complex work from being done in a rush by whoever happens to be in the chat.
- **Challenging the human.** The agent must raise a concrete objection before complex or dangerous work, and say where it looked if it found none. This exists because an agreeable assistant is the most expensive kind.
- **When to dispatch and when to just do it.** Two questions (is it dangerous, is it complex) decide. Small work is done directly; dispatching for a rename wastes time.
- **How agents report.** Short, one subject per topic, every problem paired with a question and a recommendation. The human reads the latest message only.
  - **Every finished piece of work asks the human to confirm it** with a "Read?" question, and it comes back in every message until it is confirmed. A quick review of completed work cannot be skipped by accident.
  - **Every topic ends with a numbered question, and every decision offers lettered choices** (A, B, C) with the agent's recommendation. The human answers in a few characters, such as "3A, 4 read", even from a phone. A question keeps its number until it is answered, so an answer always lands on the question that was read.
- **Deciding and solving.** An agent that can solve a problem solves it. The human hears about a problem only when no workable fix exists, every fix breaks something, or the call is a business one.
- **Documents.** The code is the truth; a document says why, not what. A stale document is treated as a defect, because the next agent will believe it.
- **Evidence.** Claims about the outside world need a tool call in the same turn. Nothing is done until it was run.
- **Files and deletion.** Only delete what was named. Prefer moving. Scratch work goes in one ignored folder and is cleaned up.
- **Security.** Never print, send or log a credential or the owner's identity, even to check it.
- **Git.** The agent owns git so the human does not have to think about it; publishing to a public repository is the one event that needs explicit approval.

The text of these rules is not published here. The structure above is the point: each rule exists because of a specific failure that happened.
