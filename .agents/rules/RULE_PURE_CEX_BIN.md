# Rule: Pure Cex Binaries in bin/ (RULE_PURE_CEX_BIN)

> **Severity: MANDATORY / STRICT**

## 1. Requirement
The `bin/` directory must contain exclusively native `.cex` files (e.g., `bin/cex-pack.cex`, `bin/cex-log.cex`).
Under no circumstances are JavaScript files (`.js`), Node.js scripts (`#!/usr/bin/env node`), or non-`.cex` interpreted scripts permitted in `bin/`.

## 2. Invariants
1. **Pure .cex Execution**: All executable entrypoints and tools in `bin/` must be written in the Cex native language (`.cex`).
2. **Runtime Invocation**: Scripts and binaries in `bin/` are driven strictly by the native Cex toolchain (`cexr run bin/<tool>.cex` or direct machine compilation via `cexp`).
3. **No Mixed Languages**: Do not introduce hybrid or shim JS/Node wrappers.
4. **Rule 29 Conformance**: All `.cex` files must terminate with a trailing newline.
