# Code Distillation Protocol (aka Knowledge Extraction)

Distill engineering wisdom from a codebase into a **Global Library** of atomic patterns and tricks. Output: optional learning objectives → extraction plan → findings → pattern library + README.

---

## Repository workflow

- **No pull requests.** Work is done on version branches (e.g. `v0.2.04`, `v0.2.05`).
- **Merges only after confirmation.** Do not merge any branch into `main` until the maintainer has confirmed.

---

## How to Use and Invoke v0.2.05 (Step by Step)

### Prerequisites

- **Target repository** — The codebase you want to distill (e.g. cloned locally). All outputs will be written _inside_ this repo under `_distillation/` (or the path you set as `extraction_root`).
- **Protocol folder** — The v0.2.05 workflow files live in **V0.2.05/**: `workflow.md`, `workflow.yaml`, `steps/` (step-00-director, step-01-survey, step-02-atomize, step-03-librarian), `checklist.md`, `extraction-state.schema.md`, plus templates and prompts. They can live in an Obsidian vault or anywhere you can point the agent. (Previous version: **V0.2.04/** — unchanged; use V0.2.05 for new runs.)
- **Execution environment** — Cursor (Composer, Cmd+I) or Claude Code. The protocol is run by an AI agent that reads the workflow and steps and executes them in order.

---

### Step 1: Open the target repository in your editor

1. Open **Cursor** (or your chosen editor).
2. **File → Open Folder** and select the **root of the repository you want to distill** (e.g. `E:\workspaces\_distillations\BMAD-METHOD`).
3. The agent will treat this folder as `{project-root}`. All outputs (extraction_plan.json, learning_objectives.md, findings/, library/) will go under this repo, typically in `_distillation/`.

You must have the **codebase to analyze** as the opened workspace. The protocol folder (**V0.2.05**) can be inside that repo, in a sibling folder, or in an Obsidian vault—as long as the agent can read those files (e.g. by path or by you pasting/attaching them).

---

### Step 2: Point the workflow at the target repo (config)

1. Open **workflow.yaml** from the **V0.2.05** folder.
2. Ensure **extraction_root** is set so the agent knows where the codebase lives:
   - If the protocol and the target repo are the same project: `extraction_root: "{project-root}"` (default).
   - If the target repo is elsewhere: set `extraction_root` to the absolute or relative path to that repo (the agent will resolve `{project-root}` from the opened folder).
3. Decide whether to use the **Director** (Phase 0):
   - **use_director: true** (default) — Run step-00 first: read README/docs (and optional Blueprint), produce `learning_objectives.md`, then Survey → Atomize → Librarian. Use this when you want learning intent to drive the extraction.
   - **use_director: false** — Skip step-00; start at Survey. The Surveyor will identify “nerve centers” by complexity instead of Learning Targets.
4. _(Optional)_ **Blueprint/theory-guided extraction:** If you have a theory document (e.g. a book chapter or paper summary in Markdown), set one of:
   - `blueprint_file: "path/to/theory.md"`
   - `theory_context_path: "path/to/theory.md"`
     The Director will use it to derive learning_objectives and can output `implementation_map.json`; the Atomizer can add “Theory vs. Practice” to findings. See **V0.2.05/Blueprint Extraction Protocol (add-on).md**.
5. _(Optional)_ Adjust **batch_size** (default 15) if the extraction plan has many targets; Phase 2 (Atomizer) will process in batches.
6. _(Optional)_ Add or edit **exclude_paths** (e.g. `node_modules`, `.git`, `dist`) so the Surveyor skips those directories.

Save workflow.yaml. Paths like `output_plan`, `output_objectives`, `output_findings`, `output_library` are resolved from `extraction_root`; normally they end up under `{extraction_root}/_distillation/`.

---

### Step 3: Invoke the workflow (run the agent)

1. Open **Cursor Composer** (Cmd+I on Mac, Ctrl+I on Windows/Linux).
2. **Copy the full “Protocol instructions” block** from **V0.2.05/v0.2.05 The Distillation Workflow (Orchestrator).md** (the section that starts with “Act as a **Reverse Engineering Agent**…” and includes Initialization, MANDATE, Phase 0–3, and References).
3. **Paste** it into the Composer chat.
4. Add one short instruction so the agent knows where the workflow lives, for example:
   - _“Use the Knowledge Extraction workflow v0.2.05. Workflow and config are in: [path to V0.2.05 folder].”_
   - Or, if V0.2.05 is inside the open repo: _“Use the workflow in V0.2.05/ in this repo.”_
5. If the agent cannot read the folder directly, **attach or paste** the contents of `workflow.md`, `workflow.yaml`, and the step files from **V0.2.05** so it has the full protocol.
6. **Send** the message. The agent will:
   - **Initialize:** Load workflow.md and workflow.yaml, resolve paths, and check for extraction-state.json (resume).
   - **Phase 0 (if use_director):** Run step-00-director → write learning_objectives.md (and optionally implementation_map.json).
   - **Phase 1:** Run step-01-survey → write extraction_plan.json.
   - **Phase 2:** Run step-02-atomize → for each target in the plan, write finding\_\*.md under \_distillation/findings/.
   - **Phase 3:** Run step-03-librarian → merge/dedupe, write pattern\_\*.md and README in \_distillation/library/, run checklist.

After Phase 1 the agent will ask: **[c] Continue to Atomize**, **[p] Pause** (review plan), or **[y] YOLO** (no further confirmations for this run).

---

### Step 4: What happens in each phase (summary)

| Phase             | Step file            | What the agent does                                                                                                                                                 | Main outputs                                                               |
| ----------------- | -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| **0 – Director**  | step-00-director.md  | Reads README/docs (and optional Blueprint); no code scan. Defines 3–7 Learning Targets.                                                                             | `_distillation/learning_objectives.md`; optional `implementation_map.json` |
| **1 – Surveyor**  | step-01-survey.md    | Scans repo; if learning_objectives.md exists, maps code to Learning Targets; else finds “nerve centers.” Builds work breakdown.                                     | `_distillation/extraction_plan.json`                                       |
| **2 – Atomizer**  | step-02-atomize.md   | For each target in the plan: reads source at `path`, extracts patterns/tricks, writes one finding per target. Optional “Theory vs. Practice” when Blueprint used.   | `_distillation/findings/finding_*.md`                                      |
| **3 – Librarian** | step-03-librarian.md | Reads all findings; merges duplicates; discards common knowledge; keeps only cunning/novel/masterful. Writes pattern library and README; runs validation checklist. | `_distillation/library/pattern_*.md`, `library/README.md`                  |

All paths are resolved from workflow.yaml (output_plan, output_objectives, output_findings, output_library). The agent must create output directories (e.g. \_distillation, \_distillation/findings, \_distillation/library) if they do not exist.

---

### Step 5: Where outputs go

- **Plan and objectives:** `extraction_plan.json` and `learning_objectives.md` → under `extraction_root`, typically `_distillation/`.
- **Findings:** One file per extraction target → `_distillation/findings/finding_<target_id>_<short_name>.md`.
- **Library:** Deduplicated patterns → `_distillation/library/pattern_<kebab-name>.md` and `_distillation/library/README.md`.
- **State (resume):** `extraction-state.json` → usually next to the plan (e.g. `_distillation/extraction-state.json`). Tracks current_phase, director_done, targets_done, findings_count, etc.

Do not delete the findings folder until the Librarian has written the library and the validation checklist has been run.

---

### Step 6: Resuming a run

If a run is interrupted or you want to continue later:

1. Leave **extraction-state.json** and existing outputs (learning*objectives.md, extraction_plan.json, finding*\*.md) in place.
2. Invoke the workflow again (Step 3). The agent will:
   - **Discover state:** Read extraction-state.json.
   - **Skip Phase 0** if use_director and director_done is true and learning_objectives.md exists.
   - **Skip Phase 1** if the plan already exists and state says Survey is done.
   - **Resume Phase 2** by processing only targets _not_ in `targets_done` (and updating state after each target or batch).
   - **Skip to Phase 3** if Phase 2 is complete (all targets in targets_done) and run the Librarian.

Resume behavior is defined in **V0.2.05/extraction-state.schema.md**. v0.2.05 adds an explicit post–Phase 1 ask (Continue / Pause / YOLO).

---

### Step 7: Optional features

- **Blueprint (theory-guided):** Set `blueprint_file` or `theory_context_path` in workflow.yaml. Director uses the theory to shape Learning Targets; Atomizer can add “Theory vs. Practice” to findings. See **V0.2.05/Blueprint Extraction Protocol (add-on).md**.
- **Batch size:** If extraction_plan has many targets (e.g. >15), set `batch_size` in workflow.yaml; Phase 2 will process in batches and update state between batches.
- **Exclude paths:** Edit `exclude_paths` in workflow.yaml so the Surveyor does not scan e.g. node_modules, .git, dist, build.
- **Validation:** After Phase 3, the agent runs **checklist.md** from the V0.2.05 folder. Fix any reported issues before treating the run as complete.

---

### Step 8: Quick reference (file locations)

**Current version (v0.2.05) — use for new runs:**

| Item                                                 | Location                                                    |
| ---------------------------------------------------- | ----------------------------------------------------------- |
| Workflow definition                                  | V0.2.05/workflow.md                                         |
| Config (paths, Director, Blueprint, batch, excludes) | V0.2.05/workflow.yaml                                       |
| Step 0 (Director)                                    | V0.2.05/steps/step-00-director.md                           |
| Step 1 (Surveyor)                                    | V0.2.05/steps/step-01-survey.md                             |
| Step 2 (Atomizer)                                    | V0.2.05/steps/step-02-atomize.md                            |
| Step 3 (Librarian)                                   | V0.2.05/steps/step-03-librarian.md                          |
| Orchestrator (paste into Composer)                   | V0.2.05/v0.2.05 The Distillation Workflow (Orchestrator).md |
| State schema                                         | V0.2.05/extraction-state.schema.md                          |
| Validation checklist                                 | V0.2.05/checklist.md                                        |
| Blueprint add-on                                     | V0.2.05/Blueprint Extraction Protocol (add-on).md           |
| Outputs (plan, objectives, findings, library)        | Under extraction_root, usually \_distillation/              |

**Previous version:** V0.2.04/ (unchanged; do not edit — use for reference or legacy runs).

---

## Summary

1. Open the **target repo** in Cursor.
2. Set **extraction_root** and **use_director** (and optional Blueprint) in **workflow.yaml**.
3. In **Composer**, paste the **Orchestrator** protocol from **V0.2.05** and tell the agent where the workflow folder is.
4. The agent runs Director (optional) → Survey → [ask: c/p/y] → Atomize → Librarian and writes plan, findings, and library under \_distillation/.
5. Use **extraction-state.json** to resume; run **checklist.md** after Phase 3 to validate.

For full step contracts, templates, and prompts, see the **V0.2.05** folder (workflow.md, step files, Standardized Output Templates, Codebase Distillation Prompts). v0.2.05 adds a mandate block, write-as-you-go rule, validation on-demand, workflow map, and post–Phase 1 ask (patterns from BMAD-METHOD distillation/library). The **V0.2.04** folder is kept unchanged for reference; new runs should use **V0.2.05**.
