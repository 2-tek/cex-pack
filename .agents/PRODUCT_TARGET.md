# 2-TEK Cex-Pack: Syntax Checker & Manifest Validator Specification

<!-- Standard Specification: .agents/PRODUCT_TARGET.md -->
<!-- Rule Conformance: Rule 69 (Target), Rule 29 (EOF), Rule 72 (Dynamic Paths) -->

> **Canonical Target of Cex-Pack (`@2tek/pack`)**:
> **`cex-pack` is the canonical syntax validation engine, schema enforcer, and diagnostic tool for `cex-pack.json` manifests across the Cex ecosystem.**
> **Toolchain Standard**: Pure Cex Native Engine (`cexr` runtime engine + `cexp` direct machine compiler).
> **Target Definition**: `"target": "runtime"`, `"required": {"cexr": "^8", "cexp": "^8"}`.

---

## 1. High-Level Vision & Scope

1. **Syntax Parsing & Tokenization**:
   - Parses `cex-pack.json` files with strict structural JSON validation.
   - Detects unclosed strings, trailing commas, missing colons, and malformed arrays/objects.
   - Computes exact line number and column offsets for syntax errors.

2. **Manifest Schema Validation**:
   - Validates mandatory attributes (`name`, `version`, `main`, `target: "runtime"`, `required`).
   - Validates optional attributes (`description`, `author`, `license`, `dependencies`, `includeDirs`, `cFlags`, `scripts`, `lint`).
   - Enforces pattern formatting for package scopes (e.g. `@2tek/...` or `@cex-...`).

3. **Governance & Rule Enforcement**:
   - Strictly enforces Rule 69 (`"target": "runtime"` must be present and unmodified).
   - Validates EOF single newline integrity according to Rule 29.
   - Flags non-standard or deprecated keys in configuration manifests.

4. **ANSI Diagnostics & Reporting**:
   - Outputs color-coded terminal reports with code snippets, line highlights, and actionable remediation steps.
   - Returns precise POSIX exit codes (`0` for success, `1` for syntax error, `2` for schema violation).

---

## 2. Invariants & Rules

| Key | Value | Specification |
|---|---|---|
| Package Name | `@2tek/pack` | Official Cex Pack Manifest Validator |
| Version | `1.0.0` | Cex Manifest Validator Standard |
| Target | `"target": "runtime"` | Strictly Immutable (Rule 69) |
| Runtime Requirement | `"cexr": "^8"` | CexR v8 Runtime Engine |
| Compiler Requirement | `"cexp": "^8"` | CexP v8 Direct Machine Compiler |
