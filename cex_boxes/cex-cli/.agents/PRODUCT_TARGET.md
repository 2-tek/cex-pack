# 2-TEK Cex-CLI: Terminal Build Command Runner & Task Automation Engine Specification

<!-- Standard Specification: .agents/PRODUCT_TARGET.md -->
<!-- Rule Conformance: Rule 69 (Target), Rule 29 (EOF), Rule 72 (Dynamic Paths), Rule (No Symbols) -->

> **Canonical Target of Cex-CLI (`@2tek/cli`)**:
> **`cex-cli` is the canonical terminal build command runner, task automation dispatcher, and interactive terminal UI window engine for the Cex ecosystem.**
> **Toolchain Standard**: Pure Cex Native Engine (`cexr` runtime engine + `cexp` direct machine compiler).
> **Target Definition**: `"target": "runtime"`, `"required": {"cexr": "^8", "cexp": "^6"}`.

---

## 1. High-Level Vision & Scope

1. **Terminal Build Command Dispatcher**:
    - Executes build commands (`build`, `run`, `test`, `watch`, `doctor`, `clean`) with zero external shell dependencies.
    - Smart environment resolution supporting dev, staging, and production build flags (`--target`, `--env`).
    - Task pipeline automation supporting pre-build, build, post-build, and lint hooks.

2. **ANSI Colorized Terminal UI Window System**:
    - Visual ASCII framed output windows with titles (`+-----+`, `+-----+`).
    - Dynamic progress bars, status indicators (`[OK]`, `[FAIL]`, `[WARN]`, `[BUILD]`), and ANSI color formatting.
    - Interactive terminal menu selection for quick command invocation.

3. **Governance & Standards Integration**:
    - Built-in `.agents` inspector validating project rules (`Rule 29`, `Rule 69`, `Rule 72`, `Rule (No Symbols)`).
    - Integrated `.cvm` Change Record repository status check and patch application.
    - Integrated `.github` workflow trigger and CI environment detection.

---

## 2. Invariants & Rules

| Key                  | Value                 | Specification                   |
| -------------------- | --------------------- | ------------------------------- |
| Package Name         | `@2tek/cli`           | Official Cex CLI Package        |
| Version              | `1.0.0`               | Cex Terminal CLI Standard       |
| Target               | `"target": "runtime"` | Strictly Immutable (Rule 69)    |
| Runtime Requirement  | `"cexr": "^8"`        | cexp v8 Runtime Engine          |
| Compiler Requirement | `"cexp": "^6"`        | CexP v8 Direct Machine Compiler |
