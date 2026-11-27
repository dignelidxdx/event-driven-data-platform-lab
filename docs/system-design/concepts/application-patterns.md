# Application Patterns

Application patterns organize user interaction or application behavior. They are distinct from system-wide deployment and architecture patterns.

## Model-View-Presenter (MVP)

Separates presentation through a view interface, a presenter that coordinates user interaction, and a model containing data or business behavior. For example, a checkout presenter can validate the selected delivery option, call a place-order use case, and tell the view to show confirmation or actionable errors; the view itself stays focused on rendering and forwarding user actions. MVP can make UI behavior testable and keep view logic focused, especially in UI frameworks where views are difficult to test directly. It may add ceremony for simple screens. Do not confuse MVP with Model-View-Controller or Model-View-ViewModel; their responsibility boundaries differ by framework and implementation.

**Consider MVP when** presentation behavior has meaningful branching, the view is difficult to test directly, or UI logic is becoming coupled to rendering code. First check the framework's native model (MVC/MVVM) and team conventions; do not impose MVP if it duplicates them or a screen is a simple form with no independent presentation logic.

## Adapter

Converts one interface into another expected by a caller. In hexagonal architecture, an adapter can implement an application port for HTTP, persistence, or an external provider. Keep translation at the boundary and avoid leaking vendor-specific types into domain logic.

**Consider an Adapter when** integrating a vendor or legacy interface whose data shape or semantics differ from the application's own contract, or when multiple implementations must fit one port. Keep conversion and error mapping at the boundary; avoid a pass-through wrapper that introduces no isolation or translation.

## Repository

Provides a domain-oriented interface for retrieving and storing aggregates or entities. It can keep persistence concerns outside domain rules; overly generic repositories may obscure useful query behavior.

**Consider a Repository when** domain operations need persistence behind a collection-like abstraction, especially for aggregate loading and saving that preserves invariants. For reporting or specialized queries, an explicit query service may be clearer than forcing every access through a generic repository.

## Strategy

Encapsulates interchangeable algorithms behind a common interface. Use it when behavior genuinely varies; a single implementation rarely needs a strategy abstraction.

**Consider Strategy when** a business rule has multiple real variants selected by context, configuration, market, or policy (such as shipping-price calculation by region). Make the selection rule explicit and test each variant; if there is only one stable algorithm, a direct function is easier to maintain.

Patterns should clarify responsibilities and make likely changes safer. If indirection obscures the control flow, revisit whether the pattern fits.
