# Deepening

Use the vocabulary and principles in [SKILL.md](./SKILL.md). Assess dependencies before recommending consolidation.

## Dependency Categories

1. **In-process**: pure computation or in-memory state without I/O. Consider consolidating tightly coupled behavior and testing through its interface without extra adapters.
2. **Local-substitutable**: dependencies with realistic local stand-ins. Exercise the deepened module using the stand-in and keep substitution internal where callers do not need to know about it. State fidelity limitations; a stand-in does not prove production compatibility.
3. **Remote but owned (ports & adapters)**: services controlled by the team across a network. Keep domain behavior in the module and inject transport through a port. Test with an in-memory adapter and retain integration checks for the real transport.
4. **True external (mock)**: third-party services outside the team's control. Inject a narrow external port and use a mock adapter for deterministic behavior tests. Retain contract or integration coverage where needed.

## Substitution Discipline

Justify adapters with real production or test needs. An interface with one implementation is not automatically defective; account for repository conventions and documented plans. Distinguish internal replacement points from the public interface rather than exposing test-only internals to callers.

## Testing Strategy

- Test observable outcomes through the deepened module's interface, including relevant error paths and invariants.
- Identify which existing tests protect unique behavior before proposing removal.
- Replace redundant implementation-coupled tests only after equivalent behavioral coverage exists and passes. Never delete coverage merely because a module was consolidated.
- Preserve tests of external contracts that substitutes cannot verify.
- During an architecture survey, describe this migration rather than editing tests or application code.