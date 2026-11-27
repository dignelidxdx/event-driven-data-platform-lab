# C4 Diagram Guide

C4 provides a hierarchical way to describe software architecture. Use the level that answers the audience's question; a diagram should clarify boundaries and relationships rather than inventory every implementation detail.

## C4 is notation-independent

The C4 model does not require a particular file format or diagramming tool. **Structurizr DSL** is one text-based option for defining a C4 architecture model as code; PlantUML, Mermaid, and visual editors are alternatives. C4 supplies the abstractions and diagram types, while the team chooses a notation that is readable, reviewable, and maintainable. See [Diagram Source Formats](diagram-source-formats.md) for the comparison and repository guidance.

## Diagram levels

- **System context:** the system, its users, and external systems.
- **Container:** major applications or data stores and their relationships. A C4 container is a separately running or deployable unit, not necessarily a Docker container.
- **Component:** responsibilities inside a container when that detail matters.
- **Code:** implementation structure, usually unnecessary for stakeholder architecture communication.

The model also has supporting diagram types, including system landscape, dynamic, and deployment diagrams. Use only views that answer a concrete question; context and container views are often sufficient.

## Diagram quality

Give each diagram a title and scope. Explain shapes, colors, and line styles in a legend. Name element types and technologies when relevant, and label each directional relationship with its intent or protocol. Keep a diagram understandable without relying on a long verbal explanation.

## Lab guidance

Begin with context and container views for the modular monolith and its external actors. Add component-level detail only when it helps explain module boundaries, adapters, event flow, or a proposed change. A deployment diagram can show runtime placement when deployment decisions are in scope; a provider-specific cloud diagram is a separate view, not a C4 level by itself.

Link diagrams from the [High-Level Design stage](../delivery-framework/05-high-level-design.md). Preserve editable source alongside rendered output when practical, and avoid keeping stale exports as a second source of truth.
