# Rule: Allow Git and Cex Engine Commands

> **Severity: PERMISSIVE / GUIDELINE**

## 1. Requirement
Agents working within `cex-pack` are authorized to execute:
- `git` commands (`git status`, `git add`, `git commit`, `git diff`, `git log`)
- `cexr` runtime commands (`cexr run src/index.cex`, `cexr run tests/cex_pack.test.cex`)
- `cexp` compiler commands (`cexp build src/index.cex -o bin/cex-pack`)

## 2. Rationale
Enables automated testing, verification, and change tracking during agent operations.
