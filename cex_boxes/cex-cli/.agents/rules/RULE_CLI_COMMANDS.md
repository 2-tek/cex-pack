# Rule CLI: Terminal Command Invocation Standards

<!-- Rule Conformance: Rule 69 (Target), Rule 29 (EOF), Rule 72 (Dynamic Paths), Rule (No Symbols) -->

> **Severity: MANDATORY / STRICT**

## 1. Requirement
All `cex-cli` terminal commands must return exit code `0` on success and non-zero on failure (`1` for invalid flags, `2` for build errors). Terminal outputs MUST support ANSI color codes and fallback cleanly when color is disabled (`NO_COLOR=1`). All outputs must use pure ASCII text without emojis.

## 2. Standard Output Formatting
- Success messages: `\033[32m[OK]\033[0m`
- Warning messages: `\033[33m[WARN]\033[0m`
- Error messages: `\033[31m[FAIL]\033[0m`
