# Security Policy & HANCORE Architecture Standard

## 1. Scope & Supported Repositories

This global security policy governs all open-source repositories maintained by **Ozan Özdil (`@ozdil`)**, including the **Omarchy Linux Security & Application Suite**, **PolyCodex**, **OmaSend (Linux / Android / Web)**, **RustGIS**, **OSINT**, and **DeckOpt**.

| Release Channel | Status | Security Patch SLA |
| :--- | :--- | :--- |
| `main` / Latest Tagged Release | Actively Supported | Critical: `< 48h` / High: `< 7d` |
| Previous Minor Releases | Security Fixes Only | Best-Effort Backport |

---

## 2. HANCORE Defensive Architecture Invariants

All native system daemons, Rust binaries, C++ extensions, and Quickshell/QML components across this account are engineered and audited against the **HANCORE Security Standard**:

1. **Subprocess Group Isolation (`INV-01`):** External child processes are spawned in isolated process groups (`process_group(0)`) and deterministically reaped via RAII `ProcessGroupGuard` structures to prevent orphan processes or privilege persistence.
2. **Bounded Memory & Stream Caps (`INV-02`):** Every file, socket, and IPC reader enforces a strict upper byte ceiling (`take(MAX_BYTES + 1)`, typically `1 MiB`) to eliminate memory-exhaustion and Denial-of-Service (DoS) vectors.
3. **Atomic Storage & Symlink Rejection (`INV-03`):** Configuration and state files are written atomically via temporary `.tmp_*` files with `0600` file permissions and `0700` directory permissions. Symbolic links are explicitly rejected via `symlink_metadata` prior to I/O.
4. **Command & Argument Hardening (`INV-04`):** External binary invocations never interpolate shell strings; arguments are passed as discrete `argv` arrays with explicit flag terminators (`--`) and strict allowlist validation (IPv4/IPv6, MAC, alphanumeric).
5. **RFC-Scoped Network Operations (`INV-05`):** Local discovery, ARP/ICMP scanning, and bridge synchronization are strictly bounded to private LAN (`RFC 1918`), link-local (`RFC 3927`), or loopback address spaces.

---

## 3. Reporting a Vulnerability

Please **do not** report security vulnerabilities through public GitHub issues, discussions, or pull requests.

- **Primary Reporting Method:** Enable/use **GitHub Private Vulnerability Reporting** (`Security` -> `Report a vulnerability`) on the affected repository.
- **Direct Contact:** Reach out via the verified contact channels listed on **[ozanozdil.com](https://ozanozdil.com)**.

### What to Include in Your Report
- Affected repository name, commit SHA or release version, and target Linux distribution/kernel version.
- Minimal reproduction steps or Proof-of-Concept (PoC) demonstrating the boundary violation.
- Impact assessment (e.g., privilege escalation, TOCTOU race, parser memory exhaustion, or argument injection).
