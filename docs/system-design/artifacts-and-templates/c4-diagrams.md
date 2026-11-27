# C4 Diagram Guide

C4 provides a hierarchical way to describe software architecture. Use the level that answers the audience's question; a diagram should clarify boundaries and relationships rather than inventory every implementation detail.

- **System context:** the system, its users, and external systems.
- **Container:** major applications or data stores and their relationships. A C4 container is a separately running or deployable unit, not necessarily a Docker container.
- **Component:** responsibilities inside a container when that detail matters.
- **Code:** implementation structure, usually unnecessary for stakeholder architecture communication.

Label relationships with purpose or protocol where useful. Keep names aligned with domain language and contracts. Consider dynamic diagrams for a particular runtime interaction and deployment diagrams for infrastructure topology; these complement the core hierarchy.

In this lab, begin with context and container views for the modular monolith and its external actors. Add detail only when it helps explain module boundaries, adapters, event flow, or a proposed change. Link diagrams from the [high-level design stage](../delivery-framework/05-high-level-design.md).
