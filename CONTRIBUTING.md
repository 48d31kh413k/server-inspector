# Contributing to Server Inspector

Thank you for considering a contribution! 🎉

## How to contribute

### Reporting bugs

1. Search [existing issues](https://github.com/48d31kh413k/server-inspector/issues) to avoid duplicates.
2. Open a new issue and include:
   - A clear title and description
   - Steps to reproduce
   - Expected vs actual output
   - OS and Bash version (`bash --version`)

### Suggesting enhancements

Open an issue with the label `enhancement` describing:
- What you'd like added or changed
- Why it would be useful
- Any relevant examples

### Submitting a pull request

1. Fork the repository and create your branch from `main`:
   ```bash
   git checkout -b feature/my-feature
   ```
2. Make your changes, keeping code style consistent with the existing script.
3. Ensure ShellCheck passes with no errors:
   ```bash
   shellcheck server_info.sh
   ```
4. Update `CHANGELOG.md` under `[Unreleased]` describing your change.
5. Open a pull request against `main` with a clear description of what you changed and why.

## Code style

- Follow [Google Shell Style Guide](https://google.github.io/styleguide/shellguide.html).
- Use `set -euo pipefail` at the top of every script.
- Provide graceful fallbacks when commands may not exist (use `command_exists`).
- Keep output human-readable — use the existing `section` / `info` / `warn` helpers.

## ShellCheck

All shell scripts are linted with [ShellCheck](https://www.shellcheck.net/).
You can install it locally:

```bash
# Ubuntu / Debian
sudo apt-get install shellcheck

# macOS
brew install shellcheck
```

Then run:

```bash
shellcheck server_info.sh
```

## License

By contributing, you agree that your contributions will be licensed under the
[MIT License](LICENSE).
