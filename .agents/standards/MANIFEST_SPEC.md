# Cex-Pack Manifest Specification (`cex-pack.json`)

## Required Fields

- **`name`**: String. Package identifier (e.g. `@2tek/pack`, `@cex-lint`, `@2tek/cli`).
- **`version`**: String. SemVer compliant string (e.g. `1.0.0`).
- **`main`**: String. Main entrypoint path relative to package root (e.g. `src/index.cex`).
- **`target`**: String. Must strictly be `"runtime"` (Rule 69).
- **`required`**: Object. Required execution engine versions (e.g. `"cexr": "^8"`).

## Optional Fields

- **`description`**: String. High-level package overview.
- **`author`**: String. Package author or team.
- **`license`**: String. License identifier (e.g. `MIT`).
- **`dependencies`**: Object. Map of dependency names to versions.
- **`includeDirs`**: Array of Strings. Include paths for compiler resolution.
- **`cFlags`**: Array of Strings. Compiler build flags.
- **`scripts`**: Object. Named task commands (`build`, `start`, `dev`, `lint`, `test`).
- **`lint`**: Object. Linting rules configuration (`onBuild`, `include`, `rules`).
