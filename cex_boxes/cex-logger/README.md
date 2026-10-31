# @cex-log (CexLog)

**CexLog**: Native high-resolution logging, timing profiler and delay measurement engine for the C-ext (Cex) ecosystem.

## Features

- **Microsecond/Nanosecond Precision**: Powered by C++20 `<chrono>` high-resolution clocks.
- **Stopwatch Profiler**: Start, stop, elapsed nanoseconds, microseconds, milliseconds, seconds.
- **Delay Time Logging**: Auto-formatting output (`Log::delay(label, durationMs)`).
- **Log Levels**: `info`, `debug`, `warn`, `error`, and delay profiling.
- **Zero Overhead**: Inlined static methods with native machine code compilation.

## Usage

```typescript
import "@cex-log/src/log.cex";

fn main(): int {
    let sw: Stopwatch* = Log::start("Database Query");
    // ... execute logic ...
    sw->stop();
    Log::delay("Database Query", sw->elapsedMs());
    return 0;
}
```
