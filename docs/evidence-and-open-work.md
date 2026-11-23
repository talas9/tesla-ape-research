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

## Latest evidence milestone

Firmware 3.0 / UI 0.16.20 delivered a new warning report with independently aged receiver context. The November 18–19 windows include native Autosteer reason 91 and ACC reason 69 before, at and after `APP_w462`. A separate November 21 stored wake-up snapshot supplies native maps-support and vision values. A retained transmission journal also distinguishes source payloads from scanner-modified output. These are three separate evidence sets, not a synchronized causal trace. [Details and limits](../LLM.md#november-18-21-evidence-update)

## Open work

| Route | What is missing |
|---|---|
| HW4 request receiver | Matching `0x3FD` decoder and accepted-state mapping. |
| Positive acceptance indicator | Independent accepted/enabled state correlated with the request, separate from activity. |
| GUI writers and child checks | Remaining profile/reset/invalidation paths and matching input producers. |
| Managed-feature policy | Server decision logic beyond the recovered client parser and cache. |
| Hardware inference | The 2026.26.6.5 catalogue labels `GTW_dasHw` raw 5 `TESLA_AP4`. Target producer/layout applicability remains unverified; the scanner reports a candidate separately from configured hardware. |
| Feature sweep | Cases and combinations not reached before the interrupted run. |
| Driving visualization flicker | Submitted TX cadence and downstream rendering/receiver behavior. |
| `APP_w462` engagement refusal | The archived producer and service UI agree on the reason fields. Warning-aligned reason pairs are now captured. Attribution still needs the driver-input timestamp, synchronized eligibility/request values and the actual current HW4 predicate. [Diagnostic evidence](../LLM.md#november-13-eligibility-and-rejection-evidence) |
| OTA and reconnect | FW 3.0 / UI 0.16.20 diagnostic delivery is verified. This milestone is not a comprehensive OTA, AP-mode or physical-replug reliability test. [Latest evidence](../LLM.md#november-18-21-evidence-update) |
| Native eligibility | Stored maps-support and vision values are available; missing feature keys, warning-time values and the managed-feature decision producer remain open. |
| DevMenu | The pictured button exists in the supplied screenshot, but its concrete visibility/enabled binding and callback were not established in the inspected archived build. |

Signal consolidation is complete and independently reviewed: 3 tests, strict parsing and source-pointer checks passed for 55 selected IDs, 37 definitions, 2700 signals and 18 missing definitions. [Profiles](signal-profiles.md).

The [route index](route-index.md) tracks all branches, including documented GUI work that is currently parked.
