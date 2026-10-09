# 2-TEK Cex-Pack (`cex-pack`)

> **Native Syntax Checker, Manifest Validator & Dependency Downloader for `cex-pack.json` & `cex-pack-linked.json` in the C-ext (`.cex`) Ecosystem**

`cex-pack` is the canonical syntax validation and package dependency download tool designed to parse, analyze, and enforce governance rules on `cex-pack.json` manifests, lock encrypted repository URLs in `cex-pack-linked.json`, and download real Git / CVM dependencies into `.cex_boxes/`. Integrated with `cex-cli`.

---

## Features

- **`cex-pack-linked.json` Encrypted Lockfile**: Links package versions to AES-256 encrypted repository download URLs (`cex-enc:...`) specifically scoped for the project.
- **Git & CVM Dependency Downloader (`.cex_boxes`)**: Downloads package dependencies declared in `cex-pack.json` and resolved in `cex-pack-linked.json` via real `git clone --depth 1` into `.cex_boxes/{dependencyName}` without `@` alias symbols.
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
  "packages": {
    "cex-cli": {
      "version": "1.0.0",
      "url": "https://github.com/2-tek/cxvm.git#packages/cex-cli",
      "encryptedUrl": "cex-enc:e13758ce539c69f44fbecab52e88b21b:d40adf66..."
    },
    "cexr": {
      "version": "https://github.com/2-tek/cexr.git",
      "url": "https://github.com/2-tek/cexr.git",
      "encryptedUrl": "cex-enc:d068f398735204539d388becede6c6dd:e9fc88f9..."
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

## Governance & Integrity

This package is managed under `.agents` rule governance:
- **`PRODUCT_TARGET.md`**: Specification & Target Definition
- **`standards/`**: Architectural and Manifest Specifications
- **`rules/`**: Enforcement Rules (`Rule 29`, `Rule 69`, `Rule 72`, `Rule Syntax Validation`)
