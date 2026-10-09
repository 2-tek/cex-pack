# Standard for Project

> **Severity: MANDATORY**

1. Maintain modular code structure in `src/` (`parser.cex`, `schema.cex`, `validator.cex`, `reporter.cex`, `cli.cex`, `index.cex`).
2. Maintain comprehensive test coverage in `tests/cex_pack.test.cex`.
3. All source code and metadata files must adhere to Rule 29 (EOF newline) and Rule 72 (Dynamic Paths).
4. Pure Cex Binaries: The `bin/` directory must contain exclusively `.cex` files (`bin/cex-pack.cex`, `bin/cex-log.cex`). No `.js` files or JavaScript/Node.js scripts are permitted.
5. Documents Update: Whenever a new feature or command is introduced, update the central documentation book at `documents/` under `../{projectName}`.
