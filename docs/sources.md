# Sources and attribution

## Inspected upstream source

[hypery11/flipper-tesla-fsd](https://github.com/hypery11/flipper-tesla-fsd) was inspected at a revision retained privately. Links below point to upstream `main`, which may change.

| Source | Finding in the inspected revision |
|---|---|
| [ForceFSD predicate](https://github.com/hypery11/flipper-tesla-fsd/blob/main/fsd_logic/fsd_can_ops.h#L59) | No HW4 exclusion. |
| [ESP32 handler](https://github.com/hypery11/flipper-tesla-fsd/blob/main/esp32/.firmware/fsd_handler.cpp#L393) | Assist bit 38 predicate precedes the hardware split; unlock writes 46/60 on HW4 and 46 on HW3. |
| [ForceFSD description](https://github.com/hypery11/flipper-tesla-fsd/blob/main/README.md#L102) | Describes bypassing GUI selection. |
| [Regression note](https://github.com/hypery11/flipper-tesla-fsd/blob/main/README.md#L243) | Version-specific HW4 failure report, not universal failure. |
| [Flipper C-handler Nav Lane path](https://github.com/hypery11/flipper-tesla-fsd/blob/main/fsd_logic/fsd_handler.c#L280) | Separate from the ESP32 reference; Nav/LaneGraph coverage is qualified as untested. |

## Upstream reports versus recovered code

[Discussion 163](https://github.com/hypery11/flipper-tesla-fsd/discussions/163) reports preflight aborts after modified VehicleCAN `0x3FD` messages while exact duplicates were tolerated. Party-only reportedly produced neither abort nor effect; SELF_DRIVING plus Restore did not prevent that case. The author corrected the event to turn-signal/intent, not an attempted lane change.

[Discussion 168](https://github.com/hypery11/flipper-tesla-fsd/discussions/168) presents an off-CAN regional-gate conclusion for newer versions. That is an upstream conclusion, not a condition independently recovered on the live platform. Its cited `0x39B` status candidate cannot be substituted for `0x399` without matching evidence.

## Firmware and signal references

Firmware and APE images acquired from Lunars.

Static findings use an ICE-based CID artifact and an older HW2.5 APE artifact. Modern candidates come from a separately recovered signal database. Original artifacts and database redistribution permissions are not established, so binaries, bulk definitions and decompilation exports remain excluded.

Live results are derived summaries of privately retained captures. Artifact availability and hardware limits are described in [methodology](methodology.md#artifact-availability).

## Recovered CH signal profiles

[tesla_can_signals](https://github.com/talas9/tesla_can_signals) is the verified repository origin recorded for the two recovered CH profile files used in consolidation. Their identical contents describe26messages and39unique signals, represented as78profile rows. Hardware and firmware identity are unknown; a directory label conflicts with the declared product label. The retained checkout reference does not prove file membership in that commit. This repository is distinct from the separately recovered canonical modern signal database; the latter is not attributed to this repository. Neither establishes current HW4 compatibility. No bulk definitions are republished here.
