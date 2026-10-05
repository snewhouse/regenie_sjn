# AGENTS.md

> Instructions for AI coding agents working in this repository. The full walkthrough is
> `ONBOARDING.md`. Last reviewed: 2026-10-05 · 65ec67f · by understand-codebase.

## Commands

- Prerequisite (once, outside the repo, needs network): build BGEN v1.1.7 with `python3 waf configure` then `python3 waf` (`.github/workflows/test.yml:54-65`).
- Build: `make BGEN_PATH=/abs/path/v1.1.7 HAS_BOOST_IOSTREAM=0` — not_run
- Smoke test (all CI runs): `./regenie --help && ./regenie --version` — not_run
- Full test, before every PR: `bash test/test_bash.sh --path .`, which must print "SUCCESS" — not_run
- Also build with CMake before pushing: `mkdir -p build && cd build && BGEN_PATH=/abs/path/v1.1.7 cmake .. && make` — not_run
- Lint / format / type check: none configured; `-Wall -pedantic` is the only gate (`Makefile:33`).
- Tool versions: a compiler with C++17 for remeta (`external_libs/remeta/Makefile:6`), gfortran, and CMake at or above the minimum in `CMakeLists.txt:9`.

## Gotchas

- `external_libs/` is vendored, partly hand-modified third-party code. Never edit it.
- `docs/site/` is generated mkdocs output. Edit `docs/docs/` instead.
- A new source file in src/ must be added to the hand-written list in `CMakeLists.txt:97-118`; Make finds it by wildcard but CMake, CI and Docker will not.
- src/ is C++11 (`CMakeLists.txt:29`). Do not use newer language features there.
- `-ffast-math` (`CMakeLists.txt:121`): do not change compiler flags; numerical output depends on them.
- CI never runs `test/test_bash.sh`, so run it yourself; a green CI proves only that it builds.
- Options in the `Extra` group (`src/Regenie.cpp:306`) never appear in help output.
- Match the style of the file you edit; there is no formatter, and the Cox files differ from the core style.
- Report user errors with throw, never exit() in library code (`src/NNLS.cpp:1145` is the anti-pattern).
- Never commit secrets or read `.env`, `*.key`, `*.pem` or credential files. The repo has none; CI uses the secret named `GH_PAT`.

## Rules pointers

- No CLAUDE.md, CONTRIBUTING.md or CODEOWNERS. Upstream (rgcgithub/regenie) git-ignores `CLAUDE.md` and `.claude/` (`.gitignore:7-9`).
- Commits: short free-form messages, upstream style. PRs go to the rgcgithub/regenie master branch. CI that blocks: CMake build plus `--help`/`--version` (`.github/workflows/test.yml:67-75`).
