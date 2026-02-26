# Cowork Context Kit

A lightweight system for managing Claude's context window in large Cowork folders.

## The Problem

When you point Claude at a folder with dozens (or hundreds) of files, it tries to read everything. It burns through its context window on files that aren't relevant to the task, then runs out of room for the actual work. Output quality drops. Sessions stall.

## The Fix

Three components that work together to make Claude read the right files, in the right order, and ignore the rest.

### 1. Global Instructions

**What:** A set of rules you paste into Cowork's global settings. They tell Claude to apply a tiered access model to every folder — read canonical documents first, load domain-relevant files only when the task needs them, and ignore archival content unless you specifically ask for it.

**Install:** Claude Desktop → Settings → Cowork → Edit (next to Global Instructions) → paste the contents of [`global-instructions.md`](global-instructions.md) → Save.

**Customise:** The file includes placeholder sections for your working style preferences, document formatting defaults, and terminology. Fill these in to match how you work.

### 2. Folder Manifests

**What:** A `_MANIFEST.md` file you drop into any working folder. It tells Claude which documents are the source of truth (Tier 1), which subfolders map to which domains (Tier 2), and what to skip (Tier 3).

**Install:** Copy [`manifest-template.md`](manifest-template.md) into any folder, rename it to `_MANIFEST.md`, and fill it in.

**When to use:** Any folder where you do regular Cowork tasks and there are enough files (roughly 10+) that Claude needs guidance on what to prioritise. Small folders don't need one.

The underscore prefix keeps it sorted to the top of the folder listing.

### 3. Tiered Context Navigator Skill

**What:** A Cowork skill that enforces the orientation protocol — read the manifest, load Tier 1, assess relevance, load Tier 2 selectively. It also handles conflict resolution between files and scopes sub-agents to the minimum context they need.

**Install:**
1. Download or clone this repo
2. Zip the `tiered-context-navigator/` folder
3. Claude Desktop → Settings → Capabilities → Upload skill
4. Upload `tiered-context-navigator.zip`
5. Toggle it on in your Skills list

The skill activates automatically whenever Claude detects a large folder or finds a `_MANIFEST.md` file.

## How They Work Together

1. **Global instructions** set the rules (tiered access, formatting, preferences) across every session
2. **The manifest** in each folder tells Claude exactly which files matter and why
3. **The skill** enforces the protocol so Claude follows it consistently, even in complex multi-step tasks

The result: Claude spends its context window on the files that matter instead of reading everything and running out of room for actual work.

## The Tier Model

| Tier | What | When Claude Reads It |
|------|------|---------------------|
| **Tier 1 — Canonical** | Source-of-truth documents, manifests, master references | Always, before any work |
| **Tier 2 — Domain-Relevant** | Working files in domain subfolders | Only when the task touches that domain |
| **Tier 3 — Archival** | Old versions, drafts, superseded files | Only when you explicitly ask |

## Rollout Suggestion

You don't need to set up everything at once.

**Start here:** Paste the global instructions into Cowork settings. This alone changes Claude's behaviour — it will stop reading everything and start orienting from folder structure first.

**Next:** Add a `_MANIFEST.md` to your busiest folder. See if it changes output quality. If it does, add manifests to other folders as needed.

**Later:** Install the skill for consistent enforcement, especially if you're running complex tasks with sub-agents.

## Maintenance

- Update `_MANIFEST.md` files when you add or retire canonical documents
- The skill will flag when a manifest references files that no longer exist
- Claude won't auto-update manifests — you retain editorial control
- Periodically check that your Tier 1 lists are still accurate; stale references degrade output quality

## Repo Structure

```
cowork-context-kit/
├── README.md                          ← This file
├── global-instructions.md             ← Paste into Cowork global settings
├── manifest-template.md               ← Copy into folders, rename to _MANIFEST.md
└── tiered-context-navigator/
    └── SKILL.md                       ← Zip this folder and upload as a Cowork skill
```

## License

MIT — use it however you like.
