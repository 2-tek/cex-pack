# Rule 29: End of File (EOF) Line Ending Integrity

> **Severity: MANDATORY / STRICT**

## 1. Requirement
All source files (`.cex`, `.json`, `.md`, `.yml`, `.sh`) MUST terminate with a single trailing newline character (`\n`).

## 2. Rationale
Prevents POSIX compliance issues, diff noise in git commits, and line truncation during build parsing.
