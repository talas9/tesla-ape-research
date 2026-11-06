# Route index

Each route records the recovered result and the next missing link.

| Route | Scoped finding/status | Detail / next boundary |
|---|---|---|
| R01 GUI warnings | Immediate predicates recovered in CID source. | [Gates](trace-details.md#gui-warnings); matching live-platform code missing. |
| R02 Maps/localization | Distinct map-region, navigation-country and on-map paths. | [Inputs](trace-details.md#value-producers); runtime/map-content boundary. |
| R03 Managed features | Client request/response flow recovered; backend policy external. | [Inputs](trace-details.md#value-producers). |
| R04 Persistence | Feature cache/startup restore recovered in bounded source. | Actual runtime rows and paths not included or fully resolved. |
| R05 Original force-map settings | No consuming reads in bounded object/reference search. | Indirect/runtime/other-version absence not proved. |
| R06 Stock config/fuse | Consumers, defaults and environment gates partly recovered. | Named settings alone do not prove a viable override. |
| R07 GUI request writers | Confirmation/off/Autosteer and some reset/profile paths recovered. | Remaining writer/invalidation recursion open. |
| R08 Child predicates | Camera/calibration/park/cruise branches partly recovered. | Matching receive/timeout/source boundaries remain. |
| R09 Encoder/transport | CID eligibility rechecks, request assembly and sender recovered. | [Requests](trace-details.md#requests-and-transport). |
| R10 Gateway | Comparative receiver evidence only. | Matching target receiver/forwarding missing. |
| R11 Older APE receiver | Older request decoder/accepted state recovered. | Modern request-ID/current-HW4 equivalence open. |
| R12 Acceptance/status | Older ASM consumer and separate status publisher recovered. | [Chain](trace-details.md#older-ape-chain); independent modern acceptance oracle missing. |
| R13 Emitters | Identified registry: 52 entries/48 unique IDs. | Potential emissions, not observed/active message list. |
| R14 Passive shortlist | Partial ID presence observed. | Presence does not establish matching semantics or publisher. |
| R15 Short Stop/Start | Measured candidate operational state unchanged. | No acceptance/rejection inference. |
| R16 Restore | Already-target input produced no modification. | [Experiments](experiments.md). |
| R17 Assist | Modification/submission behavior recovered. | Different from GUI full-FSD selection or unlock. |
| R18 Hardware applicability | HW4 live, historical HW3 and HW2.5 source kept distinct. | [Matrix](research-paper.md#hardware-and-applicability-matrix). |
| R19 Passive diagnostics | Local implementation verified, not deployed. | [Implementation](evidence-and-open-work.md#implementation-state). |
| R20 Deployment | Not deployed. | Device validation remains. |
| R21 Debug setter | Generic setter/access path partly recovered. | Authoritative GUI propagation/viable override not proved. |
| R22 EU/DCAS/restart | Feature policy and conditional restart path partly recovered. | External sender/concrete runtime dependencies remain. |
| R23 Modern specification | Bounded extraction/review completed. | Candidate meanings, not matching HW4 decoder proof. |
| R24 Missing-modern layouts | Older packing groups recovered/reviewed. | Older semantics/current applicability incomplete. |
| R25 Semantic matching | Operational/warning/request directions distinguished. | No request bits relabelled as ACK. |
| R26 Visualization | Per-frame source/callback updates recovered. | Downstream rendering/current-HW4 behavior unresolved. |
| R27 Isolated phases | No stable receiver-candidate changes measured. | [Isolated experiment](experiments.md#isolated-features). |
| R28 Host CLI | Local tests/review and read-only checks passed. | Not the same harness as the interrupted sweep. |
| R29 Actual TX journal | Built/reviewed, not deployed. | Submission lifecycle is not ECU acceptance. |
| R30 Parked observation | Stable visualization during measured window. | [Stationary observation](experiments.md#stationary-visualization); driving flicker open. |
| R31 Feature feedback | Candidate mapping/implementation verified locally. | Not deployed; matching semantics open. |
| R32 Consolidation | Verification and independent review complete; no blocking findings. | [Profiles](signal-profiles.md); source network/provenance retained separately. |
| R33 Hardware inference | Not implemented; raw enum unresolved. | Preserve manual hardware selection. |
| R34 Follow-up sweep | Interrupted; original settings restored by verified recovery. | [Recovery](experiments.md#interrupted-follow-up-and-recovery); remaining cases not run. |
| R35 Upstream interactions | Bounded source review completed. | [References](sources.md); reports remain attributed upstream. |
