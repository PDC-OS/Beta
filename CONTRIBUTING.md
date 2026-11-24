# Contributing to PDC-OS

Welcome to **PDC-OS**—a standalone, vertically integrated, 100% open-source (GNU GPLv3) operating system written from scratch in Rust and NASM Assembly.

Our goal is simple: deliver the cohesive, unified experience of a single-vendor OS without the walled garden—and without the distro, toolkit, and package fragmentation of the Linux ecosystem.

---

## 1. Core Principles & Technical Rules

* **Standalone & Original:** PDC-OS is built from first principles, not a Linux distro or POSIX clone. Learn from other systems, but **never copy GPLv2 Linux modules** (PDC-OS is GPLv3).
* **Pure Rust & Assembly Only:** All kernel subsystems, drivers, system services, and official apps are compiled native Rust (with NASM assembly for low-level bootstrap).
  * **No C** in the base system.
  * **No Python** or interpreted runtimes in any system paths.
  * **No TypeScript, CSS, or web-stack UIs**—all UI layouts and themes are pure typed Rust.
* **Unified System Theming & Dark Mode:** Every official and community app must use PDC-OS’s canonical Rust theming tokens (`Theme`). Rogue styling or hardcoded palettes will not be merged.
* **Primary + Backup Tool Cap:** Official toolchain repositories (`core-tools` and `full-toolset`) allow at most **one primary app and one backup** per category. Improve existing tools rather than submitting redundant clones.
* **Core Invariants Are Locked:** Core memory management (`src/memory.rs`), the scheduler (`src/scheduler.rs`), and the default native filesystem are **not** open for armchair rewrites. If you build a working replacement that passes `make test` and demonstrably beats `make benchmark`, we will happily review it—otherwise, build *on top* of existing interfaces.

---

## 2. Contribution Tracks & Workflow

Because PDC-OS moves at a very high daily commit velocity, we separate userland and kernel workflows to prevent merge drift.

### Track A: Userland, Apps, & Documentation (`core-tools`, `full-toolset`, `community`)
* **Target the 1-Month Stable Snapshots:** Build userland apps and tools against the tagged **1-month post-stable kernel snapshots** (`make release`) so you are developing against a frozen, guaranteed syscall/IPC ABI.
* **Community Repo:** Getting an app into `community` is straightforward—it simply needs to follow our pure-Rust, GPLv3, and system-theming guidelines and pass a code review on each update. We strongly encourage contributing improvements to existing apps in `core-tools` and `full-toolset` first!

### Track B: Kernel & Core Drivers (`kernel`)
* **Talk First, Then Build:** Open an issue or reach out before starting any large feature so nobody wastes time on misaligned architecture.
* **Email Before Touching Kernel Code:** If you plan to work on the kernel, **email the maintainer first** so we can coordinate active areas and prevent merge collisions.
* **Small Daily Merges (Trunk-Based):**
  * Open an issue first so everyone sees what you are working on.
  * Break work into the **smallest valid, passing steps** and merge daily whenever possible. Branches left open for a week will drift and hit merge conflicts.
  * If your feature genuinely requires a long-lived parallel branch, coordinate directly with the team up front so we can keep interfaces aligned and avoid deprecated paths.

---

## 3. Pull Request Checklist

Before opening a PR, verify your changes locally:
1. `make doctor` passes cleanly.
2. `make test` (all QEMU hardware smoke profiles) passes with zero panics or exceptions.
3. `make benchmark` shows no regressions against baseline TSC and frame-reclaim targets.
4. Built-in `security` adversarial checks pass completely.
