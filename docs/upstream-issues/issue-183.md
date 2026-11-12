# Issue 183

**Automatic hardware and OTA detection both misclassified input.**

A HW3 reporter says Auto selected HW4 and a wake-up-related OTA indication stopped operation. A [follow-up explicitly says FSD does not work and only Summon is used](https://github.com/hypery11/flipper-tesla-fsd/issues/183#issuecomment-5751648063), narrowing the original “everything works” wording. The [maintainer](https://github.com/hypery11/flipper-tesla-fsd/issues/183#issuecomment-5800562063) attributes the OTA latch to treating a changing `0x318` counter as an installing flag. It also says missing `0x398` led to a guess from `0x39B`, which can appear on HW3 too. A fix and preserved manual choice are described, without a final follow-up result.

Review covered the issue and 3 visible comments. [Upstream discussion](https://github.com/hypery11/flipper-tesla-fsd/issues/183).
