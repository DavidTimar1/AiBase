# Skills

A skill is a short playbook an agent loads when a job calls for it. It holds the procedure, the rules, and the hard-won gotchas for one area, so they do not sit in every session's context. The standing rules say which skill to invoke before which kind of work.

Each skill has a one-line description that decides when it loads, and a source record stating where its content came from. Long reference material sits beside the skill and is read only when needed.

## The skills

- **content-retrieval.** Looking things up outside the conversation. It sets an order from most authoritative to least: local code search first, then library source, then official docs, then structured services, then rendered pages, and a plain fetch last. Reason: a quick fetch often returns a block page that looks like content.
- **data-safety.** Before changing the shape of a database holding data that cannot be recreated, and when setting up backups. Code comes back from git; data does not. Expand, migrate, then contract. Backups live outside the project.
- **debugging.** A fixed procedure for defects: establish what actually happens before proposing a cause, and verify the explanation. Reason: agents are fluent at inventing plausible causes.
- **deploy.** Moving a project's files to a destination by one of several transports, or rolling back. Reason: one tested path instead of ad hoc scripts copied between projects.
- **generate-media.** Generating an image from a text prompt through a small command-line tool rather than a standing server, so it costs nothing when unused.
- **housekeeping.** Listing, purging and restoring old agent sessions and pruning memories. Sessions pile up and hold history that should not live forever.
- **how-we-build.** The stack and the standard a user-facing interface is held to, including form-field rules. The designer decides against it.
- **i18n.** Rules for text a person reads: no hand-assembled sentences, no hand-formatted dates, always an explicit locale.
- **playwright-scrape.** Capturing pages that only exist after their scripts run, and extracting data from the rendered view.
- **project-init.** Audits a project for the baseline files every project should have and creates only what is missing. It never overwrites.
- **public-web-deliverables.** A gate before anything public ships: readable by people, discoverable by tools, fast, cheap to serve.
- **security.** Verify security against an attacker's intent. Covers secret handling, authentication, untrusted input, personal data and system access.
- **semble-codebase-search.** How to search a codebase by meaning with the least reading, so context stays small.
- **syncness.** The deploy engine for the whole canonical. See [syncness](syncness/README.md).
- **verification-safety.** What to check before and after edits, installs, builds and deploys, and what counts as proof. Enforces "nothing is done until it was run".
- **hooks.** How the hooks are configured and what to read before changing their behavior.
- **work-management.** The folder layout and brief format for work that has several steps or outlives a chat, so a new session can resume from files.

## Why skills and not one big instruction file

- **Context is paid for on every step.** A skill that is not needed is not loaded.
- **A skill can be wrong in one place.** It is fixed and redeployed without touching the rest.
- **It gives agents a specific procedure.** "Invoke the skill first" is a checkable instruction; "be careful with data" is not.
