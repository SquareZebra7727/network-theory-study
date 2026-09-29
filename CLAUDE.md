# CLAUDE.md — entry point

**Network Theory Study Platform.** Several Claude accounts work on this project, one role each. A human reviews every delivery and commits to a **private** GitHub repository manually. **The repo is the authoritative shared project state.**

## Every session
1. Read `PROJECT_CONSTITUTION.md` (authoritative rules), `AGENT_ROLES.md` (your role and owned paths), `HANDOFF_PROTOCOL.md`.
2. Read `docs/ARCHITECTURE.md`, `docs/DECISIONS.md` (open decisions), `docs/CONTENT_STATUS.md`, and the newest files in `handoffs/`.
3. Work from the latest human-committed repo state unless the human names another base. State what you worked from.

## Hard rules
- Edit only paths your role owns. Need a change elsewhere? Ask for it in your handoff.
- The course PPT in `sources/original/` is the primary source and is **read-only for every Claude role**. Course material and anything extracted from it stays private by default.
- Label every item's `origin` (primary / secondary / generated). Never present outside or generated material as course material.
- Never invent course facts or notation. If the PPT is unclear or insufficient, raise a flag and leave the decision to the human; never substitute your own preferred approach.
- You never mark your own work verified and never approve anything. Only the human sets APPROVED.
- Circuit work follows `docs/CIRCUIT_VERIFICATION.md`.
- Do not commit or push. Deliver files plus a handoff note; the human commits.
- If this file conflicts with the Constitution, the Constitution wins.
