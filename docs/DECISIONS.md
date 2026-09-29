[DECISIONS.md](https://github.com/user-attachments/files/32818508/DECISIONS.md)
# OPEN DECISIONS

Unresolved only. Resolved decisions live in the Constitution, roles, protocol, and architecture. Owner of all: the human, advised by the Architect.

| ID | Decision | Recommendation | Blocks |
|---|---|---|---|
| D-01 | Tech stack and hosting (framework, host, math renderer). Hosting must not expose the private repo or `sources/`. | Static-first site; LaTeX-based math. Choose the framework, host, and math renderer after D-02/D-03 are sufficiently defined. | UI/UX, Learning Features |
| D-02 | Content file format and schema v1 (including final file granularity) | Markdown + YAML front matter for prose items; YAML for circuits and the decision tree. Ratify after PPT analysis. | Any authoring under `content/`; not the PPT analysis |
| D-03 | Circuit representation and rendering (generated SVG from the spec vs hand-authored SVG vs image with checks); final `variants[]` form | Spec as source of truth with generated figures where possible. Decide after seeing the PPT figures. | Circuit Specialist |
| D-05 | Language(s) of the course and the site | Confirm the course and learner-facing site language(s); affects content/schema, typography/fonts, UI labels, and accessibility. | Schema |
| D-06 | Persistence of bookmarks/progress: browser-only vs user accounts/sync | Browser-only unless cross-device sync is needed | Learning Features, hosting |
| D-07 | Exam format (question types, weightings, past papers available?) | Provide if available; shapes checklists and quiz design | Non-blocking; Content Analyst, Problem/Solution |
