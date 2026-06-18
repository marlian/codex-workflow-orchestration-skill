---
name: codex-agent-usage
description: "Orient and extend Codex Desktop or Codex CLI workflows: discover installed/available plugins, skills, connectors, MCP servers, and artifact runtimes; choose ephemeral subagents versus stable prompt-layered agents; preserve main-thread context during repo/folder/document reviews; guide safe MCP/plugin setup via config.toml; and create/update AGENTS.md project guidance. Use when the user asks about agents, subagents, delegation, parallel review, Codex capabilities, missing tools, plugin/skill selection, Word/Excel/PDF/PPTX or business-plan package reviews, prompt layering/custom agents, reusable workflows, or any review that would otherwise require manually reading many files or writing ad hoc extraction scripts."
---

# Codex Workflow Orchestration

Use this skill to orient Codex in its current runtime, choose the right capability layer, and turn plain AI chat into a visible agentic workflow: the main Codex thread coordinates, specialized agents inspect or execute narrow tasks, and the final answer merges evidence into decisions.

This skill is domain-generic. It works for code, business plans, documents, spreadsheets, research packs, presentations, proposals, contracts, and operational plans. Review is the best first demonstration because role separation is easy to understand and catches real mistakes.

Codex Desktop, Codex CLI, plugins, connectors, and subagent tools can change over time. Treat the visible runtime as ground truth. Do not rely on memory of the app, old product knowledge, or hardcoded UI paths when local discovery is available.

Codex is often more powerful than a prepackaged AI workspace because the app can sit close to the Codex CLI, local files, MCP servers, shell tools, and persistent configuration. The tradeoff is that a fresh setup may start comparatively bare: it may expose only basic file/shell tools and not yet include document, spreadsheet, PDF, or presentation tooling. Treat missing artifact tools as a setup/capability gap, not as a reason to improvise low-quality extraction in the main thread.

## Scope Checklist

This skill covers five connected jobs:

1. **Capability discovery:** inspect installed/available plugins, skills, connectors, MCP servers, and artifact runtimes before choosing an execution path.
2. **Agent pattern choice:** use ephemeral task-local agents for one-off reviews/delegation; route to stable prompt layering/custom agents only when the user wants a reusable team setup across future sessions.
3. **Tool expansion:** when needed, guide or perform safe MCP/plugin installation, including minimal `config.toml` edits after approval and backup.
4. **Project memory on disk:** when work will recur in a folder, offer to create or update `AGENTS.md` or the current runtime's project-guidance file so future main threads and subagents inherit the project rules.
5. **Environment orientation:** explain and operate the Codex environment as discovered at runtime, because app/plugin surfaces may be newer than the model's built-in product knowledge.


## Core Mental Model

```text
Main Codex thread = orchestrator
  - understands the goal
  - chooses the right roles
  - sends narrow tasks
  - merges findings
  - decides fix order

Subagents = specialists
  - inspect one failure mode or execute one bounded slice
  - return proof, not vibes
  - do not own the whole decision
```

Do not sell agents as “more AI.” Sell them as separation of concerns:

```text
One generic assistant tends to average the task.
Several focused agents inspect different failure modes.
The value is not consensus. The value is coverage + proof + merge.
```

## Main-Thread Context Budget

For review, audit, research, or package inspection, treat the main Codex thread as the orchestrator, not the warehouse.

Default posture:

- main thread: inventory, capability gate, dispatch, merge, targeted verification, user communication;
- subagents: file discovery, broad reading, independent review passes, evidence gathering, heavy artifact inspection;
- tools/scripts: deterministic extraction only when bounded and necessary.

Do not let the main thread perform a full review by manually reading every file, running many ad-hoc extraction scripts, and accumulating raw outputs. That burns context, risks compaction, and degrades the user experience. If the task is large enough that you are about to do repeated searches, conversions, or file reads, delegate first.

Hard rule for reviews: after a light inventory and capability check, spawn focused reviewers when runtime subagents are available. If no subagent facility exists, run the same roles sequentially with strict scope and say this is the fallback.

## When to Use Agents

Use agents when the task benefits from independent perspectives or parallel work:

| Task shape | Good agent pattern |
|---|---|
| Review a package before sending it | 2-3 reviewers with separate mandates |
| Inspect a code change | security, correctness, architecture reviewers |
| Check a business plan or model | numbers, bank/compliance, strategy reviewers |
| Research a decision | researcher gathers evidence, thinker evaluates tradeoffs |
| Build a feature with separable files | workers with disjoint file ownership |
| Verify a deliverable | reviewer checks outputs while main thread prepares fixes |

Avoid agents when:

- the task is tiny;
- the user did not ask for delegation and a single pass is enough;
- agents would duplicate the same work;
- the work requires one coherent judgment rather than separated evidence;
- the write scopes would overlap and create conflicts.

## Runtime Reality Check

Before promising an app-specific capability, verify what this Codex environment actually exposes:

- visible tools and connectors;
- available skills or plugins;
- current folder contents and project guidance;
- local app/tool configuration when the task depends on it.

If a local MCP registry or plugin manager is visible, query it before web research. The user's actual enabled servers and tool-call history are stronger evidence than a generic list of MCPs. Check both server enablement and tool enablement; registered tools may exist even when the server is disabled for the current profile.

If no registry exists, inspect the visible Codex MCP configuration when available. Resolve the user-level config via `CODEX_HOME` when set, otherwise `~/.codex/config.toml`; do not hardcode platform paths like `C:\Users\...`. If the needed capability is missing, propose a small vetted tool stack with benefits, limits, and risks in plain language. Only edit config or install tools after user approval, and make a backup first.

If the needed capability is not visible, use tool discovery/search when available. If it still is not available, say so plainly and choose a safe fallback. Do not invent tool names. Do not give version-specific click paths or configuration instructions unless you have verified them in the current environment.

Use plain language with non-technical users:

```text
I am checking what this Codex setup can actually do before I promise the workflow.
```

Distinguish the stable workflow from the changing app surface:

```text
The workflow is stable: roles, evidence, merge, correction. The exact buttons and tools depend on the Codex setup in front of us.
```

## Plugin / Skill Capability Discovery

In Codex App, the Plugin and Skill surfaces are first-class capability discovery. Before proposing MCP config edits or custom installs, check whether an installed or installable plugin already provides the needed workflow, connector, or artifact runtime.

Mental model:

| Surface | What it usually adds | Examples |
|---|---|---|
| Artifact plugins | File creation/editing/verification runtimes | Documents, Spreadsheets, PDF, Presentations |
| App/connector plugins | Access to private app data/actions after authorization | Gmail, Google Calendar, GitHub, Notion, Slack, Google Drive |
| Domain workflow plugins | Specialized playbooks, reviewers, report/deck/model patterns | Data Analytics, Sales, Investment Banking, Product Design |
| Control plugins | UI/web/desktop operation | Browser, Chrome, Computer Use |
| Developer plugins | API keys, SDKs, app scaffolding, deploy/test workflows | OpenAI Developers, GitHub |

Use this order when a capability is missing:

1. Check visible installed plugins/skills.
2. If a relevant OpenAI/curated plugin is available but not installed, explain the benefit and ask the user to add it.
3. If no suitable plugin exists, propose vetted MCP/community tooling with risks and limits.
4. Only then consider custom scripts or manual extraction, and keep them bounded.

For non-technical users, say this simply:

```text
Codex can do this well, but it needs the right add-on. I’ll first check whether there is an official or curated plugin for this kind of file/workflow before trying a workaround.
```

Plugin presence does not remove judgment. A plugin may provide structure or access, but the main thread still owns source verification, scope control, privacy boundaries, and final merge.

## Tool Use Rule

If the user asks for agents, subagents, delegation, parallel work, or multiple reviewer roles, prefer creating task-local runtime subagents when the runtime exposes them. The user should not need to predefine agent files, open settings, or write prompt files by hand for a one-off review.

For broad reviews, repo/package audits, business-plan/document-package reviews, or any task with many files, agents are not optional polish. They are the primary context-preservation mechanism.

Typical procedure:

1. Perform a light inventory: file types, obvious source-of-truth files, existing project guidance, available tools.
2. Spawn 2-3 focused subagents with disjoint mandates and proof requirements.
3. Continue only non-overlapping work in the main thread while they run.
4. Merge, deduplicate, calibrate severity, and decide fix order.
5. Do targeted verification only for high-impact claims.

If a native subagent/spawn tool is visible, create one task-local subagent per role. If no subagent tool is visible, use tool discovery for “subagent”, “multi-agent”, “delegate”, or “spawn agent”. If runtime subagents truly are not available, run the same role passes sequentially in the main thread and explicitly label that as a fallback.

Do not require persistent custom agents for a simple demo. Persistent agent config is useful for recurring teams, but runtime task-local agents are the default for one-off reviews and demos.

Default to ephemeral reporting: subagents return their findings to the main thread; the main thread merges them in chat. Do not create `review/`, markdown reports, or other persistent files unless the user explicitly asks to save the review. For non-technical users, avoiding surprise files is part of the usability contract.

Do not invent tool names. Do not expose prompt files to the user unless they ask. Most demos can be run with one plain-language request.

If the user asks to make the setup reusable across future sessions, separate that from runtime delegation: reusable prompt/project configuration is a configuration task, while task-local agents are an execution pattern. Use dedicated prompt-layering or app-configuration guidance when it is installed; otherwise inspect the current configuration before advising.

## Anti-Patterns This Skill Must Prevent

- **Heroic main-thread review:** reading a whole repo/package in the main thread until context is saturated.
- **Python bricolage spiral:** spending many turns writing custom parsers/converters for Office/PDF files when the missing problem is tooling.
- **PDF image/OCR spiral:** converting PDFs to images and trying to visually infer everything unless the task explicitly requires OCR/layout recovery and the user accepts the limitation.
- **Filename-only review:** issuing findings from filenames or extracted snippets without reading the actual artifacts.
- **False capability performance:** pretending the setup can edit/verify Office/PDF files when it only has text extraction.

When one of these patterns starts, stop. Explain the missing capability or dispatch agents instead of continuing the spiral.

## Capability Gate for Real Artifact Work

Before doing serious work on a folder, inspect the artifact types and verify the required capabilities. Do this early, before deep reading, before spawning artifact-specific reviewers, and before promising deliverables.

| Artifact type | Needed capability | If missing |
|---|---|---|
| Word / DOCX | read, edit, render, verify documents | offer to enable/install document tooling or do text-only review |
| Excel / XLSX / CSV | inspect formulas, recalculate/check sheets, edit workbooks | offer to enable/install spreadsheet tooling or limit scope |
| PDF | extract text, render pages, inspect layout | offer to enable/install PDF tooling or use text-only fallback |
| Fillable PDF/forms | inspect fields, fill, flatten/verify | offer to enable/install PDF form/filler tooling |
| Slide decks | inspect/edit/render presentation files | offer to enable/install presentation tooling or review exported PDF only |

If the needed tools are visible, use them. If they are not visible, use tool discovery for the missing capability. If an install/configuration mechanism is available, ask the user before changing app configuration or installing tools.

Capability hierarchy for Office/PDF packages:

1. Use installed OpenAI artifact plugins when visible: Documents, Spreadsheets, PDF, and Presentations are the first-choice path for ordinary Word/Excel/PDF/PPTX work.
2. Use other native or plugin-provided document/spreadsheet/PDF/presentation tools when visible.
3. If editing is not required and the user only needs a text/structure review, prefer a trusted document-to-Markdown/text tool if available.
4. If only limited extraction is possible, use it inside a bounded subagent pass and label the review as limited.
5. Do not construct a custom Office/PDF processing pipeline in the main thread unless the user explicitly asks for that engineering work.

Use plain language with non-technical users:

```text
This folder contains Word, spreadsheet, and PDF files. To work on them properly I need document/spreadsheet/PDF tools. I can either do a limited text-only review, or help enable the right tools first.
```

Do not default to fragile workarounds for serious deliverables. Avoid turning PDFs into images, rewriting Office files with ad-hoc scripts, or reconstructing spreadsheets by hand unless the proper capability is unavailable, the task is small, and the user accepts the limitation. If a fallback is used, state what it cannot guarantee.

Configuration changes are not silent. Before editing app config, installing tools, or adding persistent capabilities, explain the change and ask for confirmation.

When the missing capability is an MCP/tool issue, inspect the local MCP registry or app configuration before suggesting generic plugins. Prefer the user's actual MCP servers over adjacent plugin capabilities. For Microsoft/Office-oriented options and artifact fallback policy, read `references/mcp-artifact-tools.md`.

## Optional Project Instruction Bootstrap

For ongoing work in a folder, Codex can create a small project-guidance file from the user's plain-language briefing. This turns a one-off chat contract into reusable guidance that the main thread and task-local agents can inherit later.

Use this only when the user wants continuing work in the same folder, repeated reviews, generation/regeneration, or a stable workflow. Do not create project instruction files for a one-off review unless the user explicitly asks.

Explain it in user-friendly terms, not as configuration work:

```text
I can save these working rules in this folder so future runs remember the project goal, source of truth, and review process.
```

Do not hide filesystem writes. The user does not need to know the mechanics, but they should know that a small project-guidance file will be created or updated.

A good project instruction file captures:

- what the project/folder is for;
- what artifacts are final-facing vs internal;
- the source of truth for numbers/data/config;
- what must not be hardcoded;
- how to regenerate outputs;
- which checks/reviews to run;
- privacy/secrets boundaries;
- default agent roles for review.

Keep it short. It is project guidance, not a transcript.

Suggested workflow:

1. Discuss the project in plain language.
2. Draft concise project instructions from the conversation and visible files.
3. Ask before writing if the file does not exist or if the change is substantial.
4. Write/update the instruction file in the project root.
5. Use runtime task-local agents for review or execution; they inherit the project guidance.

For non-technical users, the visible story is:

```text
You explain the job once.
Codex saves the working rules in the folder.
From then on, Codex and its agents know how this folder should be handled.
```

## Standard Review Workflow

Review is the safest and clearest entry point for non-technical users.

### 1. Inventory the package

Before delegating, identify what exists and whether the needed tools are available:

- source documents;
- spreadsheets and formulas;
- PDFs/exports;
- slide decks;
- code or config;
- prior review notes;
- README, project instructions, AGENTS.md, or other local guidance if present.

Never review from filenames alone. If binary files need inspection, use the appropriate document/spreadsheet/PDF/presentation capability to extract or render them.

### 2. Choose 2-3 roles

Use roles that are genuinely different. Three is usually enough.

| Generic role | Looks for |
|---|---|
| Correctness reviewer | arithmetic, formulas, broken references, contradictions, edge cases |
| Risk/compliance reviewer | permissions, legal/regulatory gaps, security, readiness blockers |
| Strategy/architecture reviewer | narrative logic, structure, user/client fit, integration, missing evidence |

For code, rename the roles but keep the separation:

| Code role | Looks for |
|---|---|
| Security reviewer | auth, secrets, permissions, injection, sensitive data |
| Correctness reviewer | contracts, state transitions, edge cases, tests |
| Architecture reviewer | boundaries, coupling, reachability, maintainability |

### 3. Give each agent a narrow contract

Every reviewer prompt needs:

```text
ROLE:
SCOPE:
FOCUS:
IGNORE:
OUTPUT FORMAT:
PROOF REQUIREMENT:
```

A good review prompt does not ask “is this good?”. It asks one role to hunt one class of failure.

### 4. Merge, do not concatenate

By default, keep reviewer reports ephemeral: collect subagent outputs in the main thread and present the merged result in chat. Persist files only if the user explicitly asks for saved reports.

The main thread must synthesize:

- deduplicate overlapping findings;
- raise confidence when independent reviewers find the same issue;
- separate Blocker, Risk, Ok, and Questions;
- assign fix order;
- identify the source of truth to edit first;
- verify critical claims before presenting them as facts.

### 5. Fix from source of truth outward

When the user asks to fix issues:

1. Fix the canonical source first: spreadsheet, source code, master document, config, or schema.
2. Recalculate or rerender derived outputs.
3. Propagate numbers or decisions across PDFs, slides, docs, and summaries.
4. Re-run targeted checks for the corrected issue.
5. Leave optional improvements for a second tier.

## Generic Review Prompt

Use this when the user asks for a multi-agent review but the domain is not specialized:

```text
Use agents to review this folder/package before I rely on it.
Run three focused reviewers:
1. Correctness: arithmetic, formulas, internal consistency, broken references, edge cases.
2. Risk/compliance: permissions, privacy, legal/regulatory, client-readiness, missing evidence.
3. Strategy/structure: narrative logic, positioning, user/client fit, architecture, completeness.

Each reviewer must return:
## Blocker
## Risk
## Ok
## Questions
For every Blocker or Risk include proof: file path, line, cell, slide, or exact section.

After all reviewers finish, merge the findings into one action plan with fix order.
```

If runtime subagents cannot be created in this environment:

```text
Runtime subagents are not available here, so I will run the same three role passes sequentially in this thread and merge the results.
```

## Delegation Prompt Patterns

### Reviewer agent

```text
ROLE: <focus> reviewer.
SCOPE: Review <folder/file/change>.
FOCUS: <specific failure modes>.
IGNORE: <what other reviewers cover>.
OUTPUT:
## Blocker
## Risk
## Ok
## Questions
For every Blocker/Risk include:
- Finding
- Impact
- Proof: file/line/cell/section/command
- Fix
```

### Researcher agent

```text
ROLE: Researcher.
SCOPE: Gather evidence for <decision/question>.
FOCUS: primary sources, current facts, contradictions, source quality.
OUTPUT:
- Short answer
- Evidence table with links/citations
- What is uncertain
- Recommendation with confidence
```

Use web search for current or unstable facts. Prefer primary sources for technical, legal, medical, financial, or product/API claims.

### Worker agent

```text
ROLE: Worker.
TASK: Implement <bounded change>.
OWNERSHIP: You may edit only <files/modules>.
CONSTRAINTS: Do not modify <out of scope>.
COORDINATION: Other agents may work in the repo; do not revert unrelated edits.
OUTPUT: Files changed, tests/checks run, risks remaining.
```

Use workers only when write scopes are disjoint and the user has asked for delegated implementation.

## Demo Explanation for Non-Technical Users

Use this explanation:

```text
The first step is not “make the AI smarter.”
The first step is to stop using one generic chat for every job.

We can ask the main assistant to coordinate specialists:
- one checks facts, numbers, and internal consistency,
- one checks risks, permissions, and readiness,
- one checks whether the structure, story, and assumptions hold.

Then the main assistant merges the findings, fixes the source files, and regenerates the outputs.
That is the beginning of agentic work: roles, tools, evidence, correction.
```

Good live demo shape:

```text
1. Open a realistic folder and explain the goal in plain language.
2. If this will be repeated, let Codex save a short project-guidance file.
3. Ask Codex for three focused reviewers.
4. Show that reviewers find different issues with proof.
5. Ask Codex to merge the findings and fix Tier 1 issues.
6. Show regenerated outputs.
```

Keep the demo small. The point is not complexity; the point is the shift from chat answer to workflow.

## Relationship to Prompt Configuration

Prompt configuration and agent usage are different layers:

| Layer | Purpose |
|---|---|
| Shared instructions | Common rules every assistant or specialist should inherit |
| Main assistant prompt | Default behavior of the coordinator in the main thread |
| Specialist prompts | Stable job descriptions for recurring roles |
| Runtime agent usage | Choosing roles, delegating tasks, and merging outputs for a specific job |

Use prompt configuration when setting up reusable files, defaults, or long-lived assistant behavior. Use this skill when the user wants to actually run agentic work on a specific task.

If the user asks how to configure Codex itself, custom prompts, project instructions, or reusable specialist roles, do not improvise from memory. Load the relevant prompt-layering/configuration skill if available, or inspect the current local configuration and product docs before giving setup instructions.

## References

- `references/agentic-review-pattern.md` — reusable review structure and role contracts.
- `references/business-plan-demo.md` — case-study arc: plain chat → prompt contract → generated package → tripartite review → fixes.
- `references/delegation-patterns.md` — when to use reviewers, researchers, thinkers, or workers.
- `references/mcp-artifact-tools.md` — MCP/tool capability gate for Word, Excel, PDFs, and forms.
- `assets/review-prompts/` — copyable prompt cards for demos or custom agent files.
