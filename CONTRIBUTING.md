# Contributing Guidelines & Engineering Standards

Thank you for contributing to the open-source ecosystem maintained by **Ozan Özdil (`@ozdil`)**. All repositories under this account follow strict architectural, cryptographic, and visual consistency rules.

---

## 1. Typography & UI Consistency Standard

- **Mandatory Default Font:** All Omarchy desktop applications, Quickshell bar widgets, QML panels, and SVG diagrams must use **`JetBrainsMono Nerd Font`**.
- **Fallback Chain:** `"JetBrainsMono Nerd Font, JetBrains Mono, monospace"`.
- Generic sans-serif fonts (`Inter`, `Noto Sans`, `Cantarell`, `sans-serif`) must never be set as the primary default font family.

---

## 2. Zero-Emoji Policy

- Source code, UI labels, documentation, Markdown files, commit messages, pull requests, and issue descriptions must contain **zero Unicode emojis**.
- Use clean vector SVG icons or Nerd Font monospace glyphs inside desktop UI components where visual indicators are needed.

---

## 3. HANCORE Security & Systems Checklist

Every pull request touching system I/O, networking, or subprocess execution must satisfy all five HANCORE invariants:

- [ ] **Process Group Isolation:** Subprocesses use `process_group(0)` and deterministic RAII `ProcessGroupGuard` cleanup.
- [ ] **Bounded Reads:** File and stream readers enforce `take(MAX + 1)` ceilings to prevent unbounded allocations.
- [ ] **Atomic Persistence:** State/config writes use `.tmp_*` atomic renames (`0600` file / `0700` dir modes) and reject symlinks via `symlink_metadata`.
- [ ] **Argument Hardening:** External commands use discrete argument arrays, explicit `--` flag terminators, and strict input validation.
- [ ] **Network Scope:** Any network discovery or scanning is restricted to RFC 1918, RFC 3927, or loopback blocks.
