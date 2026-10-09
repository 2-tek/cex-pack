# 2-TEK Cex-Pack (`cex-pack`)

> **Native Syntax Checker, Manifest Validator & Dependency Downloader for `cex-pack.json` & `cex-pack-linked.json` in the C-ext (`.cex`) Ecosystem**

`cex-pack` is the canonical syntax validation and package dependency download tool designed to parse, analyze, and enforce governance rules on `cex-pack.json` manifests, lock encrypted repository URLs in `cex-pack-linked.json`, and download real Git / CVM dependencies into `.cex_boxes/`. Integrated with `cex-cli`.

---

## Features

- **Real-Time Progress Tracking (`.cex_boxes/PROGRESS.md`)**: Automatically updates `.cex_boxes/PROGRESS.md` with total `.cex_boxes` directory size (`current: 0.5Mb`) and formatted dependency lines following the template: `[state][percent][size][totalsize][name][linked]`.
- **Terminal Progress Logger (`cex-log`)**: Outputs styled and timestamped progress logs (`[cex-log] [INFO] (50%)`) into the terminal during downloads.
- **Native `.cex` CLI Implementation (`bin/*.cex`)**: CLI commands and loggers implemented as native `.cex` files (`bin/cex-pack.cex`, `bin/cex-log.cex`).
- **`cex-pack-linked.json` Encrypted Lockfile**: Links package versions to AES-256 encrypted repository download URLs (`cex-enc:...`) specifically scoped for the project.
- **Git & CVM Dependency Downloader (`.cex_boxes`)**: Downloads package dependencies declared in `cex-pack.json` and resolved in `cex-pack-linked.json` via real `git clone --depth 1` into `.cex_boxes/{dependencyName}` using real canonical project names without `@` alias symbols.
- **Integrated with `cex-cli`**: Full integration with the `cex-cli` task dispatcher and terminal UI engine.
- **Package Manifest Script Lifecycle**: Supports `"install": "cex-pack downloads"` in `cex-pack.json` scripts section for automated dependency installation.
- **JSON Syntax & Structure Validation**: Strict checks for proper brace pairing, quotes, comma separators, and JSON type integrity.
- **Manifest Schema Governance**: Validates mandatory fields (`name`, `version`, `main`, `target: "runtime"`, `required`, `dependencies`, `includeDirs`, `scripts`, `lint`).
- **Rule Enforcer Integration**:
  - **Rule 69**: Ensures `"target": "runtime"` remains strictly immutable across all manifest files.
  - **Rule 29**: Verifies EOF single newline integrity.
  - **Rule 72**: Enforces clean dynamic path references.

---

## Installation & Usage

### Downloading Dependencies via `cex-pack-linked.json`
```bash
cex-pack downloads
# or targeting specific manifest
cex-pack downloads path/to/cex-pack.json
```

When `cex-pack downloads` executes:
1. It reads declared dependencies and versions in `cex-pack.json`.
2. Resolves and decrypts the download URLs defined in `cex-pack-linked.json`.
3. Clones or pulls real Git / CVM repositories into `.cex_boxes/{cleanName}`.

### Generating or Updating `cex-pack-linked.json`
```bash
cex-pack link
```

### `cex-pack-linked.json` Format
```json
{
  "name": "@2tek/sample-pack-app",
  "version": "1.0.0",
  "lockfileVersion": 1,
  "encryption": {
    "algorithm": "aes-256-cbc",
    "scope": "project"
  },
  "dependencies": {
    "cex-cli": {
      "version": "1.0.0",
      "url": "cex-enc:1031f8219144a2e4d34b264ea78d0777:62535b17ee387ee55fc11319834df9e8..."
    },
    "cexr": {
      "version": "1.0.0",
      "url": "cex-enc:3726758e935b97dd338db36620f8777b:c707960ca9519f40cff68e2ca8903315..."
    }
  }
}
```

---

## Command Line Options

| Command / Option | Description |
|---|---|
| `cex-pack downloads [path]` | Download dependencies defined in `cex-pack.json` via `cex-pack-linked.json` into `.cex_boxes/{dependencyName}` |
| `cex-pack link [path]` | Generate or refresh project-encrypted `cex-pack-linked.json` lockfile |
| `cex-pack check [path]` | Validate syntax and schema of specified `cex-pack.json` file |
| `cex-pack lint [path]` | Run deep linter and rule compliance check |
| `cex-pack init` | Scaffold a valid standard `cex-pack.json` manifest template |
| `--verbose` | Output detailed step-by-step diagnostic information |
| `--help`, `-h` | Display usage instructions and available subcommands |

---

## Examples

- **`examples/pack-with-downloads`**: Basic standalone Cex application installing dependencies via Git into `.cex_boxes/`.
- **`examples/project-have-packages-insides`**: Multi-package workspace project containing internal modular packages nested under `packages/` (`packages/core`, `packages/ui`) alongside external `.cex_boxes/` dependencies.

---

## Governance & Integrity

This package is managed under `.agents` rule governance:
- **`PRODUCT_TARGET.md`**: Specification & Target Definition
- **`standards/`**: Architectural and Manifest Specifications
- **`rules/`**: Enforcement Rules (`Rule 29`, `Rule 69`, `Rule 72`, `Rule Pure Cex Binaries`, `Rule Syntax Validation`)


