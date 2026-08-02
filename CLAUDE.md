# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository State

This repository is an unimplemented scaffold. The entire tracked working tree is four files:

| File | Contents |
| --- | --- |
| `README.md` | Two lines: the repo name and `WIP`. No stated purpose or scope. |
| `LICENSE` | GNU AGPL-3.0. |
| `.gitignore` | GitHub's upstream Python template, unmodified. |
| `renovate.json` | Fleet-wide Renovate policy. |

There is no source code, no test suite, no package manifest, no lockfile, no CI workflow, and no `.github/` directory. Git history is two commits (`ba95815` initial scaffold, `aebcd1c` Renovate policy), and GitHub detects no language.

Consequences for any task here:

- Do not search for an existing module, entry point, or test to extend. Nothing is there.
- Do not run or propose build, test, lint, format, or type-check commands. No toolchain is committed, so any such command would be invented. Establish the intended stack and its commands with the user before writing them down.
- Neither the repository name nor the README states what this project does. Ask rather than infer a purpose from the name.

## Committed Decisions

These are the only choices the repository has actually made. Treat them as constraints on new work.

- **License is AGPL-3.0.** New source files and any added dependency must be AGPL-3.0 compatible.
- **Python is the intended language.** The sole evidence is `.gitignore`, which is GitHub's Python template and covers pip, uv, Poetry, PDM, pixi, pytest, mypy, and Ruff. It expresses no preference among them, so package manager, test runner, and linter remain undecided.
- **`renovate.json` is fleet-standardized, not repo-tuned.** Commit `aebcd1c` applied `group:allNonMajor`, `rebaseWhen: conflicted`, `prConcurrentLimit: 10`, and the `gitIgnoredAuthors` bot entries deliberately across repositories. Edit it only when the user asks for a Renovate change specifically; do not adjust it as a side effect of adding dependencies.

## Critical Gotchas

**`.gitignore` silently swallows several plausible source paths.** Because it is the stock Python template, its packaging patterns are unanchored and match at any depth, not just at the repository root. Verified with `git check-ignore -v`:

| Ignored at any depth | Ignored at root or by name |
| --- | --- |
| `lib/`, `lib64/`, `build/`, `dist/`, `target/`, `var/`, `parts/`, `eggs/`, `downloads/` | `docs/_build/`, `*.log`, `.env`, `.envrc`, `db.sqlite3` |

So `src/lib/foo.py` and `app/build/x.py` are untracked with no warning at `git add` time. Before committing a new directory, run `git check-ignore -v <path>`. If it matches, rename the directory (for example, put package code under `src/wtw_arr/`) rather than adding a negation pattern to `.gitignore`, which would diverge this repo from the shared template. `src/` and `tests/` are clear.
