# @cex-service (CexService Framework)

High-Performance, Next.js-inspired Backend & Service Framework (CexService) natively implemented in **C-ext (Cex)** with Dynamic Auto-Scaling and Enterprise DDoS Mitigation.

`@cex-service` provides an enterprise-grade backend service architecture combining Next.js routing ergonomics with C++20 machine code execution. It features parametric URL routing, full SSR document rendering with client hydration data payloads, composable middleware pipelines (CORS, Auth, Timing, Logger), intelligent Auto-Scaling under large transfer transactions, and high-frequency DDoS protection (> 100 req/s per IP default) configurable directly from `.env`.

---

## 🚀 Key Features

1. **Intelligent Auto-Scaling & DDoS Defense (`src/autoscale.cex`)**:
    - **DDoS Mitigation**: Automatically tracks per-IP request frequency. Rejects traffic exceeding the sliding window threshold (default: **> 100 req/s for an IP**) with HTTP `429 Too Many Requests`, `Retry-After: 60`, and `X-DDoS-Protection: Triggered`.
    - **Defensive Auto-Scale Trigger**: When a DDoS attack or traffic spike is detected, the engine scales up worker pool capacity (e.g. from 2 to 4, 8, or configured max) into `DDOS_MITIGATION` status to preserve service availability.
    - **Large Transfer Transactions**: Detects high-bandwidth transactions and payload surges (default: >= 1 MB or burst of transfer transactions), dynamically scaling workers to prevent IO starvation.
    - **Graceful Scale-Down**: Automatically scales worker capacity back down to `minWorkers` once traffic normalizes and the cooldown period expires.
    - **.env & Environment Configuration**: Transparently loads configuration from `.env` files or system environment variables with type-safe parsing and sensible defaults.

2. **Next.js-style Architecture & Routing (`src/router.cex`)**:
    - **Parametric Routes**: Dynamic URL parameter extraction (e.g. `/api/users/:id`, `/api/teams/:teamId/members/:memberId`).
    - **Method Routing**: Supports `GET`, `POST`, `PUT`, `DELETE`, and wildcard `*`.
    - **Unified Registry**: Manage both SSR pages and JSON API endpoints under a single declarative router.

3. **Server-Side Rendering (SSR) (`src/ssr.cex`)**:
    - Next.js style `page(pattern, doc)` producing full HTML5 documents with responsive viewport tags, SEO metadata, scoped styling, and embedded JSON hydration state (`__PERFECT_DATA__`).
    - Integrated with Cex terminal cards for visual debugging.

4. **Composable Middleware Pipeline (`src/middleware.cex`)**:
    - Next.js / Express style middleware execution chain with early-abort error handling.
    - Built-in middlewares:
        - `AutoScaleMiddleware`: Automated DDoS shielding, large transfer scaling, and response header injection (`X-Auto-Scale-Workers`, `X-Auto-Scale-Status`).
        - `TimingMiddleware`: Sub-millisecond latency tracking and `X-Response-Time` headers.
        - `CorsMiddleware`: Cross-Origin Resource Sharing configuration.
        - `AuthMiddleware`: Bearer token validation and automatic 401 Unauthorized handling.
        - `LoggerMiddleware`: Formatted request and response logging.

5. **Rich Request & Response Primitives (`src/request.cex`, `src/response.cex`)**:
    - `PerfectRequest`: Client IP resolution (`X-Forwarded-For`, `X-Real-IP`, or local client IP), query parameter parsing, extracted URL parameters (`:id`), headers map, and body access.
    - `PerfectResponse`: Fluent status chaining (`res->status(200)`), header setting, JSON serialization, HTML responses, redirects, and HTTP/1.1 wire protocol formatting.

6. **Application Core Engine (`src/app.cex`)**:
    - `CexService` / `PerfectApp`: Unified application controller coordinating routes, middlewares, SSR pages, API routes, fallback 404 handlers, auto-scaling telemetry, and performance metrics.

---

## ⚙️ Environment Configuration (`.env`)

`@cex-service` can load configuration from `.env`, `config/.env`, or system environment variables:

```bash
# Auto-Scale Settings
AUTOSCALE_ENABLED=true
AUTOSCALE_MIN_WORKERS=2
AUTOSCALE_MAX_WORKERS=16
AUTOSCALE_SCALE_UP_THRESHOLD=500     # req/s global threshold
AUTOSCALE_SCALE_DOWN_THRESHOLD=50    # req/s global threshold
AUTOSCALE_LARGE_TRANSFER_BYTES=1048576 # 1 MB (Large transfer transaction threshold)
AUTOSCALE_LARGE_TX_THRESHOLD=5       # tx count threshold per second
AUTOSCALE_COOLDOWN_SECONDS=10        # Stabilization cooldown before scale down
AUTOSCALE_SCALE_UP_STEP=2
AUTOSCALE_SCALE_DOWN_STEP=1

# DDoS Protection Settings
DDOS_PROTECTION_ENABLED=true
DDOS_RATE_LIMIT_PER_IP=100           # Default: > 100 req/s per IP triggers DDoS mitigation
DDOS_WINDOW_SECONDS=1                # 1-second sliding measurement window
DDOS_BAN_DURATION_SECONDS=60         # IP block duration in seconds
DDOS_AUTO_SCALE_TRIGGER=true         # Auto-scale worker pool on attack detection
```

---

## 📦 Package Structure

```text
cex-service/
├── cex-pack.json                  # Package manifest (@cex-service)
├── README.md                      # Package documentation
├── src/
│   ├── types.cex                  # HTTP status codes, headers, and parameter definitions
│   ├── request.cex                # Request abstraction with client IP, query & param extraction
│   ├── response.cex               # Response abstraction with JSON, HTML, and HTTP/1.1 serialization
│   ├── autoscale.cex              # Auto-Scale Engine, DDoS Shield (>100 req/s), Large Transfer Monitor & .env Loader
│   ├── async_handler.cex          # Native Cex v8 Promise & Async Route Handlers
│   ├── middleware.cex             # Composable middleware pipeline (AutoScale, Timing, CORS, Auth, Logger)
│   ├── router.cex                 # Parametric and dynamic routing engine
│   ├── ssr.cex                    # Server-Side Rendering document generator with hydration state
│   ├── app.cex                    # Unified application engine and CexService pipeline
│   └── index.cex                  # Package entry point
├── tests/
│   ├── autoscale.test.cex         # Comprehensive Auto-Scale & DDoS unit & integration test suite
│   └── promise_async.test.cex     # Native Promise & asynchronous route handling test suite
└── examples/
    ├── 01_rest_api.cex            # Next.js /api style CRUD microservice
    ├── 02_ssr_pages.cex           # Next.js style SSR pages with HTML & hydration
    ├── 03_middleware_pipeline.cex # Chained authentication, CORS, and latency timing
    ├── 04_autoscale_ddos.cex      # Auto-Scale with large transfer transactions & DDoS protection
    └── 05_promise_async.cex       # Native Cex v8 Promise & asynchronous microservice
```

---

## 🛠️ Usage & Verification

### Run Comprehensive Test Suites

```bash
# Auto-Scale & DDoS Protection Tests
cxvm test cex-service/tests/autoscale.test.cex

# Promise & Asynchronous Execution Tests
cxvm test cex-service/tests/promise_async.test.cex
```

### Run Examples

```bash
# 1. REST API with Parametric Routes (:id)
cexr cex-service/examples/01_rest_api.cex

# 2. Server-Side Rendered (SSR) HTML Pages
cexr cex-service/examples/02_ssr_pages.cex

# 3. Security & Middleware Pipeline (Auth, CORS, Timing)
cexr cex-service/examples/03_middleware_pipeline.cex

# 4. Auto-Scale & DDoS Mitigation Demonstration
cexp build cex-service/examples/04_autoscale_ddos.cex -o bin/demo && ./bin/demo

# 5. Asynchronous Promise-Based Microservice
cexp build cex-service/examples/05_promise_async.cex -o bin/demo_async && ./bin/demo_async
```
