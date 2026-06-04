# wcmd

Windows commands on Linux, for the muscle memory — a declarative engine that compiles one JSON config into Linux shell scripts.

## Stack
- Python 3.6+ (standard library only — no dependencies, no package manifest)
- Output: Bash shell scripts (`#!/bin/bash`)
- Config: single `commands.json` (optional `$schema` → `schema.json`, not present in repo)
- License: MIT

## Directory structure

```
wcmd/
  generate.py       ← Compiler: reads commands.json, writes out/* shell scripts
  commands.json     ← Command mappings (the only file you normally edit)
  out/              ← Generated Bash scripts, one per command (gitignored)
  README.md
```

## Key concepts

- **Mapping engine**: Each entry in `commands.json` maps a Windows command name to a
  Linux equivalent. Schema per entry: `prefer` (list of modern tools tried first),
  `fallback` (standard command used if none of `prefer` exist), `description`, and
  optional `passArgs` (default `true`).
- **Cascading tool detection**: Generated scripts run `command -v <tool>` in `prefer`
  order, exec the first one found, and `exit $?`. If none exist, they run `fallback`.
  Example: `dir` → `eza` → `lsd` → `ls -la`.
- **Arg passing**: `passArgs: true` appends `"$@"`; `false` appends `"$1"`.
- **Two templates** in `generate.py`: `TEMPLATE_WITH_PREFER` (has prefer checks) and
  `TEMPLATE_SIMPLE` (fallback only). Each script carries a generated-timestamp header.
- **Config filtering**: keys starting with `$` (e.g. `$schema`, `$comment`) are skipped
  by `load_config()`.
- **Output**: scripts written to `out/<name>` (no file extension), `chmod 0o755`,
  with `newline='\n'` to keep LF endings. `out/` is regenerable and gitignored.
- Currently ships ~35 command mappings (dir, type, copy, del, cls, findstr, tasklist,
  ipconfig, notepad, tree, etc.).
- Sibling projects (per README): `cmdx` (Unix → Windows) and `winbat` (CLI toolkit).

## Commands

```bash
python generate.py           # Generate all shell scripts into out/
python generate.py --clean   # Remove the out/ directory
python generate.py --list    # Print all mappings
python generate.py --help    # Show usage

# Install (after generating)
export PATH="$(pwd)/out:$PATH"   # temporary
sudo cp out/* /usr/local/bin/    # permanent
```

There is no build system, test suite, or external dependency.

## Coding rules

- Pure standard-library Python; do not add dependencies without reason.
- Generated scripts must keep LF line endings (`newline='\n'`) so they run on Linux.
- `commands.json` is the source of truth — add/edit commands there, then regenerate;
  never hand-edit files in `out/`.
- New config keys starting with `$` are reserved for metadata and ignored by the compiler.
