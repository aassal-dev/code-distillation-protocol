# extraction-state.json — Schema (v0.2.05)

This document describes the schema for **extraction-state.json**, used for resume and progress tracking. The file is optional; when present, the runner or in-editor protocol can skip completed phases or targets.

## Location

Typically next to the extraction plan, e.g. `_distillation/extraction-state.json`, or under the same base path as `output_plan` from workflow.yaml.

## Schema

| Field                              | Type     | Required | Description                                                                                                                                                   |
| ---------------------------------- | -------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `current_phase`                    | number   | Yes      | 0 = Director done (if used), 1 = Survey done, 2 = Atomize in progress or done, 3 = Librarian done. Phase names in docs: director, survey, atomize, librarian. |
| `director_done`                    | boolean  | No       | When use_director is true: set to true after step-00 (Director) has produced learning_objectives.md.                                                          |
| `plan_target_count`                | number   | No       | Number of targets in extraction_plan.json (set after Phase 1 / Survey).                                                                                       |
| `targets_done`                     | string[] | No       | List of `target_id` values already processed in Phase 2. Enables "resume from Phase 2" without re-surveying.                                                  |
| `findings_count`                   | number   | No       | Number of finding\_\*.md files written (approximate or exact).                                                                                                |
| `last_updated`                     | string   | No       | ISO 8601 datetime of last state update.                                                                                                                       |
| `batches_completed`                | number   | No       | If Phase 2 is batched, number of batches completed.                                                                                                           |
| `discarded_common_knowledge_count` | number   | No       | Set in Phase 3; number of findings discarded as common knowledge.                                                                                             |

## Example

```json
{
  "current_phase": 2,
  "director_done": true,
  "plan_target_count": 24,
  "targets_done": [
    "sys-installer-orchestration",
    "mod-module-manager",
    "mod-config-collector"
  ],
  "findings_count": 7,
  "last_updated": "2026-02-20T14:30:00Z",
  "batches_completed": 1
}
```

## Resume behavior

- **Resume from Phase 0 (Director):** If use_director and director_done is already true and learning_objectives.md exists, skip step-00; proceed to Survey.
- **Resume from Phase 2 (Survey done):** If current_phase >= 1 and extraction_plan.json exists, skip step-00 (if used) and step-01; run step-02 only for targets not in `targets_done`.
- **Resume from Phase 3 (Librarian):** If current_phase >= 2 and findings exist, skip step-00, step-01, and step-02; run step-03 only.
- **Full run:** If no state file or state is cleared, run step-00 (when use_director) then step-01 → step-02 → step-03 in order.

Implementers: ensure state is written after Director when used (director_done), after Phase 1 (plan_target_count, current_phase), updated after each target or batch in Phase 2 (targets_done, findings_count), and updated after Phase 3 (current_phase, discarded_common_knowledge_count if applicable).
