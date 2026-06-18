# Codex Workflow Orchestration Skill

This repository contains a Codex skill that helps Codex use the Codex Desktop / Codex CLI environment more effectively: plugins, skills, MCP servers, artifact tools, project instructions, and task-local agents.

It is written primarily **for Codex to read and install**, not as a long manual for end users. The README is the sharing wrapper: give people this link, tell Codex to install the skill folder, then use `$codex-agent-usage` to move from browser-chat guessing to tool-backed workflow.

## Why this exists

A common failure pattern is asking a plain browser chat to produce a whole business plan, financial model, document package, or review in one shot, then judging the model because it invents numbers or loses consistency. That comparison is structurally unfair: the chat tab usually has no source-of-truth spreadsheet, no local file access, no artifact render/verification loop, and no focused reviewers.

This skill teaches Codex to change the working method before judging the model:

- discover what the current Codex setup can actually do;
- find or ask for document, spreadsheet, PDF, presentation, browser, and connector tools when the task needs them;
- keep calculations in tool-backed sources of truth, not in prose;
- split review into focused agents with proof;
- merge findings into an action plan and fix source files before regenerating outputs.

The point is not vendor tribalism. The point is that **chat-in-a-tab** and **workspace-with-tools-and-agents** are different operating environments.

## What this skill is not

- It is not a business-plan generator. Business plans are only a good demo because calculation, evidence, narrative, and risk all collide.
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

Use prompts like:

```text
Use $codex-agent-usage to review this business plan folder before I rely on it. First check whether Codex has the right tools for documents, spreadsheets, PDFs, and slides. Then run separate correctness, risk/readiness, and strategy reviewers with proof, merge their findings, and tell me what to fix first.
```

For a non-technical audience, explain it simply: the AI is not being trusted because it sounds confident; it is being put into a workflow with tools, evidence, review, and correction.

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
- **Browser / Chrome / Computer Use** when the task depends on an app or website UI;
- **Gmail / Google Calendar / GitHub / Notion / Slack / Drive** when the work depends on connected account data;
- domain plugins such as **Data Analytics**, **Sales**, **Investment Banking**, or **Product Design** when the task matches them.

If the required plugin is not installed or visible, Codex should say that plainly and propose the smallest safe setup path.

## Safety boundaries for Codex

When using this skill, Codex should ask before it:

- edits Codex configuration files;
- installs MCP servers, plugins, packages, or CLIs;
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
```

The skill folder is intentionally self-contained. The repository README is only the installation and orientation wrapper.


## License

This repository is released under the MIT License. See [`LICENSE`](LICENSE).

## Notes for maintainers

This skill is expected to evolve as Codex Desktop, Codex CLI, plugins, and skill locations change. Keep the skill's guidance runtime-discovery-first: the model should inspect the actual environment before giving path-specific or version-specific advice.

Before publishing changes:

1. Validate `codex-agent-usage/SKILL.md`.
2. Search for private names, local absolute paths, credentials, and organization-specific implementation details.
3. Keep examples generic enough for public reuse.
4. Prefer improving trigger language in `SKILL.md` over adding long prose to this README.
