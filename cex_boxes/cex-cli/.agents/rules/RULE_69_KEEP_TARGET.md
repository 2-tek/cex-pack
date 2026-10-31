# Rule 69: Immutable Target Definition

> **Severity: MANDATORY / STRICT**

## 1. Requirement
The manifest `cex-pack.json` must strictly maintain:
```json
"target": "runtime"
```
Do not alter target to external native binaries or node dependencies.

## 2. Rationale
Ensures consistent binary loading within `.cex_boxes` across all host environments.
