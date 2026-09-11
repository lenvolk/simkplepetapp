---
name: diagnosing-bugs
description: "Evidence-driven diagnosis of hard bugs, intermittent failures, and specific performance regressions. Use when explicitly asked for systematic diagnosis, or when a defect resists a focused first fix. Not for quick code explanations, obvious local errors, or proactive performance audits without a named symptom."
---

# Diagnosing Bugs

Adapted for GitHub Copilot from [mattpocock/skills](https://github.com/mattpocock/skills/tree/main/skills/engineering/diagnosing-bugs).

Diagnose one reported failure through a reproducible signal, falsifiable hypotheses, targeted probes, and a verified fix. Scale the investigation to the defect. Reuse existing reproduction evidence and tests; do not restart work already completed by the user or another workflow. Explain any phase that is skipped and why.

## Scope and Safety

- Start from the reported symptom, failing command, test, or source location. Read only enough nearby code to locate the real behavior and a discriminating check; do not conduct an unrelated repository survey.
- Read applicable repository instructions and relevant domain glossary or ADRs if present. Missing domain documentation is not a blocker.
- Preserve the user's changes. Do not create commits, branches, PRs, or reset/check out repository state unless explicitly requested.
- Honor diagnosis-only or approval-before-fix requests. A debugging request does not authorize production instrumentation, deployments, destructive data changes, or high-load tests against shared systems; obtain explicit approval for those actions.
- Never expose credentials, cookies, tokens, personal data, or full captured artifacts in output. Use existing environment-based credentials without printing them. Filter or redact at the source before tool output enters the conversation, and share only the signal-bearing excerpts.
- Never request secrets through chat or question tools. If authentication requires sensitive input, tell the user to enter it directly in the terminal or application and stop until they complete it.

## Phase 1: Establish a Feedback Loop

Define the expected result and the exact observed symptom. Identify the smallest check that exercises the real failing path and distinguishes the bug from a successful result. Prefer, in roughly this order:

1. An existing failing unit, integration, or end-to-end test.
2. A targeted HTTP request against a local development server, asserting response content as well as status.
3. A CLI invocation with a small fixture and an expected output assertion.
4. A browser automation script asserting the relevant DOM, console, or network behavior.
5. A sanitized captured request or event replayed through the real code path.
6. A temporary minimal harness that invokes the actual implementation, not a rewritten imitation of it.
7. A seeded property test, bounded stress run, or differential comparison between known states.
8. Structured human observation when automation cannot reach the failure (see below).

Name the command or repeatable tool procedure, run it, and report its sanitized result. A passing build alone is not a reproduction of a behavioral bug. Prefer a fast, deterministic assertion on the user's symptom over a broad "did not crash" check.

Keep initial reasoning local: form one falsifiable working hypothesis and identify a cheap check before editing. A small reversible test or probe can establish the missing signal. Do not claim a root cause before evidence supports it or keep reading indefinitely instead of testing the hypothesis.

### Intermittent Failures

Record the seed, environment, number of attempts, and failure count. Use bounded repetitions and isolated test data; increase reproduction frequency only within an authorized local environment. Do not insert arbitrary sleeps or unbounded polling. Use explicit event synchronization, controlled scheduling, or test-tool timeouts where appropriate. One passing attempt does not establish that a flake is fixed.

### Human-Assisted Reproduction

No Bash script is required. Use Copilot's question tool when available, otherwise ask in chat:

- State the exact action and expected result.
- Ask for the observed result and a sanitized error excerpt, if any.
- Record pass/fail/unknown and which environment was used.
- Repeat the same steps after the fix; label the result as human-verified, not automated.

If an interactive terminal command is waiting for non-sensitive input, collect and send one answer per prompt using the available terminal tools. Keep authentication and secret entry directly with the user.

### When Reproduction Is Blocked

List what was attempted and the specific missing access, fixture, or sanitized observation. Ask for the smallest missing item. Clearly label any remaining hypothesis as unverified. Do not invent a substitute failure, report success, or apply a speculative production fix.

## Phase 2: Reproduce and Minimize

Confirm that the check detects the user's failure rather than an incidental setup error. Preserve the original scenario for final verification.

Reduce inputs, configuration, callers, or steps one at a time, rerunning the check after each reduction. Stop when the reproduction is small enough to discriminate between plausible causes; explain any required environment or timing dependencies that cannot be removed. Preserve real call-site interactions when they are necessary to trigger the bug.

For performance regressions, establish a repeatable baseline first. Record workload, environment, warm-up, sample count, and a meaningful metric or threshold. Distinguish measurement noise from the reported regression.

## Phase 3: State Falsifiable Hypotheses

For a narrow defect, use the strongest local hypothesis first. For an ambiguous or resistant failure, present a short ranked set of plausible alternatives without inventing extras to meet a quota.

For each, state:

- Evidence supporting it.
- A prediction: "If X is the cause, observing or changing Y should produce Z."
- The cheapest check that could disprove it.

Share the hypothesis and next check before testing. Continue within the authorized scope unless the user requested an approval checkpoint. If evidence falsifies the hypothesis, update it rather than repeatedly patching the same assumption.

## Phase 4: Probe One Variable at a Time

Prefer debugger inspection, then targeted instrumentation at the point that distinguishes the competing predictions. Do not log everything.

Tag temporary logs with a unique prefix such as `[DEBUG-a4f2]`, and track any temporary fixtures or harnesses created by this investigation. Do not log secrets or sensitive payloads.

After each substantive edit, immediately run the narrowest relevant check before further reading or patching. If the check exposes a local defect, repair that slice and rerun the same check. Expand scope only when the result points elsewhere.

For performance issues, use timings, a profiler, or a query plan instead of relying on log volume. Consider bisection only when a known-good and known-bad state and a reliable check exist. Do not run state-changing Git bisection in the user's working tree without explicit permission; agree on an isolated setup first.

## Phase 5: Regression Test and Fix

When a suitable test surface exists:

1. Turn the minimized reproduction into a regression test using the repository's existing test framework and helpers.
2. Run it and confirm it fails for the intended reason before fixing the implementation.
3. Apply the smallest root-cause fix.
4. Rerun that test immediately and confirm it passes.
5. Rerun the original scenario, then relevant neighboring tests and required build or lint gates according to the change's risk.

The test must exercise the actual failure pattern, including multiple callers or asynchronous interactions where relevant. Do not add a shallow test that merely reproduces an assumption in a mock.

If no meaningful regression test can be written within scope, state why and retain the strongest available verification. Do not redesign the architecture or delete existing coverage as part of an incidental fix. For diagnosis-only requests, report the supported cause and proposed fix without applying it.

## Phase 6: Clean Up and Report

- Verify the original scenario no longer fails. For flakes, report the before/after failure counts and residual uncertainty; for performance, report comparable measurements.
- Remove only instrumentation and throwaway artifacts introduced by this investigation. Keep permanent regression tests and useful failure fixtures.
- Search for the unique debug prefix to confirm cleanup, then rerun the focused check if cleanup changed executable code.
- Report the supported root cause, fix, checks run, outcomes, and any missing coverage or blocked validation. Do not claim unrun tests passed.
- Include the root cause in a commit or PR description only when the user requests that operation; otherwise include it in the final response.

If the code's structure prevents useful regression coverage, recommend a separate [architecture survey](../improve-codebase-architecture/SKILL.md) after diagnosis. Do not invoke it automatically or broaden the current task into a refactor.