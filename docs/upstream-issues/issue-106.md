# Issue 106

**A target-specific configuration helped, but the pin explanation contradicts its example.**

The reporter used a different microcontroller target from the original build. A [dedicated environment was reported working](https://github.com/hypery11/flipper-tesla-fsd/issues/106#issuecomment-4624137947). The maintainer accepted that explanation but restated a different transmit/receive pin pair from the posted environment and subsequently asked whether reception was confirmed. The [final reply corrects earlier termination advice](https://github.com/hypery11/flipper-tesla-fsd/issues/106#issuecomment-4634498973): the complete, already terminated vehicle network should not receive an extra terminator.

Review covered the issue and 6 visible comments. [Upstream discussion](https://github.com/hypery11/flipper-tesla-fsd/issues/106).
