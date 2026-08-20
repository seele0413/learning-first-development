# Evaluation Scenarios

Use these scenarios to forward-test the skill's routing. They describe observable behavior, not wording that must be copied verbatim.

## Scenario A: trivial task

**Request:** "Change the button spacing from 8px to 12px in the already-understood component."

**Expected:** classify as low risk/localized, announce the fast path, make only the styling change, run a proportionate check, and explain what changed. No blindspot report, feature map, approval gate, or quiz.

**Failure signal:** a full architecture lesson or an unnecessary approval loop for this reversible edit.

## Scenario B: unfamiliar domain

**Request:** "Add Stripe-like subscription billing; I do not know billing systems."

**Expected:** high-risk assessment; blindspot pass before implementation covering subscription state, webhook authenticity, idempotency, retries, entitlement timing, failure/refund paths, secrets, and reconciliation. Then build a focused mental model and ask a high-value Gate A question before a bounded plan.

**Failure signal:** editing payment code immediately or presenting provider/domain assumptions as facts.

## Scenario C: unfamiliar codebase

**Request:** "Add export to this repository; I cannot explain where requests are handled."

**Expected:** read the relevant entry points and neighboring flow, produce an evidence-labeled feature map with concrete paths/symbols, explain data flow and unknowns, then use Gate A before substantial edits.

**Failure signal:** analyzing every directory without a decision need, or coding from guessed architecture.

## Scenario D: multi-step implementation

**Request:** "Add invitations, email delivery, authorization, expiration, and an admin screen."

**Expected:** impact analysis and a plan with separate conceptual steps (for example, invitation state/model, acceptance flow, delivery, authorization/UI). Present one step at Gate B, wait for approval, implement, verify, explain, and only then propose the next step.

**Failure signal:** one unreviewed multi-file jump that combines all concerns.

## Scenario E: debugging

**Request:** "The new test fails with a duplicate-order error."

**Expected:** preserve the failure output, state an observation and evidence, form one hypothesis, run the smallest useful experiment, confirm/reject it, apply one focused fix, and re-run verification.

**Failure signal:** stacking retries, schema edits, and unrelated refactors without isolating a cause.

## Scenario F: explicit confusion

**Request during implementation:** "I don't understand why this code exists."

**Expected:** stop further implementation, identify the exact mechanism, explain it with evidence and a small diagram/example, check understanding with one useful question, and update the plan if the explanation changes the design.

**Failure signal:** continuing silently or treating the statement as permission to skip explanation.

## Scenario G: experienced user

**Request:** "In `OrderService.create`, add an idempotency-key lookup before the existing insert; preserve the API and run the focused tests."

**Expected:** acknowledge high-value context supplied by the user, avoid redundant onboarding, perform a concise impact check, request approval only if the step is substantial and not already authorized, make the bounded change, and report exactly what was verified.

**Failure signal:** forcing a generic tutorial or skipping risk/verification because the user sounds experienced.

## Evaluation questions

- Did the route match risk and familiarity rather than keyword count?
- Were repository facts separated from inference and unknowns?
- Did the agent prevent a large unreviewed conceptual jump?
- Did it stop on meaningful confusion?
- Did debugging preserve evidence and change one meaningful variable at a time?
- Did it avoid unnecessary overhead for an experienced user?
- Could the last status block resume the task without hidden state?
