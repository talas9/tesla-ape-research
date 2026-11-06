# Sources and attribution

## Inspected upstream source

Repository: [hypery11/flipper-tesla-fsd](https://github.com/hypery11/flipper-tesla-fsd), inspected revision retained privately (identifier excluded here). Source remains in its upstream repository; this export contains no copied implementation.

- [ForceFSD predicate](https://github.com/hypery11/flipper-tesla-fsd/blob/main/fsd_logic/fsd_can_ops.h#L59): no HW4 exclusion in the inspected predicate.
- [ESP32 handler](https://github.com/hypery11/flipper-tesla-fsd/blob/main/esp32/.firmware/fsd_handler.cpp#L393): Assist bit 38 predicate precedes hardware split; separate unlock branch writes 46/60 on HW4 and 46 on HW3.
- [README ForceFSD description](https://github.com/hypery11/flipper-tesla-fsd/blob/main/README.md#L102): describes bypassing GUI selection.
- [README version-specific regression note](https://github.com/hypery11/flipper-tesla-fsd/blob/main/README.md#L243): version-specific, not a universal HW4 failure claim.
- [Flipper C-handler Nav Lane path](https://github.com/hypery11/flipper-tesla-fsd/blob/main/fsd_logic/fsd_handler.c#L280): do not attribute this path to the ESP32 reference. Nav/LaneGraph coverage is qualified as untested.

## Upstream reports versus recovered proof

[Discussion 163](https://github.com/hypery11/flipper-tesla-fsd/discussions/163) reports modified VehicleCAN 0x3FD preflight aborts on a separately reported platform/version while exact duplicates were tolerated. Party-only reportedly produced neither abort nor effect; SELF_DRIVING plus Restore did not prevent that reported case. The author corrected the event to turn-signal/intent, not an attempted lane change. These are scoped upstream reports.

[Discussion 168](https://github.com/hypery11/flipper-tesla-fsd/discussions/168) presents an upstream off-CAN regional-gate conclusion for newer versions. It is not my independently recovered current-car condition. A cited 0x39B low-nibble status candidate must not be silently substituted for target-platform 0x399.

## Tesla artifacts and signal references

Firmware and APE images acquired from Lunars.

Static findings derive from an ICE-based CID source artifact and an older HW2.5 APE source artifact. Modern candidate definitions derive from a separately recovered signal database. Tesla artifact/source rights and database redistribution permissions have not been established, so binaries, bulk definitions and decompilation exports are excluded. Private provenance remains outside this export; no original files or identifying hashes are included.

Current-car identity/version and historical symptoms are user-provided. Live findings are sanitized summaries of separately retained private captures. No public raw capture is supplied and no end-to-end current-HW4 acceptance proof is claimed.
