---
name: spec-driven-develop
description: >-
  Automates pre-development workflow for large-scale complex tasks. Use when the user
  mentions "rewrite", "migrate", "overhaul", "refactor entire project", "transform",
  "rebuild in [language]", "spec-driven", or describes any large-scale project transformation
  that requires planning before coding. Also triggers on Chinese keywords: "改造", "重写",
  "迁移", "重构", "大规模", "规范驱动". Performs full project analysis, task decomposition,
  documentation generation, progress tracking setup, and task-specific sub-SKILL creation
  before any development begins.
version: 1.5.0
---

# Spec-Driven Develop

You are executing the **Spec-Driven Development** workflow — a standardized pre-development pipeline for large-scale complex tasks. Your job is to complete all preparation phases before any actual coding begins, ensuring the project has full analysis, a clear plan, trackable progress documents, and a task-specific SKILL.

## Configuration

| Path               | Default Value                | Purpose                                    |
|:-------------------|:-----------------------------|:-------------------------------------------|
| Analysis output    | `docs/analysis/`             | Phase 1 analysis documents                 |
| Plan output        | `docs/plan/`                 | Phase 2 planning documents                 |
| Progress output    | `docs/progress/`             | Phase 3 tracking documents (incl. MASTER.md) |
| Archive output     | `docs/archives/<project>/`   | Phase 6 archived artifacts                 |
| Sub-SKILL install  | Project level (auto-detect)  | Platform-specific: `.cursor/skills/`, `.claude/commands/`, or project-local |

Templates for all generated documents are in `references/templates/`. Behavioral rules are in `references/behavioral-rules.md`. The parallel execution protocol is in `references/parallel-protocol.md`.

## Before You Begin: Cross-Conversation Continuity Check

**CRITICAL**: Before starting any phase, check if `docs/progress/MASTER.md` already exists in the project.

- If it **exists**: Read it immediately. You are resuming an in-progress task. Identify which phase you are in, what has been completed, and continue from the exact point where the previous conversation left off. Do NOT restart from Phase 0.
- If it **does not exist**: This is a fresh start. Proceed to Phase 0.

After loading your current state from MASTER.md, populate the platform's native task tracking tool (e.g. TodoWrite) with the active phase's pending tasks. For each task, set content to the task description, status to "in-progress" for the currently active task and "todo" for the rest, and priority mapped as P0=high, P1=medium, P2=low. This gives the user real-time visual progress in their IDE. If no native task tool is available, skip this step — MASTER.md alone is sufficient.

---

## Phase 0: Intent Recognition & Confirmation

**Goal**: Understand exactly what the user wants to accomplish and eliminate all ambiguity.

**Actions**:

1. Identify the user's core intent from their message. Extract:
   - The type of transformation (language migration, framework change, architecture overhaul, new feature development, etc.)
   - The target state (e.g., "rewrite in Rust", "migrate to microservices")
   - Any constraints or preferences mentioned

2. Ask the user structured clarifying questions. At minimum, confirm:
   - **Scope**: Which parts of the project are in scope? The entire codebase or specific modules?
   - **Target**: What is the target technology/architecture/state?
   - **Constraints**: Are there hard constraints (timeline, backward compatibility, specific libraries, deployment targets)?
   - **Priorities**: What matters most — performance, maintainability, feature parity, or something else?

3. Summarize your understanding back to the user and get explicit confirmation before proceeding.

**Output**: A clear, confirmed task definition that will guide all subsequent phases.

---

## Phase 1: Deep Project Analysis

**Goal**: Build a comprehensive understanding of the current codebase.

**Actions**:

1. Launch `project-analyzer` sub-agents **in parallel** to analyze the codebase concurrently. Split the work by focus area:
   - **Agent 1 — Architecture & Stack**: Project structure, directory layout, technology stack, entry points, build/run commands
   - **Agent 2 — Module Inventory**: Each module's responsibility, public API surface, size, internal/external dependencies
   - **Agent 3 — Risks & Patterns**: Transformation risks, complexity hotspots, platform-specific code, coding conventions

   Provide each agent with the confirmed task definition from Phase 0 so they can assess findings in context of the target transformation.

   If sub-agents are not available on the current platform, perform the analysis sequentially yourself — the scope is the same either way.

2. Consolidate agent outputs and resolve any contradictions or gaps. Write analysis documents to `docs/analysis/` using the templates in `references/templates/analysis.md`:
   - `project-overview.md` — Architecture, tech stack, entry points, build system
   - `module-inventory.md` — Every module with: responsibility, dependencies, size, complexity rating
   - `risk-assessment.md` — Technical risks, compatibility risks, complexity hotspots

**Output**: Complete `docs/analysis/` directory with three documents.

---

## Phase 2: Task Decomposition

**Goal**: Break down the transformation into manageable, trackable tasks organized in phases, with explicit parallel execution lanes.

**Actions**:

1. Launch `task-architect` sub-agents with the full analysis output from Phase 1. If the project is large enough to warrant multiple strategies, launch 2 agents exploring different decomposition approaches (e.g., bottom-up vs. strangler fig) and pick the better result.

   If sub-agents are not available, perform the decomposition yourself.

2. The decomposition must produce:
   - Phased approach with natural phase boundaries, ordered by dependency
   - Concrete tasks for each phase, each with: description, priority (P0/P1/P2), effort (S/M/L/XL), dependencies, acceptance criteria
   - **Parallel execution lanes**: For each phase, group tasks that have no mutual dependencies into lanes that can run simultaneously. Assess merge risk (file overlap) between lanes.
   - Dependency graph as a Mermaid diagram — use subgraphs to visualize parallel lanes
   - Milestones at natural phase boundaries

3. Write planning documents to `docs/plan/` using the templates in `references/templates/plan.md`:
   - `task-breakdown.md` — All phases and tasks with full detail, including parallel lane assignments
   - `dependency-graph.md` — Mermaid diagram showing task/phase dependencies and parallel lanes
   - `milestones.md` — Milestone definitions with target criteria

**Output**: Complete `docs/plan/` directory with three documents.

---

## Phase 3: Progress Tracking Documentation

**Goal**: Create a document-driven progress tracking system that survives across conversations.

**Actions**:

Use the templates in `references/templates/progress.md` for all progress documents.

1. Create the **master control file** `docs/progress/MASTER.md` with:
   - Task name and description (from Phase 0)
   - Link to each analysis document
   - Link to each plan document
   - A summary table of all phases with completion percentage
   - Links to each phase's detailed progress file
   - A "Current Status" section indicating which phase/task is active
   - A "Next Steps" section for the agent to quickly orient itself

2. Create **one detailed progress file per phase**: `docs/progress/phase-N-<short-name>.md`
   - Each file contains the phase's tasks as checkbox items: `- [ ] Task description`
   - Include acceptance criteria inline for each task
   - Include a "Notes" section for recording decisions, blockers, and context

3. The MASTER.md format must follow this convention:
   - Phases use the format: `- [ ] Phase N: <name> (0/X tasks) [details](./phase-N-<name>.md)`
   - When a phase is fully done: `- [x] Phase N: <name> (X/X tasks) [details](./phase-N-<name>.md)`
   - The "Current Status" section is updated by the agent at the start and end of each work session

**Output**: Complete `docs/progress/` directory with MASTER.md and per-phase detail files.

---

## Phase 4: Task-Specific Sub-SKILL Generation

**Goal**: Create a project-level SKILL tailored to this specific task, encoding the interaction patterns and development standards needed for the actual implementation work.

**Actions**:

1. The sub-SKILL is **always installed at project level** (e.g., `.cursor/skills/`, `.claude/commands/`, or project-local directory). Do not ask the user for installation location. This keeps the sub-SKILL co-located with the project it serves and avoids polluting the global skill space.

2. Determine what the sub-SKILL should contain (see `references/templates/sub-skill.md` for the full content outline):
   - **S.U.P.E.R architecture principles** from `references/super-philosophy.md` — the architectural coding standard for all generated code
   - Task-specific coding standards and conventions for the target technology
   - The cross-conversation continuity protocol (read MASTER.md first)
   - Project-specific architecture context and implementation notes
   - Guidance on how to update progress documents after completing each task
   - Phase-specific instructions relevant to the transformation type
   - **Parallel execution protocol**: reference `references/parallel-protocol.md` for the full protocol
   - The archive trigger: when all tasks are done, initiate Phase 6

3. **Delegate creation to the platform's native skill-creator**:
   - On **Claude Code** or **Codex**: Invoke the platform's built-in `skill-creator` skill, providing it with the task context, the desired skill name, description, and content outline. Let the native tool handle the actual file generation and installation.
   - On **Cursor**: If a skill-creator skill is available, use it. Otherwise, create the SKILL.md directly following the standard frontmatter + markdown format and place it in the project's skills directory.

4. The generated sub-SKILL should instruct the agent to:
   - Always read `docs/progress/MASTER.md` at the start of every conversation
   - Update the checkbox status in the relevant phase file after completing each task
   - Update the completion count and "Current Status" in MASTER.md
   - When all checkboxes are checked, trigger Phase 6 (Archive)

**Output**: A project-level task-specific SKILL.

---

## Phase 5: Handoff & Summary

**Goal**: Present all preparation artifacts to the user and confirm readiness to begin development.

**Actions**:

1. Present a structured summary to the user:
   - Task definition (from Phase 0)
   - Key findings from analysis (high-level, from Phase 1)
   - Phased plan overview with task counts (from Phase 2)
   - Progress tracking system description (from Phase 3)
   - Sub-SKILL name and installation location (from Phase 4)

2. List all generated artifacts:
   - `docs/analysis/project-overview.md`
   - `docs/analysis/module-inventory.md`
   - `docs/analysis/risk-assessment.md`
   - `docs/plan/task-breakdown.md`
   - `docs/plan/dependency-graph.md`
   - `docs/plan/milestones.md`
   - `docs/progress/MASTER.md`
   - `docs/progress/phase-N-*.md` (one per phase)
   - The generated sub-SKILL

3. Ask the user: "All preparation is complete. Ready to begin Phase 1 development?"

**Output**: User confirmation to proceed with actual implementation.

---

## Phase 6: Archive

**Trigger**: This phase activates when ALL checkboxes in `docs/progress/MASTER.md` are marked complete (`[x]`).

**Goal**: Archive all workflow artifacts for future reference and traceability, then clean up the working directories.

**Actions**:

1. Announce to the user that all tasks have been completed. Congratulate them.

2. Determine the archive directory name from the task name established in Phase 0. Sanitize it for use as a directory name (lowercase, hyphens instead of spaces, no special characters). The archive path is: `docs/archives/<project-name>/`. See `references/templates/archive.md` for the target directory structure and index template.

3. Create the archive directory structure and move all artifacts into it:
   - Move `docs/analysis/` to `docs/archives/<project-name>/analysis/`
   - Move `docs/plan/` to `docs/archives/<project-name>/plan/`
   - Move `docs/progress/` to `docs/archives/<project-name>/progress/`
   - Move the project-level sub-SKILL file to `docs/archives/<project-name>/skill/SKILL.md`
   - Move any other temporary files generated during development into the archive

4. Create or update the archive index file `docs/archives/README.md`:
   - If the file does not exist, create it with a header and the first project entry
   - If it already exists, append a new entry for this project
   - Each entry should include: project name, one-line description, date range (started — completed), and a link to the archived MASTER.md

5. After archiving, remove the now-empty `docs/analysis/`, `docs/plan/`, and `docs/progress/` directories from the project root's `docs/` folder, and remove the sub-SKILL's original directory if it is now empty. Only `docs/archives/` should remain under `docs/`.

6. Suggest to the user that they might want to commit the archive to version control.

**Output**: All artifacts preserved under `docs/archives/<project-name>/`, with an updated index at `docs/archives/README.md`.

---

## Behavioral Rules

All rules in `references/behavioral-rules.md` apply to every phase. Read and follow them.
