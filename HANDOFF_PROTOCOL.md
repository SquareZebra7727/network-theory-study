# HANDOFF PROTOCOL (manual workflow)

There is no automatic pushing. The cycle:

`Claude account works -> human reviews -> human commits to GitHub manually -> next account reads the latest repo state`

Branches and PRs are an optional future workflow. They are not required and nothing here depends on them. The repository is private; access problems are never solved by making it public. How an account receives the repo state (project files, an upload, a read-only connector) is the human's choice.

## 1. Start of a session
- **Default base:** the latest human-committed state visible in the repository. Only if a specific base matters, the human gives a commit SHA, date, or snapshot.
- Account reads `CLAUDE.md`, the Constitution, its role, `docs/CONTENT_STATUS.md`, and the newest handoffs, and states the base it worked from.
- It checks that its topic and kind are not assigned to another account, and that upstream items it depends on are VERIFIED.

## 2. During
- Write only owned paths. Do not touch `sources/original/` or anyone else's files, not even for a typo.
- Record origin, sources and flags as you go.
- Problems in another role's work: report; do not fix.

## 3. Delivery
Deliver (a) changed files in full, at their repo paths, and (b) one note `handoffs/YYYY-MM-DD_<role>_<slug>.md`. No excerpts or diffs.

```
# Handoff: <task>
Role: <role>   Base: <SHA/date, or "latest committed state">   Status change(s): <ids: from -> to>
## Delivered           (file paths)
## Not done            (explicit)
## Origin & flags      (secondary/generated items; unclear/insufficient/conflict flags with slide refs)
## Needs human decision (flags, open decisions)
## Dependents to re-review (ids that reference anything you changed)
## Checked             (what and how; what was NOT checked)
## Requests to other roles (role, file/id, what and why)
## Suggested CONTENT_STATUS update
## Next step           (one action, role, acceptance criteria)
```

## 4. Human review and commit
The human reviews the delivery, rejects or sends back parts, then commits, and updates `docs/CONTENT_STATUS.md` (or asks the Architect to). Accounts suggest status updates; they do not edit that file. Handoff notes are not edited after commit; follow-ups are new files. When a flag needs a decision, the human chooses (Constitution §2) and instructs the owning role.

## 5. Preventing conflicts
- **Assignment.** The same topic/kind/file set is not assigned to two accounts at once. Independent topics and files may run in parallel. `CONTENT_STATUS.md` shows who holds what.
- **Dependencies win.** A role waits when its work depends on an unverified upstream item, even if the topic is free.
- **Overlapping deliveries.** If two deliveries touch the same file, the human commits the first and the second account re-reads the latest committed state and incorporates the first delivery before delivering.
- **Stale base.** If the repo changed since your base, re-read changed files before delivering, or say so.
- **ID collisions.** Controlled process, not a quiet rename: stop, report to the human, list every reference, the Architect decides the allocation, owners update their own references, validation and review are repeated (Constitution §4).
- **Upstream changes.** If you change an item others reference, list dependents in the handoff. They go on the recheck list in `CONTENT_STATUS.md` until QA decides.
- Only the Architect changes governance files and `schemas/`.

## 6. Verification handoffs
Authors submit work as REVIEW in the delivery; the human or Architect updates `docs/CONTENT_STATUS.md` as required. The human assigns QA. QA delivers an audit under `audits/` and sets VERIFIED, or returns the item to DRAFT with defects listed. QA also moves dependents to REVIEW when an upstream item changed and re-review is warranted, and records the required status and recheck result in its verification delivery; `docs/CONTENT_STATUS.md` is updated by the human or Architect according to the established ownership rule, not by QA. Only the human sets APPROVED, in their own commit.
