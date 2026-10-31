# cex-cli (Cex-CLI)

> **Terminal Build Command Runner, Task Automation & Terminal UI Engine written in C-ext (Cex)**

**Cex-CLI (`cex-cli` / `cli`)** provides automated terminal build command management, interactive terminal UI windows, task automation, and environment diagnostics for the 2-TEK Cex ecosystem:

- **Interactive Build Terminal**: Fast native CLI tool for compiling, building, testing, watching, and running Cex projects (`cex-cli build` or `cxvm cli build`).
- **Task Automation Engine**: Configurable build pipelines, preset scripts, parallel execution, and watcher routines.
- **Terminal UI Window System**: ANSI colorized framed application windows, progress spinners, live logs, and diagnostic dashboards.
- **Built-in Governance & Specs**: Embedded `.agents` specification directory, `.github` CI workflows, and `.cvm` Change Record version control metadata.

---

## 1. Directory Architecture

```text
packages/cex-cli/
+-- .agents/                      # Agent specifications, targets, rules, and standards
|   +-- PRODUCT_TARGET.md         # Canonical product specification
|   +-- rules/                    # Architectural and CLI operational rules
|   +-- standards/                # Terminal spec & architecture docs
|   +-- agents/                   # Role persona definitions (coder, leader, etc.)
+-- .github/                      # GitHub CI/CD workflows and issue templates
|   +-- workflows/                # CI build, test, and release pipelines
|   +-- ISSUE_TEMPLATE/           # Standard issue report templates
+-- .githubs                      # Symlink to .github
+-- .cvm/                         # Change Record version control store
|   +-- config                    # CVM repository configuration
|   +-- HEAD                      # Active ref pointer
|   +-- records/                  # High-fidelity .cvm change patches
+-- cex-pack.json                 # Cex package manifest
+-- README.md                     # Documentation
+-- examples/                     # Executable example programs
|   +-- 01_build_pipeline_runner.cex # Build target dispatching & task pipelines
|   +-- 02_terminal_ui_rendering.cex # ANSI framed windows & progress rendering
|   +-- 03_agents_and_cvm_governance.cex # Governance & doctor inspection APIs
+-- src/                          # C-ext native source code
|   +-- types.cex                 # Command types, options, build specs
|   +-- config.cex                # CLI configuration parser
|   +-- terminal.cex              # ANSI terminal frame renderer & UI widgets
|   +-- builder.cex               # Build task execution engine & dispatchers
|   +-- cli.cex                   # Command parser and controller
|   +-- index.cex                 # Package main entrypoint
+-- tests/                        # Comprehensive unit & integration tests
    +-- cli.test.cex              # Cex CLI unit test suite
```

---

## 2. CLI Usage & Commands

```bash
# Display help and available build commands
./bin/cex run packages/cex-cli/src/index.cex -- --help

# Execute interactive terminal build runner
./bin/cex run packages/cex-cli/src/index.cex -- build

# Run build for specific target environment
./bin/cex run packages/cex-cli/src/index.cex -- build --target production

# Run automated test suite
./bin/cex run packages/cex-cli/src/index.cex -- test

# Watch mode for auto-rebuild on file changes
./bin/cex run packages/cex-cli/src/index.cex -- watch

# Run system doctor diagnostic check
./bin/cex run packages/cex-cli/src/index.cex -- doctor

# Inspect .agents rules and standards
./bin/cex run packages/cex-cli/src/index.cex -- agents status

# Inspect .cvm repository status and patch records
./bin/cex run packages/cex-cli/src/index.cex -- cvm status
```

---

## 3. Executable Examples

Run the example programs using `cxvm` / `cexr`:

```bash
# 1. Build Pipeline Runner & Task Dispatcher Example
cxvm run packages/cex-cli/examples/01_build_pipeline_runner.cex

# 2. Terminal UI Window System & Renderer Example
cxvm run packages/cex-cli/examples/02_terminal_ui_rendering.cex

# 3. Governance Audit & System Doctor Inspector Example
cxvm run packages/cex-cli/examples/03_agents_and_cvm_governance.cex
```

---

## 4. Governance Standards (.agents, .github, .cvm)

- **`.agents`**: Contains canonical product target specification (`PRODUCT_TARGET.md`), architectural standards (`ARCHITECTURE.md`), CLI terminal standards (`CLI_TERMINAL_SPEC.md`), rule enforcement, and persona definitions.
- **`.github`**: Complete CI pipeline for cross-platform Linux, macOS, and Windows build and test verification, along with standard PR and issue templates.
- **`.cvm`**: Local Change Record format repository tracking version history and patch deltas.
