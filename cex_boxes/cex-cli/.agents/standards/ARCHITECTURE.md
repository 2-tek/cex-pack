# Cex-CLI Architecture Standard

## Component Breakdown

1. **`index.cex`**: CLI entrypoint, argument parsing dispatcher, exit code propagator.
2. **`cli.cex`**: Command router handling subcommands (`build`, `run`, `test`, `watch`, `doctor`, `agents`, `cvm`).
3. **`builder.cex`**: Build execution kernel, handling process dispatching, build target selection, environment variable injection, and task timing.
4. **`terminal.cex`**: Terminal rendering engine for ASCII framing, ANSI color management, progress indicators, and interactive menus.
5. **`config.cex`**: JSON and environment configuration parser loading project build options.
