# Contributing

Thanks for looking. declutter is one short SKILL.md, and it stays short on purpose: every line costs tokens on every run, and every rule in it is there because something went wrong without it.

## What fits

- **A bad removal.** declutter proposed or removed something that was still in use. Say what it was, how it was still alive (a dynamic import, a config reference, a caller in another repo), and what proof it showed. This is the most useful report there is.
- **A missed leftover** of a kind it should catch, with an example.
- **A clearer line.** Same rule, fewer words.

## What doesn't

- New rules that don't fix a concrete failure. Paste the failure in the issue.
- Anything that lets it remove something on a name, a pattern or its age alone.
- Anything that lets it commit, or skip the wait for your go.

## Testing a change

There's no test suite; the test is a real run. Copy your edited `skills/declutter` into `~/.claude/skills/`, run `/declutter` on a scope you know well, and check that the proposals and proof make sense. Say in the PR what you ran it on.

## Style

Plain words, short sentences, no jargon. Write to Claude in the first person, the way the file already does.
