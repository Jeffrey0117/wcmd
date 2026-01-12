<div align="center">

# wcmd

**Windows commands on Linux, for the muscle memory.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Python 3.6+](https://img.shields.io/badge/Python-3.6+-green.svg)](https://python.org)
[![Platform](https://img.shields.io/badge/Platform-Linux-orange.svg)]()

*A declarative command mapping engine that compiles a single JSON configuration into Linux shell scripts.*

**Sister project of [cmdx](https://github.com/Jeffrey0117/cmdx) (Unix commands on Windows)**

</div>

---

## How It Works

```
┌─────────────────┐      ┌──────────────┐      ┌─────────────────┐
│  commands.json  │  →   │  generate.py │  →   │   out/*         │
│   (your config) │      │  (compiler)  │      │  (shell scripts)│
└─────────────────┘      └──────────────┘      └─────────────────┘
```

Each generated shell script:
1. Checks if preferred modern tools exist (in order)
2. Uses the first available tool found
3. Falls back to standard Linux commands if nothing else works

**Example:** Type `dir` → tries `eza` → `lsd` → falls back to `ls -la`

---

## Why wcmd?

For Windows users who switched to Linux but still type `dir` instead of `ls`.

For sysadmins who jump between Windows and Linux servers.

For anyone whose fingers remember `cls` better than `clear`.

---

## Quick Start

```bash
# 1. Clone the repository
git clone https://github.com/Jeffrey0117/wcmd.git
cd wcmd

# 2. Generate shell scripts
python generate.py

# 3. Add to PATH (choose one)
export PATH="$(pwd)/out:$PATH"        # Temporary
# OR
sudo cp out/* /usr/local/bin/          # Permanent

# 4. Done! Start using Windows commands
dir          # Uses eza/lsd if installed, otherwise ls -la
type file    # Uses bat if installed, otherwise cat
cls          # Clears the screen
```

---

## Built-in Mappings

| Windows Command | Linux Equivalent | Description |
|-----------------|------------------|-------------|
| `dir` | eza → lsd → `ls -la` | List directory |
| `type` | bat → `cat` | Display file |
| `copy` | `cp` | Copy files |
| `move` | `mv` | Move files |
| `del` | `rm` | Delete files |
| `cls` | `clear` | Clear screen |
| `where` | `which` | Find command |
| `findstr` | rg → `grep` | Search text |
| `md` / `mkdir` | `mkdir -p` | Create directory |
| `rd` / `rmdir` | `rm -r` | Remove directory |
| `ren` | `mv` | Rename files |
| `fc` | delta → `diff` | Compare files |
| `more` | bat → `less` | Page through file |
| `set` | `env` | Environment variables |
| `tasklist` | procs → `ps aux` | List processes |
| `taskkill` | `kill` | Kill process |
| `ipconfig` | `ip addr` | Network config |
| `tracert` | `traceroute` | Trace route |
| `mklink` | `ln -s` | Symbolic link |
| `ver` | `uname -a` | System version |
| `systeminfo` | neofetch → `uname` | System info |
| `start` | `xdg-open` | Open with default app |
| `notepad` | nvim → vim → `nano` | Text editor |
| `explorer` | nautilus → `xdg-open .` | File manager |
| `tree` | eza --tree → `tree` | Directory tree |
| `clip` | `xclip` / `xsel` | Copy to clipboard |
| `pause` | `read -p` | Pause execution |

---

## CLI Usage

```bash
python generate.py           # Generate all shell scripts
python generate.py --clean   # Remove generated files
python generate.py --list    # Display all mappings
python generate.py --help    # Show help
```

---

## Adding Custom Commands

Edit `commands.json`:

```json
"calc": {
  "prefer": ["qalc"],
  "fallback": "bc",
  "description": "Calculator"
}
```

Then run `python generate.py`. Done.

---

## Project Structure

```
wcmd/
├── commands.json      # Your command mappings (the only file you edit)
├── generate.py        # Compiler script
├── out/               # Generated shell scripts (gitignored)
└── README.md
```

---

## Related Projects

| Project | Description |
|---------|-------------|
| [cmdx](https://github.com/Jeffrey0117/cmdx) | Unix commands on Windows |
| **wcmd** | Windows commands on Linux |

---

## License

MIT License

