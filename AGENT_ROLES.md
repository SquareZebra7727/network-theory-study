# AGENT ROLES

Seven specialist roles. A role writes only the paths it owns; everything else is read-only. Changes needed elsewhere go in the handoff ("Requests to other roles"); the human routes them. `sources/original/` is placed by the human and is read-only for all roles.

| # | Role | Owns (writes) | Responsibility | Must not |
|---|---|---|---|---|
| 1 | **Architect / Project Lead** | `CLAUDE.md`, `PROJECT_CONSTITUTION.md`, `AGENT_ROLES.md`, `HANDOFF_PROTOCOL.md`, `docs/ARCHITECTURE.md`, `docs/DECISIONS.md`, `docs/CIRCUIT_VERIFICATION.md`, `docs/CONTENT_STATUS.md`, `schemas/` | Governance, schemas, task breakdown, ID-allocation decisions, cross-role conflicts. `docs/CONTENT_STATUS.md` is maintained by the human, or by the Architect when the human requests it or the handoff/coordination process requires it; this gives the Architect no authority to set item status | Write content or app code; invent course notation or facts |
| 2 | **Network Theory Content Analyst** | `docs/PPT_ANALYSIS.md`, `sources/extracted/`, `content/taxonomy.yaml`, `content/notation.md`, `content/techniques/`, `content/formulas/`, `content/decision-tree.yaml` | Turn the PPT into structure, including visual inspection of slides. Technique guides, formula cards, decision-system content, and course notation derived from the PPT. Flags PPT gaps. **References circuits by stable ID.** | Create or modify any circuit or circuit figure, including circuits used in technique examples; add non-PPT material without `origin` labels; use unverified circuits in submitted work |
| 3 | **Circuit Specialist** | `content/circuits/`, `assets/circuits/` | Owns every circuit spec and figure: values, source polarity, reference directions, dependent sources, variants, equivalent circuits. Fills the author self-check. | Write problem solutions or technique text; verify own circuits; change course notation |
| 4 | **Problem/Solution Specialist** | `content/problems/`, `content/questions/` | Solved examples, practice problems, solutions, difficulty tags, quiz questions. Uses circuits by ID. | Edit circuits or techniques; verify own solutions |
| 5 | **UI/UX Developer** | `src/` except `src/features/`, `public/`, `docs/DESIGN_SYSTEM.md`, build config | App shell, layout, content and circuit rendering, dark/light theming, responsive design, component library | Alter content; implement learning-feature logic |
| 6 | **Learning Features Specialist** | `src/features/` | Interactive learning logic: show/hide solutions, step-through, decision wizard, quizzes/practice, bookmarks, progress, difficulty filtering. Builds on UI/UX components. | Alter content or the UI shell |
| 7 | **QA / Auditor** | `audits/`, `tests/`, `scripts/validate/`; may change only the `status` and `audit_ref` fields of items: REVIEW to VERIFIED, REVIEW or VERIFIED back to DRAFT (defects), VERIFIED to REVIEW only when an upstream dependency has changed and re-review is required | Independent verification of every role's work: circuit audits, solution re-derivation, source-origin audit, UI checks | Fix what it finds; author content or features; set APPROVED |

## Boundary rules
- **Circuit ownership.** Whenever a technique, example, problem, or question uses a circuit, the author references it by stable ID. Only the Circuit Specialist creates or modifies the circuit. Dependent content must use the VERIFIED circuit.
- **Circuit Specialist and Problem/Solution Specialist stay separate.** A problem never embeds or edits a circuit; if a circuit is wrong, the problem waits for a corrected one.
- **QA independence.** QA never audits what it authored and never fixes defects; it records them and returns the item to its owner.
- **UI/UX and Learning Features** meet at a component API (`src/components` provided, `src/features` consumed). Neither edits the other's directory. Dependency or build-config needs go to UI/UX via the handoff.
- **Status:** authors set DRAFT and REVIEW; only QA sets VERIFIED; only the human sets APPROVED (Constitution §3).
- **Parallel work:** see Constitution §5. One role per topic and kind at a time; `docs/CONTENT_STATUS.md` records who.
