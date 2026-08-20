---
name: learning-first-development
description: "Keep software progress aligned with the user's mental model and ownership. Use when explicitly invoked for coding-agent planning, onboarding, implementation, verification, or debugging of a project; adapt the amount of process to risk and familiarity."
---

# Learning-First Development

## Purpose

Optimize for **working software + user understanding**. The governing rule is:

> Project progress must not significantly outpace the user's mental model of the system.

This is an orchestration workflow, not a code-generation shortcut. The user remains the project owner. Preserve existing architecture and scope unless a justified, approved change is needed.

## Activation and modes

This skill is explicit-only. It is active when the user invokes `$learning-first-development` or clearly asks to use this workflow. Do not add its gates to unrelated requests.

Use **balanced** mode by default. Honor an explicit `learning mode`, `balanced mode`, or `fast mode` request:

- **Learning:** spend more time on blindspots, feature-level mental models, explanations, and comprehension checks.
- **Balanced:** use gates when risk, unfamiliarity, or uncertainty justifies them; use the fast path for small, reversible work.
- **Fast:** minimize ceremony, but never hide material risk, fabricate evidence, skip required verification, or silently make a high-impact change.

If the user requests speed for a high-risk task, state the material risk briefly and retain the minimum necessary blindspot, impact, approval, and verification controls.

## Start with assessment

At the beginning of an active task, classify (at least briefly):

- user familiarity and codebase familiarity;
- task complexity and architectural impact;
- reversibility and security/data risk;
- domain unfamiliarity and requirement uncertainty.

Then choose a route:

```text
Risk: LOW | MEDIUM | HIGH
Familiarity requirement: LOW | MEDIUM | HIGH
Scope: LOCALIZED | CROSS-CUTTING
Route: FAST PATH | GUIDED WORKFLOW
Reason: <one sentence>
```

### Fast path

Use for typo/text fixes, localized styling, obvious configuration changes, or a small reversible change in a well-understood area. Say, in substance:

```text
Risk: LOW
Scope: localized
Mental-model impact: minimal
Proceeding with a small direct change.
```

Make the small change, verify what is practical, and explain the result. Do not run the full workflow for trivial work.

## Guided workflow

For medium/high-risk work, unfamiliar areas, or material uncertainty, follow this adaptive sequence:

```text
GOAL / CONTEXT
  -> TASK ASSESSMENT
  -> BLINDSPOT PASS (conditional)
  -> FEATURE MAP (conditional)
  -> COMPREHENSION GATE A
  -> IMPACT ANALYSIS
  -> IMPLEMENTATION PLAN
  -> APPROVAL GATE B
  -> ONE CONCEPTUAL STEP
  -> VERIFY
       fail -> SYSTEMATIC DEBUGGING -> VERIFY
       pass -> EXPLAIN THE DIFF -> COMPREHENSION GATE C (when useful)
  -> NEXT SMALL STEP
  -> KNOWLEDGE CONSOLIDATION
```

Read [references/operating-model.md](references/operating-model.md) when this route is selected or when a transition/response template is needed.

### Blindspot pass (conditional)

Run it when the domain is unfamiliar, requirements are underspecified, assumptions could materially change the design, or the task involves payments, authentication, security, concurrency, distributed systems, infrastructure, migrations, or other consequential behavior. Surface only the current task's important known unknowns, hidden requirements, failure modes, and decisions. Do not turn it into a generic textbook lesson.

### Feature map (conditional)

If the user cannot yet explain the relevant code path, inspect the focused feature area before editing. Identify entry points, responsibilities, abstractions, data/control flow, persistence, external dependencies, and invariants. Prefer a small diagram. Label claims as:

```text
EVIDENCE: <file/symbol/observed behavior>
INFERENCE: <reasoned interpretation>
UNKNOWN: <not established yet>
Confidence: HIGH | MEDIUM | LOW
```

Never present an inference as repository fact. Do not repeatedly analyze the whole repository when a feature-level map is sufficient.

### Comprehension Gate A

Before substantial implementation, ask at most one or two high-value questions, for example: "In your own words, what path does this request take from the API entry point to persistence?" The threshold is useful conceptual understanding and knowing where to investigate if something fails, not perfect mastery. If the answer reveals a meaningful misunderstanding, explain the missing concept and re-check it. If the user explicitly says they do not understand a relevant mechanism, pause implementation and repair the mental model before continuing.

### Impact analysis and plan

Before editing, state:

```text
What will change
Where it will change
Why it needs to change
Expected behavior afterward
```

Include affected modules/contracts, data-model or migration concerns, tests, compatibility, external systems, and likely failure modes when applicable. Use concrete file/symbol evidence for repository claims.

Break the work into understandable, testable conceptual changes. **One conceptual change at a time** does not mean one file at a time; a single concept may modify several coordinated files. Do not absorb newly discovered complexity into the current step. Update the plan and seek approval when scope or risk changes.

### Approval Gate B

Before a non-trivial implementation step, summarize:

```text
Current step: <one concept>
Files likely affected: <paths or symbols>
Behavior being introduced: <observable result>
Why this step exists: <design reason>
How it will be verified: <tests/checks/manual observation>
```

Wait for user approval before mutating code for that step, unless the user has already clearly approved that exact step. Do not request ceremony for trivial, reversible actions already authorized by the original request.

### Implementation and TDD

During an approved step, make only that conceptual change, keep the diff minimal, follow repository conventions, and avoid opportunistic refactors. Use TDD for behavior-heavy or regression-prone logic when it adds value:

```text
RED (failing or identified test) -> GREEN (minimum behavior) -> VERIFY -> EXPLAIN
```

Do not create meaningless tests or require strict TDD for low-value documentation or trivial visual changes.

### Verification

Code written is not task completion. Run relevant tests, type checks, lint/build checks, targeted manual checks, or artifact inspection as appropriate. Report only what was actually observed:

```text
VERIFIED: <command/check and result>
NOT VERIFIED: <important check not run>
BLOCKED: <why a needed check could not run>
```

Never claim a pass without running or directly observing it.

### Failure branch: systematic debugging

When verification fails, stop speculative editing and preserve the evidence. Use:

```text
OBSERVATION -> EVIDENCE -> HYPOTHESIS -> SMALLEST USEFUL EXPERIMENT
-> RESULT -> CONFIRM / REJECT -> FIX -> VERIFY
```

Change one meaningful variable at a time, prefer root cause over symptom suppression, inspect working examples where available, and state uncertainty. A passing workaround does not prove the root cause is understood.

### Explain the diff and Gate C

After every meaningful step, explain conceptually:

- what changed and why;
- which files/modules changed;
- how data/control flow changed;
- how it was verified;
- what did not change.

Use a before/after diagram when architecture or flow changed. When user understanding matters, ask one high-value Gate C question (for example, "Why is this concept represented separately from that one?"). Do not quiz after trivial changes; explain from another angle if the answer shows a gap.

### Knowledge and resumability

At meaningful milestones, consolidate durable knowledge. Only add or update project documentation such as `docs/mental-model.md`, `docs/decisions.md`, `docs/unknowns.md`, or `docs/architecture.md` when repository conventions and user intent support it; do not create duplicates automatically.

If persistent workflow state is not guaranteed, end with a compact block that can be pasted into a new session:

```text
Current phase:
Current goal:
Relevant mental model:
Known unknowns:
Approved implementation step:
Last verified result:
Next action:
```

## Boundaries

Avoid AI takeover, analysis theater, quiz theater, tutorial overload, scope creep, fake certainty, fake verification, and process deadlock. Optional specialized skills may be used when independently available and relevant, but this workflow must remain useful when they are absent. Do not claim persistent memory, hidden approvals, or capabilities the host skill system does not provide.

For routing details, response templates, and enforcement limits, read [references/operating-model.md](references/operating-model.md). For self-evaluation, read [references/evaluation-and-examples.md](references/evaluation-and-examples.md).

## Invocation examples

```text
$learning-first-development Add audit logging, but keep me oriented and wait before each substantial conceptual step.
$learning-first-development Use balanced mode to investigate why checkout retries duplicate orders.
$learning-first-development Use fast mode for this localized copy change and report exactly what you verified.
```
