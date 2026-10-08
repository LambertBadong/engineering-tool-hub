# A full 50-part job, start to finish

[← Back to overview](../README.md)

> All job numbers, customer names, part numbers and paths on this page are invented.

## The job

**J20417 – Harbourview Medical.** A 2000 A switchboard with two incoming sources and a key interlock between them, in a floor-standing sheet-metal enclosure with copper bus work. About 50 fabricated parts.

```
Jobs\
└─ J20417 Harbourview Medical\
   └─ Mechanical\
      ├─ CAD\            models, drawings, assemblies
      ├─ PDFs & Flats\   drawing PDFs and flat-pattern DXFs for the shop
      ├─ Assemblies\     assembly drawing PDFs
      ├─ BOM\            bill of materials
      └─ CNC\            punch and laser programs
```

Every tool in the hub starts from the same field. Type `20417`, press Enter, and the folders above are found automatically.

## Stage by stage

| # | Stage | By hand | With the hub |
|---|---|---|---|
| 1 | **Find a reference job.** Search past jobs for the closest match to work from | 25–30 min | 30 s |
| 2 | **Copy and renumber the reference.** Bring the reference parts into the new job under new part numbers | 5–10 min | 10–15 s |
| — | *Design and modelling. Not counted: this is the engineering.* | — | — |
| 3 | **Export flat patterns.** One DXF per sheet-metal part, for CNC | 2–3 min × 50 parts | 3–6 s × 50 parts |
| 4 | **Publish drawings.** One PDF per drawing, into the right folders | 10–15 min | 5–6 min |
| 5 | **Build the BOM.** From the assembly, with stock parts marked and files pulled | 45–90 min | 1–2 min |
| 6 | **Manufacturing packet.** Assemble and print every document in shop order | 45–90 min | ≤ 10 min |
| | **Total overhead** | **~4.5 h** | **~25 min** |

The totals are the measured figures for a 50-part job. The stage-by-stage split is assembled from the per-tool timings, so it adds up to a range around those totals, not to them exactly.

## What each stage looks like now

### 1. Find a reference job

The hub reads the new job's release form (the amperage, the enclosure size, the breaker arrangement) and scores every past job against it. It reads each candidate's electrical drawings to classify how the equipment is arranged, because two jobs with the same amperage can be built completely differently.

The result is a ranked list of the five best candidates, each with a line explaining its score, and the two drawings side by side for a visual check.

<!-- SCREENSHOT SLOT 4: Reference Finder results list, invented jobs only. -->

Before this tool, the search ran through a spreadsheet of thousands of past jobs, a fifth to a third of them with blank entries. On a bad day it took up to an hour, and the job chosen was not always the best one available.

### 2. Copy and renumber

The chosen reference's parts are copied into the new job under new part numbers taken from the designer's own number range. Assemblies are re-linked to the new copies, the bend allowance for copper parts is applied, and each drawing's title block is stamped. The tool refuses any number that is already in use anywhere on the engineering drive.

### 3 and 4. Flat patterns and drawing PDFs

Batch exports. The saving on flat patterns is the largest single item on a big job: at 2–3 minutes each by hand, 50 parts is around two hours of opening, flattening, exporting and closing. After the PDF export, a check confirms that every expected file actually landed.

### 5. The BOM

Covered in detail in [A BOM that checks itself](bom-checks.md).

### 6. The manufacturing packet

The hub gathers every document the shop needs, orders them the way the shop works through them, and sends the whole set to the printer through a single Acrobat session. A simulation mode writes the packet to PDF instead, so it can be reviewed before paper is used.

<!-- SCREENSHOT SLOT 5: Doc Prep & Print, packet list in shop order. -->

## After release

When a part changes after the job is released, the [Revision Updater](revision-updater.md) handles it: 15–20 minutes per part by hand, 10–15 seconds with the hub. A typical job sees one to four such revisions.

## Why the saving grows with job size

Manual effort scales with the number of parts: every part is another file to open, export, check and file. Most of the hub's time is fixed cost, so a 120-part job takes barely longer than a 25-part one. That is why the reduction climbs from 89% on a small job to 94% on a large one.

---

[← A BOM that checks itself](bom-checks.md) · [Back to overview](../README.md) · Next: [Building it with AI →](building-with-ai.md)
