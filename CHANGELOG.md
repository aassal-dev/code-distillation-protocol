# Version history — Knowledge Extraction System

## v0.2.04 (current)

**Theme:** Consolidation of v0.2.0 + v0.2.03. Optional Director (Phase 0), learning_objectives.md, use_director flag, optional Blueprint extension, Theory vs. Practice in artifacts.

- **Director (optional Phase 0):** New step-00-director.md. When `use_director: true` (default), Director reads README/docs and optional Blueprint → produces learning_objectives.md (3–7 Learning Targets). Optional implementation_map.json when Blueprint provided. Config: `use_director`, optional `blueprint_file` / `theory_context_path`.
- **Surveyor (step-01):** When learning_objectives.md exists, extraction_plan focus is tied to Learning Targets; otherwise unchanged behaviour (nerve centers by complexity). Schema unchanged: target_id, path, focus, abstraction_level.
- **Atomizer (step-02):** Optional "Theory vs. Practice" section in findings when Blueprint run; Librarian preserves it in pattern artifacts.
- **workflow.yaml:** use_director (default true), output_objectives path, optional blueprint_file, theory_context_path. All v0.2.0 paths and options retained.
- **workflow.md:** Describes optional Phase 0, execution branch (step-00 when use_director else start at step-01), reference to Blueprint add-on.
- **State schema:** director_done, phase names director/survey/atomize/librarian. Resume behaviour for Director and all phases.
- **Checklist:** When use_director, learning_objectives.md present before Surveyor.
- **Blueprint Extraction Protocol (add-on):** Short doc for theory-guided extraction; referenced from workflow.md. Core pipeline one path; Blueprint is extension on top.
- **Templates and prompts:** v0.2.04 Standardized Output Templates (optional Theory vs. Practice), v0.2.04 The Codebase Distillation Prompts (Director + conditional Surveyor + Atomizer + Librarian), v0.2.04 The Distillation Workflow (Orchestrator), v0.2.04 Strategy Evaluation (4-phase / 3-phase, Precision/Intent, config, state, checklist, batching, tag index).
- **Naming and schema:** Everywhere target_id and abstraction_level (no id/depth). All v0.2.0 structure preserved.

---

## v0.2.0

**Theme:** Workflow structure, config, state, resume, validation, batching, tag index.

- **Workflow split:** Single orchestrator replaced by `workflow.md` (overview, init, execution) plus `steps/step-01-survey.md`, `step-02-atomize.md`, `step-03-librarian.md` with MANDATORY RULES, CONTEXT BOUNDARIES, and clear YOUR TASK / DISCOVERY SEQUENCE.
- **Config:** `workflow.yaml` for `extraction_root`, `output_plan`, `output_findings`, `output_library`, `validation` path, optional `batch_size` and `exclude_paths`.
- **State and resume:** `extraction-state.json` with `current_phase`, `plan_target_count`, `targets_done[]`, `findings_count`, `last_updated`; optional resume from Phase 2 without re-surveying.
- **Validation:** `checklist.md` for plan mix of abstractions, finding filenames/frontmatter, no common-knowledge in library, library README present and updated.
- **Batching:** Phase 2 supports batching by directory or abstraction when many targets; Librarian can process findings in batches to avoid context overflow.
- **Library index:** Librarian produces/updates `library/README.md` (or `patterns/README.md`) with summary counts, semantic tag categories table, file naming convention, quick lookup by topic.
- **Single source of truth:** One extraction_plan schema (`target_id`, `path`, `focus`, `abstraction_level`), one finding/pattern template (YAML + Problem/Solution/Code/AI Rule), one Standardized Output Templates doc.
- **Naming:** Consistent `finding_{target_id}_{short_name}.md` and `pattern_{kebab-name}.md`; frontmatter requires `source_path`, `quality` (cunning|novel|masterful).
- **Init order:** Find config → resolve paths → discover existing plan/state (find-config-resolve-path-discover pattern).

---

## v0.1.0

**Status:** First Minor Release — In-Editor "Composer" Workflow.

- **Artifacts:** Orchestrator (in-editor driver), Surveyor/Atomizer/Librarian prompts, Standardized Output Templates, Strategy Evaluation, "Gemini Said" meta-notes.
- **Flow:** Phase 1 → extraction_plan.json; Phase 2 → findings/\*.md per target; Phase 3 → merge/discard, move to library/.
- **Output schema:** extraction_plan uses target entries; findings become pattern\*.md with YAML frontmatter and AI Rule.
- **Execution:** Single-file paste into Cursor Composer; no config file, no state file, no validation checklist, no library README contract.
