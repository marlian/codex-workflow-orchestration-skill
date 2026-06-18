# Codex Workflow Orchestration Skill

This repository contains a Codex skill that helps Codex use the Codex desktop app environment more effectively: plugins, skills, MCP servers, artifact tools, project instructions, and task-local agents.

It is written primarily **for Codex to read and install**, not as a long manual for end users. The README is the sharing wrapper: give people this link, tell Codex to install the skill folder, then use `$codex-agent-usage` to move from plain-chat guessing to tool-backed workflow.

## Why this exists

A fresh Codex setup can look deceptively simple: a chat box, a folder, maybe a terminal. Its real power appears only when Codex knows how to discover and use the environment around it: local files, installed plugins, available skills, MCP servers, connectors, artifact tools, project guidance, and task-local agents.

This skill is a small **Codex unlock kit**. It teaches Codex to start important work by asking: what can this runtime actually do, what tools are missing, what needs approval, and what workflow should we run?

It helps Codex avoid common bare-setup failure modes:

- treating Codex like a plain chat box instead of a workspace;
- doing broad reviews in the main thread until context is exhausted;
- calculating important numbers in prose instead of using a source of truth;
- inventing fragile extraction scripts before checking for document, spreadsheet, PDF, or presentation tools;
- giving generic advice when the task needs focused agents, evidence, and a correction loop.

Business plans are one useful example, but not the product. The product is a reusable operating pattern: **discover capability -> choose tools -> split roles -> require proof -> merge findings -> fix source files -> regenerate outputs**.

## What this skill is not

- It is not a business-plan generator. Business plans are only one example of a workflow that needs tools, numbers, evidence, and review.
- It does not install every plugin automatically. It tells Codex to inspect the runtime and ask before persistent setup changes.
- It does not replace project instructions. If a folder has `AGENTS.md` or other local guidance, that guidance remains the contract.

## For Codex: install this skill

If a user gives you this repository URL and asks you to install or use the skill, follow this contract:

1. Inspect the repository and locate the skill folder: `codex-agent-usage/`.
2. Validate that the folder contains a valid `SKILL.md` with `name: codex-agent-usage`.
3. Discover the current Codex skill installation location before copying anything.
   - Prefer a supported user-level or project-level skill directory exposed by the current Codex runtime.
   - Do **not** assume one hardcoded path. Different Codex versions and surfaces may use different locations, such as app-managed imports, `.agents/skills`, or `~/.codex/skills`.
4. If a skill named `codex-agent-usage` already exists, do **not** overwrite it silently.
   - Compare it first.
   - Ask the user whether to replace, back up, or install into the current project only.
5. Copy the complete `codex-agent-usage/` folder into the selected skill directory.
6. Validate the installed skill if the current runtime provides a validator.
7. Tell the user whether they need to start a new Codex thread, reload skills, or reopen the app before the skill is discoverable.
8. Run a small smoke test by explaining when `$codex-agent-usage` should trigger.

Use plain language when reporting installation status. The user should not need to understand MCP, plugin internals, or skill path conventions to benefit from this.

## Quick start after installation

First, use it to unlock the current Codex setup:

```text
Use $codex-agent-usage to orient this Codex environment. Tell me what tools, skills, plugins, connectors, and artifact capabilities are visible; what is missing for the kind of work I want to do; what needs my approval to enable; and give me a first recommended workflow.
```

Then use it on real work:

```text
Use $codex-agent-usage to turn this folder into a workflow. Check available tools first, identify the source-of-truth files, decide whether we need reviewers or other agents, and give me the first safe next step.
```

For a review-heavy task:

```text
Use $codex-agent-usage to review this package before I rely on it. First verify the tools needed for the file types. Then run separate correctness, risk/readiness, and strategy reviewers with proof, merge their findings, and tell me what to fix first.
```

For a recurring project:

```text
Use $codex-agent-usage to create a short AGENTS.md from my working rules so future Codex sessions know the project goal, source-of-truth files, review process, and safety boundaries.
```

## Suggested prompt for users

Paste this into Codex together with the repository URL:

```text
Please read this repository and install the Codex skill in the `codex-agent-usage/` folder.

Before installing, discover the correct skill directory for this Codex environment. Do not hardcode the install path. If a skill with the same name already exists, ask me before replacing it. After installation, validate it if possible and tell me whether I need to reload Codex or start a new thread.

Then briefly explain when I should use `$codex-agent-usage`.
```

## What the skill does

The skill helps Codex avoid the two common failure modes of a bare setup:

- doing large reviews directly in the main thread until context is exhausted;
- inventing fragile extraction scripts instead of first checking whether proper document, spreadsheet, PDF, presentation, browser, email, calendar, or other plugins/tools are already available.

It teaches Codex to:

- discover installed and available plugins, skills, connectors, MCP servers, and artifact runtimes;
- choose task-local agents for one-off reviews and stable prompt-layered agents only for reusable workflows;
- use document/spreadsheet/PDF/presentation plugins before writing ad hoc parsers;
- explain missing capabilities clearly instead of pretending tools exist;
- propose safe MCP/plugin setup only after checking the local environment;
- create or update project guidance files such as `AGENTS.md` when a folder needs durable instructions;
- preserve main-thread context by delegating broad reading and review passes to agents when available.

## Good first use cases

Ask Codex things like:

```text
Use $codex-agent-usage to review this folder with separate strategy, correctness, and risk reviewers before I present it.
```

```text
Use $codex-agent-usage to check what tools are available for Word, Excel, PDF, and PowerPoint files before starting this analysis.
```

```text
Use $codex-agent-usage to decide whether this task needs temporary agents, a reusable custom agent setup, or only normal chat.
```

```text
Use $codex-agent-usage to create an AGENTS.md file for this project from my plain-language working rules.
```

## Recommended Codex plugins to check first

When the task involves user files, Codex should first check whether the local app already has suitable OpenAI or curated plugins installed, especially:

- **Documents** for `.docx`, Word, and Google Docs-targeted files;
- **Spreadsheets** for `.xlsx`, `.csv`, `.tsv`, and Google Sheets-ready files;
- **PDF** for PDF reading, rendering, extraction, creation, and verification;
- **Presentations** for `.pptx` and Google Slides-targeted decks;
- **Browser / Computer Use** when the task depends on an app or website UI;
- account connectors when the work depends on authorized mail, calendar, code hosting, cloud-drive, or team data;
- domain plugins such as **Data Analytics**, **Sales**, **Investment Banking**, or **Product Design** when the task matches them.

If the required plugin is not installed or visible, Codex should say that plainly and propose the smallest safe setup path.

## Safety boundaries for Codex

When using this skill, Codex should ask before it:

- edits Codex configuration files;
- installs MCP servers, plugins, packages, or command-line tools;
- overwrites an existing skill;
- creates or changes a project-level `AGENTS.md` file;
- uploads private files to external services;
- enables email, calendar, cloud-drive, or account-connected tools.

Codex should also avoid exposing secrets, tokens, private file paths, or raw configuration values in user-facing output.

## Repository layout

```text
codex-agent-usage/
  SKILL.md
  agents/openai.yaml
  references/
  assets/review-prompts/
  assets/workflow-prompts/
```

The skill folder is intentionally self-contained. The repository README is only the installation and orientation wrapper.


## License

This repository is released under the MIT License. See [`LICENSE`](LICENSE).

## Notes for maintainers

This skill is expected to evolve as Codex desktop app, plugins, and skill locations change. Keep the skill's guidance runtime-discovery-first: the model should inspect the actual environment before giving path-specific or version-specific advice.

Before publishing changes:

1. Validate `codex-agent-usage/SKILL.md`.
2. Search for private names, local absolute paths, credentials, and organization-specific implementation details.
3. Keep examples generic enough for public reuse.
4. Prefer improving trigger language in `SKILL.md` over adding long prose to this README.
