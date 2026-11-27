# Team Ownership

Clear ownership helps route decisions and reviews to people with the right context. It should make responsibility visible without turning team boundaries into barriers to collaboration.

## Define responsibilities

For each domain or technical area, identify:

- accountable team or role;
- decision scope and interfaces with other teams;
- operational responsibility, including alerts and incidents;
- escalation path and backup coverage;
- how ownership changes as the system evolves.

Prefer ownership around cohesive business capabilities and stable boundaries. Avoid assigning every shared library or cross-cutting concern to an unstaffed central team.

## Code ownership

Use `CODEOWNERS` when automatic reviewer routing or explicit stewardship is useful. Keep patterns narrow and test how the host platform resolves overlapping rules. Assign active teams or maintainers, provide backup owners, and review ownership entries when teams or paths change. Code ownership can request review; branch rules determine whether approval is mandatory.

## Working agreements

Agree on who reviews high-risk changes, expected review response windows, how conflicts are resolved, and how teams share context. Keep architecture decisions in ADRs and record operational ownership in service documentation or runbooks.

## Example

In an e-commerce lab, the orders domain can own order lifecycle invariants and the payment adapter boundary. A platform or infrastructure owner can steward deployment configuration. Changes crossing both areas should request both perspectives when they affect both responsibilities; ownership does not imply that only the owning team may contribute.
