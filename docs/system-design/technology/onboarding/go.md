# Go Onboarding

1. Learn packages, values, structs, methods, interfaces, pointers, and composition.
2. Practice explicit error handling, zero values, slices, maps, and resource cleanup.
3. Understand goroutines, channels, synchronization, context cancellation, and race detection before introducing concurrency.
4. Learn modules, formatting, vetting, tests, benchmarks, and profiling tools used by the chosen Go toolchain.
5. Build a small HTTP handler and keep domain behavior independent of transport and storage packages.
6. Add table-driven tests and a vertical slice that exercises a domain rule through an application boundary.

Favor small interfaces defined by consumers and measure performance with representative workloads rather than assuming concurrency will improve a design.
