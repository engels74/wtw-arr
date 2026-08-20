# AGENTS.md

This file provides guidance to AI coding agents when working with code in this
repository.

## Repository state

This is an unimplemented scaffold. The tracked tree is `README.md` (two lines),
`LICENSE`, `.gitignore`, and `renovate.json` — no source, no tests, no package
manifest, no lockfile, no CI, no `.github/`.

- There is no build, test, lint, or type-check command. Any command you write
  here would be invented. Agree the toolchain with the user first, then record
  the real invocations in this file.
- Nothing states what this project does — not `README.md` (`WIP`), not the
  GitHub description (`WIP`), not the repo name. Ask the user rather than
  inferring a purpose from the `-arr` suffix.

## Committed decisions

- **AGPL-3.0.** New sources and any added dependency must be AGPL-3.0
  compatible.
- **Python is the intended language**, evidenced only by `.gitignore` being
  GitHub's Python template. It covers pip, uv, Poetry, PDM, and pixi, plus
  pytest, mypy, and Ruff, and expresses no preference among them — package
  manager, test runner, and linter are all still open questions for the user.
- **`renovate.json` is fleet-standardized, not repo-tuned** (see commit
  `aebcd1c`). Edit it only when the user asks for a Renovate change
  specifically, never as a side effect of adding dependencies.
- **`AGENTS.md` is the real file and `CLAUDE.md` is a symlink to it.** Edit
  `AGENTS.md`; do not replace the symlink with a copy.

## Gotcha: `.gitignore` silently swallows plausible source paths

Because `.gitignore` is the stock Python template, its packaging patterns are
unanchored — they match at *any* depth, not only at the repository root. These
directory names are ignored wherever they appear:

`lib/` `lib64/` `build/` `dist/` `target/` `var/` `parts/` `eggs/`
`downloads/` `instance/` `cover/` `env/` `venv/` `ENV/`

`*.log` is likewise ignored at any depth. So `src/wtw_arr/lib/util.py`,
`src/wtw_arr/build/renderer.py`, and `tests/fixtures/sample.log` are all
untracked with no warning at `git add` time.

Before committing a new directory, run `git check-ignore -v <path>`. If it
matches, rename the directory (for example `src/wtw_arr/helpers/` instead of
`src/wtw_arr/lib/`) rather than adding a negation pattern to `.gitignore` —
a negation would diverge this repo from the shared fleet template. Verified
clear: `src/`, `tests/`, `.github/`, `pyproject.toml`, and lockfiles.
