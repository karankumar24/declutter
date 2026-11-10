<div align="center">

<img src="docs/banner.png" alt="declutter" width="720">

# Less code, same app

**Name a part of your codebase, and Claude clears out what it no longer needs. It proves each thing is unused first, and deletes nothing until you say so.**

<a href="#install"><img src="https://img.shields.io/badge/Claude_Code-skill-D97757?style=flat-square" alt="Claude Code skill"></a>
<a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue?style=flat-square" alt="MIT license"></a>

</div>

---

Codebases collect leftovers: a helper nothing calls since the refactor, a package nobody imports, a README section about a setting that's gone. Ask an agent to "clean up" and it either deletes something that was quietly still in use, or plays it safe and removes nothing. declutter makes it show proof for every removal and wait for your OK.

You type `/declutter src/billing`. Claude confirms which files that covers, searches, and comes back with:

```text
Remove
1. src/billing/legacy-invoice.ts: nothing imports it.
   rg -l "legacy-invoice" .  →  only the file itself.
2. date-fns-tz: installed, but never imported.
   rg "date-fns-tz" src scripts  →  no matches.
3. The README's "Billing" section explains BILLING_V1_ENDPOINT, which no code reads.

Asking first
- applyLegacyDiscount() isn't called anywhere, but it was added last week.
  Is it unfinished, or dead?
```

You say "1 to 3". Claude backs up what it's about to change, removes it, fixes the README, runs your type check, linter and the tests for that code, and tells you what went and what stayed.

## How it works

- **Scope first.** A folder, `branch`, `uncommitted`, `repo`, or a feature in plain words. On its own, `/declutter` means what this session added.
- **Proof for everything.** Each item shows the search it ran and what came back. If it can't prove something is unused, it stays.
- **Unsure means it asks.** Code nothing calls *yet*, a flag that's off everywhere, or anything that looks deliberate becomes a question. "It's old" never counts as proof.
- **Careful removal.** It backs up every file before changing it, puts things back if your checks fail, and never commits.

## Install

Paste this in your terminal:

```bash
DIR=$(mktemp -d)
git clone --depth 1 https://github.com/karankumar24/declutter.git "$DIR"
mkdir -p ~/.claude/skills
cp -r "$DIR/skills/declutter" ~/.claude/skills/
rm -rf "$DIR"
```

Then type `/declutter` in Claude Code. It only runs when you type it. Run the same lines again to update.

<details>
<summary>Or install it as a plugin</summary>

A plugin updates with one command (`claude plugin update declutter@declutter`), but its command becomes `/declutter:declutter`. Pick one way, not both.

```
/plugin marketplace add karankumar24/declutter
```

```
/plugin install declutter@declutter
```

</details>

## Your data

declutter is a set of instructions for Claude, not a program. It makes no network calls of its own, and its backups stay on your machine in `~/.claude/declutter-backups/` until you delete them. Claude follows it like any prompt, so read the list before you approve it. [SECURITY.md](SECURITY.md) has the details.

## Contributing

Issues and pull requests are welcome. [CONTRIBUTING.md](CONTRIBUTING.md) says what fits. MIT licensed.
