# Why 72 Learning Targets Produced Only 14 New Library Patterns

## Short answer

The Librarian step **merges duplicates**: it keeps **one canonical pattern per concept**. The library already had **81 patterns** from earlier runs (17 OpenClaw targets + legacy BMAD extraction). Most of the 72 learning targets (LTs) describe the **same or very similar concepts** as those 81 patterns, so they were **merged into existing patterns** instead of creating new files. Only **14** LTs referred to concepts that had **no** existing pattern, so only 14 new `pattern_*.md` files were added.

---

## The rule: one pattern per concept

From the workflow (step-03-librarian):

- **Merge** duplicate findings (same insight, different wording). Keep **one canonical pattern per concept**.
- **Write** each _unique_ concept as `pattern_{kebab-name}.md`.

So: **72 findings** (one per LT) are first reduced to **unique concepts**; each unique concept becomes **one** library pattern. If a concept already had a pattern from a previous run, we do **not** create a second file—we consider that finding “merged” into the existing pattern.

---

## Where the 81 existing patterns came from

Before this 72-LT run:

- A **previous extraction** had already run with **17 learning targets** (e.g. `sys-gateway-sidecar-startup`, `mod-command-lane-queue`, `mod-exec-approval-hitl`, `mod-heartbeat-runner`, `mod-compact-head-tail`, `mod-skills-frontmatter`, etc.).
- That run (and possibly others) plus **legacy BMAD** patterns filled the library with **81** `pattern_*.md` files.

So many of the 72 LTs were **revisiting the same code and the same ideas** that had already been turned into patterns—just with different labels (LT1, LT2, …) and slightly different wording.

---

## Examples: LTs merged into existing patterns (no new file)

These LTs did **not** get a new pattern because an existing pattern already captured the same concept.

| LT       | Learning target (concept)                                        | Existing pattern it merged into                                                                                 |
| -------- | ---------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **LT1**  | HITL async suspension (pause run, webhook, callback before exec) | `pattern_hitl-async-suspension-via-gateway.md`                                                                  |
| **LT2**  | Lane Queue concurrency (serial fs.write, lanes)                  | `pattern_lane-queue-generation-fencing.md`                                                                      |
| **LT3**  | Heartbeat daemon scheduler (HEARTBEAT.md, setInterval/cron)      | `pattern_heartbeat-daemon-scheduler.md`                                                                         |
| **LT5**  | Context compaction (slice middle, preserve head/tail)            | `pattern_context-compaction-head-tail-preservation.md`                                                          |
| **LT6**  | Synthetic “omitted” for pruned tool calls                        | `pattern_tool-result-truncation-with-suffix.md` + `pattern_protocol-file-rehydration-audit-after-compaction.md` |
| **LT11** | DOM → accessibility tree, `[ref=x]`                              | `pattern_semantic-role-snapshot-with-stable-element-refs.md`                                                    |
| **LT12** | Shell command AST / dangerous-flag blocking                      | `pattern_token-structural-safe-bin-argv-validation.md`                                                          |
| **LT15** | Gateway WebSocket + TypeBox schemas                              | `pattern_typebox-protocol-schemas.md`                                                                           |
| **LT19** | Markdown/YAML → AgentSkills tool schemas                         | `pattern_markdown-to-schema-agent-skills.md`                                                                    |
| **LT22** | Workspace bootstrap merge (bootstrap-extra-files)                | `pattern_workspace-bootstrap-merge.md`                                                                          |
| **LT69** | DeepSeek `<think>` tag regex / reasoning strip                   | `pattern_code-aware-reasoning-tag-stripping.md`                                                                 |

So for example: **LT1** (“Where does the code pause … fire a webhook … wait for callback before executing exec?”) and **LT2** (“Where is the global lock ensuring serial fs.write …?”) were **not** turned into new patterns; they were treated as the same concepts as the existing “HITL async suspension” and “Lane queue generation fencing” patterns.

---

## Examples: LTs that got new patterns (14)

These 14 LTs described concepts for which **no** existing library pattern existed, so a **new** `pattern_*.md` was created.

| LT       | Concept                                                             | New pattern file                                |
| -------- | ------------------------------------------------------------------- | ----------------------------------------------- |
| **LT29** | Circuit breaker for model endpoints (cascading 500/529, fallback)   | `pattern_circuit-breaker-model-endpoints.md`    |
| **LT31** | Prompt injection canary (regex guard before sending to model)       | `pattern_prompt-injection-canary-regex.md`      |
| **LT35** | CAPTCHA detection and user-yielding (HITL “solve captcha”)          | `pattern_captcha-detection-user-yielding.md`    |
| **LT38** | Streaming API key redaction (TransformStreams mask sk-ant-\*)       | `pattern_streaming-api-key-redaction.md`        |
| **LT49** | Distributed gateway mesh (multi-node discovery, state sync)         | `pattern_distributed-gateway-mesh.md`           |
| **LT50** | Zero-trust data plane (no keys to telemetry, authz per resource)    | `pattern_zero-trust-data-plane.md`              |
| **LT51** | Multi-agent orchestration protocol (lifecycles, budgets, aggregate) | `pattern_multi-agent-orchestration-protocol.md` |
| **LT52** | Execution sandbox monitor (resource limits around child_process)    | `pattern_execution-sandbox-monitor.md`          |
| **LT53** | Virtual File System (chroot-like barrier for AI file tools)         | `pattern_virtual-file-system-abstraction.md`    |
| **LT55** | Plugin registry + VirusTotal (fetch, verify, load community skills) | `pattern_plugin-registry-virustotal.md`         |
| **LT56** | command-logger hook (intercept shell I/O → markdown for AI)         | `pattern_command-logger-hook.md`                |
| **LT57** | boot-md initialization sequencer (order of ops on cold start)       | `pattern_boot-md-initialization-sequencer.md`   |
| **LT63** | RateLimiter token bucket (avoid spamming external APIs)             | `pattern_rate-limiter-token-bucket.md`          |
| **LT72** | Core context freeze (`Object.freeze(coreContext)` for plugins)      | `pattern_object-freeze-core-context.md`         |

So: **72 LTs** → **72 findings** → after merge, **14 new concepts** → **14 new pattern files**. The other **58** findings were considered to describe concepts already covered by the 81 existing patterns (or by other findings in the same run that merged into one of the 14 new ones).

---

## Why so many merges?

1. **Overlap with the previous 17-target run**  
   The earlier extraction had already produced patterns for HITL, lane queue, heartbeat, context compaction, tool-result truncation, rehydration audit, skills frontmatter, gateway protocol, workspace bootstrap, semantic role snapshot, safe-bin argv, etc. The 72 LTs repeat many of these (e.g. LT1–LT4, LT5–LT6, LT11–LT12, LT15, LT19, LT22, LT69).

2. **Same concept, different granularity**  
   Some LTs are “where is the exact line that does X?” (e.g. LT68 WAL PRAGMA, LT70 exec config, LT71 CDP stealth script). Those are line-level or implementation details of a broader pattern (e.g. SQLite usage, process execution, browser stealth). They were merged into the broader pattern rather than given a separate “one-line” pattern file.

3. **Narrow vs broad patterns**  
   The library prefers one pattern per _concept_ (e.g. “rate limit retry & backoff”, “graceful teardown”). So LT18 (rate limit retry) can sit under a broader “provider resilience” idea, and LT30 (graceful teardown) under “lifecycle/shutdown”. If an existing pattern already covered that concept, no new file was created; only when the concept was genuinely new (e.g. circuit breaker, zero-trust data plane, VFS, plugin registry + VirusTotal) did we add a new pattern.

---

## Summary table

| Category                                   | Count | What happened                                       |
| ------------------------------------------ | ----- | --------------------------------------------------- |
| **72 learning targets**                    | 72    | Each turned into one finding (`finding_lt-*_*.md`). |
| **Existing library (before this run)**     | 81    | Patterns from 17-target run + legacy BMAD.          |
| **Findings merged into existing patterns** | 58    | Concept already had a canonical pattern.            |
| **New concepts (no existing pattern)**     | 14    | New `pattern_*.md` files created.                   |
| **Library after this run**                 | 95    | 81 + 14.                                            |

So: **72 targets → 72 findings → merge by concept → 14 new patterns** (and 58 findings merged into existing or new patterns). The 72 learning targets are all still represented in the library; they are just **consolidated** into one pattern per concept rather than 72 separate pattern files.
