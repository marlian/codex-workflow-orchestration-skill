# Example Scenario — Business Plan Review

This is an illustrative scenario for explaining tool-backed Codex workflows to non-technical users. It is not a business-plan-specific method; it is a concrete story that makes the generic pattern visible. Replace the domain, artifacts, and reviewer roles with whatever the user's real work requires.

## Starting Objection

A user says AI is only useful for polishing emails and presentations because it:

- invents or miscalculates numbers;
- forgets to update values consistently;
- produces plausible but unreliable business plans.

The answer is not “trust the AI more.” The answer is to change the workflow. A browser chat without file tools, formulas, review roles, or regeneration is not the same operating environment as a Codex workspace with local files, plugins, MCP servers, and task-local agents.

Frame this without model tribalism:

```text
The problem is not which brand of model wins in a chat tab. The problem is asking any chat tab to act like a full workspace without giving it tools, sources of truth, and reviewers.
```

## Step 1 — Plain Chat Creates the Contract

A normal user can ask a plain chat:

```text
I need to make a business plan. A course told me I should give the AI a system prompt first. Can you help me write one?
```

Then ask:

```text
They also warned me not to let the AI do calculations in its head because chat models are not calculators. What should I tell it?
```

The useful output is a contract:

- ask before inventing missing data;
- distinguish fact, estimate, assumption;
- proceed section by section;
- challenge weak assumptions;
- never calculate mentally;
- use spreadsheets/code for arithmetic;
- keep one source of truth for numbers.

## Step 2 — Agent/Tool Workflow Produces Artifacts

The same contract can drive an artifact workflow. For a business plan, that may mean:

- source dossier documents;
- business plan document;
- spreadsheet model with formulas;
- PDF export;
- slide deck.

The important shift is that numbers live in a tool-backed model, not only in prose. If a spreadsheet/model tool is missing, the correct move is to surface the capability gap, not to let the assistant calculate in its head.

## Step 3 — Review Agents Attack the Package

Run three reviewers:

| Reviewer | Example finding |
|---|---|
| Numbers/correctness | Break-even sheet points Base/Ottimistico to wrong-year revenues. |
| Bank/compliance | Funding assumptions lack evidence; customer-contact or privacy claims are not documented. |
| Strategy/architecture | Co-founder marketing labor is described but absent from the model. |

This is the moment the workflow becomes visible: the AI-generated package is not trusted because it is polished; it is improved because independent roles inspect it with proof.

## Step 4 — Merge Findings and Fix Source of Truth

The orchestrator merges findings into tiers:

- Tier 1: formula bugs, draft markers, cross-document drift, missing labor cost.
- Tier 2: sensitivity table, monthly cash flow, VAT timing, GDPR/customer-contact annex.

Then it fixes source files first:

1. spreadsheet model;
2. canonical business plan document;
3. supporting dossiers;
4. slide deck/PDF exports.

## Workflow Takeaway

Use this phrasing:

```text
The serious workflow is not “AI writes a business plan.”
The serious workflow is produce → review → correct → regenerate.
Agents make the review legible because each one has a job.
```

## Good First Prompt

```text
Use $codex-agent-usage to review this business plan folder before I rely on it.
Create three task-local reviewer agents if runtime subagents are available:
1. numbers/correctness,
2. risk/readiness,
3. strategy/narrative.

Each reviewer must give Blocker, Risk, Ok, Questions, with proof.
Then merge the reviews into one action plan and tell me what to fix first.
```
