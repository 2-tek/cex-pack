# Cex-Pack Architecture Standard

## Component Breakdown

1. **`index.cex`**: CLI entrypoint, argument parsing, error handler, exit code manager.
2. **`cli.cex`**: Command router dispatching subcommands (`check`, `lint`, `init`, `--help`).
3. **`parser.cex`**: Tokenizer and JSON structural parser computing line numbers, syntax nodes, and parsing tokens.
4. **`schema.cex`**: Schema definition rules and property checkers for `cex-pack.json` attributes.
5. **`validator.cex`**: High-level validation engine orchestrating syntax, schema, and rule compliance (Rule 69, Rule 29, Rule 72).
6. **`reporter.cex`**: ANSI diagnostic renderer formatting human-readable error messages and line snippets.
