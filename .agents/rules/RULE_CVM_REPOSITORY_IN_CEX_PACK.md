# Mandatory Rule: CVM Repository Specification and Multi-Manifest Routing in cex-pack (RULE_CVM_REPOSITORY_IN_CEX_PACK)

<!-- Rule Conformance: Rule 69 (Target), Rule 29 (EOF), Rule 72 (Dynamic Paths), Rule 80 (Root @), Rule 82 (CXVM & Direct CexR) -->

> **MANDATORY CVM REPOSITORY & MANIFEST PATH SPECIFICATION**:
> In all `mvc-lighting`, `mvc-deepsea`, and Cex project workspaces, repositories, and packages:
>
> 1. **`cvm_repository` MANIFEST DECLARATION**:
>    - Projects may declare a CodeVersionManager repository configuration via `"cvm_repository"` inside `cex-pack.json`.
>    - Supported Formats:
>      - **String URL shorthand**:
>        ```json
>        "cvm_repository": "cvm://localhost:3110/repo/my-app"
>        ```
>      - **Config Object**:
>        ```json
>        "cvm_repository": {
>            "type": "cvm",
>            "url": "cvm://localhost:3110/repo/my-app",
>            "path": "web-pack.json",
>            "branch": "main"
>        }
>        ```
>
> 2. **CUSTOM MANIFEST PATH ROUTING (`path: "web-pack.json"`)**:
>    - The `"path"` property inside `"cvm_repository"` allows projects (especially fullstack web or frontend modules) to declare an alternate or complementary manifest (e.g. `"path": "web-pack.json"`).
>    - **Constraint & Security**: The path MUST remain within the project root. Path traversal (`..`) is strictly prohibited (`RULE_CVM_REPO_PATH_TRAVERSAL`). The path cannot be empty (`RULE_CVM_REPO_EMPTY_PATH`).
>
> 3. **MULTI-MANIFEST PUSH, COMMIT, AND DOWNLOAD WORKFLOWS**:
>    - **Dependency Resolution & Download**:
>      - When `cex-pack downloads` executes, it downloads dependencies declared in `cex-pack.json`.
>      - If `"cvm_repository"` specifies an alternate manifest like `web-pack.json` and that file exists, `cex-pack` automatically discovers and resolves dependencies from both manifests into `.cex_boxes/`.
>      - Developers and automated agents may also target any custom manifest explicitly via the `--path` CLI flag:
>        ```bash
>        cex-pack downloads --path web-pack.json
>        ```
>    - **CVM Commit & Staging**:
>      - Developers and agent tasks can easily stage and commit changes targeting specific manifests:
>        ```bash
>        cex-pack commit -m "feat(ui): update web dependencies" --path web-pack.json
>        ```
>    - **CVM Remote Push**:
>      - Developers and agents can push directly to the configured CVM repository:
>        ```bash
>        cex-pack push --path web-pack.json
>        ```
>    - **CVM Subcommand Dispatching**:
>      - `cex-pack cvm [status|commit|push|sync] [--path <manifest>]` dispatches CVM commands while synchronizing the targeted manifest.
>
> 4. **VALIDATION & LINT INTEGRITY**:
>    - Manifest validation via `cex-pack check`, `cex-pack lint`, and `cxvm test` validates all declared `cvm_repository` configurations.
>    - Any empty path, empty URL, or path traversal triggers diagnostic error codes (`RULE_CVM_REPO_EMPTY_PATH`, `RULE_CVM_REPO_EMPTY_URL`, `RULE_CVM_REPO_PATH_TRAVERSAL`).
