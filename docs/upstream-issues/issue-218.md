# Issue 218

**Read-only diagnostics shipped, with remaining timing and validation questions.**

The maintainer prioritized reliability and read-only diagnostics over a larger actuation fork. The [implementation summary](https://github.com/hypery11/flipper-tesla-fsd/issues/218#issuecomment-6162159501) describes a bus hint, CAN statistics, a source-bus correction for Continuous AP traffic and quieter persistence behavior. It treats the bus probe as a hint rather than an authoritative identity test, and proposes chunked black-box saves to avoid blocking reception. A later dashboard report shows diagnostics and saved records, while the [last maintainer response](https://github.com/hypery11/flipper-tesla-fsd/issues/218#issuecomment-6291880596) still asks for save-time information.

Review covered the issue and 7 visible comments. [Upstream discussion](https://github.com/hypery11/flipper-tesla-fsd/issues/218).
