[ARCHITECTURE.md](https://github.com/user-attachments/files/32818419/ARCHITECTURE.md)
# ARCHITECTURE

Status: v0.3. Repo at drafting: `README.md` only; the PPT has not been added. The repository is private. The layout below is **provisional** until the PPT analysis and schema v1 (D-02).

## Shape
`PPT (primary, sources/original/) -> extracted structure -> content items (structured, with origin + status) -> build -> site`
Content is structured data; UI and features render it and hold no course facts.

## Content model
Folders are by **kind**; topic, difficulty and tags are **metadata**, so regrouping after the PPT analysis moves no files. **Stable IDs are independent of file paths** and are the only linking mechanism.

Content should normally use one file per independently reviewable item. Final granularity (for example, whether examples or steps become separate files) is confirmed after PPT analysis and schema v1.

| Kind | Path | Owner |
|---|---|---|
| Technique guide | `content/techniques/` | Content Analyst |
| Formula card | `content/formulas/` | Content Analyst |
| Notation & conventions | `content/notation.md` | Content Analyst (see below) |
| Topic taxonomy | `content/taxonomy.yaml` | Content Analyst (from the PPT only) |
| "Which technique?" tree | `content/decision-tree.yaml` | Content Analyst |
| Circuit (spec, variants, figures in `assets/circuits/`) | `content/circuits/` | Circuit Specialist |
| Problem (solved example / practice) | `content/problems/` | Problem/Solution |
| Quiz question | `content/questions/` | Problem/Solution |

**`content/notation.md`** is the destination for course-derived notation and conventions (symbols, sign and reference conventions, the course's method names). It is empty until the Content Analyst derives it from the PPT analysis. Nobody writes it from general knowledge.

**Common fields:** `id, kind, title, topic, status (draft|review|verified|approved|deprecated), origin (primary|secondary|generated), source_refs[], flags[], audit_ref`. Flags: `type (unclear|insufficient|conflict), note, slide_ref, human_decision, decided_on`.

**Canonical status values** are lowercase and machine-readable: `draft | review | verified | approved | deprecated`. The Constitution's uppercase lifecycle names (`DRAFT`, `REVIEW`, `VERIFIED`, `APPROVED`) are human-readable labels for `draft`, `review`, `verified`, `approved`. Files, schemas and tooling use only the lowercase values; `deprecated` is a retirement value outside the lifecycle.

**Technique:** `when_to_use, how_to_recognize, orientations[], sign_conventions, procedure[], special_cases[], common_mistakes[], exam_checklist[], related[{id, relation}], formulas[], examples[]`. Circuits used by a technique or example appear only as `circuits[]` ID references. The Content Analyst never authors them, and dependent content uses the VERIFIED circuit.

**Problem** (conceptual; not final schema v1). The statement can be rendered without touching the solution:
```
problem
  meta:      id, difficulty (basic|intermediate|advanced), mode (solved_example|practice),
             techniques[], circuits[] (ids), origin, status, flags[]
  statement: text, given, find            # everything a learner sees before revealing anything
  hints[]                                 # progressive, each revealable on its own
  solution:                               # hidden by default; never referenced from statement fields
    steps[]   (atomic; each has expression, rationale, optional origin)
    answer    (value, unit, sign)
  pitfalls[]                              # shown with or after the solution
```
Reveal order is statement, then hints, then steps one at a time, then answer, then pitfalls. Content only guarantees the separation; the behaviour is the Learning Features Specialist's.

**Formula:** `latex, variables (symbol, meaning, unit), valid_when, sign_notes, techniques[]`. Equations are written in LaTeX regardless of renderer.

**Circuit and variants:** see `docs/CIRCUIT_VERIFICATION.md`. The spec is the source of truth; figures are generated from it where possible, otherwise checked against it. `variants[]` records `kind` (layout_rotation, mirror, reference_direction, source_polarity, equivalent_representation, electrically_different), the relation to the base, and the expected effect. A visual change is not assumed to be an electrical change. The representation is provisional until D-03.

## Requirement traceability
| Requirement | Content | Behaviour |
|---|---|---|
| Guides, when to use, recognition, procedure, special cases, mistakes, checklists, related | technique fields | rendered by UI/UX |
| Orientations, polarity/sign conventions | technique, circuit `variants`, `notation.md` | figure switching: Learning Features |
| Solved examples, difficulty | problem | filter/sort: Learning Features |
| Show/hide solution, step-by-step interaction | problem `statement` / `solution.steps[]` | Learning Features |
| "Which technique?" | decision tree data | wizard: Learning Features |
| Formula cards | formula | card component: UI/UX |
| Quizzes/practice | question, problem (practice) | engine: Learning Features |
| Bookmarks, progress | user records keyed by item ID | Learning Features |
| Dark/light, responsive | design tokens | UI/UX |

## Progress and bookmarks
Course content is referenced by stable item IDs. User progress/bookmark records are keyed by item ID but may carry additional learning-state metadata (for example attempts, completion, last step reached, timestamps, self-rating). They are not limited to an ID. User records never live inside content items. Persistence (browser-only vs synced) is open (D-06).

## Source and status in the product
The build includes only `approved` items by default; a preview mode may show others with a visible status badge. `secondary` and `generated` items show a visible origin badge. Flags never render to learners. The private repo and `sources/` are never part of build output; course slides, slide images and extracted text are not published unless the human decides otherwise.

## PPT to knowledge base (once the PPT is added to `sources/original/`)
The Content Analyst must **inspect slides visually**, not rely on text extraction alone.
1. **Render and inspect every slide as an image.** Record each slide's kind (theory / example / figure / exercise) in `docs/PPT_ANALYSIS.md`.
2. **Explicitly inspect slides containing** circuit diagrams, graphs, tables, equations embedded as images, technical figures, screenshots, and annotations. Per element record what it shows and whether it was readable.
3. **Flag, do not guess:** unreadable or ambiguous information (a smudged polarity mark, an unclear value, an equation in an image) gets a flag with slide reference.
4. Extract text and other machine-readable source material to `sources/extracted/`; these files are source references for analysis, not canonical learner-facing content. Propose `taxonomy.yaml` in the PPT's own order.
5. Derive course notation and conventions into `content/notation.md`, with slide references.
6. The initial structure of `content/decision-tree.yaml` is derived from the PPT's documented techniques, recognition cues, conditions, and relationships. Any additional generated decision guidance carries its appropriate `origin` and must not override the course approach.
7. Map slides to planned items and list gaps and ambiguities. Circuit figures are handed to the Circuit Specialist by slide reference; the Analyst does not recreate them. The human reviews the map.
8. Schema v1 is ratified (D-02); then authoring proceeds by topic. Coverage is tracked as slides mapped / slides total.
The PPT stays the primary source; nothing in the analysis substitutes another approach.

## Verification hooks
Schema, ID and reference validation live in `scripts/validate/` (QA-owned, added when the schema exists). Circuit and solution audits are records in `audits/`. Dependencies are traced by stable-ID references; no engine is planned yet.
