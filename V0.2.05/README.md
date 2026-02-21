# Knowledge Extraction — v0.2.05

This folder is the **v0.2.05** release of the Code Distillation Protocol (Knowledge Extraction). Use everything in this folder together; do not mix with files from other version folders (e.g. V0.2.06).

---

## What’s in this folder

| Item                                                    | Purpose                                                                                          |
| ------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| **workflow.md**                                         | Workflow definition: phases, init, execution, references.                                        |
| **workflow.yaml**                                       | Config: paths, `use_director`, `extraction_root`, `validation`, `batch_size`, `exclude_paths`.   |
| **checklist.md**                                        | Validation checklist run after Phase 3 (Librarian). Referenced by `validation` in workflow.yaml. |
| **steps/**                                              | One step file per phase: step-00-director, step-01-survey, step-02-atomize, step-03-librarian.   |
| **v0.2.05 Standardized Output Templates.md**            | Single template for findings and patterns (frontmatter + sections).                              |
| **v0.2.05 The Codebase Distillation Prompts.md**        | Prompt text for Director, Surveyor, Atomizer, Librarian.                                         |
| **v0.2.05 The Distillation Workflow (Orchestrator).md** | In-editor driver: copy into Composer to run the full workflow.                                   |
| **extraction-state.schema.md**                          | Schema for extraction-state.json (resume).                                                       |
| **Blueprint Extraction Protocol (add-on).md**           | Optional theory-guided extraction (Blueprint/theory file).                                       |

---

## How to use this version

### 1. Point the run at this folder

All paths in **workflow.yaml** are resolved relative to your repo. The **validation** path is `{installed_path}/checklist.md`. So when you run the protocol:

- **installed_path** must be the folder that contains this README (the V0.2.05 folder).

If you run from the **repo you are distilling**, set in workflow.yaml:

- **extraction_root** — repo to distill (e.g. your project root or a path like `E:\...\openclaw`).
- **installed_path** — path to this V0.2.05 folder so `validation` resolves to `V0.2.05/checklist.md`.

Example: if the protocol lives in an Obsidian vault and you run from the repo:

- `extraction_root: "E:\workspaces\_distillations\auto-assistant-ai\openclaw"`
- `installed_path: "D:/wkspaces/Obsidian Vaults/.../V0.2.05"`  
  so that `validation: "{installed_path}/checklist.md"` points at the checklist inside V0.2.05.

### 2. Run in Cursor (in-editor)

1. Open the **repository you want to distill** in Cursor (the codebase, not the protocol folder).
2. Open **v0.2.05 The Distillation Workflow (Orchestrator).md** from this V0.2.05 folder.
3. Copy the **full “Protocol instructions” section** (from “Act as a Reverse Engineering Agent…” through the References) into Cursor Composer (Cmd+I).
4. In the same message (or in config), tell the agent where the **V0.2.05 folder** is, e.g.:  
   _“Use the workflow and steps from: D:\wkspaces\Obsidian Vaults\...\V0.2.05”_
5. Run. After Phase 1 (Survey) you’ll get: [c] Continue, [p] Pause, [y] YOLO.

The agent will read **workflow.md**, **workflow.yaml**, and **steps/** from this folder and write outputs under **extraction_root** (e.g. `_distillation/` in the repo).

### 3. Config you must set

In **workflow.yaml** in this folder:

- **extraction_root** — Path to the repo you are distilling (e.g. your project root or a clone).
- **installed_path** — Path to **this** V0.2.05 folder (so validation uses this folder’s checklist).

Other paths (`output_plan`, `output_objectives`, `output_findings`, `output_library`) are usually under `{extraction_root}/_distillation/` and only need changing if you use a different layout.

---

## Outputs (where they go)

By default, everything is written under **extraction_root**:

- `_distillation/learning_objectives.md` (if Director runs; 3–7 Learning Targets)
- `_distillation/extraction_plan.json`
- `_distillation/findings/*.md`
- `_distillation/library/pattern_*.md`, `library/README.md`
- `_distillation/extraction-state.json` (optional, for resume)

Validation uses **checklist.md** from this folder (via `validation: "{installed_path}/checklist.md"`).

---

## Newer version

For **Code completeness**, **merge-map**, **concept_id**, **proportional Learning Targets**, and updated tagging/README rules, use the **V0.2.06** folder instead.
