# Knowledge Extraction Workflow (v0.2.06)

**Goal:** Distill engineering wisdom from a codebase into a **Global Library** of atomic patterns and tricks (cunning, novel, masterful). Output: optional learning*objectives.md → extraction_plan → findings → library/pattern*\*.md + library/README.md + library/merge-map.json (when merges occur).

**Your Role:** Reverse Engineering Agent. Run phases sequentially; optionally pause after Phase 1 for confirmation. Support resume from state when re-running.

---

## WORKFLOW ARCHITECTURE

**Micro-file workflow:** One workflow (this file) plus one step file per phase.

- **step-00-director.md** (optional) — Director: read README/docs, optional Blueprint → learning_objectives.md; optional implementation_map.json when Blueprint provided.
- **step-01-survey.md** — Surveyor: scan repo; when learning_objectives.md exists, map focus to Learning Targets; else identify nerve centers by complexity. Produce extraction_plan.json.
- **step-02-atomize.md** — Atomizer: for each target (or batch), extract findings; optional "Theory vs. Practice" section when Blueprint was used.
- **step-03-librarian.md** — Librarian: merge duplicates, discard common knowledge, write pattern\_\*.md to library, produce library/merge-map.json when merging findings, update library/README.md (quick lookup: every pattern in at least one bucket), optional Code enrichment, run validation checklist.

Config and paths are **config-driven** via `workflow.yaml`. State is tracked in `extraction-state.json` for resume.

**Execution branch:** If `use_director: true` in workflow.yaml, run step-00 then step-01 → step-02 → step-03. If `use_director: false`, run step-01 → step-02 → step-03 only.

**Blueprint extension:** When the user provides a theory/Blueprint file (e.g. via `blueprint_file` or `theory_context_path` in config), Director can use it to derive learning_objectives and optionally produce implementation_map.json; Atomizer can add a "Theory vs. Practice" section to artifacts. See **Blueprint Extraction Protocol (add-on)** for details.

---

## INITIALIZATION

**Order (find-config-resolve-path-discover):**

1. **Find config** — Locate `workflow.yaml` (same folder as this workflow or path from runner). Load `use_director`, optional `blueprint_file`/`theory_context_path`, `extraction_root`, `output_plan`, `output_objectives`, `output_findings`, `output_library`, `validation`, optional `batch_size`, `exclude_paths`.
2. **Resolve paths** — Replace placeholders `{project-root}`, `{extraction_root}`, `{installed_path}` from config. Ensure output dirs exist (findings, library; \_distillation for plan and objectives).
3. **Discover state** — If resuming: read `extraction-state.json` (same base as output_plan or configurable). Use `current_phase`, `director_done`, `targets_done`, `findings_count` to skip completed work.

**Load strategy:**

- **Load at init:** workflow.md, workflow.yaml, step files (step-00 through step-03), extraction-state.json if present.
- **Store path only; load when needed:** `validation` (checklist) — load only when running the Phase 3 (Librarian) validation step; optional `blueprint_file` / `theory_context_path` — load only in Phase 0 when use_director is true.

Do not assume paths are hardcoded; resolve from config.

---

## WORKFLOW MAP

| Phase | Step      | Produces                                                                                                                | Consumed by          |
| ----- | --------- | ----------------------------------------------------------------------------------------------------------------------- | -------------------- |
| 0     | Director  | learning_objectives.md (LTs proportional to code size), optional implementation_map.json                                | Phase 1 (plan focus) |
| 1     | Surveyor  | extraction_plan.json (optional concept_id)                                                                              | Phase 2 (targets)    |
| 2     | Atomizer  | finding\_\*.md per target (Code mandate, optional code_completeness); batch order: Line-Level after Module when helpful | Phase 3 (inputs)     |
| 3     | Librarian | pattern\_\*.md, library/README.md, library/merge-map.json (when merges); runs checklist                                 | —                    |

Each phase's output is context for the next: objectives → plan focus; plan → findings; findings → library.

---

## EXECUTION

1. If **use_director** is true: Load and execute **steps/step-00-director.md**. Output: learning_objectives.md at output_objectives; optionally implementation_map.json when Blueprint provided. Update state: director_done = true.
2. Load and execute **steps/step-01-survey.md** (Surveyor). When learning_objectives.md exists, tie extraction_plan focus to Learning Targets; otherwise current behaviour (nerve centers by complexity). Output: extraction_plan.json; optionally update extraction-state.json.
3. Load and execute **steps/step-02-atomize.md** (Atomizer). Input: extraction_plan.json; optional implementation_map.json when Blueprint run. Output: findings/\*.md per target; optional "Theory vs. Practice" in findings when Blueprint used. Update state after each target or batch.
4. Load and execute **steps/step-03-librarian.md** (Librarian). Input: findings/\_.md; output: library/pattern\_\_.md, library/README.md, library/merge-map.json (when merging N findings into patterns); run checklist; optionally archive or clear findings. Preserve "Theory vs. Practice" in pattern artifacts when present.

After Phase 3 (Librarian), validation checklist (checklist.md) is run; do not delete findings until Librarian has written and validated library artifacts.

---

## REFERENCE

- **Standardized Output Templates:** v0.2.06 Standardized Output Templates.md (single schema; code_completeness, concept_id; optional Theory vs. Practice for Blueprint artifacts).
- **Prompts:** v0.2.06 The Codebase Distillation Prompts.md (Director proportional LTs, Surveyor optional concept_id, Atomizer Code mandate, Librarian merge-map/tagging; Blueprint/Theory vs. Practice).
- **State schema:** extraction-state.schema.md (optional merge_map_path).
- **Blueprint add-on:** Blueprint Extraction Protocol (add-on).md.
- **Patterns (v0.2.06):** Load strategy and workflow map; Phase 3 outputs merge-map.json when merges occur; validation-path-on-demand, data-files-store-paths-load-on-demand, phase-based-workflow-map-context-continuity.
