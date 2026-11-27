# Communication Protocols and API Styles

This guide explains common ways clients and systems communicate and when to consider each. These choices are related but not all the same kind of thing: REST is an architectural style commonly applied over HTTP; GraphQL is an API query language and execution model, commonly served over HTTP; gRPC is an RPC framework; WebSocket is a bidirectional communication protocol; WebRTC is a set of browser real-time communication APIs and protocols; and a webhook is a notification pattern that usually delivers an HTTP request. Choose the interaction contract first, then the transport and tooling.

## Quick selection guide

| Style | Interaction | Consider it when | Main trade-offs |
|---|---|---|---|
| REST over HTTP | Client requests a resource or submits an operation; server responds | The API has resource-oriented operations, broad client compatibility matters, and standard HTTP tooling, caching, or observability is useful | Clients may need multiple requests; resource and version semantics need design; HTTP alone does not define business behavior |
| gRPC | Client calls a typed remote method; supports unary and streaming calls | Service-to-service contracts need generated, strongly typed clients, multiple languages, or efficient streaming | Requires Protobuf and compatible tooling; browser and intermediary support may require gRPC-Web or a gateway; remote calls still fail and need deadlines and retries |
| GraphQL | Client requests a selection of fields from a typed graph schema | Multiple clients need different combinations of related data and a single query contract reduces client-specific endpoints | Resolver performance, query depth/cost, authorization, caching, and schema governance need explicit controls |
| WebSocket | Persistent, bidirectional messages over a connection | A client and server need frequent low-latency updates in both directions, such as collaborative editing or live order status | Connection lifecycle, reconnect, backpressure, fan-out, authorization refresh, and horizontal scaling add operational work |
| Server-Sent Events (SSE) | Server streams text events to a client over HTTP; client does not send messages on that stream | The browser needs one-way live updates, such as notifications or job progress, and ordinary HTTP infrastructure is desirable | One-way only; reconnect, event IDs, proxies/timeouts, and connection capacity still need consideration |
| Webhooks | Provider sends an HTTP callback when an event occurs | One system should notify another without the receiver polling continuously, such as a payment provider reporting a result | Receiver availability, retries, duplicate delivery, signatures, ordering, delivery logs, and endpoint exposure must be handled |
| WebRTC | Peers exchange real-time audio, video, or data, often with direct peer connections | A product needs calls, conferencing, or interactive low-latency peer media/data | Requires signaling, session negotiation, ICE/STUN/TURN and network traversal, and careful media security and operations; it is not a general business API |

This is a use-case comparison, not a universal ranking. REST, GraphQL, gRPC, WebSockets, and webhooks can coexist in one product because they solve different interaction needs. Webhooks are a delivery pattern, not a transport protocol; WebRTC is specialized real-time communication rather than a substitute for ordinary service APIs.

## How to choose

Start with the interaction shape:

1. **Request and response for a resource or command:** consider REST. Use GraphQL when clients genuinely need flexible selection across a graph of related data. Use gRPC for typed remote procedure calls between services or clients where its generated contracts and streaming model fit the environment.
2. **Server updates a connected client:** consider SSE for one-way updates and WebSocket when both sides need to send messages over a long-lived connection.
3. **A provider notifies your system:** consider a webhook when the receiver can expose an endpoint and the sender offers retry and delivery controls. If you own both sides and need durable buffering, replay, or high-volume fan-out, assess a broker or stream platform instead.
4. **Peers exchange live media or data:** consider WebRTC for audio/video or peer-to-peer communication; use a signaling service to establish sessions and plan for relays when direct network paths fail.

Then validate client support, authentication, payload shape and size, latency and throughput needs, delivery guarantees, ordering, retry safety, caching, observability, gateway/proxy support, team skills, and operating cost. A popular protocol is not automatically the right fit. Avoid choosing a streaming connection when periodic or on-demand HTTP requests meet the product need.

## Practical examples

- **E-commerce catalog:** REST can expose products, categories, and search; GraphQL may fit a storefront with multiple clients that need different product-card fields. Define cache and freshness behavior either way.
- **Checkout and payment:** use a request/response API for placing an order, with explicit idempotency and error contracts. A payment provider can call a webhook to report asynchronous authorization or settlement; verify signatures and deduplicate delivery.
- **Order tracking page:** REST can load the current order state. SSE can stream one-way status updates; use WebSocket only if the client also needs interactive messages on the same live connection.
- **Internal service calls:** gRPC may fit typed calls between independently deployed services with compatible tooling. It does not remove the need to set deadlines, handle partial failures, or evolve schemas safely.
- **Telehealth/video support:** WebRTC may carry interactive audio/video, while separate authenticated HTTP APIs manage appointments and records. Signaling is required to establish the media session; WebRTC does not replace those business APIs.

## Contract and operational checklist

- Define consumers, operations/messages, request and response schemas, errors, authentication, authorization, and compatibility policy.
- Specify timeout, retry, duplicate, ordering, and delivery guarantees. Make retried side effects idempotent where required.
- For REST, document paths and schemas with OpenAPI. For gRPC, define Protobuf services/messages and compatibility rules. For GraphQL, govern the schema, resolver authorization, pagination, and query-cost limits. For asynchronous APIs, define event schemas and delivery behavior; AsyncAPI can document message-driven contracts.
- For webhooks, specify signature validation, replay protection, retry schedule, duplicate handling, response expectations, and a way to inspect or replay failed deliveries.
- For WebSocket/SSE, specify reconnect behavior, heartbeat/keepalive, authorization expiry, event IDs or resume behavior, and connection limits.
- For WebRTC, document signaling, ICE configuration, STUN/TURN operations, media permissions, and fallback behavior.
- Test compatibility and failure cases with representative producers and consumers; observe latency, error rates, disconnects, retries, queue/stream lag, and delivery delay as appropriate.

## Further reading

- [gRPC introduction](https://grpc.io/docs/what-is-grpc/introduction/) and [gRPC core concepts](https://grpc.io/docs/what-is-grpc/core-concepts/)
- [GraphQL introduction](https://graphql.org/learn/introduction/)
- [The WebSocket Protocol (RFC 6455)](https://datatracker.ietf.org/doc/html/rfc6455) and [MDN WebSocket API](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API)
- [MDN Server-Sent Events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events)
- [WebRTC 1.0 W3C Recommendation](https://www.w3.org/TR/webrtc/)
- [HTTP Semantics (RFC 9110)](https://datatracker.ietf.org/doc/html/rfc9110)

See the [Delivery Framework API, Protocols, and Endpoints stage](../delivery-framework/03-api-protocols-and-endpoints.md) for applying these choices to requirements, endpoint design, contracts, and review artifacts.
