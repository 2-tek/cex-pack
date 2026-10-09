# Rule Syntax Validation: Strict `cex-pack.json` Validation Rules

> **Severity: MANDATORY / STRICT**

## 1. Requirement
The `cex-pack` tool MUST perform thorough JSON syntax parsing and schema validation on `cex-pack.json` files before any build or execution phase:
1. Parse valid JSON structure (no trailing commas, properly escaped strings, matched brackets/braces).
2. Enforce required manifest fields (`name`, `version`, `main`, `target: "runtime"`, `required`).
3. Verify Rule 69 compliance (`"target": "runtime"`).
4. Verify Rule 29 EOF line ending integrity (`\n`).

## 2. Rationale
Early syntax and schema validation prevents downstream compilation failures, bad module resolutions, and runtime crashes during Cex execution.
