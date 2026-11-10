<div align="center">

<img src="docs/banner.png" alt="declutter" width="720">

# Less code, same app

**Name a part of your codebase, and Claude clears out what it no longer needs: dead code, unused files and packages, stale docs. It proves each one is dead first, and deletes nothing until you say so.**

<a href="#install"><img src="https://img.shields.io/badge/Claude_Code-skill-D97757?style=flat-square" alt="Claude Code skill"></a>
<a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue?style=flat-square" alt="MIT license"></a>

</div>

---

Codebases grow leftovers: a helper nothing calls since the refactor, a package nobody imports, a README section about a flag that was removed in spring. Ask an agent to "clean up" and it either deletes things that were quietly still in use, or plays it safe and removes nothing. declutter makes it show proof for every removal and wait for your go.

You type `/declutter src/billing`. Claude confirms the scope, does its digging, then proposes:

```text
Scope: src/billing/ (23 files), plus package.json for its dependencies.
Out of scope: tests outside src/billing.
2 files have uncommitted changes; only a backup can recover those.

Remove (most certain first)
1. src/billing/legacy-invoice.ts: no importers.
   rg -l "legacy-invoice" .  →  only the file itself. No dynamic imports, not in any config.
2. date-fns-tz in package.json: never imported.
   rg "date-fns-tz" src scripts  →  no matches.
3. README "Billing" section names BILLING_V1_ENDPOINT, which no code reads anymore.
   rg "BILLING_V1_ENDPOINT"  →  README.md only. Rewrite the section to match the code.

Asking first
- applyLegacyDiscount() has no callers yet, but it landed last week with a TODO. Unfinished, or dead?
- FEATURE_PRORATION is off in every environment. Remove the flag and its branch, or keep it?
```

You approve 1–3 and say "keep the flag". Claude copies the removed files to `~/.claude/declutter-backups/<repo>/<timestamp>/`, removes them, rewrites the README section, runs your typecheck, lint and the tests for what it touched, and reports what went and what stayed.

## How it works

- **Scope first.** Empty means what this session added. It also takes `repo`, `uncommitted`, `branch` (the diff against main), a path or glob, or a feature in plain words. It tells you the exact files and git range, and waits. If your words allow a narrow and a wide reading, it takes the narrow one.
- **Big scopes get a survey.** Over about 40 files, it counts candidates by category first and lets you narrow down. For `repo`, it checks eight kinds of leftovers, each with the command it ran: unreachable code, test-only exports, settings no environment sets, stored fields nothing reads, unused packages, orphaned scripts and assets, stale docs, and stale local files.
- **Proof before proposing.** Every candidate comes with the command and what it returned. It tries to prove a candidate is still alive first: dynamic imports, config references, string-built paths, callers outside the repo. If it can't prove something is dead, it stays.
- **Questions, not guesses.** Something with no callers *yet*, a feature flag switched off everywhere, a shim or guard that looks deliberate: these become questions. Age alone is never evidence.
- **Careful removal.** Everything removed is copied to a backup folder first. It runs your project's own checks afterwards and puts things back if something breaks. It never commits.

## Install

Copy the skill into your personal skills folder:

```bash
DIR=$(mktemp -d)
git clone --depth 1 https://github.com/karankumar24/declutter.git "$DIR"
mkdir -p ~/.claude/skills
cp -r "$DIR/skills/declutter" ~/.claude/skills/
rm -rf "$DIR"
```

For one project only, copy it into that repo's `.claude/skills/` instead. If Claude Code was open and `~/.claude/skills` is new, restart it once.

**Prefer updates through Claude Code's plugin system?** Install it as a plugin instead (one way or the other, not both). The command then becomes `/declutter:declutter`:

```
/plugin marketplace add karankumar24/declutter
```

```
/plugin install declutter@declutter
```

## Usage

```
/declutter                 # what this session added
/declutter src/billing     # a folder
/declutter branch          # everything this branch changed vs main
/declutter the old export feature
/declutter repo            # the whole repo, survey first
```

It only runs when you type it. Claude won't start it on its own.

## Your data

declutter is a set of instructions. It ships no code and makes no network calls of its own. It reads your project with the same tools Claude always uses, and the backups stay on your machine in `~/.claude/declutter-backups/`. Backups are not cleaned up automatically, so delete them when you're sure. [SECURITY.md](SECURITY.md) has the details.

## Good to know

- Claude follows it the way it follows any prompt. The proof, the wait for your go and the backups are instructions, not hard locks, so read the list before you approve it.
- Uncommitted changes can only come back from its backup folder, not from git. It tells you how much is uncommitted before it starts.
- It runs the targeted tests for what it touched, not your whole suite. Run the full suite yourself before you commit.
- It's written for Claude Code (`$ARGUMENTS`, AskUserQuestion), but it's a plain SKILL.md, so other agents that read skills can use it with small changes.

## Contributing

Issues and pull requests are welcome. [CONTRIBUTING.md](CONTRIBUTING.md) says what fits. MIT licensed.
