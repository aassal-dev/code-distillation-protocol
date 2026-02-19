# Step 3: Librarian — Consolidate and Publish to Library

## MANDATORY RULES (READ FIRST)

- ✅ **Read** all finding\_\*.md from `output_findings` (path from config). Process in batches if many files (e.g. by tag group or source path) to avoid context overflow.
- ✅ **Merge** duplicate findings (same insight, different wording). Keep one canonical pattern per concept.
- ✅ **Discard** “common knowledge” findings explicitly. **Record the count** of discarded items (in state or log). Keep only quality: cunning | novel | masterful.
- ✅ **Write** final artifacts to `output_library`: one file per pattern as `pattern_{kebab-name}.md` with YAML frontmatter (id, name, type, abstraction, quality, source_repo, source_path, tags) and sections: Problem, Solution, Code, AI Rule (single template, ordered pipeline).
- ✅ **Create or update** library/README.md (tag index): summary counts, semantic tag categories table, file naming convention, quick lookup by topic. This is the **library contract**.
- ✅ **Run** the validation checklist (checklist.md path from config) after writing library artifacts. Fix or document any failures.
- 🚫 **Do not** delete the findings folder until library artifacts are written and checklist has been run (write then validate).

## CONTEXT BOUNDARIES

- Paths: output_findings, output_library, validation (checklist) from workflow.yaml.
- Single template: same frontmatter and section order for every pattern (ordered-template-pipeline).
- Naming: pattern\_<kebab-name>.md; centralize naming so it matches Standardized Output Templates.

## YOUR TASK

Turn raw findings into a curated library: merge duplicates, drop common knowledge (and record count), format as pattern\_\*.md, and produce/update library/README.md. Then run the validation checklist.

## EXECUTION SEQUENCE

1. **Load** all finding\_\*.md from output_findings (in batches if needed).
2. **Merge** duplicates; decide canonical name and content.
3. **Quality filter:** For each finding, if quality is only “common knowledge” or equivalent, discard and increment discarded count. Keep only cunning, novel, or masterful.
4. **Write** each kept finding as library/pattern\_{kebab-name}.md using the single template (frontmatter → Problem → Solution → Code → AI Rule).
5. **Update or create** library/README.md:
   - Summary: source count, merged count, discarded count, final pattern count.
   - Semantic tag categories table.
   - File naming convention.
   - Quick lookup by topic (e.g. by tag or theme).
6. **Update** extraction-state.json: current_phase = 3, last_updated.
7. **Run** checklist.md (validation). Verify: plan mix of abstractions, finding filenames/frontmatter, no common-knowledge in library, README present and updated.
8. **Optionally** archive or clear findings folder only after checklist passes.

## REFERENCE

- **Template:** v0.2.0 Standardized Output Templates.md.
- **Checklist:** checklist.md (path from workflow.yaml validation key).
- **Pattern index:** library/README.md format per distillation/patterns/README.md (summary, tag categories, naming, quick lookup).
