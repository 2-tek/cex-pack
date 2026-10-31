# @cex-env-loader (Cex Environment Loader)

- **Repository**: `https://github.com/2-tek/cex-env-loader.git`
- **Target**: `runtime`
- **Required**: `cexr ^8`, `cexp ^8`

## Overview

`@cex-env-loader` is the canonical high-performance environment configuration loader and `.env` parser for the 2-TEK Cex native high-performance server language ecosystem.
It provides automatic hierarchical directory searching, comments and quotes parsing, type-safe casting, programmatic overrides, in-memory caching, and system environment fallback.

## Features

- **Standard DotEnv Parsing**:
  - Full-line comments (`# ...`) and inline comments (`KEY=value # comment`).
  - Single quotes (`'...'`) for raw literals and double quotes (`"..."`) with escape sequence unescaping (`\n`, `\t`, `\"`, `\\`).
  - Optional `export ` prefix support (`export PORT=8080`).
  - Whitespace trimming around keys and unquoted values.
- **Hierarchical Directory Discovery**:
  - Automatically searches up directory trees:
    1. `.env`
    2. `document/.env`
    3. `documents/.env`
    4. `config/.env`
    5. `../.env`
    6. `../../.env`
    7. `../../../.env`
    8. `../../../../.env`
- **Type-Safe Getters**:
  - `loadString(key, defaultVal)`: Returns string value.
  - `loadInt(key, defaultVal)`: Returns parsed 32-bit signed integer.
  - `loadBool(key, defaultVal)`: Returns boolean (`true`/`1`/`yes`/`on` vs `false`/`0`/`no`/`off`).
  - `loadFloat(key, defaultVal)`: Returns parsed 64-bit double float.
  - `loadPort(defaultPort)`: Convenience getter for `PORT` integer.
  - `has(key)`: Checks whether a key is defined in files or environment.
- **System Environment Integration**:
  - Seamless fallback to `std::getenv` when a key is absent from `.env` files.
- **In-Memory Caching & Overrides**:
  - Fast in-memory key-value cache avoiding repeated disk reads.
  - Programmatic overrides via `EnvLoader::set(key, value)`.
  - Cache refresh via `EnvLoader::reload()`.
  - Export to `.env` file via `EnvLoader::saveToFile(path)`.

## Usage Examples

### 1. Basic Server Port & Database Loading

```cex
import "@cex-env-loader";

// Reads PORT from .env or system environment, defaults to 8080
let port: int = EnvLoader::loadPort(8080);

// Reads string configuration
let dbHost: string = EnvLoader::loadString("DB_HOST", "127.0.0.1");
let dbPass: string = EnvLoader::loadString("DB_PASS", "");

// Reads boolean flag
let debugMode: bool = EnvLoader::loadBool("DEBUG", false);

println("Listening on port: " + std::to_string(port));
```

### 2. Custom Search Paths

```cex
// Add high-priority custom search path
EnvLoader::addSearchPath("/etc/twotek/app.env");
EnvLoader::addSearchPath("custom.env");

let apiKey: string = EnvLoader::loadString("API_KEY", "default-key");
```

### 3. Programmatic Overrides & Cache Reload

```cex
EnvLoader::set("NODE_ENV", "production");
EnvLoader::set("PORT", "3000");

let port: int = EnvLoader::loadPort(8080); // returns 3000

// Invalidate cache and re-read from disk
EnvLoader::reload();
```

## Architecture & Standards

- **Rule 69 (Target Immutability)**: Native compilation with C++20 `-O3`.
- **Rule 29 (EOF Integrity)**: Exactly one trailing newline.
- **Rule 72 (Dynamic Paths)**: Dynamic relative path resolution with zero hardcoded personal paths.
