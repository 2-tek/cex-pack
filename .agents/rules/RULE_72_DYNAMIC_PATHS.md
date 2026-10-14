# Rule 72: Dynamic Path Resolution

> **Severity: MANDATORY / STRICT**

## 1. Requirement
All file path resolutions, directory lookups, and execution commands MUST compute paths relative to project root or use dynamic runtime path helpers (`./bin/cex-pack`, `packages/cex-pack`).

## 2. Rationale
Prevents hardcoded absolute user home directory paths (`/Users/...` or `/home/...`) from breaking cross-platform execution on Linux and Windows.
