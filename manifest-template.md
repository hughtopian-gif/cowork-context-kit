# _MANIFEST.md — Folder Context Guide

> This file tells Claude which documents matter most in this folder.
> Update it when canonical documents change. Claude reads this first.

## Purpose of This Folder

[One sentence describing what this folder contains and what work happens here.]

## Canonical Documents (Tier 1 — Always Read)

These are the source-of-truth documents for this folder. Claude should read these before doing any work.

| Document | What It Covers | Last Updated |
|----------|---------------|--------------|
| `example-document.docx` | Description of what this covers | 2026-02-25 |
| `another-reference.pdf` | Description of what this covers | 2026-02-20 |

## Domain References (Tier 2 — Read When Relevant)

These are supporting documents Claude should only read when the task touches their domain.

| Subfolder or File | Domain | When to Read |
|-------------------|--------|-------------|
| `pricing/` | Pricing and commercial models | When task involves pricing, proposals, or commercial terms |
| `delivery/` | Delivery methodology | When task involves client delivery, methodology, or frameworks |
| `brand/` | Brand and messaging | When task involves external communications, content, or positioning |

## Archival / Superseded (Tier 3 — Ignore Unless Asked)

| File or Folder | Why It's Archived | Superseded By |
|---------------|-------------------|---------------|
| `old/` | Historical versions | Current canonical docs above |
| `drafts/` | Working drafts | Final versions in canonical list |

## Freshness Notes

[Any specific rules about which version of what takes precedence. For example:]
- Brand positioning: `brand-guide-v3.docx` supersedes all earlier brand files
- Revenue targets: Always use the current-year business plan, not earlier projections

## Folder-Specific Instructions

[Any rules specific to work in this folder that go beyond the global instructions. For example:]
- All documents created in this folder should use the company letterhead template
- Client names should be anonymised in any draft content
