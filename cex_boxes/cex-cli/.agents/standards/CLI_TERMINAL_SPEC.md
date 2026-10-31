# CLI Terminal Window & Specification Standard

## Terminal UI Layout

```text
+--------------------------------------------------------+
|  2-TEK Cex-CLI Build Terminal Window v1.0.0            |
+--------------------------------------------------------+
| Target Environment : [Production]                       |
| Runtime Engine     : cexp-v8-stable v8.0.0                      |
| Compiler           : CexP v8.0.0                      |
| Status             : [BUILDING] (4/4 Tasks Complete)   |
+--------------------------------------------------------+
```

## Subcommand Specification

| Subcommand | Description                          | Options                          |
| ---------- | ------------------------------------ | -------------------------------- |
| `build`    | Compiles and builds project packages | `--target [dev                   | prod]`, `--verbose` |
| `test`     | Runs test suites via CexTest         | `--grep <filter>`, `--fail-fast` |
| `watch`    | Auto-rebuilds on source changes      | `--interval <ms>`                |
| `doctor`   | Diagnostics for Cex toolchain        | `--fix`                          |
| `agents`   | Validates `.agents` specifications   | `status`, `check`                |
| `cvm`      | Inspects `.cvm` Change Records       | `status`, `diff`                 |
