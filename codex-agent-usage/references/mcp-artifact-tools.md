# Real File Tooling and MCP Capability Gate

Use this reference when a folder contains Word, Excel, PDF, slide decks, forms, or similar office artifacts and the current Codex environment does not already expose the needed tools.

This reference is for the agent, not the end user. Be technical here; translate to plain language in user-facing output.

## Core Policy

A missing Office/PDF tool is not an invitation to burn the main context window with custom parsing. It is a capability gate.

This is especially important in Codex-style setups: the environment may be a powerful CLI-backed workspace with only a small default toolbelt. A competing workspace may feel smoother because it preloads artifact tools; Codex can reach the same or better capability, but the agent must recognize the missing tool layer and configure/enable it instead of faking competence.


Order of operations:

1. Inspect visible tools/plugins/connectors.
2. Inspect local MCP config/registry if present.
3. If broad reading is needed, delegate extraction/review to subagents before deep reading in the main thread.
4. Prefer existing specialized artifact tools.
5. If missing, propose a small vetted tool stack with benefits, limits, and risks.
6. Ask before installing, enabling, connecting accounts, or changing local app configuration.
7. If no safe tool exists, give a limited review and state the limitation.

## Capability Map

| Need | Capability to look for | Minimum acceptable behavior |
|---|---|---|
| Excel / XLSX / CSV review | spreadsheet workbook tools or trusted conversion | read sheets/tables/formulas; detect formula errors or state inability |
| Excel editing/model work | spreadsheet workbook tools | preserve formulas, tables, sheets, formatting enough to re-open safely |
| Word / DOCX review | document tools or trusted conversion | read headings, paragraphs, tables, comments if possible |
| Word editing | document tools | preserve document structure and verify render/export when layout matters |
| PDF review | PDF extraction/render tools | extract text by page; render when layout matters |
| Fillable PDF/forms | form-field tools | inspect fields, fill values, validate/flatten output |
| PowerPoint / PPTX review | presentation tools or trusted conversion | inspect slide text, tables, notes, and ideally render slides |
| Cloud docs/sheets | connected cloud tools | use only when source artifacts live in that cloud account |

## Local Config / Registry First

Prefer local evidence over generic web recommendations. Useful sources, when present:

- visible tools/connectors already exposed in the current runtime;
- Codex config, usually `${CODEX_HOME:-~/.codex}/config.toml`, especially `[mcp_servers.*]` blocks;
- local MCP registry tables, manifests, or logs, such as server lists, profile-to-server mappings, tool catalogs, and recent tool-call logs;
- MCP host config for Claude Desktop, Cursor, VS Code, or other local clients.

Interpretation rules:

- server-level enablement wins over tool-level registration; a tool can be registered but unavailable if its server is disabled;
- recent successful calls are stronger evidence than catalog presence;
- do not expose credentials or raw environment values when summarizing config findings;
- if the user already trusts a server in their setup, prefer that local fact over marketplace recommendations;
- if no registry exists, do not stop: inspect config, then offer a narrow install plan.

## Plain Codex Config Installation Path

Codex can often enable MCP servers by editing the user-level `config.toml`. OpenAI Codex docs define this as `~/.codex/config.toml`, with `CODEX_HOME` defaulting to `~/.codex`. Treat this as a persistent local configuration change. On Windows, do not hardcode `C:\Users\<name>\.codex\config.toml`; resolve `~`, `$HOME`, `%USERPROFILE%`, or `CODEX_HOME`, or use the app/IDE command that opens `config.toml`.

Before editing:

0. Locate config, do not guess it:
   - if `CODEX_HOME` is set, use `<CODEX_HOME>/config.toml`;
   - otherwise use `~/.codex/config.toml`;
   - on PowerShell, `~`/`$HOME` normally resolve to the Windows user home;
   - in Codex IDE/app surfaces, prefer the built-in Settings/Open `config.toml` action when available.
1. Explain what capability is missing and why a tool helps.
2. Name the candidate package/server and whether it is official, Microsoft-published, community, local, or cloud/API.
3. State the privacy and filesystem boundary: what files the server may read/write and whether files leave the machine.
4. Verify the package source and current version from its registry or repository.
5. Ask user approval.
6. Back up `config.toml` before writing.
7. Add the smallest working `[mcp_servers.<id>]` block; do not rewrite unrelated config.
8. Tell the user Codex may need restart/reload before the new server appears.

Use placeholders in reusable instructions; verify and pin exact versions at install time.

```toml
# Word / DOCX local community MCP
[mcp_servers.word_document_server]
command = "uvx"
args = ["--from", "office-word-mcp-server==<verified-version>", "word_mcp_server"]

# Excel / XLSX local community MCP
[mcp_servers.excel]
command = "uvx"
args = ["excel-mcp-server==<verified-version>", "stdio"]
```

For PDF tools, installation varies more:

```toml
# Local PDF Tools / PDF Filler, after installing or cloning the server locally
[mcp_servers.pdf_tools]
command = "node"
args = ["<absolute-path-to-pdf-tools>/server/index.js"]

# iLovePDF API MCP, after installing/building/configuring credentials
[mcp_servers.ilovepdf]
command = "node"
args = ["<absolute-path-to-ilovepdf-mcp>/dist/index.js"]
```

Do not write credentials into visible examples. Use environment variables, `.env` files, keychain, or the user's MCP host secret mechanism.

## OpenAI Artifact Plugins First

In Codex App, the Plugin menu may already offer OpenAI-developed artifact plugins. If installed and visible, prefer these before community MCPs for ordinary user-file work. They are closer to the expected Codex artifact workflow and usually include both a skill and a bundled runtime/API.

Common OpenAI artifact plugins:

| Plugin / skill | What it enables | Boundary |
|---|---|---|
| Documents | Create, edit, redline, comment, read/review, render, and visually verify `.docx` / Word / Google Docs-targeted documents. | Strong for DOCX/Word-style artifacts; render QA is required for layout claims. |
| Spreadsheets | Create, edit, inspect, render, verify, and export `.xlsx`, `.xls`, `.csv`, `.tsv`, and Google Sheets-targeted files with formulas, formatting, charts, tables, validation, and recalculation. | Strong for workbook artifacts; still verify formulas and rendered sheets before final delivery. |
| PDF | Read, create, inspect, render, and verify PDFs; extract text with PDF libraries and render pages for layout QA. | Strong for reading/creating/verifying PDFs; form filling, signing, heavy OCR, and advanced transformations may still need PDF Tools/iLovePDF or another dedicated PDF MCP. |
| Presentations | Create, edit, render, verify, and export editable `.pptx` decks with native shapes, text, tables, charts, images, and template-following workflows. | Strong for PowerPoint decks; final slides must be editable unless the user explicitly asks for image-only output. |

Decision rule:

1. If an OpenAI artifact plugin is installed and visible for the file type, use it.
2. If it is not installed but visible in the Plugin menu, tell the user it is the safest first install and ask them to add it or approve installing it.
3. If a domain plugin can cover the workflow better than a file-only tool, use both: e.g. Data Analytics + Spreadsheets, Investment Banking + Presentations/Spreadsheets, Sales + Documents.
4. Use community MCPs when the OpenAI plugin is absent, insufficient for a specific task, or the user needs a specialized capability such as local PDF forms, signature workflows, OCR, or API-level PDF transformations.
5. Do not present community MCP setup as the default if the OpenAI plugin can do the job.

## Community MCP Candidate Stack

These are not universally "safe" just because they are useful. They are candidates to verify, explain, and install only when appropriate.

| Need | Candidate | Benefits | Limits / risks |
|---|---|---|---|
| Word / DOCX | `office-word-mcp-server` | Local DOCX create/read/edit/search/replace/tables/comments/footnotes; avoids manual OOXML hacking. | Community/PyPI package, not Microsoft-published; verify maintenance and pin version. |
| Excel / XLSX | `excel-mcp-server` | Local workbook operations without Microsoft Excel installed: read/write sheets, formulas, charts, pivots, validation. | Community/PyPI package; does not prove Excel desktop recalculation exactly; verify formula behavior. |
| Local PDF forms/content | PDF Tools / PDF Filler | Local-first PDF read/analyze/fill fields/profiles/CSV extraction; good for forms and private docs when folder access is approved. | Community/local tool; permissions matter; visual/layout claims still need render verification. |
| PDF web/API operations | iLovePDF MCP / iLovePDF API | Strong PDF operations: merge, split, compress, OCR, Office-to-PDF, PDF/A, watermark, page numbers, signatures/quota. | Files are uploaded to iLovePDF API; account/quota/credentials required; ask before using private documents. |
| Text-only fallback | Microsoft MarkItDown | Microsoft-published local conversion to Markdown for many Office/PDF formats; good for review/summarization. | Extraction only; not editing, formula fidelity, or layout proof. |

Recommendation logic:

- For private local files and forms, prefer local tools first.
- For heavy PDF transformations or OCR where upload is acceptable, consider iLovePDF.
- For quick text review when fidelity is not required, use MarkItDown or similar conversion.
- For serious Excel modeling, prefer spreadsheet tools; do not rely only on Markdown/text extraction.
- For Word editing, prefer a document MCP/tool; do not patch OOXML by hand unless unavoidable and scoped.

## Microsoft Official / Microsoft-Published Options

Verify current availability before recommending anything. Microsoft 365, Agent 365, and MCP surfaces change quickly; treat this section as a search strategy, not a frozen install recipe.

- **Microsoft MCP catalog**: `https://github.com/microsoft/mcp`  
  First place to check what Microsoft currently lists as official MCP servers. Prefer catalog entries over search-result snippets.

- **Microsoft MarkItDown**: `https://github.com/microsoft/markitdown`  
  Microsoft-published local/read-only fallback for converting PDF, PowerPoint, Word, Excel, images, CSV/JSON/XML, ZIP, and other files into Markdown for LLM consumption. It is not a high-fidelity editor and should not be used to claim visual/layout/formula correctness. It requires Python and format-specific optional dependencies; ask before installing.

- **Microsoft Learn MCP**: `https://learn.microsoft.com/api/mcp`  
  Use for current Microsoft documentation lookup. It does not read the user's local Office files.

- **Microsoft Work IQ / Agent 365 MCP servers**: check the Microsoft MCP catalog and Microsoft Learn. These are Microsoft 365 cloud/tenant tools, not simple local-file tools. They generally require Microsoft 365/Entra setup, user or admin consent, and often a Microsoft 365 Copilot/Agent 365 context. Treat them as enterprise/cloud options, not as the default for a casual Windows machine.

### Current Microsoft Office reality check

As of the last verified pass for this skill, the Microsoft-published landscape looked like this. Re-verify before acting.

| Artifact | Microsoft-published option | Boundary to preserve |
|---|---|---|
| Word | Work IQ Word MCP can create/read/comment on Word docs in OneDrive/SharePoint; MarkItDown can extract text from DOCX. | Work IQ Word is preview/cloud/tenant-oriented, not local Word desktop automation. MarkItDown is extraction, not editing/layout proof. |
| OneDrive/SharePoint files | Work IQ OneDrive/SharePoint MCPs can manage/search/read small files in cloud storage. | Preview/tenant tooling; file operations may have size limits; not for arbitrary local files unless uploaded/linked with consent. |
| Excel | Microsoft Graph Excel REST API and Power Automate Excel Online can read/modify `.xlsx` stored in OneDrive/SharePoint. MarkItDown can extract workbook text/tables. | No official Microsoft local Excel MCP for arbitrary local `.xlsx` was found in the verified pass. Graph/Power Automate are cloud/API workflows, not plug-and-play local review. Do not claim formula fidelity from text extraction. |
| PowerPoint | MarkItDown can extract PPTX text/structure; Microsoft cloud storage APIs can manage files. | No official Microsoft local PowerPoint MCP for arbitrary local `.pptx` was found in the verified pass. Use dedicated presentation tooling when available; otherwise label the review as text/structure-only. |
| PDF | MarkItDown can extract text from many PDFs; dedicated PDF tools may render/extract/fill more accurately. | MarkItDown is not a layout verifier or fillable-form editor. Scanned PDFs/OCR are a separate capability. |

## Third-Party / Community Policy

Treat community Office MCPs as useful but untrusted until verified. Check source, maintenance, security posture, install path, Windows/macOS assumptions, and whether they use COM automation, cloud credentials, or local file access.

Do not put a third-party server into a user setup recommendation just because search results mention Word/Excel/PPTX. Explain why the candidate is appropriate for this task and what it cannot guarantee.

Do not upload client documents to a SaaS converter or unofficial MCP server unless the user explicitly approves the destination and privacy/security tradeoff.

## Safe Fallback Matrix

| Situation | Do |
|---|---|
| Specialized tools visible | Use them; spawn focused reviewers; merge findings. |
| Tools missing but install/config edit is allowed | Propose a minimal MCP stack, explain risks/limits, ask approval, back up config, edit `config.toml`, and verify availability after reload. |
| Only MarkItDown/text conversion available | Run a limited text/structure review; do not claim formula/layout fidelity. |
| Only generic Python libraries available | Use them only for bounded extraction, preferably inside a subagent; do not build a long custom pipeline. |
| Need high-fidelity Office editing but no tools | Stop and explain missing capability; ask before enabling/installing tools. |
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
This folder has files that need proper document/spreadsheet/PDF tools. I can add a small tool to Codex so it can read them properly. The benefit is a cleaner review; the tradeoff is that it changes your local Codex setup, so I will show you what I plan to add before I do it.
```

If tools are missing and no install is allowed:

```text
I can do a limited review from extracted text, but I cannot safely verify formulas, layout, or editable Office structure until the right tools are enabled.
```

If the workflow would otherwise degrade:

```text
I could try to force this with temporary scripts, but that would be slower and less reliable. The cleaner path is to enable the right document/spreadsheet/PDF tool, then run the reviewers.
```

If a tool sends files to an external service:

```text
This option is powerful, but it uploads the PDF to an external service. For private documents I will not use it unless you explicitly approve that tradeoff.
```
