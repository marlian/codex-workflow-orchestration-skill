# Real File Tooling and MCP Capability Gate

Use this reference when a folder contains Word-style documents, spreadsheets, PDFs, slide decks, forms, or similar office artifacts and the current Codex desktop app does not already expose the needed tools.

This reference is for the agent, not the end user. Be technical here; translate to plain language in user-facing output.

## Core Policy

A missing Office/PDF tool is not an invitation to burn the main context window with custom parsing. It is a capability gate.

In the Codex desktop app, the right first move is to inspect the visible app capabilities: installed plugins, skills, connectors, MCP servers, and current folder access. If the needed artifact runtime is missing, say that plainly and propose the smallest safe setup path.

Order of operations:

1. Inspect visible Codex tools/plugins/connectors.
2. Inspect Codex MCP config/registry if present and relevant.
3. Prefer installed Codex artifact plugins for document, spreadsheet, PDF, and presentation work.
4. If broad reading is needed, delegate extraction/review to subagents before deep reading in the main thread.
5. If missing, propose a small vetted tool stack with benefits, limits, and risks.
6. Ask before installing, enabling, connecting accounts, or changing local app configuration.
7. If no safe tool exists, give a limited review and state the limitation.

## Capability Map

| Need | Capability to look for | Minimum acceptable behavior |
|---|---|---|
| Spreadsheet review | spreadsheet workbook tools or trusted conversion | read sheets/tables/formulas; detect formula errors or state inability |
| Spreadsheet editing/model work | spreadsheet workbook tools | preserve formulas, tables, sheets, formatting enough to re-open safely |
| Document review | document tools or trusted conversion | read headings, paragraphs, tables, comments if possible |
| Document editing | document tools | preserve document structure and verify render/export when layout matters |
| PDF review | PDF extraction/render tools | extract text by page; render when layout matters |
| Fillable PDF/forms | form-field tools | inspect fields, fill values, validate/flatten output |
| Slide deck review | presentation tools or trusted conversion | inspect slide text, tables, notes, and ideally render slides |
| Cloud-hosted docs/sheets | authorized connector tools | use only when source artifacts live in that account and the user approved access |

## Codex Config / Registry First

Prefer local Codex evidence over generic web recommendations. Useful sources, when present:

- visible tools/connectors already exposed in the current Codex runtime;
- Codex config, usually `${CODEX_HOME:-~/.codex}/config.toml`, especially `[mcp_servers.*]` blocks;
- local MCP registry tables, manifests, or logs, such as server lists, profile-to-server mappings, tool catalogs, and recent tool-call logs.

Interpretation rules:

- server-level enablement wins over tool-level registration; a tool can be registered but unavailable if its server is disabled;
- recent successful calls are stronger evidence than catalog presence;
- do not expose credentials or raw environment values when summarizing config findings;
- if the user already trusts a server in their Codex setup, prefer that local fact over marketplace recommendations;
- if no registry exists, do not stop: inspect config, then offer a narrow install plan.

## Plain Codex Config Installation Path

Codex can often enable MCP servers by editing the user-level `config.toml`. Treat this as a persistent local configuration change. Resolve the config path from `CODEX_HOME` when set, otherwise `~/.codex/config.toml`; do not hardcode platform-specific user paths.

Before editing:

1. Locate config; do not guess it.
2. Explain what capability is missing and why a tool helps.
3. Name the candidate package/server and whether it is official, curated, community, local, or cloud/API.
4. State the privacy and filesystem boundary: what files the server may read/write and whether files leave the machine.
5. Verify the package source and current version from its registry or repository.
6. Ask user approval.
7. Back up `config.toml` before writing.
8. Add the smallest working `[mcp_servers.<id>]` block; do not rewrite unrelated config.
9. Tell the user Codex may need restart/reload before the new server appears.

Use placeholders in reusable instructions; verify and pin exact versions at install time.

```toml
# Local document/spreadsheet/PDF artifact MCP, after verifying the exact package/server.
[mcp_servers.artifact_tools]
command = "<verified-command>"
args = ["<verified-arg-1>", "<verified-arg-2>"]
```

Do not write credentials into visible examples. Use environment variables, `.env` files, keychain, or the user's MCP host secret mechanism.

## Codex Artifact Plugins First

In the Codex desktop app, the Plugin menu may already offer artifact plugins. If installed and visible, prefer these before community MCPs for ordinary user-file work. They are closer to the expected Codex artifact workflow and usually include both a skill and a bundled runtime/API.

Common artifact plugin categories:

| Plugin category | What it enables | Boundary |
|---|---|---|
| Documents | Create, edit, review, render, and visually verify document files. | Strong for document artifacts; render QA is required for layout claims. |
| Spreadsheets | Create, edit, inspect, render, verify, and export workbooks/CSVs with formulas, formatting, charts, validation, and recalculation. | Strong for workbook artifacts; still verify formulas and rendered sheets before final delivery. |
| PDF | Read, create, inspect, render, and verify PDFs; extract text and render pages for layout QA. | Strong for ordinary PDF work; forms, signing, OCR, and advanced transformations may need a dedicated PDF capability. |
| Presentations | Create, edit, render, verify, and export editable slide decks. | Strong for decks; final slides must be editable unless the user explicitly asks for image-only output. |

Decision rule:

1. If a Codex artifact plugin is installed and visible for the file type, use it.
2. If it is visible but not installed, tell the user it is the safest first install and ask them to add it or approve installing it.
3. If a domain plugin can cover the workflow better than a file-only tool, use both the domain workflow and artifact tools.
4. Use community MCPs only when the Codex plugin is absent, insufficient for a specific task, or the user needs a specialized capability.
5. Do not present community MCP setup as the default if a Codex artifact plugin can do the job.

## Community / External Tool Policy

Treat community and external artifact tools as useful but untrusted until verified. Check source, maintenance, security posture, install path, platform assumptions, and whether they use cloud credentials or local file access.

Do not put an external server into a user setup recommendation just because search results mention the file type. Explain why the candidate is appropriate for this task and what it cannot guarantee.

Do not upload client documents to a cloud converter or external API unless the user explicitly approves the destination and privacy/security tradeoff.

Recommendation logic:

- For private local files and forms, prefer local tools first.
- For heavy transformations or OCR where upload is acceptable, consider a cloud/API tool only after explicit approval.
- For quick text review when fidelity is not required, use a trusted conversion tool if visible.
- For serious spreadsheet modeling, prefer spreadsheet tools; do not rely only on Markdown/text extraction.
- For document editing, prefer a document tool; do not patch archive/XML internals by hand unless unavoidable and scoped.

## Safe Fallback Matrix

| Situation | Do |
|---|---|
| Specialized tools visible | Use them; spawn focused reviewers; merge findings. |
| Tools missing but install/config edit is allowed | Propose a minimal MCP stack, explain risks/limits, ask approval, back up config, edit `config.toml`, and verify availability after reload. |
| Only text conversion available | Run a limited text/structure review; do not claim formula/layout fidelity. |
| Only generic scripting libraries available | Use them only for bounded extraction, preferably inside a subagent; do not build a long custom pipeline. |
| Need high-fidelity editing but no tools | Stop and explain missing capability; ask before enabling/installing tools. |
| Need broad package review but no subagents | Run sequential role passes with strict scope and warn that this is a fallback. |
| Need scanned PDF/OCR/layout recovery | Ask first; this is a separate extraction/OCR task, not a normal review. |

## Hard Stops

Stop and ask/explain instead of continuing when:

- the plan requires writing a custom Office/PDF pipeline in the main thread;
- the plan requires converting many PDFs to page images just to inspect ordinary text;
- the task needs formula/layout/edit fidelity but only text extraction is available;
- the only available solution is a community MCP or cloud converter that would receive private files;
- installing a tool requires credentials, admin rights, or persistent config changes and the user has not approved.

## User-Facing Language

If tools are available:

```text
I found the document/spreadsheet/PDF tools, so I can inspect the real files rather than guessing from filenames.
```

If tools are missing but install is reasonable:

```text
This folder has files that need proper document/spreadsheet/PDF tools. I can add a small capability to Codex so it can read them properly. The benefit is a cleaner review; the tradeoff is that it changes your local Codex setup, so I will show you what I plan to add before I do it.
```

If tools are missing and no install is allowed:

```text
I can do a limited review from extracted text, but I cannot safely verify formulas, layout, or editable artifact structure until the right tools are enabled.
```

If the workflow would otherwise degrade:

```text
I could try to force this with temporary scripts, but that would be slower and less reliable. The cleaner path is to enable the right document/spreadsheet/PDF tool, then run the reviewers.
```

If a tool sends files to an external service:

```text
This option is powerful, but it uploads the file to an external service. For private documents I will not use it unless you explicitly approve that tradeoff.
```
