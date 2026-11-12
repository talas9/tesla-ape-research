# Issue 191

**A reported nag reduction was followed by intermittent faults and an unresolved recording regression.**

A 2020 HW3 Model 3 report on 2026.32.7 found valid checksums and a healthy reported EPAS state during the displayed error. The recorder’s sequence gaps leave the cause open. The maintainer separately corrected the save-failure explanation: on an ESP32/MCP2515 board without PSRAM, the recorder could not start at the available memory level. The maintainer says beta.5 uses a shorter window. There is no follow-up confirming the fix on the reporter’s board. [Capture](https://github.com/hypery11/flipper-tesla-fsd/issues/191#issuecomment-6291782914), [recorder correction](https://github.com/hypery11/flipper-tesla-fsd/issues/191#issuecomment-6755001908).

Review covered the issue and 18 visible comments. [Upstream discussion](https://github.com/hypery11/flipper-tesla-fsd/issues/191).
