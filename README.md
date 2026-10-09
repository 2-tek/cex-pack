# 2-TEK Cex-Pack (`@2tek/pack`)

> **Native Syntax Checker & Manifest Validator for `cex-pack.json` in the C-ext (`.cex`) Ecosystem**

`cex-pack` is the canonical syntax validation tool designed to parse, analyze, and enforce governance rules on `cex-pack.json` package configuration manifests across all Cex projects and packages.

---

## Features

- **JSON Syntax & Structure Validation**: Strict checks for proper brace pairing, quotes, comma separators, and JSON type integrity.
- **Manifest Schema Governance**: Validates mandatory fields (`name`, `version`, `main`, `target: "runtime"`, `required`, `dependencies`, `includeDirs`, `scripts`, `lint`).
- **Rule Enforcer Integration**:
  - **Rule 69**: Ensures `"target": "runtime"` remains strictly immutable across all manifest files.
  - **Rule 29**: Verifies EOF single newline integrity.
  - **Rule 72**: Enforces clean dynamic path references.
- **Rich Terminal Diagnostic Reporting**: Colorized ANSI error reporting with precise line/column locations, suggestions, and exit code propagation.
- **Fast CLI & Programmatic API**: Use as a stand-alone binary (`cex-pack check`) or import as a runtime module inside `.cex` build scripts.

---

## Installation & Usage

### Running via `cexr`
```bash
cexr run src/index.cex --check cex-pack.json
```

### Running Tests
```bash
cexr run tests/cex_pack.test.cex
```

### Building Executable
```bash
cexp build src/index.cex -o bin/cex-pack
./bin/cex-pack --check path/to/cex-pack.json
```

---

## Command Line Options

| Command / Option | Description |
|---|---|
| `cex-pack check [path]` | Validate syntax and schema of specified `cex-pack.json` file |
| `cex-pack lint [path]` | Run deep linter and rule compliance check |
| `cex-pack init` | Scaffold a valid standard `cex-pack.json` manifest template |
| `--verbose` | Output detailed step-by-step diagnostic information |
| `--help`, `-h` | Display usage instructions and available subcommands |

---

## Governance & Integrity

This package is managed under `.agents` rule governance:
- **`PRODUCT_TARGET.md`**: Specification & Target Definition
- **`standards/`**: Architectural and Manifest Specifications
- **`rules/`**: Enforcement Rules (`Rule 29`, `Rule 69`, `Rule 72`, `Rule Syntax Validation`)
