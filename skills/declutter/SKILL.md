---
name: declutter
description: Clears out whatever a scope no longer needs (dead code, unused files and packages, stale docs and comments, needless complexity), and deletes nothing until you say so
disable-model-invocation: true
argument-hint: "a path, the repo, a feature, uncommitted, branch; empty means this session"
---

Goal: the scope in `$ARGUMENTS` ends up holding only what it needs: no dead code, unused files or packages, stale docs or comments, jargon, redundancy or needless complexity, and the project still works.

**Scope first.** Work out what I meant: empty is what this session added (if it changed nothing yet, ask what to look at); `repo`, `uncommitted`, `branch` (diff vs main), a path or glob resolve directly; for a feature or subsystem, find it in the repo. Tell me the concrete paths or git range, the file count, what's out of scope, and how much is uncommitted (only a backup can recover that), then wait. If my words allow a narrow and a wide reading, take the narrow one and mention the wide one. Never change anything outside the scope; you can name a problem out there in one line at the end.

**Big scopes:** over ~40 files, survey first (counts and categories, no file list), let me narrow, then read deeply. Read-only subagents are fine; only you remove things. For `repo` scope, the survey covers all of these, shown as a table with the command and count for each (say SKIPPED and why if one can't run):
1. code the app never reaches from its entry points
2. exports used only by tests or nothing
3. settings the code reads that no deployed environment sets (names, never values)
4. stored fields nothing reads, or reads only to refill their own edit form
5. packages listed but never imported
6. scripts, pages, assets or workflows nothing points to
7. docs and comments naming things that no longer exist
8. ignored or duplicate local files that are stale leftovers (not ones a tool regenerates; prove what writes it)

**Prove before proposing.** Show the command and what it returned for every candidate, and try to prove it's still alive first: dynamic imports, config references, string-built paths, callers outside the repo, mentions only in docs or scripts. Can't prove it dead, it stays. No callers *yet* means unfinished: ask. Switched off everywhere, or saved but never read: ask. Outside this session's own work, anything that looks deliberate (a shim, a flag, a guard, a duplicate helper) is a question, not a candidate, and age alone is never evidence.

**Ask, then wait for a go.** Ask what you're unsure about with your question tool (AskUserQuestion in Claude Code), recommendation first. Then show the list, most certain first, each with its proof, in batches small enough to actually read. Remove exactly what I approve and don't re-argue the rest.

**Remove carefully.** Before you delete or edit a file, copy the original to `~/.claude/declutter-backups/<repo>/<timestamp>/` with relative paths kept, and tell me the folder. Remove packages with the project's package manager so the lockfile stays in sync. Rewrite stale docs to match the code. Afterwards run the project's own typecheck, lint and the targeted tests for what you touched; not the whole test suite unless I ask, because on a big project that is slow and heavy. If something breaks, put it back. Never commit.

**Finish** with what went, what stayed and why, where the copies are, and the real output of the checks you ran. When in doubt, keep it and flag it.
