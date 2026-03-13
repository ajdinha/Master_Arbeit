# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

This directory is the working environment for a Master's thesis at Hochschule Ruhr West (HRW), in cooperation with RWE AG.

**Thesis title:** *Self-Healing Test Automation for Black-Box Systems using a Large Language Model*

The primary artifact is `Expose_Masterarbeit.pdf` – the thesis exposé (currently 6 pages).

## PDF Generation

The PDF is generated via a temporary Python script using **ReportLab Platypus**. The workflow:

```bash
# Generate PDF (script is written, executed, then deleted)
python generate_expose.py
# Output: Expose_Masterarbeit.pdf
```

ReportLab is installed globally (`reportlab==4.4.10`, Python 3.14.2).

**Important:** The script is always deleted after execution. To update the PDF, reconstruct the full script from scratch based on the existing PDF content and CLAUDE.md.

### Key ReportLab patterns used

- `SimpleDocTemplate` (A4, 2.5cm margins all sides)
- Layout via Flowables: `Paragraph`, `Table`, `ListFlowable`, `HRFlowable`, `Spacer`, `PageBreak`
- Footer via `onFirstPage` / `onLaterPages` callbacks (page numbers skipped on cover page)
- **No manual Y-positioning** – Platypus handles layout automatically
- Helper functions `td()` and `th()` wrap table cell content in styled `Paragraph` objects
- `make_table()` applies alternating row colors (white / `#EBF0FF`) and blue header automatically

### Known pitfall: German quotation marks

German curly quotes (`„..."`) inside Python string literals cause `SyntaxError: unterminated string literal`. Use ASCII single quotes (`'...'`) inside double-quoted Python strings instead.

### Design tokens

| Element | Font | Size | Color |
|---|---|---|---|
| Cover header block | Helvetica-Bold | 14pt | White on `#1E3C78` |
| H1 sections | Helvetica-Bold | 13pt | `#1E3C78` + HR line below |
| H2 subsections | Helvetica-Bold | 11pt | `#325096` |
| Body text | Helvetica | 11pt | `#1E1E1E`, justified |
| Reference list | Helvetica | 9pt | `#1E1E1E`, left-aligned |
| Table headers | Helvetica-Bold | 10pt | White on `#1E3C78` |
| Table rows | Helvetica | 10pt | Alternating white / `#EBF0FF` |
| Footer | Helvetica | 9pt | `#888888` |

## Document Structure (current 6-page layout)

| Page | Content |
|---|---|
| 1 | Cover page |
| 2 | Kap. 1 Thema & Problemstellung, Kap. 2 Forschungsfragen |
| 3 | Kap. 3 Stand der Forschung (3.1–3.5 incl. Quellenblock) |
| 4 | Kap. 4.1 Experimentelles Design, 4.2 Technischer Aufbau |
| 5 | Kap. 4.3 Evaluationsrahmen (4.3.1–4.3.5) |
| 6 | Kap. 5 Zeitplanung |

## Thesis Context

### Research questions

- **FF1:** Can Claude Code generate usable frontend tests for RWE.com without Sitecore backend context (black-box only: DOM, CSS, Network)?
- **FF2:** Can Claude Code reliably detect application changes via test failures and distinguish them from false positives?
- **FF3:** Can Claude Code autonomously repair failing tests after a detected change (self-healing)?
- **FF4:** Can Claude Code identify new Sitecore components from DOM/CSS alone and generate tests for them?

### Technical stack (thesis experiments)

- **LLM agent:** Claude Code – accesses browser DevTools output only (DOM snapshots, CSS, network logs, screenshots; no Sitecore source code)
- **Test framework:** Playwright (TypeScript), existing RWE infrastructure
- **Test environment:** RWE Sitecore Staging (real deployments for change-detection experiments)
- **Baseline:** Manually written Playwright tests by RWE engineers (used as gold standard for FF1/FF4)

### Evaluation methodology (decided, not to be changed without discussion)

| FF | Primary metric | Method |
|---|---|---|
| FF1 | Task-Completion-Rate | Concrete test tasks per component; compare with expert gold standard + time |
| FF2 | Precision / Recall / F1 | Confusion matrix over 3 classes: Defect / Change / False Positive |
| FF3 | Healing Success Rate + iterations | Manual verification to exclude False Greens |
| FF4 | Expert review rubric (0–3 per criterion) | 4 criteria: component ID, coverage, selector robustness, hallucination-free |

Sample size: **15–30 components / change scenarios** (enables descriptive statistics).

FF2 change scenarios: **combined** – controlled injected changes + real RWE Staging deployments.

Threats to validity are explicitly addressed in section 4.3.5 (internal / external / construct validity).

### Literature cited in Kapitel 3 (all verified on arXiv)

All 8 citations are confirmed arXiv papers – IDs directly checkable at arxiv.org/abs/<ID>:

| # | Reference | arXiv ID |
|---|---|---|
| [1] | Romano et al. (2021) – *An Empirical Analysis of UI-based Flaky Tests* | 2103.02669 |
| [2] | Liu et al. (2024) – *WEFix: Intelligent Automatic Generation of Explicit Waits for Efficient Web End-to-End Flaky Tests* | 2402.09745 |
| [3] | Brisset et al. (2021) – *Erratum: Leveraging Flexible Tree Matching to Repair Broken Locators in Web Automation Scripts* | 2106.04916 |
| [4] | Ricca, Marchetto, Stocco (2024) – *A Multi-Year Grey Literature Review on AI-assisted Test Automation* | 2408.06224 |
| [5] | Schäfer et al. (2023) – *An Empirical Evaluation of Using Large Language Models for Automated Unit Test Generation* | 2302.06527 |
| [6] | Kang, Yoon, Yoo (2023) – *Large Language Models are Few-shot Testers: Exploring LLM-based General Bug Reproduction* | 2209.11515 |
| [7] | Yuan et al. (2023) – *No More Manual Tests? Evaluating and Improving ChatGPT for Unit Test Generation* | 2305.04207 |
| [8] | Yang et al. (2024) – *On the Evaluation of Large Language Models in Unit Test Generation* | 2406.18181 |
