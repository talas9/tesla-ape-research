# Evidence and next work

## Reading the findings

| Label | Meaning |
|---|---|
| Recovered | Static branch or producer/consumer path in an inspected image. |
| Observed | Live capture result; RX producer and own-TX visibility may be unknown. |
| Reported | Attributed user, upstream or tool/test report. |
| Candidate | Layout or meaning awaiting matching hardware validation. |
| Open | Missing artifact, causal edge or semantic meaning. |

Raw captures and original artifacts remain private. Some reported addresses lack retained raw exports; the public notes are summaries rather than a standalone reproduction package.

## Implementation state

| Tool | Verification | Deployment |
|---|---|---|
| Host CLI | 42 targeted offline tests; reviewer ran 35; read-only live status/capture left settings unchanged. | Host tool checked live. |
| Actual backend TX journal | 16-event payload/submission/completion journal with boot cursor; 53 host cases / 8022 assertions; firmware build 1,161,872 bytes, 41% free; review clean. | Deployed. The journal describes scanner transport only. |
| Passive feature feedback | 5 IDs / 11 fields; 58 APE cases / 8302 assertions; 42 UI tests; build/check passed with no warnings; review clean. | Deployed with later additions. HW4 field applicability remains open. |

Test totals describe separate snapshots and are not additive.

## Open work

| Route | What is missing |
|---|---|
| HW4 request receiver | Matching `0x3FD` decoder and accepted-state mapping. |
| Positive acceptance indicator | Independent accepted/enabled state correlated with the request, separate from activity. |
| GUI writers and child checks | Remaining profile/reset/invalidation paths and matching input producers. |
| Managed-feature policy | Server decision logic beyond the recovered client parser and cache. |
| Hardware inference | Authoritative meaning of raw value 5; manual selection remains in use. |
| Feature sweep | Cases and combinations not reached before the interrupted run. |
| Driving visualization flicker | Submitted TX cadence and downstream rendering/receiver behavior. |
| `APP_w462` engagement refusal | Tesla documents the failed engagement attempt. The expanded Service Alert and any companion alerts from the same event may add context. [Official alert evidence](../LLM.md#tesla-alert-dictionary) |
| OTA and reconnect | Wi-Fi OTA and passive report delivery verified. Physical replug and AP-mode OTA remain open. [Release evidence](../LLM.md#november-12-diagnostics-and-source-limits) |

Signal consolidation is complete and independently reviewed: 3 tests, strict parsing and source-pointer checks passed for 55 selected IDs, 37 definitions, 2700 signals and 18 missing definitions. [Profiles](signal-profiles.md).

The [route index](route-index.md) tracks all branches, including documented GUI work that is currently parked. The issue comment remains a private draft. No Obsidian tool or destination was available; research persistence used local project artifacts.
