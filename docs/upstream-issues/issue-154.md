# Issue 154

**Third-bus receive load can disturb shared processing.**

The reporter describes an out-of-tree third CAN interface over a custom serial link. Local-bus behavior reportedly improves when third-bus reception stops while the heartbeat continues, including with a receive-only satellite. The [maintainer emphasizes transport and per-bus routing](https://github.com/hypery11/flipper-tesla-fsd/issues/154#issuecomment-5323459043); the [reporter's follow-up](https://github.com/hypery11/flipper-tesla-fsd/issues/154#issuecomment-5324073384) describes bounded work, nonblocking writes and explicit source assignment.

Review covered the issue and 4 visible comments. [Upstream discussion](https://github.com/hypery11/flipper-tesla-fsd/issues/154).
