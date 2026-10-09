# Example: Project with Internal Packages Inside (`project-have-packages-insides`)

This example demonstrates a canonical Cex workspace project containing internal modular packages nested under the `packages/` directory, while managing external dependencies via `cex-pack` and `.cex_boxes/`.

---

## 1. Directory Structure

```text
project-have-packages-insides/
├── .cex_boxes/               # Installed external dependencies
│   └── PROGRESS.md           # Progress tracking and sizes
├── cex-pack.json             # Root workspace manifest (target: "runtime")
├── cex-pack-linked.json      # Encrypted version-to-URL lockfile
├── packages/                 # Internal packages directory
│   ├── core/                 # Core domain service package
│   │   ├── cex-pack.json     # Internal package manifest
│   │   └── src/index.cex     # Core service implementation
│   └── ui/                   # Visual presentation package
│       ├── cex-pack.json     # Internal package manifest
│       └── src/index.cex     # UI component implementation
├── src/
│   └── main.cex              # Root application entrypoint consuming internal packages
└── README.md                 # Architectural guide and documentation
```

---

## 2. Key Architecture Concepts

1. **Root Workspace Manifest (`cex-pack.json`)**:
   - Declares the project metadata, engine prerequisites (`required.cexr: "^8"`), and workspace paths:
     ```json
     "workspaces": [
       "packages/*"
     ],
     "packages": [
       "packages/core",
       "packages/ui"
     ]
     ```
   - Maintains `"target": "runtime"` in conformance with Rule 69.
   - Includes internal package paths in `"includeDirs"`.

2. **Internal Package Manifests**:
   - Each nested package in `packages/<name>` maintains its own `cex-pack.json` with independent versioning, entrypoint (`main`), and metadata.

3. **Dependency Isolation**:
   - External dependencies (`cex-cli`, `cexr`, `cex-service`, `cex-test`) are downloaded into `.cex_boxes/` using `cex-pack downloads`.
   - `.cex_boxes/PROGRESS.md` tracks download percentage, individual package sizes, and overall summary.

---

## 3. Usage & Execution

### Validate Root and Internal Manifests
```bash
# Validate root manifest
cexr run ../../bin/cex-pack.cex check cex-pack.json

# Validate internal package manifests
cexr run ../../bin/cex-pack.cex check packages/core/cex-pack.json
cexr run ../../bin/cex-pack.cex check packages/ui/cex-pack.json
```

### Install / Download External Dependencies
```bash
cexr run ../../bin/cex-pack.cex downloads
```

### Run Root Application
```bash
cexr run src/main.cex
```
