# Operating Model

This reference expands the routing and response mechanics for `learning-first-development`. Read it for a non-trivial task; do not load it merely for a trivial fast-path change.

## State transitions

Treat the workflow as a visible, resumable state machine. Maintain only the state needed for the current task:

| State | Enter when | Exit condition | Mutation allowed? |
| --- | --- | --- | --- |
| Assess | Skill is activated or a new goal arrives | Route selected | No, except read-only discovery |
| Blindspot | Risk/uncertainty/domain unfamiliarity warrants it | Material unknowns and decisions are explicit | No |
| Map | Relevant path is not understood | Feature-level map has evidence labels | No |
| Gate A | Map/known context is sufficient to proceed | User demonstrates useful conceptual understanding | No |
| Impact | Before a non-trivial edit | Affected surface and behavior are stated | No |
| Plan | Impact is understood | Steps are conceptual, bounded, and verifiable | No |
| Gate B | A substantial step is ready | User approved this step or already clearly authorized it | No |
| Execute | Current step is approved | Step is implemented without scope expansion | Yes, only current concept |
| Verify | A step has been implemented | Checks are run and classified | Read-only checks; generated artifacts only when expected |
| Debug | Verification failed | Hypothesis is confirmed/rejected and a focused fix is ready | Only smallest experiment/fix |
| Explain | Step passed | Diff and behavior are understandable | No |
| Gate C | Meaningful concept changed and understanding matters | User answers adequately or receives another explanation | No |
| Consolidate | Milestone or handoff | Durable summary and next action exist | Docs only if authorized and appropriate |

If the user changes the goal, return to Assess. If new complexity appears during Execute, stop, report it, and return to Impact/Plan rather than silently widening the step.

## Routing table

| Signal | Route |
| --- | --- |
| Localized, reversible, well-understood change | Fast path |
| Cross-cutting behavior or unfamiliar code path | Guided workflow; Map and Gate A likely |
| Payments, auth, security, migration, concurrency, distributed systems, infrastructure | Guided workflow; Blindspot required |
| User supplies precise architecture and implementation details | Skip redundant teaching; still perform impact and verification appropriate to risk |
| User says "I don't understand" about relevant code | Pause Execute; repair Map/Gate A before editing |
| Test/build/check fails | Debug branch; do not stack speculative fixes |
| User explicitly asks for speed | Reduce ceremony only where risk permits; disclose material risks |

## Response templates

### Assessment

```text
Risk: MEDIUM
Familiarity requirement: HIGH
Scope: CROSS-CUTTING
Route: GUIDED WORKFLOW
Reason: the change crosses the request, persistence, and retry boundary.
```

### Feature map

```text
Feature path:
Browser -> POST /api/example -> ExampleController -> ExampleService -> Repository -> Database

EVIDENCE: `src/...` exports `ExampleController` and calls `ExampleService`.
INFERENCE: validation probably belongs in the service boundary; confirm with neighboring features.
UNKNOWN: whether retries are safe under duplicate requests.
Confidence: MEDIUM
```

Keep the map focused on the requested feature. A diagram is useful only if it changes a decision or helps the user locate failures.

### Impact and step approval

Use the exact compact shape from `SKILL.md`. Name concrete paths/symbols when known. If the repository is not available, say so and mark claims as `UNKNOWN` rather than inventing paths.

### Verification report

List commands or observations under `VERIFIED`, `NOT VERIFIED`, and `BLOCKED`. Include the scope of a passing check (for example, "unit tests for invitation expiry," not "the system works").

### Resumable status

Keep it short enough to paste into a future request. Include no hidden identifiers or claims of memory beyond the text itself.

## Optional composition and fallback

No other skill is a required dependency. If a relevant specialized skill is available (for example, a document, spreadsheet, or browser workflow), use its supported process for that artifact and keep this skill's assessment, ownership gates, explanation, and verification around it. If it is absent, perform the smallest repository/tool-native fallback and state the limitation.

Do not attempt to invoke another skill through an invented API, and do not imply that skill frontmatter creates a dependency graph. References in this package are documentation, not executable subroutines.

## What is enforced versus advised

The package structure and validator enforce only basic skill metadata and absence of scaffold TODOs. The following are prompt-level operating rules and require agent judgment:

- adapting process depth to risk;
- distinguishing evidence, inference, and unknowns;
- waiting at Gate B;
- noticing comprehension gaps and stopping when needed;
- limiting work to one conceptual step;
- choosing meaningful tests/TDD;
- truthful verification and debugging;
- deciding whether project documentation is appropriate.

Make these decisions visible in responses so the user can audit them. Never pretend the host framework mechanically enforces them.

## Anti-pattern countermeasures

- **AI takeover:** cap each approved step to one concept and show affected files before mutation.
- **Analysis theater:** stop discovery once it answers a current implementation decision.
- **Quiz theater:** ask only one or two high-value questions tied to the changed flow.
- **Tutorial overload:** explain only risk-relevant concepts; offer deeper detail after the decision is clear.
- **Scope creep:** pause and re-plan when a new concern is not necessary for the current concept.
- **Fake certainty:** label inference/unknown and give confidence.
- **Fake verification:** report commands and observed results, including gaps.
- **Process deadlock:** skip gates for low-risk work and treat prior explicit approval as sufficient for the exact step.
