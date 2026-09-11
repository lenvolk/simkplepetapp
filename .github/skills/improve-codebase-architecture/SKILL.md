---
name: improve-codebase-architecture
description: "Survey a codebase or selected area for deepening opportunities, present evidence-backed candidates in a visual HTML report, then explore a candidate only after the user selects it. Use when explicitly asked for an architecture survey or structural improvement opportunities."
disable-model-invocation: true
---

# Improve Codebase Architecture

Adapted for GitHub Copilot from [mattpocock/skills](https://github.com/mattpocock/skills/tree/main/skills/engineering/improve-codebase-architecture).

Find refactors that put substantial behavior behind a smaller, stable interface. The goal is testability and easier navigation of the code, not more abstractions.

Before surveying, read and follow [codebase-design](../codebase-design/SKILL.md) and its [deepening guidance](../codebase-design/DEEPENING.md). Use that architecture vocabulary and the project's own domain terms. Preserve actual code identifiers in citations.

## Scope and Permissions

- This is a survey, not implementation. Do not change application code, tests, configuration, or Git state.
- During the survey, the only file to create is the HTML report in the OS temp directory. Read existing domain documentation without modifying it.
- After the user selects a candidate, terminology and ADR edits may follow the domain-modeling workflow. Refactoring requires a separate implementation request.
- Honor a report-only request: produce the report and stop without beginning an interview.

## Process

### 1. Choose the Area

If the user names a module, subsystem, pain point, or upcoming feature, use that direction. A supplied spec describes the intended change, not permission to implement it.

Otherwise, inspect a bounded stretch of recent history, such as `git log -n 50 --name-only --format=oneline`, and favor files that change repeatedly. Widen only when there is no useful concentration. Without Git history, use the source structure and disclose that prioritization is not history-based.

Read applicable repository instructions, `CONTEXT.md`, any root `CONTEXT-MAP.md` pointing to area-specific glossaries, and relevant ADRs before evaluating designs. Missing domain docs are not a blocker. Exclude generated outputs such as `bin/`, `obj/`, build artifacts, and vendored dependencies.

### 2. Explore Read-Only

Use the available sub-agent tool for a bounded, read-only exploration of the selected area. Where independent areas justify it, explore them in parallel. Supply the scope, relevant paths, architectural principles, and domain vocabulary in each brief.

Include in every brief: "Do not invoke improve-codebase-architecture or spawn additional agents. Perform this exploration directly. Do not edit files or run commands that change repository state. Return evidence with file paths and line numbers."

If sub-agent tools are unavailable, disclose that and perform a bounded direct survey; do not claim independent exploration occurred.

Look for concrete friction:

- Understanding one concept requires jumping among many small modules.
- An interface is nearly as complex as the implementation it exposes.
- Extracted pure functions are tested, but important behavior in their orchestration is not.
- Tightly coupled modules expose details their callers should not need.
- Current interfaces make meaningful behavior difficult to test.

Apply the deletion test: would removing or consolidating a suspected shallow module eliminate indirection and concentrate complexity, or merely redistribute it among callers? Only recommend changes that explain where the complexity goes and why callers benefit.

Validate candidate evidence against the source before reporting it. Classify dependencies using the deepening guidance and explain which observable behavior becomes easier to test. Respect documented repository conventions; a single implementation alone is not proof that an existing interface should be removed.

### 3. Present the Report

Read [HTML-REPORT.md](./HTML-REPORT.md) for report content and rendering requirements.

Resolve the OS temp directory using the platform's API, for example `[System.IO.Path]::GetTempPath()` in PowerShell. Create `architecture-review-<timestamp>.html` there, using a filename-safe timestamp with sufficient precision to avoid overwrites. Do not place the report in the repository unless requested.

For each candidate include:

- Files and line references supporting the finding.
- Problem: the observed friction, not a generic cleanliness claim.
- Solution: what responsibilities would move or consolidate, without designing concrete interfaces yet.
- Benefits: locality, leverage, and specific test improvements.
- Before/after visualization, clearly distinguishing existing structure from a proposal.
- Strength: `Strong`, `Worth exploring`, or `Speculative`, with uncertainty stated.
- Dependency category and any ADR conflict worth revisiting.

Do not pad the report with speculative refactors. It is valid to find no worthwhile candidates. Recommend reopening an ADR only when concrete friction justifies doing so, and cite it explicitly.

Finish with a top recommendation and its rationale, or explain why none merits action. Open the report with an available browser tool or OS opener (PowerShell: `Start-Process -FilePath <report-path>`), and give the user the absolute path. If opening or browser verification is unavailable, say so.

Stop after presenting the report. Unless the user requested report-only output, ask which candidate they want to explore. Do not automatically interview them about the top recommendation.

### 4. Explore One Selected Candidate

Only after selection, read and follow both [grilling](../grilling/SKILL.md) and [domain-modeling](../domain-modeling/SKILL.md). Apply them together in this conversation; the user need not invoke them separately.

Discuss constraints, dependencies, responsibilities, the proposed interface, and which tests survive. Record resolved domain terms inline using domain-modeling's glossary format. Offer an ADR for a durable rejection only when it passes that skill's three gates; temporary prioritization is not an architectural decision.

If the user requests alternative interface designs, follow [design-it-twice](../codebase-design/DESIGN-IT-TWICE.md). Do not start that additional design exercise during the survey.

End with the agreed decision and unresolved questions. Recommend capturing the decision in a spec before implementation. Do not assume optional downstream skills are installed or automatically execute them.