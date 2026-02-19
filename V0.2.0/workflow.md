# Knowledge Extraction Workflow (v0.2.0)

**Goal:** Distill engineering wisdom from a codebase into a **Global Library** of atomic patterns and tricks (cunning, novel, masterful). Output: extraction*plan → findings → library/pattern*\*.md + library/README.md.

**Your Role:** Reverse Engineering Agent. Run phases sequentially; optionally pause after Phase 1 for confirmation. Support resume from state when re-running.

---

## WORKFLOW ARCHITECTURE

**Micro-file workflow:** One workflow (this file) plus one step file per phase.

- **step-01-survey.md** — Surveyor: scan repo, produce extraction_plan.json.
- **step-02-atomize.md** — Atomizer: for each target (or batch), extract findings and write finding\_\*.md immediately.
- **step-03-librarian.md** — Librarian: merge duplicates, discard common knowledge, write pattern\_\*.md to library, update library/README.md, run validation checklist.

Config and paths are **config-driven** via `workflow.yaml`. State is tracked in `extraction-state.json` for resume.

---

## INITIALIZATION

**Order (find-config-resolve-path-discover):**

1. **Find config** — Locate `workflow.yaml` (same folder as this workflow or path from runner). Load `extraction_root`, `output_plan`, `output_findings`, `output_library`, `validation`, optional `batch_size`, `exclude_paths`.
2. **Resolve paths** — Replace placeholders `{project-root}`, `{extraction_root}`, `{installed_path}` from config. Ensure output dirs exist (findings, library).
3. **Discover state** — If resuming: read `extraction-state.json` (same base as output_plan or configurable). Use `current_phase`, `targets_done`, `findings_count` to skip completed work.

Do not assume paths are hardcoded; resolve from config.

---

## EXECUTION

1. Load and execute **steps/step-01-survey.md** (Surveyor). Output: extraction_plan.json; optionally update extraction-state.json.
2. Load and execute **steps/step-02-atomize.md** (Atomizer). Input: extraction_plan.json; output: findings/\*.md per target; update state after each target or batch.
3. Load and execute **steps/step-03-librarian.md** (Librarian). Input: findings/_.md; output: library/pattern\__.md, library/README.md; run checklist; optionally archive or clear findings.

After Phase 3, validation checklist (checklist.md) is run; do not delete findings until Librarian has written and validated library artifacts.

---

## REFERENCE

- **Standardized Output Templates:** v0.2.0 Standardized Output Templates.md (single schema for finding and pattern).
- **Prompts:** v0.2.0 The Codebase Distillation Prompts.md (Surveyor, Atomizer, Librarian prompts with config/state/batching).
- **State schema:** extraction-state.schema.md.
