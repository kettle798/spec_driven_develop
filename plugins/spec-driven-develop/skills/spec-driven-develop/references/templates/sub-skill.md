# Sub-SKILL Template

When generating a task-specific sub-SKILL in Phase 4, delegate to the platform's native skill-creator. The sub-SKILL is always installed at **project level** to keep it co-located with the project it serves.

---

## Metadata

- **Name**: `<task-type>-dev` (e.g., `rust-migration-dev`, `microservice-overhaul-dev`)
- **Description**: Should reference the specific task and include trigger keywords
- **Installation**: Project level (e.g., `.cursor/skills/`, `.claude/commands/`, or project-local directory)

---

## Content Outline

The generated sub-SKILL must include these sections:

1. **Cross-conversation continuity protocol** — read `docs/progress/MASTER.md` first, always
2. **S.U.P.E.R architecture principles** — reference `references/super-philosophy.md` as the architectural coding standard; all code produced during development should follow Single Purpose, Unidirectional Flow, Ports over Implementation, Environment-Agnostic, and Replaceable Parts
3. **Target technology coding standards** — conventions for the target language/framework
4. **Progress update instructions** — how to update checkboxes and MASTER.md counts
5. **Phase-specific development guidance** — architecture context and implementation notes
6. **Parallel execution protocol** — reference `references/parallel-protocol.md` for the full protocol
7. **Archive trigger** — when all tasks are done, initiate Phase 6 (Archive)
