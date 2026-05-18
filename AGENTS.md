# Knowledge Base Agent Manual

You are maintaining a git-backed knowledge base.

Keep the repository simple.

Your job is to help preserve useful knowledge and give answers to help the users who interact with this keep on top of things.

## Working Style

Start from the files that already exist.

When the user gives new information, update the relevant existing file if there is one. If there is no obvious place to put it, just create it.

It's your job is to keep things orderly enough such that future agents can make sense of things. This means organizing files into folders, potentially nesting things. Exercise restraint here, you don't want to over-do things, only do what is necessary.

Avoid updating the AGENTS.md at all times, we need to keep this as pristine as possible for the sake of maintaining clarity.

## Intake Workflow

When a user gives new information or asks for an update:

1. Run `git status --short --branch`.
2. Check whether a remote exists with `git remote -v`.
3. If a remote exists and the working tree is clean, run `git pull --ff-only` before editing.
4. If the working tree has local changes, do not pull over them. First understand whether they are user work or agent work in progress.
5. Update the smallest set of files needed.
6. Record what happened in `.agent-local/sync-log.md`.
7. Run `git status --short --branch` again.
8. If the work is coherent and ready, commit it.
9. If a remote exists, pull again with `git pull --ff-only`, then push.
10. Record the commit and push result in `.agent-local/sync-log.md`.

## Sync Log

The sync log is important. It is the agent's local memory of what happened between the user's request, the working tree, and the remote.

Always keep a local sync log at:

```text
.agent-local/sync-log.md
```

Create it if it does not exist. Keep `.agent-local/` out of git.

Each entry should be short and dated. Record:

- what the user asked for
- whether a remote exists
- whether you pulled from the remote
- what files you changed
- whether you committed or pushed
- anything unusual, blocked, or risky

Example:

```text
2026-05-18T12:30:00+03:00
- Request: simplify AGENTS.md.
- Remote: none configured.
- Pull: skipped.
- Changed: AGENTS.md.
- Commit/push: not requested.
- Notes: kept repo lightweight.
```

## README

Once the knowledge base starts being used, always keep this file:

```text
README.md
```

`README.md` is where you should go first to understand this particular knowledge base.

Create it if it does not exist. Keep it under 100 lines. If it is getting long, tighten the writing instead of adding more.

It should explain the subject of the KB, what it is trying to preserve, what is where, how the current setup works, and any important context a future agent needs before making changes.

Update it when the knowledge base changes in a way that would confuse a future reader or agent. Keep it brief, current, and practical.

## Git Rules

Git is for recovery and history. Non-technical users should not need to touch it.

Agents must:

- check status before and after work
- avoid overwriting user changes
- pull before editing when a remote exists and the tree is clean
- commit coherent completed work without waiting for the user to micromanage Git
- push completed commits when a remote exists
- keep commits small and understandable
- never use force push
- never rewrite shared history
- never run destructive commands like `git reset --hard` unless explicitly instructed

If conflicts occur, stop and explain the conflicting files before editing further.

## Commit Messages

Use the following generally accepted standard for commit messages.

Preferred shape:

```text
<type>(<scope>): <subject>

Optional longer explanation when useful.
```

Use a scope when it helps, and omit it when it does not.

Good examples:

```text
docs: simplify agent instructions
```

```text
docs(sync): clarify pull and push flow
```

```text
fix: correct stale knowledge entry
```

Common types, following the Angular convention:

- `docs`: documentation or knowledge-base text
- `fix`: correction to existing content
- `feat`: a new capability or meaningful new knowledge area
- `refactor`: reorganizing existing content without changing its meaning
- `chore`: maintenance, cleanup, or repo setup
