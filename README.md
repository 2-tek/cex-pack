# 2-TEK Cex-Pack (`cex-pack`)

> **Native Syntax Checker, Manifest Validator & Dependency Downloader for `cex-pack.json` & `cex-pack-linked.json` in the C-ext (`.cex`) Ecosystem**

`cex-pack` is the canonical syntax validation and package dependency download tool designed to parse, analyze, and enforce governance rules on `cex-pack.json` manifests, lock encrypted repository URLs in `cex-pack-linked.json`, and download real Git / CVM dependencies into `.cex_boxes/`. Integrated with `cex-cli`.

---

## Features

- **Real-Time Progress Tracking (`.cex_boxes/PROGRESS.md`)**: Automatically updates `.cex_boxes/PROGRESS.md` with total `.cex_boxes` directory size (`current: 0.5Mb`) and formatted dependency lines following the template: `[state][percent][size][totalsize][name][linked]`.
- **Terminal Progress Logger (`cex-log`)**: Outputs styled and timestamped progress logs into the terminal during downloads via the `@cex-log` dependency pulled into `.cex_boxes/cex-log`.
- **Native `.cex` CLI Implementation (`bin/cex-pack.cex`)**: CLI commands implemented as pure native `.cex` files.
- **`cex-pack-linked.json` Encrypted Lockfile**: Links package versions to AES-256 encrypted repository download URLs (`cex-enc:...`) specifically scoped for the project.
- **Git & CVM Dependency Downloader (`.cex_boxes`)**: Downloads package dependencies declared in `cex-pack.json` and resolved in `cex-pack-linked.json` via real `git clone --depth 1` into `.cex_boxes/{dependencyName}` using real canonical project names without `@` alias symbols.
- **Integrated with `cex-cli`**: Full integration with the `cex-cli` task dispatcher and terminal UI engine.
- **Package Manifest Script Lifecycle**: Supports `"install": "cex-pack downloads"` in `cex-pack.json` scripts section for automated dependency installation.
- **JSON Syntax & Structure Validation**: Strict checks for proper brace pairing, quotes, comma separators, and JSON type integrity.
- **Manifest Schema Governance**: Validates mandatory fields (`name`, `version`, `main`, `target: "runtime"`, `required`, `dependencies`, `includeDirs`, `scripts`, `lint`).
- **Environment Auto-Loading (`options.envs`)**: Auto-loads `.env` configuration files based on the active environment (`NODE_ENV`, `CEX_ENV`, `APP_ENV`, `ENVIRONMENT`) mapped via `options: { envs: { [environmentName]: [root env path] } }` or top-level `envs`.
- **Process Environment Injection (`process.env`)**: Maps and injects variables into `process.env` using dynamic `"ENV::[variableName]"` or `"ENV::variableName"` references resolved directly from `.env` files or system environment.
- **Pure Cex Runtime API (`ProcessEnv` & `process.env`)**: Zero-dependency runtime environment manager supporting `ProcessEnv::get(...)`, `ProcessEnv::getInt(...)`, `ProcessEnv::getBool(...)`, `ProcessEnv::set(...)`, and `process.env.get(...)`.
- **CVM Repository & Multi-Manifest Routing (`cvm_repository`)**: Declare CodeVersionManager remote endpoints and alternate manifests (e.g. `"path": "web-pack.json"`) directly inside `cex-pack.json` for multi-manifest sync, commit, push, and dependency installation.
- **Rule Enforcer Integration**:
    - **Rule 69**: Ensures `"target": "runtime"` remains strictly immutable across all manifest files.
    - **Rule 29**: Verifies EOF single newline integrity.
    - **Rule 72**: Enforces clean dynamic path references.
    - **RULE_CVM_REPOSITORY_IN_CEX_PACK**: Enforces valid `cvm_repository` format, URL integrity, and forbids path traversal (`..`).

---

## Installation & Usage

### Downloading Dependencies via `cex-pack-linked.json`

```bash
cex-pack downloads
# or targeting alternate manifest (e.g. web-pack.json)
cex-pack downloads --path web-pack.json
```

When `cex-pack downloads` executes:

1. It reads declared dependencies and versions in `cex-pack.json`.
2. If `cvm_repository.path` points to a secondary manifest (e.g. `web-pack.json`) and exists, it automatically merges dependencies from both manifests.
3. Resolves and decrypts the download URLs defined in `cex-pack-linked.json`.
4. Clones or pulls real Git / CVM repositories into `.cex_boxes/{cleanName}`.

### CVM Repository & Multi-Manifest Workflows

```bash
# Push working tree and manifest to CVM remote repository
cex-pack push --path web-pack.json

# Commit manifest changes to CVM repository
cex-pack commit -m "feat(web): update client bundle" --path web-pack.json

# Dispatch CVM subcommands
cex-pack cvm status
cex-pack cvm sync
```

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

| Command / Option            | Description                                                                                                    |
| --------------------------- | -------------------------------------------------------------------------------------------------------------- |
| `cex-pack env [path]`       | Auto-load environment variables from path defined in `options.envs` or `envs` for `process.env`                |
| `cex-pack downloads [path]` | Download dependencies defined in `cex-pack.json` via `cex-pack-linked.json` into `.cex_boxes/{dependencyName}` |
| `cex-pack push [path]`      | Push manifest and working tree changes to CVM repository                                                       |
| `cex-pack commit [msg]`     | Stage and commit manifest changes to CVM repository (`-m "message"`)                                           |
| `cex-pack cvm [action]`     | Dispatch CVM commands (`status`, `push`, `commit`, `sync`)                                                     |
| `cex-pack link [path]`      | Generate or refresh project-encrypted `cex-pack-linked.json` lockfile                                          |
| `cex-pack check [path]`     | Validate syntax and schema of specified `cex-pack.json` file                                                   |
| `cex-pack lint [path]`      | Run deep linter and rule compliance check                                                                      |
| `cex-pack thunder [path]`   | Validate zero-file Thunder container configuration                                                             |
| `cex-pack init`             | Scaffold a valid standard `cex-pack.json` manifest template                                                    |
| `--path <file>`             | Target alternate manifest (e.g. `web-pack.json`) for check, downloads, push, or commit                         |
| `-m <message>`              | Commit message for `cex-pack commit`                                                                           |
| `--verbose`                 | Output detailed step-by-step diagnostic information                                                            |
| `--help`, `-h`              | Display usage instructions and available subcommands                                                           |

---

## CVM Repository Configuration (`cvm_repository`)

Declare CodeVersionManager repository and secondary manifest routing in `cex-pack.json`:

```json
{
    "name": "@2tek/my-application",
    "version": "1.0.0",
    "main": "src/index.cex",
    "target": "runtime",
    "cvm_repository": {
        "type": "cvm",
        "url": "cvm://localhost:3110/repo/my-application",
        "path": "web-pack.json",
        "branch": "main"
    },
    "dependencies": {
        "cex-cli": "1.0.0"
    }
}
```

Or string URL shorthand:

```json
"cvm_repository": "cvm://localhost:3110/repo/my-application"
```

---

## Environment Configuration & `process.env`

Declare environment configuration in `cex-pack.json`:

```json
{
    "name": "@2tek/my-service",
    "version": "1.0.0",
    "main": "src/index.cex",
    "target": "runtime",
    "options": {
        "envs": {
            "development": "./.env",
            "production": "./.env.production",
            "test": "./.env.test"
        }
    },
    "process.env": {
        "PORT": "ENV::[PORT]",
        "DATABASE_URL": "ENV::[DATABASE_URL]",
        "APP_SECRET": "ENV::SECRET_KEY",
        "APP_NAME": "MyService"
    }
}
```

### Auto-Loading in Cex Source Code:

```cex
import "@cex-pack/src/env.cex";

// Auto-loads active environment from cex-pack.json options.envs
ProcessEnv::autoLoad("cex-pack.json", "production");

// Access environment variables
let port: int = ProcessEnv::getInt("PORT", 3000);
let dbUrl: string = process.env.get("DATABASE_URL", "");
```

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
