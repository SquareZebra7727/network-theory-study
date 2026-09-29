# PROJECT CONSTITUTION

Status: v0.3 draft for human ratification. Authority order: human owner's instructions > this Constitution > `AGENT_ROLES.md`, `HANDOFF_PROTOCOL.md`, `docs/CIRCUIT_VERIFICATION.md` > `docs/ARCHITECTURE.md` > everything else.

## 1. Purpose and scope
An interactive Network Theory study platform for exam preparation, built from the human owner's course PPTs. The GitHub repository is **private**. Required capabilities (mapped to content fields and owners in `docs/ARCHITECTURE.md`):
technique/theorem guides; when to use a technique; how to recognize it; different circuit orientations; polarity/sign conventions; step-by-step procedures; special cases; solved examples; common mistakes; exam checklists; related techniques; a "Which technique should I use?" decision system; Basic/Intermediate/Advanced difficulty; show/hide solutions; step-by-step interaction; formula cards; bookmarks/favorites; progress tracking; quizzes/practice; dark/light mode; responsive/mobile UI.

## 2. Sources, provenance and flags
| Origin | Meaning | Rule |
|---|---|---|
| `primary` | The human's course PPTs, including the course's own notation, conventions and methods | Authoritative. Cite file and slide. |
| `secondary` | Other academic sources, used only when genuinely necessary | Cite the reference. Never overrides the course approach. |
| `generated` | Claude-created examples, explanations, diagrams, practice problems | Marked as generated everywhere, including in the UI. |

- Every item, and every block or step whose origin differs from its item, carries an `origin` tag. Supplementary or generated material must never appear as if it came from the PPT.
- The original PPT lives in `sources/original/`, placed there by the human. **No Claude role may modify, rename, or delete anything in `sources/original/`.**
- Course material, including slides, slide images and extracted text, is private. The built site includes only approved learner-facing items. Verbatim slides, slide images, extracted files and `sources/` are never included in build output or otherwise exposed publicly unless the human explicitly decides so.
- **No invention.** Unknown course facts and notation are flagged, not filled in.
- **Flags** (`unclear | insufficient | conflict`) are raised when the PPT is unclear, incomplete, or appears to conflict with standard practice or with itself. A flag carries a note and slide reference. The role keeps the course approach as written and does not resolve the flag itself. The **human decides** whether to (a) accept the course ambiguity, (b) request clarification or correction, (c) resolve it through an explicit project decision, or (d) instruct the owning role to revise the item. The decision is recorded on the flag (`human_decision`, date) by the owning role at the human's instruction. A flag with no human decision stays open and is listed in the handoff and `docs/CONTENT_STATUS.md`.

## 3. Lifecycle and human review
`DRAFT -> REVIEW -> VERIFIED -> APPROVED`

| State | Set by | Meaning |
|---|---|---|
| DRAFT | authoring role | Work in progress. Not canonical. |
| REVIEW | authoring role | Submitted through a handoff; awaiting QA. |
| VERIFIED | QA / Auditor only | Independently checked; an audit record exists. |
| APPROVED | **human owner only** | Canonical. Only APPROVED items are published by default. |

- Claude generation never makes anything canonical. Nothing skips a state.
- QA may return an item to DRAFT (defects) or move VERIFIED back to REVIEW (upstream change, see §4). Only the human moves anything out of APPROVED.
- Any substantive edit to a VERIFIED item resets it to DRAFT. An APPROVED item is edited only on the human's instruction and then also resets to DRAFT.
- An author never verifies own work.

## 4. Correctness rules
**Solutions.** QA re-derives every worked solution independently from the problem statement and circuit spec, not from the author's steps. Answers carry units and sign.

**Circuits.** The structured circuit spec is the source of truth. Wherever technically possible the figure is generated from it; otherwise the figure is explicitly checked against the spec and, when applicable, the original PPT figure. `docs/CIRCUIT_VERIFICATION.md` is binding.

**Notation.** Course notation and sign conventions live in `content/notation.md`, derived from the PPT by the Content Analyst only. Nobody writes notation before PPT analysis. If items use inconsistent notation without a documented course/source basis, that inconsistency is a defect.

**Stable IDs.** IDs are independent of file paths, are never reused, and are never renumbered once referenced. Retire with `status: deprecated`. The owning role proposes IDs within its assigned scope. Collision handling is a controlled process:
1. Duplicate detected (by any role or by validation): **stop** work on the affected items and report to the human.
2. Identify every reference to each colliding ID.
3. The **Architect decides the ID allocation**. Normally the unreferenced or newer item is the one re-identified, but the Architect decides.
4. Each owning role updates its own references in a follow-up delivery.
5. Validation and review are repeated on affected items; VERIFIED items return to REVIEW.
No one renames a referenced ID on their own.

**Dependencies.** Items refer to each other by ID only (`circuits[]`, `techniques[]`, `formulas[]`, `related[]` and similar).
- *Upstream first.* Work may enter REVIEW only if the items it depends on are VERIFIED. A role may draft against an unverified upstream ID, but must wait before submitting.
- *Change after verification.* When a referenced item changes, its author lists the dependents (by searching for its ID) in the handoff. Dependents may not be treated as VERIFIED until QA determines whether re-review is required. For an APPROVED dependent, the human owner decides whether it remains valid or must return to DRAFT. Any required re-review is tracked in `docs/CONTENT_STATUS.md`. No dependency tooling is required yet.

## 5. Ownership and workflow
- One role owns each path (`AGENT_ROLES.md`). The Architect alone edits governance files and `schemas/`.
- Manual workflow: Claude account -> human review -> human commits -> next account reads the repo (`HANDOFF_PROTOCOL.md`). No automatic pushing. Branches and PRs are an optional future workflow, not required.
- **Default base:** unless the human names another base (commit SHA, date, or snapshot), work from the latest human-committed state visible in the repository.
- **Parallel work:** the same topic, kind, or file set is never assigned to two accounts at once. Genuinely independent topics and files may run in parallel. Dependencies take priority: a role waits when its work depends on an unverified upstream item. `docs/CONTENT_STATUS.md` is the coordination record.
- Do not reformat, rename, or "clean up" files outside the task.
- Schema changes that break existing content or consumers require an entry in `docs/DECISIONS.md` and human sign-off. Additive, backward-compatible changes are documented by the Architect and do not require a decision unless they affect project behavior or scope.

## 6. Definition of Done (to enter REVIEW)
**All work**
1. Task acceptance criteria are met.
2. Every changed file is in the role's owned paths; nothing in `sources/original/` was touched.
3. Provenance is complete: `origin`, source references, flags with slide refs.
4. Handoff note written, including what was NOT checked.

**Content that depends on circuits:** circuits used are VERIFIED, and the author's circuit self-check is filled in where the role authors circuits.

**Code (UI/UX and Learning Features):**
5. Build and validation pass once tooling exists; until then the handoff lists what was checked by hand.
6. Where relevant to the change: verified at mobile and desktop widths, in dark and light mode, and math renders correctly.

**Analysis and content authoring** are not subject to the UI checks in 6.

VERIFIED additionally needs a QA audit. APPROVED additionally needs human sign-off.

## 7. Escalation
Anything ambiguous that affects architecture, schema, scope, or sources goes to the human through the handoff and, if unresolved, `docs/DECISIONS.md`. Do not resolve it silently.
