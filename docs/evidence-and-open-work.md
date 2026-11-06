# Evidence scope and open work

## Evidence classes

| Class | Meaning |
|---|---|
| Recovered | Bounded static branch or producer/consumer path in a named inspected image. |
| Observed | Derived current-car capture result, with unknown RX producer/own-TX visibility. |
| Reported | User/upstream observation or another lane's tool/test result; attributed accordingly. |
| Candidate | Layout/meaning awaits matching current hardware/version validation. |
| Open | Missing artifact, causal edge, semantic meaning or pending report. |

Private raw exports/captures remain excluded. Not every reported address has a retained raw export. Test/build totals below are separate snapshots, not additive counts.

## Implementation state

- Host CLI:42 targeted offline tests; reviewer 35; read-only live status/capture left settings unchanged.
- Actual backend TX journal:16 events, payload/submission/terminal lifecycle and boot cursor;53 hostcases/8022 assertions; IDF build 1,161,872 bytes/41%free; review clean. **Not deployed.** Current firmware lacks this command.
- Passive feature feedback:5 IDs/11 fields;58 APEcases/8302 assertions;42 UItests; check 0 warnings/build passed; review no blockers. **Not deployed.**

## Open work

| Route | Status | Next evidence |
|---|---|---|
| Matching HW4 request receiver | Open | Current 0x3FD receiver/acceptance mapping. |
| Positive acceptance oracle | Open | Independently mapped accepted/enabled state and correlation, separate from control activity. |
| GUI child/writer recursion | Parked/documented | Matching image and remaining profile/reset/child-input leaves. |
| Backend managed-feature policy | External boundary | Authoritative server decision logic; client parser/cache is insufficient. |
| Hardware inference | Not implemented | Authoritative raw 5 enum/profile mapping; preserve manual selection. |
| Consolidation | Verified and independently reviewed | 3 tests, strict parsing and source-pointer checks passed for 55 IDs, 37 definitions, 2700 signals and 18 missing IDs. |
| Live feature sweep | Interrupted, recovered | Remaining cases not run; review partial evidence before any separately routed test. |
| Driving visualization flicker | Unresolved | Actual backend TX cadence and matching receiver/display consumers under the relevant behavior. |
| Deployment | Pending | Separately authorized deployment and device validation; no new diagnostic deployment claimed. |

## Publication boundary

Publication integration is handled separately. The issue comment remains a private review draft and is excluded from this export. No raw-data upload is included.

This staging tree contains no repository-specific private branding or connection configuration. Review it before publishing; the included privacy audit is a bounded automated scan plus curated-content review, not a claim about unexported private data. No Obsidian destination/tool was available during this documentation lane; persistence used local project research artifacts.
