# Knowledge Extraction — Validation Checklist (v0.2.06)

Run this checklist after Phase 3 (Librarian). Path: referenced from workflow.yaml key `validation`. Use it to verify the extraction run is complete and consistent.

## 0. Director (when use_director is true)

- [ ] **Learning objectives present:** learning_objectives.md is present at output_objectives path before Surveyor (step-01) was run. Number of Learning Targets is proportional to the size of the code (no fixed minimum or maximum).

## 1. Extraction plan

- [ ] **Plan exists:** extraction_plan.json is present at output_plan path.
- [ ] **Mix of abstractions:** Plan contains at least one target at each of: System, Module, Class, Line-Level (or at least Line-Level + one other).
- [ ] **Schema:** Every entry has required `target_id`, `path`, `focus`, `abstraction_level`; optional `concept_id` (v0.2.06). No `id`, `depth`, or other names.

## 2. Findings (pre–Phase 3)

- [ ] **Filenames:** Each finding file is named `finding_{target_id}_{short_name}.md` (kebab-case).
- [ ] **Frontmatter:** Each finding has YAML frontmatter with: id, name, type (pattern|trick), abstraction, quality (cunning|novel|masterful), source_repo, source_path, tags (optional: code_completeness, concept_id).
- [ ] **Sections:** Each finding follows the single template: Problem, Solution, Code, AI Rule. Optional: "Theory vs. Practice" when Blueprint was used.

## 3. Library

- [ ] **No common knowledge only:** No pattern in library has quality that is only "common knowledge"; discarded count recorded (in state or log).
- [ ] **Code completeness (v0.2.06):** No pattern has a Code section that consists only of comments or file references; each has at least a minimal working slice or a complete small example, or is explicitly marked as pointer-only (`code_completeness: pointer`).
- [ ] **Filenames:** Each pattern file is named `pattern_{kebab-name}.md`.
- [ ] **Frontmatter:** Each pattern has id, name, type, abstraction, quality, source_repo, source_path, tags (optional: code_completeness, concept_id).
- [ ] **Sections:** Each pattern uses the ordered pipeline: frontmatter → Problem → Solution → Code → AI Rule (and "Theory vs. Practice" when present).
- [ ] **Merge map / state transparency:** When Librarian produced more than one pattern from N findings (or merged findings), a merge map or equivalent is present (e.g. library/merge-map.json or merge summary in README).

## 4. Library README (tag index)

- [ ] **Present:** library/README.md (or patterns/README.md) exists under output_library.
- [ ] **Summary:** README includes summary counts (e.g. source findings, merged, discarded, final pattern count).
- [ ] **Tag categories:** README has a semantic tag categories table for retrieval.
- [ ] **Naming:** README documents file naming convention (pattern\_<kebab-name>.md).
- [ ] **Quick lookup (v0.2.06):** README includes quick lookup by topic (e.g. by tag or theme); every pattern appears in at least one bucket (no orphan patterns).

## 5. State (optional)

- [ ] **State file:** If using resume, extraction-state.json is updated (current_phase, last_updated; when use_director: director_done; and optionally targets_done, findings_count, discarded_common_knowledge_count, merge_map_path).
- [ ] **Orchestrator and workflow (v0.2.06):** Orchestrator includes mandate block and write-as-you-go mandate; workflow map (or section) present in workflow.md.

---

**Note:** Do not delete the findings folder until this checklist has been run and any critical failures are fixed or documented.
