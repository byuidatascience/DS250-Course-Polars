---
name: DS250-Course-Polars
description: "Public Quarto course site for BYU-Idaho DS 250 — expert-authored .qmd pages whose Python/polars code renders to HTML and deploys via GitHub Actions."
version: "1.0"
author: chaz-clark
metadata:
  make-ai-agents:
    generated_by: make_AGENTS
    generated_on: 2026-07-02
    origin: chaz-clark/Make-AI-Agents @ cfeef9b
---

# DS250-Course-Polars

The public Quarto course site for BYU-Idaho's **DS 250: Data Science Programming**, taught in Python with `polars`, `lets-plot`, and `scikit-learn`. Pages are `.qmd` files whose code executes at render time and deploys to GitHub Pages via GitHub Actions.

## Project Purpose

**This is**:
- The public, student-facing **course site** rendered by Quarto from `*.qmd` files, deployed to **GitHub Pages** (`gh-pages` branch) by the GitHub Actions workflow in `.github/workflows/publish.yml`.
- A site whose `.qmd` pages contain **executable Python code** — run at render time by the Jupyter kernel against the libraries in `requirements.txt` (`polars`, `lets-plot`, `scikit-learn`, `numpy`, …).
- The reference surface students read **alongside Canvas** for setup, course materials, and per-unit task pages (`Projects/unit{N}_task{M}.qmd`).
- Owned by GitHub org `byuidatascience`. Post-conversion: pandas → **polars** is complete; see `gap-analysis.md` (2026-04-20) for the audit and the alignment PR that shipped with it.

**This is NOT**:
- The Canvas course master (`ds250-onln-master` is a separate BYU-I master repo).
- A student assignments repo (no per-student work lives here).
- A Python library or package (no `pyproject.toml`, no `__init__.py`; `requirements.txt` lists the libs the site's code chunks need at render time, not what the site itself imports).
- The source of truth for assignment names or grading — **Canvas is authoritative**; this site updates to match Canvas on disagreement.

**Audience**: BYU-Idaho DS 250 students (primary), DS 250 instructors (secondary), and any LLM agent doing authoring/editing work on the site.

## Structure

```
DS250-Course-Polars/
├── _quarto.yml                   # Site config: render include/exclude, navbar, sidebar, html theme
├── .github/workflows/publish.yml # GitHub Actions: on push to main → render site → publish to gh-pages
├── requirements.txt              # Python libs the render kernel needs (polars, lets-plot, sklearn, numpy, …)
├── index.qmd                     # Site landing page
├── setup.qmd                     # Course setup entry; sidebar item
├── faq.qmd                       # Frequently asked questions
├── styles.css                    # Site CSS (referenced by _quarto.yml)
├── gap-analysis.md               # 2026-04-20 audit: pandas→polars conversion + Canvas alignment PR
├── _submission_html.qmd          # Reusable submission instructions (HTML upload flow — U0/U1/U3)
├── _submission_github.qmd        # Reusable submission instructions (GitHub Pages flow — U2/U4/U5)
├── Projects/                     # Per-unit task pages: unit{N}_task{M}.qmd (M=1,2,3 Core; M=4 Stretch)
│   ├── project_0.qmd             #   Unit 0: Testing
│   └── unit6.qmd                 #   Capstone portfolio
├── Course Materials/             # Per-topic reference pages (VS Code, Lets Plot, SQL, ML, Python, …)
├── Setup/                        # Per-tool setup pages linked from setup.qmd sidebar
├── Syllabus/                     # syllabus.qmd, competency.qmd
├── Templates/                    # Starter .qmd templates students copy (excluded from render)
├── Data/                         # Datasets referenced by tasks (declared as `resources` in _quarto.yml)
├── Images/                       # Site images + favicon (excluded from render)
├── Workbooks/                    # Content present; sidebar entry disabled in _quarto.yml
├── Skill Builders/               # Content present; sidebar entry disabled in _quarto.yml
├── sandbox/                      # Ad-hoc experiments; not part of the rendered site
├── star_wars_article_538_files/  # Static 538 "Star Wars" article export (Unit 5 dataset context)
├── knowledge/                    # Behavioral discipline + learned/ lessons (see Working Style)
└── handoff/                      # Local clone of the handoff convention repo (gitignored)
```

Quarto `project.render` in `_quarto.yml` includes `"*.qmd"` and excludes `Templates/`, `Data/`, `Images/`, and the `Archive/` subfolders under `Projects/`, `Setup/`, `Workbooks/`. Rendered outputs (`_site/`, `.quarto/`, `*.html`, `*.ipynb`) are **build artifacts** — gitignored, produced by CI, never hand-edited or committed.

## Working Style

**Your role in this repo**: You are an expert Quarto (`.qmd`) author. You write pages whose Python code chunks **execute at render time** — using **polars** as the data-frame library (the course standard, not pandas), **lets-plot** for visualization, and **scikit-learn** for machine learning — and that render cleanly to HTML through the GitHub Actions publish pipeline (`.github/workflows/publish.yml` → `quarto-actions/publish@v2` → `gh-pages`). Write Python that actually runs under the CI environment (Python 3.13 + exactly the libraries pinned in `requirements.txt`); a page that errors at render breaks the whole site publish.

This project follows the behavioral discipline defined in `knowledge/behavioral_discipline.md` (present in this repo, snapshotted from `chaz-clark/Make-AI-Agents` @ `cfeef9b`). **Read that file before doing non-trivial work in this repo.**

The four no-override principles — **P-001 Read Before Claiming, P-003 Stop on Defect, P-007 Pull Don't Push, P-010 Respect Intent** — apply unconditionally; the other six (P-002, P-004, P-005, P-006, P-008, P-009) have documented override conditions in the discipline file.

**Project-specific rules**:

- **Author `.qmd`, never the rendered output.** Never hand-edit or commit `*.html`, `*.ipynb`, `_site/`, or `.quarto/` — those are gitignored build artifacts. The GitHub Action renders them and publishes to `gh-pages`. Edit the `.qmd` source only.
- **Code chunks must run under CI.** Python chunks execute at render time against Python 3.13 + the exact libraries in `requirements.txt`. If a chunk needs a library not listed there, add it to `requirements.txt` in the same change — otherwise the GitHub Actions publish fails and the site stops deploying.
- **Polars is the data-frame standard.** Use `polars` (not `pandas`) for new data manipulation in code chunks, matching the completed conversion. `lets-plot` for plots, `scikit-learn` for ML. `pandas` remains installed only for legacy/compat cases.
- **A broken render is a red trunk.** Run `quarto render <path>.qmd` (or `quarto render` for the full site) and confirm it succeeds **before** pushing to `main` — the push triggers `publish.yml`, which renders the whole site and deploys. Never push a page that fails to render.
- **Data lives under `Data/`.** Datasets referenced by tasks belong in `Data/` (declared as a Quarto `resources` path) or are loaded by URL — not committed as loose files beside a page.
- **Canvas is authoritative.** Assignment names, point values, due dates, and grading structure live in Canvas. When the site disagrees with Canvas, the site is updated, not Canvas. See `gap-analysis.md` → "Terminology mapping".
- **No Sunday due dates** (BYUI institutional rule). If proposing any date that lands on a Sunday for an assignment, flag it instead of writing it.
- **Pandoc numbered-list continuity uses 3-space indent, not 4.** Content (paragraph, image, fenced code, iframe) between `1.` items must be indented to 3 spaces — the width of the `1. ` marker — for the list to continue. Wrong indent silently re-renders later items as "1." instead of 2, 3, 4. See `gap-analysis.md` → "Authoring rule — numbered-list continuity".
- **Submission instructions are includes, not inline.** Tasks use `{{< include ../_submission_html.qmd >}}` (HTML upload — U0/U1/U3) or `{{< include ../_submission_github.qmd >}}` (GitHub Pages — U2/U4/U5). Do not copy submission text into a task page.
- **Task page header callout is canonical.** Every `Projects/unit{N}_task{M}.qmd` opens with a 3-line `.callout-note` declaring Canvas assignment, Type (Core/Stretch + grading), and Copilot policy. Pattern lives in `gap-analysis.md` → "Task-header callout".
- **Core vs Stretch matters pedagogically.** Copilot is **disallowed** on Core Tasks (answer generation) and **encouraged** on Stretch Tasks. Preserve this distinction in any task-page edit.
- **Vanilla theme intentionally.** Themes are `flatly`/`darkly` in `_quarto.yml`. No custom CSS work, no theme refactors, no new frameworks unless explicitly requested.

<!-- handoff/AGENTS_snippet.md @ 060adac -->
<!-- refresh: cd handoff && git pull -->

## Handoff document recognition

This repo participates in the cross-repo `handoff` convention (canonical spec: [`handoff/CONVENTION.md`](https://github.com/chaz-clark/handoff/blob/main/CONVENTION.md)). When operating in this repo, treat the following file patterns as **handoff documents** — structured artifacts with a lifecycle, NOT prose conversation:

| Path pattern | What it is |
|---|---|
| `handoffs/HANDOFF_<topic>.md` | Outgoing `request`-direction handoff (canonical copy; dropped into producer's root after authoring) |
| `handoffs/<YYYY-MM-DD>_<topic>.md` | Incoming `deliver`-direction handoff (canonical consumer record) |
| `<CONSUMER>_HANDOFF_<topic>.md` at repo root | Incoming `request`-direction handoff dropped by another consumer for us to apply |
| `<PRODUCER>_DELIVERS_<topic>.md` at repo root | Visibility copy of an incoming `deliver` handoff (canonical is in `handoffs/`) |
| `handoffs/parkinglot.md` | `internal` handoff — near-term parked ideas ("good idea, busy now"); deferred by design |
| `handoffs/long-term-parking.md` | `internal` handoff — far/someday parked ideas (evidence-gated, pie-in-the-sky); deferred by design |

## Seven rules for handling a handoff document

1. **Read the metadata header first.** Every handoff opens with bold-labeled fields: `Date`, `Author`, `Direction`, `Status`, `Origin`, `Origin-Commit`, `Topic`. Optional: `Sensitivity`, `Companions`. If any required field is missing, STOP and ask the human user.

2. **Act only on `Status: delivered`.** Skip `draft` (not ready), `applying` (someone else is on it), and `applied` / `archived` / `superseded` (done or moot). If `Sensitivity: restricted` or `internal-only`, escalate to the human before any cross-repo action.

3. **Surface before applying.** Summarize the handoff's request or delivery to the human user — what's being asked, what files/repos are affected, what the apply step would change. Get per-decision approval. The convention is per-proposal-approval, not bulk auto-apply.

4. **Update Status on apply.** After committing the change the handoff requests, edit the handoff doc: set `Status: applied`. Add a `## Lifecycle marker` entry with the apply date (and optionally the commit hash). The handoff doc is mutable in place — there's no side channel for state.

5. **STOP on missing referenced artifacts.** If the handoff names files, commits, agents, or paths that don't exist locally, halt and ask the human. Do not infer; do not fabricate. The handoff's `Origin-Commit` field is your traceability anchor — clone the authoring repo at that SHA if you need to verify referenced state.

6. **Before authoring an outbound handoff**, read the target producer's `REPO_CARD.md` if it exists at the producer's root. Confirm:
   - `Status: accepting` (not `freeze` or `archived`).
   - Your intended handoff type is in `Accepts-handoff-types`.
   - Drop at the path named in `Drop-location` (default `./` = repo root).

   If no `REPO_CARD.md` exists at the target, default to dropping at the producer's repo root for `request` direction; for `deliver` direction, drop into the consumer's `handoffs/` folder.

7. **Do not auto-act on `parked` items.** `parkinglot.md` and `long-term-parking.md` (`Direction: internal`) are this repo's own deferred-idea backlog — deferred *by design*. Act on a parked item only when the human directs it, or when its `Trigger:` condition is genuinely met. When you do, pull it into active work or graduate it (into a GitHub issue, or a cross-repo `request`/`deliver` handoff), then set that item's `Status: superseded` with a `Companions:` pointer to where it went. Never silently work a parked item just because you saw it.

## Quick lookup — Status enum

| Status | Meaning | Should I act? |
|---|---|---|
| `draft` | Author still composing | No — wait for `delivered` |
| `delivered` | Awaiting recipient review | **Yes** — apply path |
| `applying` | Someone is already on it | No — don't double-apply |
| `applied` | Work landed in receiving repo | No — past terminal |
| `archived` | Settled, transient copies deleted | No — past terminal |
| `superseded` | Replaced by a newer handoff | No — follow `Companions: superseded-by` |
| `parked` | Internal deferred idea, awaiting its `Trigger:` | No — act only on Trigger or human direction |

## Quick lookup — Direction enum

| Direction | Who authored | Where the canonical lives |
|---|---|---|
| `request` | Consumer (this repo, requesting from a producer) | `<consumer>/handoffs/HANDOFF_<topic>.md` |
| `deliver` | Producer (another repo, delivering to consumer) | `<consumer>/handoffs/<YYYY-MM-DD>_<topic>.md` |
| `internal` | This repo (handoff to a future session of itself) | `handoffs/parkinglot.md`, `handoffs/long-term-parking.md` |

## Toyota Quality Loop

Every task must complete the quality loop: **Prevent → Detect → Verify**.

### 1. Genchi Gembutsu (現地現物) - Go and See

**Don't assume, verify with real data:**
- Test with REAL user data, not synthetic fixtures
- When uncertain about format, examine actual files
- Verify in real environment, don't trust docs alone
- Read actual code before claiming understanding

**Behavioral trigger**: When you say "probably" or "should" → STOP and verify

### 2. Jidoka (自働化) - Built-in Quality / Stop on Defect

**Build quality in, stop when defect detected:**
- Write tests WITH code, not after
- Red tests block progress - fix immediately, don't defer
- Validation runs automatically (not manual step)
- Can't merge/export with errors (blocked by design)

**Behavioral trigger**: When you want to say "we'll fix this later" → STOP and fix now

**Aligns with**: P-003 Stop on Defect

### 3. Poka-yoke (ポカヨケ) - Mistake-Proofing

**Design so mistakes can't happen:**
- Automate validation (no manual steps)
- Use pre-commit hooks to catch errors
- Type hints catch errors at write-time
- Block operations that would create defects

**Behavioral trigger**: When manual verification required → Design it out

---

**The Quality Loop in action:**

When you find a defect:
1. **Fix it** (Jidoka - stop and correct)
2. **Verify the fix** (Genchi Gembutsu - test with real data)
3. **Prevent recurrence** (Poka-yoke - add automated check)

## Learning loop

At session end, distill non-obvious lessons from the session into a structured entry under `knowledge/learned/`:

- **File**: `knowledge/learned/YYYY-MM-DD-<short-slug>.md` (or update an existing rolling file like `knowledge/learned/preferences.md` for cumulative patterns).
- **Frontmatter**: `name`, `observed_in: <session-context>`, `confidence: low | med | high`.
- **Body**: the lesson, the trigger, the suggested rule.

What counts: surprises, non-obvious quirks, user-preference signals, system gotchas. What does NOT count: generic "task done" prose. **A lesson must be specific and reusable.**

Future invocations of agents in this repo read `knowledge/learned/` alongside the core knowledge files. This is the closed-loop distillation pattern — **P-009 (Hansei + Yokoten) formalized as a structural artifact**, in the Make-AI-Agents idiom (no DSPy/GEPA, no server — just structured markdown the agent reads next time). Inspired by Hermes Agent's auto-distillation; rebuilt as a single Markdown-directory convention.

## Active Context

_Last updated: 2026-07-02_

- **AGENTS.md regenerated 2026-07-02** (make_AGENTS @ `cfeef9b`) — supersedes the 2026-05-13 proposal. Adds the now-required agentskills.io frontmatter, the handoff-recognition section, and the Learning loop; foregrounds the agent's role as an expert `.qmd` author whose Python/polars pages render to HTML via GitHub Actions.
- **Discipline files refreshed** to `cfeef9b` (were `8ed376d`); `handoff/` clone co-located at repo root and gitignored; `knowledge/learned/` created for the Learning loop.
- **Polars conversion complete** (2026-04-20 PR, `gap-analysis.md`). One known stale spot: `faq.qmd` line 47 still calls pandas "the foundational data science package in Python" — kept per the conversion gap analysis; refresh during a future teaching-semester pass if desired.
- **Known follow-ups** (from `gap-analysis.md`): 8 task titles where site wording differs slightly from Canvas master (Canvas to update on next sync); per-task bolded skill-verb phrasing and Project 6 capstone framing deferred Tier 2; `unit6.qmd` has no standard submission block (instructor decision deferred).
- **Sidebar-disabled folders** — `Workbooks/` and `Skill Builders/` have content but their sidebar entries are commented out in `_quarto.yml`. Treat as inactive surface; don't link new pages into them without surfacing the navigation decision.

Refresh this section after each teaching-semester touchpoint or any significant PR.

## Existing Tooling

The site renders with Quarto and publishes via GitHub Actions. Reuse these before writing new equivalents.

| Tool | Purpose | When to use |
|---|---|---|
| `quarto render <path>.qmd` | Render a single page (executes its Python chunks) | Iterating on one task page; **verify success before pushing** |
| `quarto render` | Render the full site to `_site/` | Before committing changes that affect multiple rendered pages |
| `quarto preview` | Live-reload local preview | While authoring `.qmd` pages |
| `.github/workflows/publish.yml` | GitHub Actions: render + publish to `gh-pages` on push to `main` | Automatic on push; the reason a broken render must never reach `main` |
| `pip install -r requirements.txt` | Install the render-time Python lib set (Python 3.13 in CI) | Setting up a local env that matches CI before rendering |
| `_submission_html.qmd` / `_submission_github.qmd` includes | Standard submission blocks | Any task using HTML upload (U0/U1/U3) or GitHub Pages (U2/U4/U5) flow |
| `Templates/unit{N}_task{M}_template.qmd` | Per-task starter template students copy | Reference shape when editing the matching task page; do NOT include `Templates/` in render |
| `gap-analysis.md` | 2026-04-20 audit + Canvas terminology mapping | Source of truth for Canvas-to-site assignment mapping and the authoring rules from the alignment PR |
