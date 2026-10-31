# Sprint Architecture — Streamy Delivery Board

Working agreement for the GEIAL314-B2a practical labs: how work is decomposed,
scheduled and moved across the Kanban board before any code is written.

Board: [Streamy Development Sprint Board](https://github.com/users/qwertyuiopczz2021-byte/projects/2)

## 1. Sprint Lifecycle

The delivery uses a three-column pull system. A card only moves when its exit
criteria are met, so the board is always an honest picture of the sprint.

| Column | Meaning | Entry rule | Exit rule |
| --- | --- | --- | --- |
| **Todo** | Committed to the sprint, not started | Issue body states the requirement IDs it satisfies | Work branch created from `main` |
| **In Progress** | Actively being worked on (WIP limit: 1) | `git checkout -b feature/<scope>` has been executed | Pull request opened against `main` |
| **Done** | Reviewed and merged | Pull request approved, checks green, branch deleted | Issue closed by the merge commit |

The WIP limit of one is deliberate: the module is documentation-first, so two
parallel features would produce contradictory diagrams in `main`.

## 2. Issue Decomposition

Each lab produces exactly one feature issue, written in the imperative and
scoped to a single architectural artefact.

| Issue | Title | Requirement trace | Deliverable | Lab |
| --- | --- | --- | --- | --- |
| #1 | feat: Design dynamic sequence interaction flows for Viewer authentication | FR-4 | `interactions.md` | Lab 6 |
| #2 | feat: Document advanced structural object-oriented class properties | FR-1, FR-2, FR-5 | `domain_model.md` | Lab 7 |
| #3 | feat: Construct relational data database schemas via Entity Relationship maps | NFR-3 | ERD section of `domain_model.md` | Lab 7 (Extra) |
| #4 | feat: Translate architectural diagrams into runnable Python code skeletons | NFR-1 | Python skeleton (next sprint) | Backlog |

Issues #3 and #4 stay in `Todo`: they are intentionally deferred so the sprint
stays small enough to finish and review inside one laboratory session.

## 3. Branch, Review and Merge Policy

1. `main` is protected by convention — nobody commits to it directly.
2. Every feature branch is named `feature/<kebab-scope>` and lives for one issue only.
3. The pull request body carries an **Architecture Checks Validated** list plus a
   `Closes #<issue>` line, so the issue closes automatically on merge.
4. After the merge the local branch returns to `main` with
   `git checkout main && git pull origin main && git branch -d feature/<scope>`,
   and the remote branch is deleted from the pull request page.

## 4. Definition of Done

A card is Done only when all of the following hold:

- [ ] The artefact is committed on `main` and renders correctly in the GitHub preview.
- [ ] The pull request is merged with the `Closes #<issue>` reference honoured.
- [ ] The feature branch (local and remote) is deleted.
- [ ] `git status` on `main` reports a clean working tree.
- [ ] The screenshot of every executed step is filed in the lab Word document.
