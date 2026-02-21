# Step 3: Librarian — Consolidate and Publish to Library

## MANDATORY RULES (READ FIRST)

- ✅ **Read** all finding\_\*.md from `output_findings` (path from config). Process in batches if many files (e.g. by tag group or source path) to avoid context overflow.
- ✅ **Merge** duplicate findings (same insight, different wording). Keep one canonical pattern per concept. When a pattern is merged from multiple findings, keep **one** primary source_repo/source_path (e.g. the most canonical finding); optionally list others in an "Also derived from" line in the pattern body or in the merge-map.
- ✅ **Discard** "common knowledge" findings explicitly. **Record the count** of discarded items (in state or log). Keep only quality: cunning | novel | masterful.
- ✅ **Write** final artifacts to `output_library`: one file per pattern as `pattern_{kebab-name}.md` with YAML frontmatter (id, name, type, abstraction, quality, source_repo, source_path, tags; optional code_completeness, concept_id) and sections: Problem, Solution, Code, AI Rule (single template, ordered pipeline). **Preserve** any "Theory vs. Practice" section present in findings when copying into library patterns.
- ✅ **Code enrichment (optional, v0.2.06):** When promoting a finding to a pattern, if the Code section is **thin** (comment-only or single reference), the Librarian may **enrich** by extracting one minimal slice from the source at source_path and appending it to the Code block, with a one-line comment that it was extracted for stand-alone use.
- ✅ **Create or update** library/README.md (tag index): summary counts, semantic tag categories table, file naming convention, **quick lookup by topic** so that **every pattern appears in at least one bucket** (no orphan patterns). One line per pattern in quick lookup by topic/tag.
- ✅ **Merge map (v0.2.06):** When merging N findings into patterns, produce `library/merge-map.json` (or equivalent under output_library). Schema: map from finding identifier (e.g. `finding_lt-1-hitl_hitl-async-suspension` or target_id + short_name) to pattern id (e.g. `pattern_hitl-async-suspension-via-gateway`) or `"new:<pattern_id>"` when the finding created a new pattern. Enables audit trail (e.g. "72 findings → 14 new patterns") and traceability from learning target → finding → pattern.
- ✅ **Tagging:** Prefer existing tags from the README semantic tag table; add new tags only when no existing tag fits, and consider adding the new tag to the README table in the same run.
- ✅ **Run** the validation checklist (checklist.md path from config) after writing library artifacts. Fix or document any failures.
- 🚫 **Do not** delete the findings folder until library artifacts are written and checklist has been run (write then validate).

## CONTEXT BOUNDARIES

- Paths: output_findings, output_library, validation (checklist) from workflow.yaml.
- Single template: same frontmatter and section order for every pattern (ordered-template-pipeline). When a finding includes "Theory vs. Practice", keep that section in the corresponding pattern.
- Naming: pattern\_<kebab-name>.md; centralize naming so it matches Standardized Output Templates.

## YOUR TASK

Turn raw findings into a curated library: merge duplicates, drop common knowledge (and record count), format as pattern\_\*.md, and produce/update library/README.md. Preserve "Theory vs. Practice" in pattern artifacts when present. Then run the validation checklist.

## EXECUTION SEQUENCE

1. **Load** all finding\_\*.md from output_findings (in batches if needed).
2. **Merge** duplicates; decide canonical name and content. When merging, choose one primary source_repo/source_path per pattern; note others for "Also derived from" or merge-map.
3. **Quality filter:** For each finding, if quality is only "common knowledge" or equivalent, discard and increment discarded count. Keep only cunning, novel, or masterful.
4. **Optional Code enrichment:** For any finding whose Code section is thin (comment-only or single reference), optionally extract one minimal slice from source_path and append to the Code block (with a one-line comment that it was extracted for stand-alone use).
5. **Write** each kept finding as library/pattern\_{kebab-name}.md using the single template (frontmatter → Problem → Solution → Code → AI Rule). When a finding has a "Theory vs. Practice" section, include it in the pattern. When a pattern was merged from multiple findings, use one primary source; optionally add "Also derived from: finding_X, finding_Y" in the pattern body.
6. **Write** library/merge-map.json: map each finding identifier to the resulting pattern id (or "new:<pattern_id>"). Place under output_library.
7. **Update or create** library/README.md:
   - Summary: source count, merged count, discarded count, final pattern count.
   - Semantic tag categories table.
   - File naming convention.
   - Quick lookup by topic (e.g. by tag or theme)—**one line per pattern so every pattern appears in at least one bucket.**
8. **Update** extraction-state.json: current_phase = 3, last_updated; optionally merge_map_path.
9. **Run** checklist.md (validation). Verify: plan mix of abstractions, finding filenames/frontmatter, no common-knowledge in library, Code completeness, merge-map when applicable, README present and updated.
10. **Optionally** archive or clear findings folder only after checklist passes.

## REFERENCE

- **Template:** v0.2.06 Standardized Output Templates.md (code_completeness, concept_id, "Also derived from").
- **Checklist:** checklist.md (path from workflow.yaml validation key).
- **Pattern index:** library/README.md format per distillation/patterns/README.md (summary, tag categories, naming, quick lookup with no orphan patterns; tagging: prefer existing tags, add new only when needed).
