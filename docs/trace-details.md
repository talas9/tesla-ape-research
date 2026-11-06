# Recovered trace details

These are independent summaries of bounded binary/source analysis. no proprietary binary or decompilation is included.

## GUI warnings

Image class: ICE-based CID source artifact. Relative file: `usr/tesla/UI/lib/libQtCarGUI.so.1.0.0`.

| Function | Recovered branch |
|---|---|
| `topLevelApFeaturesDependencyChanged` | FSD offer uses Autopilot/navigation capability and FSD control capability. Entry enablement additionally checks parked, FSD maps and cameras. |
| Main warning string /reference  | “Full Self-Driving (Supervised) requires updated navigation maps. Connect to WiFi to download.” Selected from the maps predicate branch. |
| `hasFSDNavMaps` | Reads `FEATURE_hasFullSelfDrivingNavMaps`. |
| `fsdStopsControlDependencyChanged` | TLSSC visibility, request clearing and toggle enablement are distinct branches; warning precedence is eligibility/region, maps, parked, calibration. |
| TLSSC warning  | “Traffic Light and Stop Sign Control is not supported in current region.” |
| `isFSDStopsControlAllowed` | Capability AND localized approval. |

A managed “maps supported” predicate differs from confirming a particular installed map package is outdated. The screenshots establish wording, not the current vehicle's exact failed gate.

## Value producers

- Internal ETH 0x7FFfields feed country, hardware, tier, package and map-region VAPI values through CRC-keyed DataValue registration. This does not prove physical bus routing or current-HW4 enum meanings.
- Feature-configuration response parsing writes a feature map; registered policy then updates feature DataValues. Persisted startup values exist, so an observed feature need not be a fresh backend response.
- Backend policy is not contained in the recovered client. It remains an external boundary.
- Localized-country checks use navigation country and on-map state, separate from gateway factory country. Map-match administrative country ultimately depends on installed map data.
- Map-region/wrong-region logic includes selected folder/version/mount guards and suppression. Some early returns retain the previous flag.
- Named stock force settings require consumer and environment/fuse analysis. Two original force-map objects had no consuming reads in the stated 15-ELF/30-object inspection; this is bounded negative evidence, not proof about all builds.

## Requests and transport

In the inspected GUI, FSD confirmation → writes `GUI_fsdControlEnabled=true`. TLSSC confirmation → writes `GUI_fsdStopsControlEnable=true`. The three-mode selector also has off/Autosteer writers; it does not simply write the TLSSC request.

Inspected VAPI `AutopilotControlMessage::assembleMessage`ssembles request fields after rechecking eligibility. Bit 38 is the effective TLSSC request. FSD control/availability bits 46/47 and DCAS-related bit 60 are separate. Stock interpretation in this build must not be used to declare another hardware/version's scanner handler wrong.

The CID sender packages a 10-byte UDP record with a BE16 DLC/ID header and 8 payload bytes for its gateway transport. Current matching target-platform forwarding/acceptance remains unproved. No device connection addresses are included in this export.

## Older APE chain

Image class: older HW2.5 APE source artifact. The recovered old 0x3EE path decodes TLSSC bit 38, applies its receiver state guard, and writes accepted state into RoadRunnerSignals. ASM reads the selected snapshot+0x4dc; other ArbiterTrajectoryOutput+0xdf0and RoadRunner+0x104conditions apply.

A toggle-loss branch produces internal reason 8 `STOPS_BUTTON_TOGGLE` and conditional HOLD state. A published ASM Boolean at+0x68 has no established CAN mapping. This is distinct from the 0x247abort source at ASM+0x84. Separately, the older 0x399emitter reads ASM/MissionPlanner state; its enum mapping is not the modern ACTIVE_FSD enum automatically.

Older visualization bit 37 is updated per received request and reaches a RoadRunner snapshot byte. That proved branch is not a boot-only latch. Rendering, timeout and current-HW4 consumers remain open.

## Workflow grid

| Stage | Positive evidence | Still missing |
|---|---|---|
| GUI eligibility | Inspected strings, callers and predicates. | Matching current-car code/runtime gate. |
| GUI action | Request setters and consumers. | Full writer/profile/reset recursion. |
| Scanner modifier | Eligible-frame mutations; Restore input can already match target. | Vehicle acceptance. |
| Submission | Counters and locally implemented journal. | ECU receipt/accepted state. |
| Older receiver | Old request→shared state→ASM reads. | Modern 0x3FD/current-HW4 equivalent. |
| Feedback | Older emitters and modern candidate definitions. | Independent request-correlated accepted/enabled/active oracle. |
