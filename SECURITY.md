# Security

## What declutter does

declutter is a prompt. It ships no scripts and makes no network calls of its own. When you run it, Claude:

- reads files in your project and runs search and git commands to find leftovers
- copies anything it removes to `~/.claude/declutter-backups/<repo>/<timestamp>/` first, keeping relative paths
- removes only what you approve, then runs your project's own typecheck, lint and targeted tests
- never commits

The backups are plain copies of your files, readable by your user, and are never cleaned up automatically.

## Limits

These are instructions, and Claude follows them the way it follows any prompt. Nothing in this skill can stop a tool call. Read the proposal list before you approve it, and keep your work committed so git can bring things back too.

## Reporting a problem

If declutter removed something its rules say it must keep, or acted without your go, please open an issue with what happened. If it involves secrets or private code, report it privately instead: open this repository's **Security** tab and choose **Report a vulnerability**.
