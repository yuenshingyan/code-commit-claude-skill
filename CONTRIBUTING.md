# Contributing

Thanks for your interest in improving this skill.

## Reporting bugs

Open an issue using the **Bug report** template. Include:

- What you asked Claude Code to commit (number of files, repo setup)
- What went wrong (wrong message style, file skipped, hook failure not handled, secrets not flagged)
- Your Claude Code version (`claude --version`)

## Suggesting features

Open an issue using the **Feature request** template. Describe the use case and any alternatives you considered.

## Making changes

The skill is defined entirely in **SKILL.md**. Edit this to change the commit procedure, message rules, or safety checks.

### Testing locally

1. Clone the repo into your skills directory:
   ```bash
   git clone https://github.com/yuenshingyan/code-commit-claude-skill ~/.claude/skills/code-commit
   ```
2. Make your changes to `SKILL.md`.
3. Open any git project in Claude Code, make some changes, and ask for per-file commits.
4. Verify that each file gets its own commit with a style-matching message.

### Pull requests

- Keep PRs focused — one change per PR.
- Test that the commit procedure works correctly for modified, added, deleted, and renamed files.
- Update `CHANGELOG.md` with a summary under an "Unreleased" heading.

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
