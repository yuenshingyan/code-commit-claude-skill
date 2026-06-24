# code-commit

A Claude Code skill that creates one git commit per changed file with an auto-generated message matching the repo's existing style. No Co-Authored-By trailers, no Claude attribution.

## Features

- **Per-file commits**: Each changed file gets its own commit, staged and committed individually
- **Style matching**: Reads the repo's recent `git log` to match capitalization, tense, prefixes, and scope conventions
- **Multi-repo support**: Detects and handles multiple repos in a single workspace
- **Safety checks**: Flags secrets (`.env`, `*.pem`, `credentials.json`), respects pre-commit hooks, never uses `--force` or `--amend`
- **No attribution**: No `Co-Authored-By`, `Signed-off-by`, or AI mentions in commit messages
- **Sanity guard**: Confirms with the user before proceeding when more than 20 files are changed

## Install

Clone this repo directly into your Claude Code skills directory:

**macOS / Linux:**

```bash
git clone https://github.com/yuenshingyan/code-commit-claude-skill ~/.claude/skills/code-commit
```

**Windows (PowerShell):**

```powershell
git clone https://github.com/yuenshingyan/code-commit-claude-skill "$env:USERPROFILE\.claude\skills\code-commit"
```

That's it. Claude Code auto-discovers skills in `~/.claude/skills/` (`%USERPROFILE%\.claude\skills\` on Windows).

### Update

**macOS / Linux:**

```bash
cd ~/.claude/skills/code-commit && git pull
```

**Windows (PowerShell):**

```powershell
cd "$env:USERPROFILE\.claude\skills\code-commit"; git pull
```

### Uninstall

**macOS / Linux:**

```bash
rm -rf ~/.claude/skills/code-commit
```

**Windows (PowerShell):**

```powershell
Remove-Item -Recurse -Force "$env:USERPROFILE\.claude\skills\code-commit"
```

## Usage

In any git project with uncommitted changes, use one of:

- "commit each file separately"
- "one commit per file"
- "per-file commits"
- "commit without using your name"

Claude will read each diff, draft a style-matching commit message, stage the file, and commit it. After all commits, it reports the resulting hashes and messages.

## License

MIT
