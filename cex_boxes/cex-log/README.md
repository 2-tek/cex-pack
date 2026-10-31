# @cex-log (CexLog)

**CexLog**: Native high-resolution logging, timing profiler and delay measurement engine for the C-ext (Cex) ecosystem.

## Features

- **Microsecond/Nanosecond Precision**: Powered by C++20 `<chrono>` high-resolution clocks.
- **Stopwatch Profiler**: Start, stop, elapsed nanoseconds, microseconds, milliseconds, seconds.
- **Always Delay Time Display**: Every log line automatically includes high-resolution `[${delaytime}]`.
- **Structured Standard Format**: `(cmd color)[${time}][${status}][${delaytime}] content` without icons or linked symbols.
- **ANSI Terminal Text Colors**: Colored status tags (INFO green, DEBUG magenta, WARN yellow, ERROR red, DELAY cyan, HTTP status codes 2xx/3xx/4xx/5xx).
- **Log Levels**: `info`, `debug`, `warn`, `error`, `delay`, and `delayWithStatus`.
- **Zero Overhead**: Inlined static methods with native machine code compilation.

## Usage

```typescript
import "@cex-log/src/log.cex";

fn main(): int {
    Log::info("Application starting");
    let sw: Stopwatch* = Log::start("Database Query");
    // ... execute logic ...
    sw->stop();
    Log::delay("Database Query", sw->elapsedMs());
    return 0;
}
```
