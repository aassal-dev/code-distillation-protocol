# Step 2: Atomizer — Extract Atomic Patterns and Tricks

## MANDATORY RULES (READ FIRST)

- ✅ **Read** extraction_plan.json from config path `output_plan`. If missing, do not invent; report and stop (or fall back to discovery per manifest-as-source-of-truth pattern).
- ✅ **One finding file per distinct insight.** Filename: `finding_{target_id}_{short_name}.md` in `output_findings`. Use kebab-case for short_name.
- ✅ **Write immediately** after each target (write-as-you-go). Do not accumulate all file contents in context; write then optionally summarize for next target.
- ✅ Use the **single** Standardized Output Template (v0.2.05): frontmatter (id, name, type, abstraction, quality, source_repo, source_path, tags) and sections: Problem, Solution, Code, AI Rule. **When Blueprint was used:** add optional **"Theory vs. Practice"** section to the finding (how textbook theory was adapted in this codebase).
- ✅ Every finding must have **source_path** (where in repo) and **quality** (cunning | novel | masterful). No generic "common knowledge" only.
- ✅ **Update** extraction-state.json after each target or batch: append to targets_done, update findings_count, last_updated.
- 🚫 Do not use a different template or field set; one schema for all findings.

## CONTEXT BOUNDARIES

- extraction_plan.json is the source of truth for targets (target_id, path, focus, abstraction_level).
- When implementation_map.json exists (Blueprint run), use it to enrich extraction: add "Theory vs. Practice" where the target maps to a theory concept.
- Paths (output_plan, output_findings) come from workflow.yaml; do not hardcode.
- If plan has many targets (e.g. > batch_size from config), process in **batches** (e.g. by directory or by abstraction_level). Per batch: read → extract → write → update state → purge full content from context.

## YOUR TASK

For each target in extraction*plan.json (or each batch): read the source at `path`, extract atomic patterns/tricks per `focus` and `abstraction_level`, and write one or more finding*\*.md files using the single template. When this run used a Blueprint, add a "Theory vs. Practice" section to relevant findings.

## FRACTAL LEVELS (what to look for)

- **System:** Queue architecture, caching layers, microservice communication.
- **Module:** Clean architecture, dependency injection, factory patterns.
- **Class:** Smart API surfaces, ergonomic method signatures.
- **Line-Level:** Bitwise ops, regex tricks, loop unrolling, masterful use of language features (e.g. decorators, lifetimes). For Line-Level, explain **how** it works and **why** it is faster/safer than the standard approach.

## EXECUTION SEQUENCE

1. Load extraction_plan.json from output_plan. If implementation_map.json exists (Blueprint run), load it for Theory vs. Practice context.
2. For each target (or batch of targets):
   - Read source at `path` (relative to extraction_root).
   - Identify patterns/tricks matching focus and abstraction_level.
   - For each distinct finding: write `finding_{target_id}_{short_name}.md` to output_findings using the Standardized Output Template; include source_path and quality. When Blueprint was used and the target maps to a theory concept, add a **Theory vs. Practice** section (how theory was adapted in this codebase).
   - Update extraction-state: targets_done, findings_count, last_updated.
3. Do not delete or overwrite extraction_plan.json. Do not proceed to Phase 3 until all targets are processed (unless resuming and some are already in targets_done).

## REFERENCE

- **Template:** v0.2.05 Standardized Output Templates.md (single template; optional Theory vs. Practice for Blueprint runs).
- **Prompts:** v0.2.05 The Codebase Distillation Prompts.md — Atomizer section (focus, fractal levels, filename convention, Theory vs. Practice).
