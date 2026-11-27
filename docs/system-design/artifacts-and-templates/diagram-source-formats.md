# Diagram Source Formats

Choose a diagram format based on the question, authors, audience, review workflow, and documentation renderer. Keep editable source in the repository when practical; an exported image alone is hard to review and update.

## Formats at a glance

| Format | Typical source | Useful for | Repository guidance |
|---|---|---|---|
| Structurizr DSL | `.dsl` and optional Markdown | C4 architecture models and multiple consistent views generated from one model | Store the model in Git; export static views if readers cannot access the interactive workspace. |
| PlantUML / C4-PlantUML | `.puml` or `.plantuml` | Text-based UML and C4 diagrams that are easy to diff | Keep source beside the docs; render SVG/PNG for previews where needed. |
| Mermaid | Markdown code fences or `.mmd` | Lightweight flowcharts, sequences, states, classes, and ER diagrams | Prefer inline Markdown for small diagrams; confirm the host supports the required syntax. |
| diagrams.net / draw.io | `.drawio` XML | Free-form visual layouts and collaborative editing | Commit the editable source; export SVG/PNG only as a view or compatibility artifact. |
| BPMN modelers | `.bpmn` XML | Business-process diagrams with tasks, events, gateways, and participants | Keep BPMN source editable and provide an image or web-rendered view for reviewers. |
| SQL schema / ER tools | `.sql`, tool-specific model, or diagram source | Relational schema and entity relationships | Treat SQL DDL as schema source, not as a general diagram format; link generated ER views to the schema version. |
| Cloud-provider diagram tools | Tool-specific source or `.drawio` | Provider services, network topology, and deployment placement | Keep source/export together and record provider and scope; do not confuse vendor icons with architecture validation. |

You may be thinking of **Structurizr DSL**, not SQL. Structurizr DSL is a text-based language for defining an architecture model based on C4. C4 itself is notation-independent; Structurizr DSL, PlantUML, Mermaid, and draw.io are different ways to represent or render architecture diagrams.

## Match the format to the diagram

| Diagram or model | Common formats | Main question it answers |
|---|---|---|
| C4 context, container, or component | Structurizr DSL, C4-PlantUML, Mermaid, draw.io | What is in the software system and how are the main parts related? |
| UML sequence / interaction | Mermaid, PlantUML, UML model files | What messages happen, and in what order? |
| UML state machine | Mermaid, PlantUML, UML model files | Which states and transitions are valid? |
| Domain model / class diagram | Mermaid class, PlantUML, UML model files | Which concepts and relationships form the domain or static structure? |
| Entity-relationship (ER) diagram | Mermaid ER, PlantUML, SQL DDL plus a database modeling tool | What tables/entities, keys, and cardinalities represent stored data? |
| BPMN process | `.bpmn` XML and BPMN modelers | Which business tasks, decisions, events, and handoffs make up a process? |
| Data-flow diagram | Mermaid, PlantUML, draw.io, threat-modeling tools | Where does data enter, move, transform, and cross trust boundaries? |
| Deployment diagram | Structurizr DSL, PlantUML, draw.io, UML tools | Which runtime nodes host which software and connections? |
| Cloud architecture diagram | Provider-specific diagram tools, draw.io, Structurizr DSL | Which cloud services, networks, and managed resources are proposed? |

## Project conventions

- Put diagram source near the documentation it explains, or in a clearly named `diagrams/` directory when shared by multiple pages.
- Use relative links from Markdown and keep filenames descriptive.
- Include title, scope, legend, and directional labels; explain acronyms and technology-specific symbols.
- Prefer text-based source for diagrams that benefit from line-by-line review. Use visual editors when layout or stakeholder collaboration makes them more effective.
- Render to SVG or PNG when the documentation host cannot render the source. Keep one authoritative source and regenerate exports consistently.
- Avoid diagrams that combine multiple abstraction levels or attempt to show every component at once. Split views around a clear audience question.
