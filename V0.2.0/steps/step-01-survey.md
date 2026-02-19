# Step 1: Surveyor — Map the Territory

## MANDATORY RULES (READ FIRST)

- ✅ Output **only** valid JSON; no prose report. Save to path from config: `output_plan` (e.g. `_distillation/extraction_plan.json`).
- ✅ Use the **exact schema**: each entry has `target_id`, `path`, `focus`, `abstraction_level`. Use `abstraction_level` (not `depth` or `id`).
- ✅ **At least one target** must have `abstraction_level: "Line-Level"` (e.g. utils, parsers, math, regex).
- ✅ Ensure a **mix of abstraction levels**: System, Module, Class, Line-Level.
- ✅ Do **not** limit the number of targets; capture every area of high engineering complexity.
- 🚫 Do not assume paths; use paths resolved from workflow.yaml (extraction_root, exclude_paths).

## CONTEXT BOUNDARIES

- Variables from workflow.md and workflow.yaml are in memory (extraction_root, output_plan, exclude_paths).
- Scope: repository under extraction_root. Exclude by prefix: node_modules, .git, dist, build, and any exclude_paths from config.
- Focus on: core logic, custom algorithms, state management, performance layers, utilities, “metal” optimizations.
- Exclude: standard CRUD boilerplate, trivial UI, assets, generated code, plain config files.

## YOUR TASK

Scan the repository and produce a Work Breakdown Structure for deep analysis. Every listed target will be processed in Phase 2 (Atomizer).

## DISCOVERY SEQUENCE

1. **Scan** file tree, package.json / README, and key dirs (e.g. core, lib, utils, engine).
2. **Identify** “nerve centers”: complex logic, custom engines, performance layers, non-obvious patterns.
3. **For each** such area, add one JSON entry with:
   - `target_id`: unique kebab-case id (e.g. `mod-config-driven-ide`, `line-regex-escape`).
   - `path`: path relative to extraction_root (file or folder).
   - `focus`: what to look for (e.g. “Memory Management”, “API Design”, “Regex safety”).
   - `abstraction_level`: one of `"System"` | `"Module"` | `"Class"` | `"Line-Level"`.
4. **Write** the JSON array to `output_plan` (path from config). Ensure at least one Line-Level target.
5. **Optionally** create or update extraction-state.json: current_phase = 1, plan_target_count = N, last_updated = now.

## OUTPUT SCHEMA (single source of truth)

```json
[
  {
    "target_id": "unique-kebab-id",
    "path": "path/to/folder_or_file",
    "focus": "What specific aspect to look for",
    "abstraction_level": "System | Module | Class | Line-Level"
  }
]
```

Do not use `id`, `depth`, or other field names; only `target_id`, `path`, `focus`, `abstraction_level`.
