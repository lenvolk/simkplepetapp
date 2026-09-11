---
name: codebase-design
description: "Shared vocabulary and principles for designing deep modules. Use when designing or improving a module's interface, evaluating deepening opportunities, choosing replaceable dependencies, or making code more testable and easier to navigate. This is a reference, not an instruction to launch a codebase-wide survey."
---

# Codebase Design

Adapted for GitHub Copilot from [mattpocock/skills](https://github.com/mattpocock/skills/tree/main/skills/engineering/codebase-design).

Design deep modules: substantial behavior behind a small interface, with clear dependency substitution and tests of observable behavior. Apply these principles to the current task, not as permission to refactor unrelated code. Repository constraints and justified design decisions take precedence over heuristics.

## Glossary

- **Module**: anything with an interface and an implementation, such as a function, class, package, or feature spanning tiers.
- **Interface**: everything a caller must know: types, invariants, ordering, error modes, configuration, and performance characteristics. It is broader than a language's `interface` keyword or method signatures.
- **Implementation**: behavior hidden inside the module. Use **adapter** instead when describing a concrete dependency's substitution role.
- **Depth**: behavior callers can exercise per unit of interface they must understand. A **deep** module hides substantial complexity behind a small interface; a **shallow** module exposes nearly as much complexity as it implements.
- **Seam**: a place where behavior can be substituted without editing the code at that location. Its placement is distinct from what the implementation hides.
- **Adapter**: a concrete implementation satisfying an interface at a seam, for example a network implementation or an in-memory test substitute.
- **Leverage**: capability gained by callers from learning one interface, reused across call sites and tests.
- **Locality**: change, bugs, knowledge, and verification concentrating in one place instead of spreading across callers.

Use this vocabulary consistently in architectural reasoning, while retaining the project's domain terms and actual identifiers in source references.

## Principles

- Depth concerns the caller's burden, not the number of implementation lines. Do not reward larger implementations or artificially fewer methods.
- Apply the deletion test: if removing a module eliminates forwarding without spreading complexity, consolidation may help. If its responsibilities reappear in many callers, it was earning its place.
- The interface is the primary behavioral test surface. Internal refactors should not break tests that express unchanged observable behavior.
- A deep module can contain small replaceable parts and internal seams. Do not expose those internals solely for tests.
- One adapter suggests a hypothetical seam; two concrete needs, often production and testing, justify substitution. Treat this as a design heuristic, not a rule to remove repository-mandated interfaces.
- Accept dependencies rather than constructing hard-wired external dependencies inside business logic.
- Prefer returned results for pure computation. Keep necessary side effects explicit and test observable outcomes.
- Seek smaller, clearer caller-facing surfaces, but retain distinct entry points when they express meaningful operations or invariants.

## Task-Specific References

- For dependency categories and replacement testing, read [DEEPENING.md](./DEEPENING.md).
- When the user requests competing designs for a chosen module, read [DESIGN-IT-TWICE.md](./DESIGN-IT-TWICE.md).

Do not launch the alternative-design workflow merely because this reference was loaded by another skill.