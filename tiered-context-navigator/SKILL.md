---
name: Tiered Context Navigator
description: "Manages context window efficiency when working in large file folders. Automatically applies a three-tier access model: reads canonical/manifest files first (Tier 1), loads domain-relevant files only when the task requires them (Tier 2), and ignores archival content unless explicitly requested (Tier 3). Use whenever working in a folder with more than 10 files, when a _MANIFEST.md file is present, or when the user references tiered access, context management, or near-line storage patterns."
---

# Tiered Context Navigator

## Overview

This skill implements a near-line storage pattern for context window management. Large folders contain more content than is useful to load at once. This skill ensures Claude reads the minimum necessary files to do excellent work, loading additional context progressively as the task demands it.

## Orientation Protocol

When entering any folder, follow this sequence before doing any work:

### Step 1 — Read the Manifest

Look for `_MANIFEST.md` in the folder root. If it exists, read it first. It defines:
- What the folder is for
- Which documents are canonical (Tier 1)
- Which subfolders/files map to which domains (Tier 2)
- What to ignore (Tier 3)

If no `_MANIFEST.md` exists, scan the folder structure (directory listing only — do not open files yet) and infer the tier assignments:
- Files in `_canonical/`, `reference/`, or `source-of-truth/` folders → Tier 1
- Files with "master", "final", "canonical", "approved" in names → Tier 1
- Files in named domain subfolders → Tier 2
- Files in `archive/`, `old/`, `drafts/`, `wip/`, `superseded/` → Tier 3
- Everything else → Tier 2 (read only if task-relevant)

### Step 2 — Read Tier 1 Documents

Open and read all Tier 1 canonical documents. These provide the ground truth for the folder. Hold this context throughout the task.

### Step 3 — Assess Task Relevance

Based on the user's request, identify which Tier 2 domains are relevant. Open only those files. Skip everything else.

### Step 4 — Begin Work

Start the task using Tier 1 context plus relevant Tier 2 files. If you discover mid-task that additional files are needed, load them then — not before.

## Conflict Resolution

When information conflicts between tiers:

1. Tier 1 canonical documents always win unless the user explicitly overrides
2. Between two Tier 2 files, prefer the more recently modified file
3. Between a Tier 2 file and a Tier 3 (archival) file, always prefer Tier 2
4. If genuinely ambiguous, flag it to the user: "I found conflicting information between [File A] and [File B] on [topic]. Which should I treat as authoritative?"

## Sub-Agent Scoping Rules

When decomposing work into parallel sub-agents:

- Each sub-agent receives: the full Tier 1 context + only the Tier 2 files relevant to its specific subtask
- No sub-agent should receive the entire folder contents
- If a sub-agent discovers it needs a file it wasn't given, it should request it rather than scanning the whole folder
- The coordinating agent maintains the manifest context and routes file access

## Manifest Maintenance

- Do not modify `_MANIFEST.md` unless the user explicitly asks
- If you create a new document that should be canonical, suggest to the user that they add it to the manifest
- If you notice the manifest is out of date (e.g., references files that no longer exist), mention it but do not auto-fix

## Context Budget Awareness

When working in very large folders (50+ files):
- After orientation, summarise what you found: "This folder has X files. I've identified Y canonical documents and Z relevant domain files for this task. I'm proceeding with those."
- If a task would require reading more than 20 files, flag this: "This task touches many files. I'll work through them in batches to stay efficient."
- Prefer extracting key information from files over holding entire file contents in context. Read, extract what's needed, and move on.

## Examples

**User asks:** "Draft a proposal for a new client based on our standard approach"
**Correct behaviour:**
1. Read `_MANIFEST.md`
2. Read canonical docs (company positioning, methodology overview)
3. Read only the `proposals/` or `templates/` subfolder for proposal templates
4. Ignore `archive/`, client-specific folders for other clients, internal operations docs

**User asks:** "What does our 2024 strategy document say about pricing?"
**Correct behaviour:**
1. Read `_MANIFEST.md`
2. Note this references a 2024 document — check if it's Tier 3 (archival)
3. Even though it's archival, the user explicitly asked for it — read it
4. Cross-reference with any Tier 1 canonical pricing document if one exists
5. Flag any discrepancies: "The 2024 strategy says X, but the current pricing guide says Y"

**User asks:** "Clean up and reorganise this folder"
**Correct behaviour:**
1. Read `_MANIFEST.md`
2. Do a full directory scan (listing only, not opening files)
3. Propose a reorganisation plan
4. Wait for user approval before moving any files
5. Offer to update `_MANIFEST.md` after reorganisation
