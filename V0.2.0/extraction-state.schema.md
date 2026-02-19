# extraction-state.json — Schema (v0.2.0)

This document describes the schema for **extraction-state.json**, used for resume and progress tracking. The file is optional; when present, the runner or in-editor protocol can skip completed phases or targets.

## Location

Typically next to the extraction plan, e.g. `_distillation/extraction-state.json`, or under the same base path as `output_plan` from workflow.yaml.

## Schema

| Field                              | Type     | Required | Description                                                                                                  |
| ---------------------------------- | -------- | -------- | ------------------------------------------------------------------------------------------------------------ |
| `current_phase`                    | number   | Yes      | 1 = Survey done, 2 = Atomize in progress or done, 3 = Librarian done.                                        |
| `plan_target_count`                | number   | No       | Number of targets in extraction_plan.json (set after Phase 1).                                               |
| `targets_done`                     | string[] | No       | List of `target_id` values already processed in Phase 2. Enables "resume from Phase 2" without re-surveying. |
| `findings_count`                   | number   | No       | Number of finding\_\*.md files written (approximate or exact).                                               |
| `last_updated`                     | string   | No       | ISO 8601 datetime of last state update.                                                                      |
| `batches_completed`                | number   | No       | If Phase 2 is batched, number of batches completed.                                                          |
| `discarded_common_knowledge_count` | number   | No       | Set in Phase 3; number of findings discarded as common knowledge.                                            |

## Example

```json
{
  "current_phase": 2,
  "plan_target_count": 24,
  "targets_done": [
    "sys-installer-orchestration",
    "mod-module-manager",
    "mod-config-collector"
  ],
  "findings_count": 7,
  "last_updated": "2026-02-18T14:30:00Z",
  "batches_completed": 1
}
```

## Resume behavior

- **Resume from Phase 2:** If `current_phase` >= 1 and extraction_plan.json exists, skip step-01-survey; run step-02 only for targets not in `targets_done`.
- **Resume from Phase 3:** If `current_phase` >= 2 and findings exist, skip step-01 and step-02; run step-03 only.
- **Full run:** If no state file or state is cleared, run step-01 → step-02 → step-03 in order.

Implementers: ensure state is written after Phase 1 (plan_target_count, current_phase), updated after each target or batch in Phase 2 (targets_done, findings_count), and updated after Phase 3 (current_phase, discarded_common_knowledge_count if applicable).
