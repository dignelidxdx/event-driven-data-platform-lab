# API, Protocols, and Endpoints

Define how actors and components interact across boundaries. Specify operations, inputs, outputs, errors, authorization expectations, and compatibility rules at the level needed to validate the design.

Choose a communication style based on interaction needs: synchronous request/response, asynchronous messaging, or a combination. Name protocols and endpoint shapes only after clarifying consumers, latency expectations, delivery semantics, and operational constraints. Do not assume that every internal boundary needs a network API.

For a conceptual comparison of REST, gRPC, GraphQL, WebSockets, Server-Sent Events, webhooks, and WebRTC—including when to consider each and their trade-offs—see [Communication Protocols and API Styles](../concepts/communication-protocols.md). This stage applies those options to the system's specific consumers, contracts, and requirements.

## Common interaction styles

The list below is ordered by broad reported adoption where a comparable survey exists, not by technical suitability. Protocols overlap and survey samples differ. Postman's 2025 State of the API survey reports REST at 93%, webhooks 50%, WebSockets 35%, GraphQL 33%, and gRPC 14% among its respondents; these are survey figures, not universal market shares. WebRTC is not directly comparable because it is a real-time peer/media communication technology rather than a general-purpose business API style.

| Approximate broad adoption in the cited survey | Style | Good fit | Considerations |
|---|---|---|---|
| 1 | REST over HTTP | Public or internal resource-oriented request/response APIs | Resource and status semantics, pagination, caching, auth, versioning; document with OpenAPI |
| 2 | Webhooks | Notifying another system when an event occurs | Receiver availability, retries, signatures, duplicate delivery, ordering, and delivery logs |
| 3 | WebSockets | Bidirectional, long-lived browser/client communication | Connection lifecycle, authentication refresh, backpressure, fan-out, and reconnect semantics |
| 4 | GraphQL | Clients need flexible graph-shaped data selection across related resources | Schema governance, query cost limits, caching, authorization at field/resolver boundaries |
| 5 | gRPC | Typed service-to-service calls, efficient binary transport, or streaming | Protobuf schema evolution, HTTP/2 and intermediary support, generated clients, operational visibility |
| Context-specific; not comparable in that survey | WebRTC | Low-latency peer media/data communication such as calls | Signaling, NAT traversal, STUN/TURN, session negotiation, and media/security complexity |

The survey does not prove that REST is the best choice for a particular design. Choose by client compatibility, interaction pattern, payload, latency, streaming needs, tooling, and operational constraints. For event streams and durable asynchronous work, evaluate a message broker or stream platform separately; these are not interchangeable with the API styles above.

## Endpoint and contract design

For REST, define resource-oriented paths and HTTP methods, status codes, request/response schemas, pagination, filtering, authentication/authorization, idempotency for retryable writes, error shape, and compatibility policy. Avoid encoding every action as an unconstrained endpoint; model business commands explicitly where needed.

Use **OpenAPI** as a machine-readable description of HTTP paths, methods, parameters, schemas, constraints, responses, and security schemes. Swagger commonly refers to tooling that edits, renders, or serves OpenAPI descriptions; the specification and the tooling are distinct. For GraphQL, publish the schema and examples; for gRPC, define Protocol Buffers and compatibility rules; for async events, define message schemas and delivery semantics. Consider AsyncAPI for documenting event-driven APIs.

Use contract-first design when teams need parallel implementation or stable integration agreements: review the contract, generate or validate clients where useful, and add provider/consumer contract tests. Use [RFC](../artifacts-and-templates/request-for-comments.md) for broad review and the [API and event contract template](../artifacts-and-templates/api-and-event-contract.md) to record behavior. Postman reports API documentation/testing practices but survey results should be read in their stated sample context.

Document HTTP endpoints, event contracts, and schemas in the [API and event contract](../artifacts-and-templates/api-and-event-contract.md). Include examples, validation rules, error behavior, idempotency where needed, and versioning or compatibility expectations. Use an RFC when a proposed interface needs broad discussion before agreement.

Validate contracts with representative producer and consumer scenarios. Link each contract to the relevant requirements and data-flow steps.
