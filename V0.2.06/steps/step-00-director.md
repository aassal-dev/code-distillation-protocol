# Step 0: Director — Set Learning Intent (Optional Phase 0)

**When:** Run only when `use_director: true` in workflow.yaml. Otherwise skip and start at step-01-survey.

## MANDATORY RULES (READ FIRST)

- ✅ **Output** learning_objectives.md to the path from config: `output_objectives` (e.g. `_distillation/learning_objectives.md`). Use only that path; do not hardcode.
- ✅ **Learning Targets:** Produce a number of Learning Targets **proportional to the size of the code** (no fixed minimum or maximum). Each target should be a concrete architectural or algorithmic question the Surveyor will answer by mapping code (e.g. "How does the Context Engine manage token limits during graph traversal?"). Surveyor and Atomizer process all listed targets.
- ✅ **No code scanning yet.** Director reads README, docs, and optional theory/Blueprint only; does not deep-scan source files.
- ✅ When a **Blueprint** (theory file) is provided via config (`blueprint_file` or `theory_context_path`): use it to derive learning_objectives; optionally output `implementation_map.json` (theory concept → file/line) to \_distillation if the workflow requests it.
- ✅ **Optionally** update extraction-state.json: `director_done: true`, `last_updated`.
- 🚫 Do not assume paths; use paths resolved from workflow.yaml (extraction_root, output_objectives).

## CONTEXT BOUNDARIES

- Variables from workflow.md and workflow.yaml: extraction_root, output_objectives, optional blueprint_file / theory_context_path.
- Scope: repository root (README, user-facing docs), plus any user-provided theory/Blueprint file. Do not scan full codebase; that is the Surveyor's job.
- Focus: core value proposition of the repo, unique features, and—when Blueprint is provided—mapping theory concepts to what we want to learn from the code.

## YOUR TASK

Before any code survey, define _what_ we are hunting for. Read README and docs (and optional Blueprint); produce a short learning_objectives.md with Learning Targets **proportional to the size of the code** (no fixed 3–7) so the Surveyor can map code to those targets.

## DISCOVERY SEQUENCE

1. **Read** README.md and any user-facing documentation under extraction_root.
2. **If** a Blueprint/theory file path is in config: read it and use it to shape Learning Targets (and optionally produce implementation_map.json).
3. **Analyze** the core value proposition: what does this repo do exceptionally well?
4. **Formulate** Learning Targets proportional to code size: specific questions or goals (e.g. "How is X implemented?", "Where does Y handle Z?"). No fixed minimum or maximum.
5. **Write** learning_objectives.md to output_objectives (path from config). Format: clear list or short sections; one target per item.
6. **Optional:** When Blueprint was used and implementation mapping is requested, write implementation_map.json to \_distillation (concept → path/line).
7. **Optionally** update extraction-state.json: director_done = true, last_updated.

## OUTPUT

- **Primary:** `learning_objectives.md` at output_objectives. Content: Learning Targets proportional to code size (specific questions or goals).
- **Optional (Blueprint runs):** `implementation_map.json` in \_distillation linking theory concepts to file paths (and optionally line numbers) for "Theory vs. Practice" in Atomizer.

## REFERENCE

- **Config:** workflow.yaml (use_director, output_objectives, blueprint_file, theory_context_path).
- **Blueprint add-on:** Blueprint Extraction Protocol (add-on).md.
