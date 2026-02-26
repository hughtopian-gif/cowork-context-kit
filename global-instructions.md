# Cowork Global Instructions

## Context Management — Tiered File Access

When working in any folder, apply a tiered access model to manage context efficiently. Do not read everything upfront. Instead, orient first, then load progressively.

### Tier 1 — Canonical (Always Read First)

Before doing any work, scan the folder for a `_MANIFEST.md` file or a `_canonical/` subfolder. These contain the authoritative reference documents for that folder. Read these first to orient yourself. If neither exists, look for files with "canonical", "master", "source-of-truth", or "final" in their names.

Tier 1 files define the ground truth. If any other file in the folder contradicts a Tier 1 document, the Tier 1 document takes precedence unless I explicitly say otherwise.

### Tier 2 — Domain-Relevant (Read When Task Requires)

Only read files in subfolders or domains that are directly relevant to the current task. For example, if the task is about brand positioning, read brand-related files but skip pricing models or delivery frameworks unless they're needed.

Use folder names, file names, and any folder instructions as signals for relevance. Do not load files speculatively.

### Tier 3 — Archival (Read Only When Explicitly Asked)

Files in folders named `archive/`, `old/`, `drafts/`, `wip/`, `superseded/`, or files with dates older than 6 months should be treated as archival. Do not read these unless I specifically direct you to them.

If you encounter a file that appears to be a newer version of an archival file, prefer the newer version.

### Freshness Rule

When two or more files cover the same topic, prefer the most recently modified file unless a `_MANIFEST.md` designates a specific file as canonical. If dates are ambiguous, ask me which to use rather than guessing.

### Sub-Agent Scoping

When decomposing a task into sub-agents, scope each sub-agent to the minimum set of files it needs. Do not give every sub-agent access to the entire folder. Pass only the relevant Tier 1 context plus the specific Tier 2 files needed for that sub-agent's task.

## How I Work

<!-- Customise these to match your working style -->
- I prefer narrative prose over bullet points for reports and documents. Use bullets only for genuinely list-shaped content.
- If a task is ambiguous, make your best judgment call and flag the assumption rather than stopping to ask. I'll correct if needed.

## Document Formatting Defaults

<!-- Set your preferred defaults here so Claude uses them consistently -->
<!-- Example:
- Executive Reports: 10.5pt Georgia body, 16pt leading, Arial headings.
- Word Documents: Arial 11pt body. Bold headings with clear hierarchy. US Letter with 1" margins.
-->

## File Naming

When creating new files, use this convention: `YYYY-MM-DD — Description.ext` (with em dash, not hyphen). Keep names descriptive but concise.

## What Not To Do

- Do not read every file in a large folder before starting work. Orient from the manifest and folder structure first.
- Do not create README files or folder documentation unless I ask for it.
- Do not reorganise my folder structure unless I explicitly request it.
- Do not update the `_MANIFEST.md` unless I ask you to or you've created new canonical documents that should be registered there.
