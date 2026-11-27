# Backend Frameworks

Choose a backend framework based on service shape, ecosystem needs, team experience, lifecycle, operational behavior, and the amount of framework convention the team wants. Framework capabilities and versions change; verify current support and compatibility when making a project decision.

| Language | Examples to evaluate | Potential fit | Trade-offs to investigate |
|---|---|---|---|
| TypeScript | NestJS, Fastify, Express | Structured applications, HTTP APIs, or smaller Node.js services | Abstraction and decorator conventions, plugin ecosystem, validation and typing boundaries |
| Go | Standard library HTTP, Chi, Gin | Small services and APIs with explicit composition | Convention level, middleware model, validation choices, and ecosystem fit |
| Java | Spring Boot, Quarkus, Micronaut | Enterprise services and JVM-based ecosystems | Startup and memory needs, build complexity, framework conventions, native-image requirements |
| Ruby | Rails, Hanami, Sinatra | Product-oriented web applications or smaller HTTP services | Convention trade-offs, runtime profile, modularity, and deployment model |

Evaluate a candidate with a thin vertical slice: one endpoint, validation, domain call, persistence boundary, logging, and a focused test. Record why the framework fits the requirements and what operational cost it introduces. See the [language comparison](language-comparison.md) and [technology decision template](../artifacts-and-templates/technology-decision.md).
