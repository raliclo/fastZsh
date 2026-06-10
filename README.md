# 🚀 Cross-Platform Optimized Zsh Configuration
> Other Languages: [繁體中文](README.zh-TW.md) 

![Zsh Support](https://img.shields.io/badge/Shell-Zsh%205.0+-blue?style=for-the-badge&logo=gnu-bash)
![Platform Support](https://img.shields.io/badge/Platform-Linux%20%7C%20macOS-orange?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

This repository contains a highly optimized, cross-platform `.zshrc` configuration featuring **dynamic environment detection**, **on-the-fly compiler switching**, and an **anti-glitch multi-line prompt** tailored for modern workflows on both Linux and macOS (Darwin).

---

## ✨ Core Features

### 1. 🌍 Dynamic Cross-Platform Detection & Optimization
* **Hardware Awareness**: Automatically detects the operating system (`Linux` or `Darwin`), parses `/proc/cpuinfo` or `sysctl` to count logical CPU cores, and dynamically sets parallel compilation threads to `$((Cores * 2))` via `$PACORES`.
* **Path & Tool Alignment**: Adapts smoothly to pathing environments used by either Linuxbrew or native macOS Homebrew (e.g., dynamically handles `$BREW_PREFIX` and `readlink`/`greadlink`).

### 2. 🛠️ Dynamic Compiler Environment Switcher (`setcc`)
The built-in `setcc` utility function allows you to switch and export a complete set of compilation variables (`CC`, `CXX`, `FC`, `CPP`, `GCC_FLAGS`, etc.) instantly within the same terminal session:
* `setcc gcc`: Switches to the system default GCC environment (Default).
* `setcc gccx`: Switches to a custom-version GCC environment (e.g., `gcc-7`).
* `setcc clang`: Switches to the standard or custom Clang/LLVM environment.
* `setcc mpicc`: Switches to the High-Performance Computing (HPC) dedicated MPI (Message Passing Interface) environment.

### 3. 🎨 Anti-Glitch Multi-Line Prompt
* **Glitch Prevention**: Leverages Zsh's unique `%{%}` color control-code wrapping. This completely solves the common glitch in traditional terminal setups where the cursor gets misplaced, overlaps, or breaks during long command wraps or reverse history searches.
* **Clear Navigation**: The upper section features a complete visual divider line alongside a highlighted absolute directory path (`%d`), leaving the lower line dedicated to a clean `=>` prompt so your typing space is never cramped by long paths.

---

## 🚀 Quick Start & Installation

### Step 1: Backup Your Current Configuration
Before making any changes, it is highly recommended to back up your existing `.zshrc`:
```bash
mv ~/.zshrc ~/.zshrc.bak
```

### Step 2: Copy This Configuration
Copy the contents of this repository's `zshrc` into your home directory:
```bash
cp zshrc ~/.zshrc
```

### Step 3: Reload Zsh
```bash
source ~/.zshrc
```

## 🔧 Utility Functions & Aliases

### 📂 Directory Navigation Enhancements
* `cd()` : Overrides the built-in `cd` command to automatically cache the path of your last directory into a global `$prevfolder` variable before switching locations.
* `prev` : Toggles or instantly switches back to the immediately preceding directory path (`$prevfolder`) — perfect for snapping back and forth between two active project folders.
* `orig` : Instantly jumps back to the terminal-login root directory captured when the session was first initialized (`$termfolder`).

---

### 🛠️ Common Core Aliases
* `f` : *(macOS only)* Instantly open the current terminal working directory inside a graphical Finder window.
* `sll` : Quick-launch or open targeted files and folders using Sublime Text.
* `cgrep` : Force-enables standard color syntax highlighting on all pipeline `grep` search results.
* `xxargs` : Dynamically binds a parallelized `xargs` pipeline utilizing your system's maximum hardware thread capacity (`xargs -n 1 -P $PACORES`).
* `make` : Leverages lazy evaluation to automatically scale with your parallelism thread limit flags (`make $MAKEJOBS`).

### Changing the Default Terminal Editor
Use the `cheditor` function to quickly update your `$EDITOR` environment variable:
```bash
cheditor vi       # Switch default editor to vi
cheditor nano     # Switch default editor to nano
```

## � File and Folder Utilities
* `nofiles` : Counts non-hidden files in the current directory using efficient Zsh glob modifiers.
* `make1mb`, `make5mb`, `make10mb` : Creates dummy files sized 1MB, 5MB, or 10MB using macOS `mkfile`.
* `fsize` : Lists files in the current directory with human-readable sizes, sorted from largest to smallest.
* `trash` : Safely moves files or directories to `~/.Trash` instead of deleting them permanently.
* `extract` : Smartly extracts a wide range of archive formats from a single command, including `.tar.gz`, `.zip`, `.7z`, `.xz`, `.rar`, `.lz4`, and more.
* `lzfseX` : Compresses using Apple's propiertary LZFSE algorithm.


### 🔬 `lz4bench` — Parallel LZ4 benchmark
* **Purpose**: Run end-to-end compression and decompression benchmarks that compare `tgz`, `lz4a` and `tlz4` workflows using high-resolution timestamps. The function verifies correctness by checking that decompressed contents are identical.
* **Usage**: `lz4bench <directory-name>` — pass the base directory name (the script expects matching archives like `<name>.tgz`, `<name>.lz4a`, `<name>.tar.lz4`).
* **Output**: Prints per-step timing for compression and extraction, plus integrity checks. Sample output (example — results will vary by machine and dataset):

```
## Note , this result is based on Mac Mini M4 16GB RAM, 10-core CPU, the best results is using tlz4 and extract.
[Info] 開始執行 tgz, lzfse , tlz4 基準測試 / Starting benchmark for tgz, lzfse, tlz4...


[Info] 測試 getar 壓縮 / Testing getar compression:
140M	sample_wix
 25M	sample_wix.tgz
==> Process getar sample_wix took: 2419937134 奈秒/nanoseconds

[Info] 測試 lzfseX  壓縮 / Testing lzfseX compression:
140M	sample_wix
 21M	sample_wix.lzfse
==> Process lzfseX sample_wix took: 0753859997 奈秒/nanoseconds

[Info] 測試 tlz4  壓縮 / Testing tlz4 compression:
140M	sample_wix
 38M	sample_wix.tar.lz4
==> Process tlz4 sample_wix took: 0232564926 奈秒/nanoseconds

==================================================
[Info] 開始評測解壓縮速度 / Benchmarking decompression score:
==================================================

[Info] 測試 tgz 解壓 / Testing tgz extraction:
nanoTimeElapsed extract sample_wix.tgz
==> Process extract sample_wix.tgz took: 0380614996 奈秒/nanoseconds

[Info] 測試 lzfse 解壓 / Testing lzfse extraction:
nanoTimeElapsed extract sample_wix.lzfse
lzfse -decode -i sample_wix.lzfse -so | tar -xf - 
==> Process extract sample_wix.lzfse took: 0405045033 奈秒/nanoseconds

[Info] 測試 tlz4 解壓 / Testing tlz4 extraction:
nanoTimeElapsed extract sample_wix.tar.lz4
==> Process extract sample_wix.tar.lz4 took: 0343693018 奈秒/nanoseconds

[Success] tgz,lzfse 解壓後的內容完全一致！ / Decompressed contents are identical!

[Success] tgz,tlz4 解壓後的內容完全一致！ / Decompressed contents are identical!

[Info] 基準測試完成！ / Benchmark finished!
```

Note: To get meaningful results, run `lz4bench` on representative data and ensure `lz4`, `tar`, and `xargs` are installed. Results will vary depending on CPU, I/O, and dataset compressibility.

## 🚀 Startup Script & Environment Boot
* `START_UP@BEGIN` and `START_UP@END` are lifecycle hooks that execute during shell startup to initialize aliases and finalize environment injection.
* `xxargs` is aliased to `xargs -n 1 -P $PACORES` for parallel pipelines.
* `sll` is aliased to `subl` for quickly opening files in Sublime Text.
* `cgrep` is aliased to `grep --color=always` to ensure colored search results in pipelines.
* `MAKEJOBS` is initialized to `-j16` for default parallel builds.
* `setcc` runs automatically on startup to apply the chosen compiler toolchain configuration.

## �👥 Authors & Acknowledgments

This configuration was inspired by and built upon excellent dotfiles, utility scripts, and technical guidance provided by the following authors:

* **Ralic Lo** (ralic.lo@gmail.com) - Core architecture design and dynamic environment setups.
* **Nathaniel Landau** ([Website / Resume](https://natelandau.com/nathaniel-landaus-resume/)) - Robust bash/zsh shell scripting structures.
* Syntax and command logic insights driven by: [ExplainShell](https://explainshell.com/)

---

## 📄 License

This project is licensed under the **MIT License**. 

Feel free to copy, modify, distribute, or incorporate this configuration into your own personal dotfiles setup. For more details, please refer to the `LICENSE` file in the repository root.
