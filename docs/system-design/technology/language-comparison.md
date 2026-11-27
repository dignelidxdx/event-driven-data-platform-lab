# Language Comparison

Compare languages against the actual workload, product constraints, team expertise, ecosystem, and operational environment. The notes below are starting points for investigation, not benchmarks or universal rankings.

| Language | Consider it when | Strengths to evaluate | Trade-offs to evaluate |
|---|---|---|---|
| TypeScript | A shared language across web clients and Node.js services is useful | JavaScript ecosystem, static checking, broad hiring and tooling | Runtime behavior still depends on JavaScript; type checking does not validate external data; dependency and build complexity vary |
| Go | Small deployables, straightforward concurrency, and simple service operations fit | Fast compilation in many workflows, compact deployment, built-in concurrency primitives | Smaller abstraction ecosystem in some areas, explicit error handling, and less runtime dynamism may require different design choices |
| Java | Mature enterprise libraries, JVM capabilities, and established operational practices fit | Broad ecosystem, mature tooling, static typing, and runtime optimization | Build and runtime footprint, configuration surface, and framework complexity need consideration |
| Ruby | Product iteration and a concise application model are priorities | Expressive syntax, mature web ecosystem, productive conventions | Runtime throughput and concurrency needs should be measured; dependency and deployment choices affect operations |

## Decision method

1. Eliminate options that conflict with platform, security, or staffing constraints.
2. Compare remaining options against weighted requirements such as latency, throughput, memory, delivery speed, maintainability, ecosystem, and hiring.
3. Prototype the uncertain or high-impact parts using representative workloads.
4. Record assumptions and the selected option in an [ADR](../artifacts-and-templates/architecture-decision-record.md).

Compile speed is one factor in developer feedback and build pipelines; it does not alone determine service performance or total delivery speed. Distinguish compile/build time, startup time, runtime throughput, memory, and operational effort. Avoid treating language-level generalizations as current performance facts without measurements on the intended workload.
