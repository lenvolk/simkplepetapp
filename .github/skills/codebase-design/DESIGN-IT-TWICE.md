# Design It Twice

Use only when the user wants alternative interfaces for a selected module. Apply [SKILL.md](./SKILL.md) and classify dependencies with [DEEPENING.md](./DEEPENING.md). This workflow proposes designs; it does not implement them.

## 1. Frame the Problem

Explain the chosen module's constraints, dependency categories, caller needs, and an illustrative code sketch. Distinguish settled requirements from assumptions. Use the project's domain vocabulary if documented.

## 2. Produce Independent Alternatives

Use the available sub-agent tool to run three independent, read-only designs in parallel, giving each a different emphasis:

1. Minimize the interface and maximize leverage per entry point.
2. Favor flexibility for the known use cases, without inventing speculative requirements.
3. Make the most common caller's workflow straightforward.

Include the same verified requirements, source paths, coupling evidence, dependency strategy, and glossary in each brief. Include: "Do not invoke skills or spawn additional agents. Produce this design directly. Do not edit files or create commits."

Each sub-agent returns:

- Interface: types, methods, parameters, invariants, ordering, and error modes.
- A caller usage example.
- What the implementation hides.
- Dependencies, adapters, and test strategy.
- Trade-offs in depth and locality.

If independent sub-agents are unavailable, disclose that and ask whether the user wants alternatives produced by one agent instead. Do not claim independent design work occurred.

## 3. Compare and Recommend

Present each design, then compare depth, locality, substitution placement, compatibility, and migration cost. Recommend one or a justified combination. Do not equate fewer methods with a better interface without examining the caller's burden.

Wait for the user's choice before any implementation. Respect the enclosing workflow's scope and permissions.