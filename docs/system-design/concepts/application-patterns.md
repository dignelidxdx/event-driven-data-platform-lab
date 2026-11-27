# Application Patterns

Application patterns organize user interaction or application behavior. They are distinct from system-wide deployment and architecture patterns.

## Model-View-Presenter (MVP)

Separates presentation through a view interface, a presenter that coordinates user interaction, and a model containing data or business behavior. It can make UI behavior testable and keep view logic focused. It may add ceremony for simple screens. Do not confuse MVP with Model-View-Controller or Model-View-ViewModel; their responsibility boundaries differ by framework and implementation.

## Adapter

Converts one interface into another expected by a caller. In hexagonal architecture, an adapter can implement an application port for HTTP, persistence, or an external provider. Keep translation at the boundary and avoid leaking vendor-specific types into domain logic.

## Repository

Provides a domain-oriented interface for retrieving and storing aggregates or entities. It can keep persistence concerns outside domain rules; overly generic repositories may obscure useful query behavior.

## Strategy

Encapsulates interchangeable algorithms behind a common interface. Use it when behavior genuinely varies; a single implementation rarely needs a strategy abstraction.

Patterns should clarify responsibilities and make likely changes safer. If indirection obscures the control flow, revisit whether the pattern fits.
