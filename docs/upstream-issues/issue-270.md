# Issue 270

**A vanished dashboard client is a reported scheduling failure.**

On this upstream ESP32 setup, a stalled WebSocket broadcast shared a task with CAN reception. A vanished client delayed reception long enough for RX queues to overflow. The maintainer reports shipping a heartbeat and shorter send timeout in v2.17-beta.5. Beta.6 adds an optional second-core split, which requires a dual-core chip. The thread contains no post-fix result from the reporter. [Heartbeat release](https://github.com/hypery11/flipper-tesla-fsd/issues/270#issuecomment-6755001500), [second-core follow-up](https://github.com/hypery11/flipper-tesla-fsd/issues/270#issuecomment-6756597796).

Review covered the issue and 3 visible comments. [Upstream discussion](https://github.com/hypery11/flipper-tesla-fsd/issues/270).
