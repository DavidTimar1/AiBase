# The deploy engine

One Node program that takes the canonical and makes every AI tool on the machine match it. It has no build step and no dependencies beyond Node itself.

## Why it exists

The canonical is written once, in one tool's format. Each other tool wants different folders, file layouts, frontmatter and hook models. Translating by hand is slow and the copies drift. Worse, a deploy that reports success proves nothing about whether the tool can actually see what was copied.

## What it does

- **Translate.** It reads the canonical and writes each tool's version: copying where the format is shared, rewriting where it differs, and building the other tools' hook definitions from the single roster.
- **Deploy only what differs.** Files are compared by hash on both sides, so a repeat run with nothing changed writes nothing.
- **Preview by default.** Run with no subcommand, it reports the plan and writes nothing. Only an explicit `apply` writes. A flag that sometimes writes turns a dry run into a deploy by habit, so there isn't one.
- **One run at a time.** Each tool's folder is locked during a deploy, so two runs cannot interleave their writes.
- **Health checks.** A separate question from the deploy: did the right thing land, and does the tool see it? `health` checks the result on disk. The deploy's own clean report is not accepted as proof.
- **Project permission files.** It generates each project's permission settings from one rule set, so a project cannot quietly end up with broader access.
- **Drift detection.** The reference notes about each tool's behavior are checked against the installed tool's releases, so documentation does not silently go out of date when a tool changes.
- **Scheduled jobs.** The daily scan of session records and the expiry of deleted sessions are installed as timers by the same engine.
- **Removal is deliberate.** Anything removed from the canonical is removed from the tools in a tracked way. Unknown or hand-made files are kept and reported, not deleted.
- **Exit status tells the truth.** A failed step makes the run exit non-zero, so a script or an agent cannot read a partial deploy as a success.

## Design choices

- **One script, several subcommands.** They share the same canonical, settings and harness map, so one source of truth for all of them.
- **No hooks of its own.** Nothing here runs automatically in a session. It is run on purpose.
- **Write gate separate from planning.** The cost of a mistaken deploy is every tool misconfigured at once, so the write step is the one place with a deliberate command.
