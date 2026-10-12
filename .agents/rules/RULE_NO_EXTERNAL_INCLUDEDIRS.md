# Mandatory Rule: Prohibition of External and CexP/CexR includeDirs (RULE_NO_EXTERNAL_INCLUDEDIRS)

<!-- Rule Conformance: Rule 69 (Target), Rule 29 (EOF), Rule 72 (Dynamic Paths) -->

> **MANDATORY DIRECTIVE FOR MANIFEST INCLUDE DIRECTORIES**:
> In all project workspaces, package manifests (`cex-pack.json`), build configurations, and CI pipelines:
>
> 1. **STRICT PROHIBITION OF OUTSIDE-PROJECT `includeDirs`**:
>     - Manifest files (`cex-pack.json`) must strictly encapsulate within their own project root directory.
>     - Any paths in `includeDirs` that traverse outside the project root (e.g. paths starting with `..`, `../other-package/src`, or absolute paths) are **STRICTLY PROHIBITED**.
> 2. **STRICT PROHIBITION OF DIRECT CEXP / CEXR INCLUDE PATHS**:
>     - Manifests must NEVER declare `includeDirs` pointing into CexP or CexR repositories (e.g. `../../packages/CexP/...`, `../../packages/CexR/...`, `../../runtime/...`).
>     - `cexp` (the machine compiler) and `cexr` (the runtime engine) are external toolchains managed via `cxvm`. Standard native libraries and compiler built-ins are provided directly by the toolchain.
> 3. **CLEAN SELF-CONTAINED RESOLUTION & DEFAULT TO `src`**:
>     - When `includeDirs` is omitted from `cex-pack.json`, the compiler and runtime engine automatically default includes to `src/` (auto-add `src` to includes).
>     - When explicitly specified, `includeDirs` must only reference local subdirectories within the package (e.g. `["./src"]` or internal monorepo packages `["./src", "./packages/core/src"]`).
>     - Cross-package dependencies must be resolved cleanly via `.cex_boxes/` or module aliases, never via cross-directory `includeDirs`.
> 4. **AUTOMATED GOVERNANCE & ENFORCEMENT**:
>     - `cex-pack` validates and rejects any `cex-pack.json` declaring external or CexP/CexR paths in `includeDirs`.
