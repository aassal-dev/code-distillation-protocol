# Step 1: Surveyor — Map the Territory

## MANDATORY RULES (READ FIRST)

- ✅ Output **only** valid JSON; no prose report. Save to path from config: `output_plan` (e.g. `_distillation/extraction_plan.json`).
- ✅ Use the **exact schema**: each entry has `target_id`, `path`, `focus`, `abstraction_level`. Use `abstraction_level` (not `depth` or `id`).
- ✅ **At least one target** must have `abstraction_level: "Line-Level"` (e.g. utils, parsers, math, regex).
- ✅ Ensure a **mix of abstraction levels**: System, Module, Class, Line-Level.
- ✅ Do **not** limit the number of targets; capture every area of high engineering complexity.
- ✅ **When learning_objectives.md exists** (path from config `output_objectives`): cross-reference it; set each plan entry's `focus` to align with one or more Learning Targets. **When it does not exist:** use current behaviour (identify nerve centers by complexity; focus = what to look for).
- 🚫 Do not assume paths; use paths resolved from workflow.yaml (extraction_root, output_plan, output_objectives, exclude_paths).

## CONTEXT BOUNDARIES

- Variables from workflow.md and workflow.yaml are in memory (extraction_root, output_plan, output_objectives, exclude_paths).
- Scope: repository under extraction_root. Exclude by prefix: node_modules, .git, dist, build, and any exclude_paths from config.
- **When learning_objectives.md is present:** Focus on mapping code locations to the Learning Targets listed there; `focus` in the plan should reference those targets.
- **When learning_objectives.md is absent:** Focus on: core logic, custom algorithms, state management, performance layers, utilities, "metal" optimizations.
- Exclude: standard CRUD boilerplate, trivial UI, assets, generated code, plain config files.

## YOUR TASK

Produce a Work Breakdown Structure for deep analysis. When learning_objectives.md exists, tie each target's focus to those Learning Targets; otherwise identify nerve centers by complexity. Every listed target will be processed in Phase 2 (Atomizer).

## DISCOVERY SEQUENCE

1. **If** learning_objectives.md exists at output_objectives: read it and list the Learning Targets.
2. **Scan** file tree, package.json / README, and key dirs (e.g. core, lib, utils, engine).
3. **Identify** areas that answer the Learning Targets (when objectives exist) or "nerve centers" by complexity (when not).
4. **For each** such area, add one JSON entry with:
   - `target_id`: unique kebab-case id (e.g. `mod-config-driven-ide`, `line-regex-escape`).
   - `path`: path relative to extraction_root (file or folder).
   - `focus`: what to look for—when objectives exist, reference the Learning Target(s); otherwise e.g. "Memory Management", "API Design", "Regex safety".
   - `abstraction_level`: one of `"System"` | `"Module"` | `"Class"` | `"Line-Level"`.
5. **Write** the JSON array to `output_plan` (path from config). Ensure at least one Line-Level target.
6. **Optionally** create or update extraction-state.json: current_phase = 1, plan_target_count = N, last_updated = now.

## OUTPUT SCHEMA (single source of truth)

```json
[
  {
    "target_id": "unique-kebab-id",
    "path": "path/to/folder_or_file",
    "focus": "What specific aspect to look for (or reference to Learning Target)",
    "abstraction_level": "System | Module | Class | Line-Level"
  }
]
```

Do not use `id`, `depth`, or other field names; only `target_id`, `path`, `focus`, `abstraction_level`.
