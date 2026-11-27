# E-commerce Architecture Lab Functional Specification

## Purpose and motivation

Build a small, executable learning application that makes software architecture concepts observable through a familiar commerce workflow. Learners should be able to connect requirements and domain rules to code boundaries, events, tests, and operational signals.

## Stakeholders and actors

- **Learner/developer:** runs and studies the lab, follows architectural decisions, and experiments with implementation choices.
- **Store customer (demonstration actor):** browses products, creates a cart, and places an order.
- **Maintainer:** evolves examples and documentation while keeping the project reproducible and understandable.

## Goals and non-goals

### Goals

- Demonstrate a catalog → cart → order journey.
- Teach domain concepts, module boundaries, ports/adapters, events, and gradual architectural evolution.
- Keep the application small enough to run locally and trace into the documentation.

### Non-goals

- Real commerce, real payment processing, production identity, or handling real personal/payment data.
- Starting with independently deployed microservices or distributed infrastructure solely for demonstration.
- Choosing a particular UI or HTTP framework before project requirements justify it.

## Use cases

1. A learner browses the product catalog and sees product price and availability.
2. A learner adds a product to a cart and reviews the cart contents.
3. A learner confirms a valid cart and creates an order.
4. The system requests a demonstration payment through an explicit boundary.
5. The system publishes an `OrderPlaced` event and updates a query projection.
6. A learner follows documentation and decisions to understand how the flow is implemented.

## Business rules and boundaries

- A cart can be confirmed only when its contents satisfy the defined catalog and quantity rules.
- The order records the accepted purchase information according to a decision that must be made before implementation (for example, whether prices are snapshotted at confirmation).
- Payment is a demonstration adapter and must not call a real payment provider.
- Domain behavior is expressed in business terms; framework and transport details are outside the functional specification.

## Acceptance criteria

- The catalog-to-cart-to-order flow can be demonstrated end to end.
- Invalid or unavailable cart contents are handled with an understandable outcome.
- The order lifecycle and payment outcome are observable in the example.
- A learner can identify the relevant module and design documentation for each step.
- All sample data is synthetic.

## Assumptions and open questions

- The first user experience may be CLI, API, or a minimal web UI; this is not decided here.
- Exact product, quantity, cancellation, and payment demonstration rules need clarification before implementation.
- No customer traffic, DAU, request rate, or production SLO has been specified. Collect these only if the lab becomes a deployed product rather than a local learning application.
