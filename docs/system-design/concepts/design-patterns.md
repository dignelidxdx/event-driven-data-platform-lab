# Design Patterns

Design patterns are reusable approaches to recurring object and component design problems. They are not code recipes: apply one when its structure makes a real variation, dependency, or collaboration easier to understand. A pattern can also add indirection, so prefer the simplest design that meets current needs.

The patterns below follow the three classic categories: creational, structural, and behavioral. The ordering within each category is a practical learning heuristic based on how often the pattern commonly appears in business application code, not a measured universal usage ranking. Teams, languages, and frameworks differ.

## Creational patterns

These patterns control how objects or related families of objects are created.

| Typical frequency | Pattern | Business example and when to use it | Trade-off |
|---|---|---|---|
| Common | Factory Method / Simple Factory | An order application creates a `PaymentMethod` adapter based on a configured provider or payment type. Use when callers should request a valid abstraction without knowing concrete construction details. | A factory can become a branching registry; use dependency injection or configuration when that already solves the problem clearly. |
| Common | Builder | Construct a report request or shipment with many optional filters, destinations, and policies while validating required values. | Adds a separate construction API; avoid it for objects with only a few stable fields. |
| Occasional | Singleton | A process-wide immutable configuration or stateless registry when exactly one instance per process is a real requirement. | Global state makes tests, lifecycle, and concurrency harder. Prefer explicit dependency injection; singleton does not mean one instance across a distributed system. |
| Occasional | Abstract Factory | A tenant or deployment selects a consistent family of related integrations, such as a set of payment, storage, and notification adapters for a provider environment. | Can over-generalize families that do not actually vary together. |
| Less common | Prototype | Clone a complex campaign or workflow configuration as a starting point for a new one. | Deep-copy and identity semantics can be subtle; explicit copy operations may be clearer. |
| Less common | Object Pool | Reuse expensive, bounded resources when measurement shows allocation or initialization cost is material. | Pooling adds lifecycle and concurrency complexity; many runtimes and clients already manage connections. |

## Structural patterns

These patterns compose objects and interfaces into larger structures.

| Typical frequency | Pattern | Business example and when to use it | Trade-off |
|---|---|---|---|
| Common | Adapter | Translate a payment provider's SDK into the application's `PaymentPort`, or map an external API response to an internal model. | Mapping layers need maintenance as either contract changes; keep provider types from leaking inward. |
| Common | Facade | Offer a single checkout operation that coordinates cart validation, pricing, order creation, and payment initiation behind a focused interface. | A facade should simplify a workflow, not become a “god service” that owns unrelated rules. |
| Common | Decorator | Add metrics, tracing, caching, or retry policy around a repository or outbound client without changing its core implementation. | Layers can obscure call order and behavior; retries may be unsafe for non-idempotent operations. |
| Common | Proxy | Apply access checks or lazy loading before accessing a protected resource, such as a document download. | Hidden behavior can surprise callers; explicit authorization and loading boundaries may be preferable. |
| Occasional | Composite | Treat a bundle of products and an individual product uniformly for catalog display or pricing traversal when the domain genuinely has a tree. | Tree semantics can complicate identity, updates, and validation. |
| Occasional | Bridge | Vary notification policy independently from delivery channel, such as alert type × email/SMS provider, when both dimensions evolve separately. | More abstractions than needed when only one dimension varies. |
| Less common | Flyweight | Share immutable data such as repeated rendering metadata across many product cards in a memory-constrained process. | Requires careful separation of shared intrinsic state from per-instance state; usually optimize only after profiling. |
| Less common | Protection Proxy / Virtual Proxy | Restrict access to a sensitive export or delay loading a large document until requested. | Prefer explicit security and data-loading APIs when proxy behavior would be invisible or difficult to audit. |

## Behavioral patterns

These patterns define how objects communicate, distribute responsibility, or change behavior.

| Typical frequency | Pattern | Business example and when to use it | Trade-off |
|---|---|---|---|
| Common | Strategy | Select a shipping-price or discount calculation policy by market, customer tier, or promotion. Use when algorithms vary independently and selection is explicit. | Too many strategies can scatter a small, stable rule; keep selection and policy ownership clear. |
| Common | Observer / Publish–Subscribe | Notify subscribers when an order is placed or a user changes notification preferences. In-process observers are distinct from durable distributed messaging. | Delivery order, failure, duplicate handling, and hidden coupling matter; use a broker only when requirements justify it. |
| Common | Command | Represent a business action such as `PlaceOrder` or `RefundPayment` with validated input and an explicit handler. | Command objects can become ceremony if they merely wrap a simple function; commands are not automatically queued or event-sourced. |
| Common | State | Model order or payment lifecycle transitions so behavior depends on explicit states such as Pending, Authorized, Failed, and Refunded. | A class per state may be excessive; a well-defined enum/state machine can be enough. |
| Common | Chain of Responsibility | Run an incoming order through validation, fraud screening, pricing, and policy checks where handlers can accept, reject, or pass control. | Execution order and short-circuit behavior must be visible and testable. |
| Common | Template Method | Keep a common report-generation sequence while subclasses customize a defined step, if inheritance is already a natural fit. | Inheritance couples extensions to the base algorithm; composition or Strategy is often easier to change. |
| Occasional | Iterator | Traverse a paginated set of invoices or catalog results without exposing storage details. | Distributed pagination and snapshot consistency require explicit semantics beyond a local iterator. |
| Occasional | Mediator | Coordinate interactions among checkout UI components or workflow participants through one explicit coordinator. | The mediator may accumulate too much responsibility and become a central bottleneck in code structure. |
| Occasional | Memento | Save and restore a user's draft order or editor state when the domain permits a snapshot. | Snapshot size, privacy, and stale-state handling need consideration. |
| Occasional | Visitor | Apply several operations to a stable hierarchy of document or product types, such as export or validation. | Adding a new element type is awkward; avoid when the type hierarchy changes frequently. |
| Occasional | Specification | Encapsulate a reusable business predicate, such as whether an account qualifies for a promotion. | Composed predicates can become hard to read; keep business language and query translation aligned. |
| Situational | Null Object | Supply a no-op notification preference or optional collaborator to avoid repeated null checks when absence has a safe default meaning. | Can hide missing configuration or mandatory behavior; do not use it to disguise errors. |
| Situational | Interpreter | Evaluate a deliberately small business rule language, such as configurable eligibility expressions. | A parser and language semantics create ongoing security and maintenance work; do not build one for ordinary conditionals. |

## Choosing and applying a pattern

1. State the concrete change or problem that is difficult today.
2. Identify which part varies and who owns that variation.
3. Compare a direct implementation with the pattern's extra types and indirection.
4. Add the pattern only if it makes the rule, boundary, or change safer to understand and test.
5. Revisit it if the expected variation never appears or navigation becomes harder.

For the lab, an Adapter around the payment provider and a Strategy for genuinely variable pricing are plausible examples. A broker-backed Observer, object pool, or microservice is not implied by the pattern name; each needs a requirement and an operational justification.

Patterns connect to the [Core Entities](../delivery-framework/02-core-entities.md), [API and protocols](../delivery-framework/03-api-protocols-and-endpoints.md), and [High-Level Design](../delivery-framework/05-high-level-design.md) stages. For broader structural choices, see [architecture patterns](architecture-patterns.md); for principles and warning signs, see [design principles and anti-patterns](design-principles-and-anti-patterns.md).
