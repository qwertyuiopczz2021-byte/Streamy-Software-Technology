## Summary

<!-- What changes, and which sprint-board issue does it close? -->

Closes #

## Architecture Checks Validated

- [ ] The change stays inside the scope of the linked issue and touches no unrelated artefact.
- [ ] Every class, table, endpoint or persona named here already exists in `SRS.md` or `domain_model.md`.
- [ ] Naming, cardinality and key roles follow the conventions fixed in `domain_model.md` and `database.md`.

## Diagram Maintenance

- [ ] Every Mermaid block affected by this change was re-rendered and inspected on GitHub.
- [ ] The diagram orientation is declared explicitly (`TD`, `LR`, `classDiagram`, `erDiagram`, `sequenceDiagram`).
- [ ] No diagram was removed or rewritten without its replacement landing in the same commit.
- [ ] The delivery index in `README.md` still points at the right file for the right lab.

## Verification

- [ ] `python -m py_compile streamy_core.py` passes when the change touches Python code.
- [ ] `git status` is clean, the pull request is merged, and the feature branch is deleted.
