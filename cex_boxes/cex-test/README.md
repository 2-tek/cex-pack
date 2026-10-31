# 2-TEK CexT: Native Test & Coverage Engine for C-ext (Cex)

<!-- Target: use C-ex for native test runner, assertions, benchmarks, and test coverage generation (LCOV, JSON, summary report) -->

`@cext` (also `cex-test` / `packages/CexTest`) is the high-performance native test runner, assertion library, micro-benchmarking suite, diagnostics debugger, and **code coverage generator** for the Cex ecosystem, integrated with **`cexr-factory`** as its native execution engine.

---

## 1. Features

- **Integrated `cexr-factory` Runtime**: Dispatches and isolates test processes via `cexr-factory` for JIT execution, continuous debug monitoring, and fullstack assertions.
- **Unified `@cext` Package & CLI**: `cext` is completely synonymous with `cex-test`. Tests can import `@cext/src/assert.cex` and `@cext/src/runner.cex` or run via `cext <test.cex>`.
- **Code Coverage Generator (`CoverageCollector`, `CoverageInstrumenter`, `CoverageGenerator`)**:
  - **Executable Line Detection**: Intelligently identifies executable lines while skipping comments, imports, empty lines, and declarations.
  - **ASCII Terminal Report**: Beautiful formatted table showing Files, Executable lines, Hits, Misses, Coverage %, and Missing Line ranges.
  - **Standard LCOV (`lcov.info`)**: Industry-standard LCOV format compatible with GitHub Actions, VS Code Coverage Gutters, Codecov, and `genhtml`.
  - **Structured JSON (`coverage.json`)**: Machine-readable JSON reporting statement-by-statement hit counts and statistics.
  - **Interactive HTML Dashboard (`index.html`)**: Modern, self-contained HTML report with responsive cards, progress bars, and file status badges.
  - **Threshold Enforcement**: Validate minimum test coverage thresholds (e.g. `--min 80`) for continuous integration gates.
- **Assertion Engine (`Assert`)**:
  - Rich assertion primitives (`isTrue`, `isFalse`, `equalsInt`, `equalsString`, `notNull`, `greaterThan`, `lessThan`, `greaterOrEqual`, `lessOrEqual`, `containsString`).
- **Structured Test Runner (`TestSuite`, `TestCase`, `TestReporter`)**:
  - Organize tests into named suites with elapsed execution timing and summary pass/fail reporting.
- **Micro-Benchmarking Harness (`Benchmark`, `BenchmarkResult`)**:
  - High-precision throughput benchmarking (ops/sec and milliseconds elapsed).
- **Native Debugger (`Debugger`)**:
  - Diagnostic checkpoints, hex dumps, and memory pointer inspection.

---

## 2. CLI Usage & Native cexp-v8 / cexr-factory Toolchain

Build and compile the standalone CLI binary using `cexp-v8`:

```bash
# Build standalone native binary via cexp-v8
cexp-v8 build scripts/cli.cex -o dist/cext

# Or run tests via cext
cext tests/cex_test.test.cex
```

Run tests or coverage using the standalone binary (`cext`), the `cexr-factory` runtime, or the primary Cex toolchain reserve path (`$CEX_DIR/cex-test-v1/$version/bin`, `$CEX_DIR/cexr-factory/$version/bin`):

```bash
# Display version and target with active compiler & runner
cext version

# Run test suite via cext powered by cexr-factory
cext tests/cex_test.test.cex

# Run test file directly
cext run tests/cex_test.test.cex

# Execute test suite with coverage report
cext coverage tests/cex_test.test.cex

# Generate LCOV, JSON, and HTML reports to directory with threshold enforcement
cext coverage tests/cex_test.test.cex --out-dir coverage/ --min 80

# Compile standalone native binary via cexp-v8 dispatcher
cext build scripts/cli.cex -o dist/cext
```

---

## 3. Programmatic Usage

### Running Assertions & Suites
```typescript
import "@cext/src/assert.cex";
import "@cext/src/runner.cex";

fn main(): int {
    let suite: TestSuite* = new TestSuite("Math Suite");
    Assert::reset();
    Assert::equalsInt(10 + 20, 30, "10 + 20 == 30");
    suite->addResult("test_addition", Assert::getFailures() == 0, "", 1);
    TestReporter::printSummary(suite);
    return 0;
}
```

### Collecting & Generating Coverage
```typescript
import "@cext/src/coverage.cex";

fn main(): int {
    CoverageCollector::reset();

    // Register source file
    let source: string = "fn calc(x: int): int { return x * 2; }";
    CoverageCollector::registerFile("src/calc.cex", source);

    // Record statement hit
    CoverageCollector::hit("src/calc.cex", 1);

    // Compute stats
    let stats: CoverageStats* = CoverageCollector::computeStats();

    // Print ASCII Table Report
    let asciiReport: string = CoverageGenerator::generateAsciiReport(stats);
    print(asciiReport);

    // Export LCOV and JSON reports
    CoverageGenerator::exportReports(stats, "coverage", "app");
    return 0;
}
```
