# ⚙️ Engineering Tool Hub

> One desktop app that automates the non-design overhead of every switchgear job — BOMs, revisions, exports, manufacturing packets and printing.

![Status](https://img.shields.io/badge/status-in%20production-2ea44f)
![Version](https://img.shields.io/badge/version-v3.6-blue)
![Tests](https://img.shields.io/badge/tests-400%2B-brightgreen)
![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![SolidWorks API](https://img.shields.io/badge/SolidWorks%20API-DA291C)
![Claude Code](https://img.shields.io/badge/Claude%20Code-D97757?logo=anthropic&logoColor=white)

> **Showcase repository.** The source code is proprietary to my employer and is not published. This repo documents what the tool does and the impact it had.

---

## Impact

Per-job overhead (excluding design & modelling time) cut by **89–94%**:

| Job size | Before | After |
|---|---|---|
| 25 parts | ~3 h | **~20 min** |
| 50 parts | ~4.5 h | **~25 min** |
| 120+ parts | ~9 h | **~30 min** |

Projected **500–1,000 engineer-hours per year** returned across a 4-designer team — with zero new software licences.

## What it is

An in-house Windows desktop app that puts a mechanical design team's entire post-modelling workflow behind **one job number**. Type the job number once; the hub finds the job folders, drives SolidWorks, Excel and Acrobat, and runs the repetitive steps with archive-first guardrails and audits. Built in ~6 months (v1.0 → v3.6, ~47 releases).

## The tools

| # | Tool | What it does | Time saved (before → after) |
|---|---|---|---|
| 1 | **Reference Finder** | Scores and ranks past jobs against a new job's release form; side-by-side drawing comparison | 25–30 min → **30 s** (~55×) |
| 2 | **BOM Filler / Generate** | Builds the BOM straight from the SolidWorks assembly; two-way CAD/PDF/DXF sanity checks; marks stock parts; pulls latest revisions | 45–90 min → **1–2 min** (~45×) |
| 3 | **Doc Prep & Print** | Assembles and prints the full manufacturing packet in shop order through one Acrobat session | 45–90 min → **≤10 min** (~7×) |
| 4 | **Revision Updater** | Watches SolidWorks saves; one click bumps revs, exports PDF/DXF, archives old files, regenerates the BOM, finds affected CNC programs, drafts the programmer email | 15–20 min → **10–15 s** per part (~75×) |
| 5 | **Pack & Go** (macro) | Copies/renumbers components, rewires assemblies, applies copper bend deduction, stamps the title block | 5–10 min → **10–15 s** (~30×) |
| 6 | **Export to DXF** (macro) | Batch flat-pattern DXF export for CNC | 2–3 min → **3–6 s** per part (~30×) |
| 7 | **PDF Publish** (macro) | Batch drawing → PDF export, routed to the right folders, with sanity check | 10–15 min → **5–6 min** (~2×) |
| 8 | **Duplicate Fixer** | Renumbers a duplicated part end-to-end (CAD, flats, CNC, PDFs, BOM, email) with backups first | — |
| 9 | **Mech Parts Tracker** | Next free part number per category, collision checks, gap analysis, duplicate detection | — |
| 10 | **Parts Master** | Searchable library of ~3,500 models / ~75 categories with SolidWorks thumbnails; matches electrical BOMs to library parts | — |
| 11 | **Refresh Files / eDrawings** | Keeps published PDFs/DXFs in sync with CAD; publishes 3D eDrawings for the shop floor | — |
| 12 | **Training · Changelog · Report-a-Bug · Settings** | In-app onboarding prints, release notes, one-click bug reports with logs attached | — |

*Timings are the team proposal's measured before/after figures.*

## How it works

```
             ┌──────────── Engineering Tool Hub (Python / Tkinter) ────────────┐
 Job no. ──► │ Home · Workflow tools · SW Macros · Parts · Training · Settings  │
             └───────┬──────────────┬──────────────┬──────────────┬─────────────┘
                     ▼              ▼              ▼              ▼
             SolidWorks API    Excel (xlwings)   Acrobat COM    File system
             + VBA macros      BOMs              packet print   archive-first
```

- **SolidWorks COM API** — attaches to the running session: BOM generation, PDF/DXF/eDrawings export, reference repair; launches VBA macros from the hub.
- **Guardrails** — archive-first (never overwrites), pre-flight checks, two-way CAD↔BOM checks, final audit, built-in bug reporter.
- **AI** — Claude Code skills mirror the tools from the command line, plus assistants for purchased-parts extraction (two-model cross-check), design Q&A and drawing self-checks.
- **Quality** — 400+ automated tests (pytest); PyInstaller .exe with auto-updating deployment.

## Tech

Python · Tkinter · PyInstaller · SolidWorks API (COM) · VBA · xlwings · Acrobat COM · PyMuPDF · pytest · Claude Code

## Related

- [Engineering Second Brain](https://github.com/LamboProjects/foxfab-engineering-vault) — the Obsidian knowledge base this hub works alongside.

---

Built by **Lambert Badong** · [GitHub](https://github.com/LamboProjects)
