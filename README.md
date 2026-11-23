# mend: one patch tool for every AI edit format

[![make.fun · hidden-spec-cli-01-run-20261122](https://make.fun/badge/hidden-spec-cli-01-run-20261122.svg)](https://make.fun/bounty/hidden-spec-cli-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=hidden-spec-cli-01-run-20261122)
[![rank](https://make.fun/badge/hidden-spec-cli-01-run-20261122/mutiafebrizki-ship-it/hidden-spec-cli-01-run-20261122/rank.svg)](https://make.fun/bounty/hidden-spec-cli-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=hidden-spec-cli-01-run-20261122)
[![reward](https://make.fun/badge/hidden-spec-cli-01-run-20261122/mutiafebrizki-ship-it/hidden-spec-cli-01-run-20261122/reward.svg)](https://make.fun/bounty/hidden-spec-cli-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=hidden-spec-cli-01-run-20261122)

`mend` applies an edit that an AI model wrote to a tree of text files. Models write
edits in four formats, and `mend` reads them all into the one specification in
`SPEC.md`:

- a unified diff (often with wrong line numbers),
- SEARCH/REPLACE blocks,
- the V4A patch format,
- text-editor tool calls as JSON.

It finds the lines each edit names at the first of three matching tiers (exact,
then ignoring trailing whitespace, then ignoring leading and trailing whitespace,
re-indenting new lines at the last tier), and writes every change or none:
if one edit fails, no file is changed at all.

## Usage

    node main.ts [--format=NAME] [--check] [--strict] [PATCH]

- The working directory is the root of the tree; every path in a patch is relative to it.
- `PATCH` is the patch file, or `-` / absent for standard input.
- `--format=NAME` reads one format: `unified`, `search-replace`, `v4a`, `json`;
  `auto` (the default) detects it.
- `--check` does everything but write: the same output and exit code, no file touched.
- `--strict` matches lines exactly (tier 0 only).

On success it prints one line per changed path (`A`, `D`, `M`, or `R old -> new`
for a move) and `ok: <n> files`. On failure it prints one `error: <code> <path>`
line per failed edit and `failed: <k> of <n> edits`, exit 1; a usage, missing
patch, undetected format or unparsable patch error exits 2 with a single line.

## Files

- `main.ts`: the CLI, Node 22 and its standard library alone. `runner.ts` is the
  kit's trial runner, kept as handed out.
- `check.ts`: runs `main.ts` over the kit's public practice set
  (`node check.ts <practice.json>`).
- `SPEC.md`: the whole specification the CLI implements.

## Entry

Built for the make.fun bounty
[Build mend, one patch tool for every AI edit format](https://make.fun/bounty/hidden-spec-cli-01-run-20261122).
