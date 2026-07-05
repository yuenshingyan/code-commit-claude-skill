---
name: code-commit
description: >-
  Create one git commit per changed file with an auto-generated message
  that matches the repo's existing style. No Co-Authored-By, no Claude
  attribution.
when_to_use: >-
  TRIGGER when the user asks to "commit each file separately",
  "one commit per file", "one msg for one file",
  "commit without using your name", "separate commits",
  "per-file commits", "commit files individually",
  or similar per-file commit patterns.
disable-model-invocation: true
effort: max
allowed-tools:
  - Bash(git status:*)
  - Bash(git diff:*)
  - Bash(git log:*)
  - Bash(git add:*)
  - Bash(git commit:*)
disallowed-tools:
  - Agent
---

# code-commit

One commit per file. Message matches the target repo's existing style. Never adds attribution trailers.

## When to invoke

The user asks something like:

- "generate commit msg and commit for each file"
- "commit each file separately"
- "one msg for one file"
- "commit without using your name"
- "separate commits"
- "per-file commits"
- "commit files individually"

If they want a single bundled commit, use the normal commit flow instead.

## Procedure

1. **Find changes in the current repo only.** Run `git status --short` in the primary working directory. Do not scan or commit in other repos, even if additional working directories are configured.

2. **Sanity check.** If more than 20 files are changed, confirm with the user before proceeding — they may prefer grouped commits instead.

3. **Detect the repo's commit-message style** from recent history:

   ```
   git log --oneline --no-merges -10
   ```

   Observe capitalization of the first word, tense (usually imperative), typical length, and any prefixes/scopes (Conventional Commits, JIRA tags, etc.). Match what's there. Don't invent a new style.

4. **For each file to commit (modified / added / deleted / renamed / untracked):**

   a. **Read the diff:**
      - Tracked, unstaged changes: `git diff -- <path>`
      - Already-staged changes: `git diff --cached -- <path>`
      - New untracked files: `git diff --no-index -- /dev/null <path>` shows the full file as an added diff. (It exits 1 when the file has content — that is expected, not an error.)
      - Binary files (images, fonts, etc.): no meaningful diff — describe the action (add/update/remove) and the filename.
      - Renames: `git status` shows `R old -> new`. Stage both paths: `git add -- <old> <new>`.

      Never guess the contents.

      If a hunk is ambiguous on its own, get more context of *that same file* via git — `git diff --function-context -- <path>` or `git diff -U30 -- <path>` — or check how past commits to it were worded with `git log --oneline -- <path>`. Do not open other files.

   b. Draft a concise one-line subject (≤ 72 chars) describing *what changed in that file*, matching the repo's style. Each commit stands alone — don't narrate the broader task.

   c. Stage only that file: `git add -- <path>`. **Never** `git add -A` or `git add .`.

   d. Commit: `git commit -m "<message>"`.

   e. Move to the next file.

5. **Prefer committing new files before files that reference them** so each commit is independently valid where possible.

6. **After all commits, verify the repo is clean** with `git status --short`. Report resulting commit hashes and messages.

## Read scope

- **The per-file diff is the sole source of truth** for each commit message. Do not open any other file "for context".
- Only inspect paths that `git status --short` listed as changed.
- Do not explore the repo structure or read configs, READMEs, or related source files. Commit-message style comes from `git log --oneline --no-merges -10` only.

## Message rules

- **No `Co-Authored-By:` trailer.** Not "Claude", not anyone.
- **No `Signed-off-by:`** unless the user asked.
- **Don't mention Claude, AI, or this tool** in the message.
- One subject line. No body unless the user asks.
- Match the repo's observed style (capitalization, tense, scope prefixes).

## Safety

- **Secrets**: flag files like `.env`, `*.pem`, `credentials.json`, `*.p12`, private keys, or anything in a `secrets/` folder before committing. Skip and ask.
- **No `--no-verify`.** Let pre-commit hooks run. If a hook fails, the commit didn't happen. Note that linters/formatters may have modified the file — re-read the diff, revise the commit message if the change is now different, re-stage, and commit again. Do **not** `--amend` after a hook failure; that would modify the *previous* commit.
- **No `--force`, no `--amend`, no destructive flags** unless explicitly requested.
- **Don't push.** Committing only.
- **Don't edit git config.**

## What this skill does NOT do

- Push to remote.
- Open pull requests.
- Bundle related changes into one commit.
- Sign commits.
- Rebase or squash commits.
- Add issue or ticket references unless the user provided them.
