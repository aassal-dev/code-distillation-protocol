# Blueprint Extraction Protocol (add-on)

**Version:** v0.2.04 (add-on to Knowledge Extraction workflow)

**Type:** Protocol extension — theory-guided distillation.

**Purpose:** Use a theory document (e.g. book chapter, paper summary) to guide the Director and tie extraction to specific domain concepts. Produces learning_objectives aligned with the Blueprint and optional "Theory vs. Practice" sections in artifacts.

---

## Concept

Instead of only "find what's interesting" (unsupervised), you provide a **Blueprint** (theory file) and ask: _"Show me how this codebase implements or adapts Theory X."_ The Director uses the Blueprint to set Learning Targets; the Surveyor maps code to those targets; the Atomizer can add **Theory vs. Practice** to findings when a target maps to a theory concept.

In v0.2.04, the Blueprint is the optional high-level input for Phase 0 (Director). It is **add-on**: the core pipeline (Director → Survey → Atomize → Librarian) stays one path; Blueprint is enabled when the user supplies a theory file path in config or context.

---

## Prerequisites

1. **Codebase** — Target repo (e.g. cloned locally).
2. **Blueprint** — A Markdown file summarizing theory (e.g. book chapter, paper). Path can be set in workflow.yaml as `blueprint_file` or `theory_context_path`, or provided in chat.

---

## Workflow integration

- **Config (workflow.yaml):** Optional `blueprint_file: "path/to/theory.md"` or `theory_context_path: "..."`. When set, Director uses it to derive learning_objectives; can optionally output implementation_map.json (theory concept → file/line).
- **Director (step-00):** Reads Blueprint + README/docs; produces learning_objectives.md (3–7 Learning Targets derived from or aligned with the Blueprint). Optionally produces implementation_map.json.
- **Surveyor (step-01):** Uses learning_objectives.md to map code locations; focus in extraction_plan references Learning Targets.
- **Atomizer (step-02):** When implementation_map.json exists (or Blueprint was used), add **Theory vs. Practice** to findings that map to a theory concept: how the textbook theory was adapted, optimized, or worked around in this codebase.
- **Librarian (step-03):** Preserves "Theory vs. Practice" in pattern artifacts when present.

---

## Summary

- **When to use:** You have a theory document and want extraction focused on how the codebase implements or deviates from that theory.
- **Outputs:** learning_objectives.md (always when Director runs); optional implementation_map.json; pattern/finding artifacts with optional "Theory vs. Practice" section.
- **Reference:** workflow.md points to this add-on for Blueprint behaviour. Core workflow does not require a Blueprint; it is an extension on top.
