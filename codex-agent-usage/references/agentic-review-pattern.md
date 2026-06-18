# Agentic Review Pattern

Use review as the first agentic workflow because the contrast is visible: one assistant can produce a plausible deliverable, but focused reviewers catch different failure modes.

## The Pattern

```text
produce deliverable
  -> split review into roles
  -> require proof
  -> merge findings
  -> fix source of truth
  -> regenerate outputs
  -> targeted re-check
```

## Role Design

Do not create three agents that all answer the same question. Separate by failure mode.

| Role | Failure mode |
|---|---|
| Correctness | wrong formulas, contradictions, stale references, broken assumptions |
| Risk/compliance | missing permissions, privacy, security, regulatory, readiness blockers |
| Strategy/architecture | weak story, bad structure, missing route to outcome, incoherent positioning |

For software:

| Role | Failure mode |
|---|---|
| Security | token leakage, auth bypass, privilege escalation, unsafe input/output |
| Correctness | contract drift, edge cases, state transitions, race conditions |
| Architecture | reachability, coupling, API design, migration path, test seams |

## Severity Calibration

| Severity | Meaning |
|---|---|
| Blocker | Must fix before relying on/sending/shipping. Current artifact has a concrete failure. |
| Risk | Acceptable only if disclosed, mitigated, or consciously deferred. |
| Ok | Strong point worth preserving. |
| Question | Needs human/business input or external evidence. |

Do not inflate future-only improvements into Blockers. A Blocker needs a current defect, contradiction, missing evidence, or readiness failure.

## Proof Standard

Every Blocker and Risk needs proof:

- file path + line range;
- spreadsheet sheet/cell/formula;
- slide number;
- document section title plus short excerpt;
- command/check used;
- exact mismatch across files.

Unsupported phrases like “seems weak” are not findings. Convert them into evidence: what is missing, where, and why it matters.

## Merge Standard

The main orchestrator must not paste reviewer reports back-to-back. It should:

1. group duplicate findings;
2. preserve the best proof;
3. raise confidence when independent reviewers agree;
4. separate current defects from future improvements;
5. list fix order by source-of-truth dependency.

## Fix Order Template

```markdown
## Suggested fix order
1. Freeze the delivery package: decide final-facing vs internal files.
2. Fix calculation/source-of-truth defects first.
3. Reconcile assumptions across documents.
4. Add missing evidence or annexes.
5. Regenerate derived PDFs/decks/exports.
6. Run targeted checks for the corrected findings.
```
