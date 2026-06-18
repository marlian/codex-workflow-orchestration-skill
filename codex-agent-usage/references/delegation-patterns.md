# Delegation Patterns

Use this reference when the user wants agents but the task is not a review.

## Explorer / Searcher

Use for codebase or document discovery.

Good tasks:

- “Find where this concept is implemented.”
- “Trace all consumers of this API.”
- “Inventory files that mention this assumption.”

Bad tasks:

- “Understand the whole repo.”
- “Tell me if this is good.”

## Researcher

Use for external evidence, docs, papers, or current facts.

Good tasks:

- “Find primary sources for this market/regulatory/API claim.”
- “Compare current provider pricing from official pages.”
- “Summarize evidence with citations and uncertainty.”

Use web browsing for current or unstable facts.

## Thinker

Use for hard-to-reverse decisions and tradeoff analysis.

Good tasks:

- architecture direction;
- migration path;
- product strategy;
- buy/build decisions;
- irreversible commitments.

The thinker should converge to recommendation, confidence, and what evidence would change the decision.

## Worker

Use for bounded implementation only when write scopes are separate.

Rules:

- assign ownership by files/modules;
- tell workers not to revert others' edits;
- require changed files and checks run;
- integrate results in the main thread.

## Verifier

Use when the main thread is producing/fixing and a parallel check can catch concrete risk.

Good tasks:

- visual QA of slides/PDFs;
- formula verification;
- smoke-test a specific flow;
- compare generated output against acceptance criteria.

## The Default Shape

```text
Main thread: goal, scope, integration, user communication.
Subagents: narrow evidence or bounded work.
Main thread: merge, decide, fix order, final answer.
```
