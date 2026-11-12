# Issue 104

**A single-ID filter improved a trace; later exports added explicit loss metadata.**

The issue attributes downsampled streaming to controller receive-queue overflow and implements single-ID hardware filtering plus a deeper general queue. TWAI and external-controller paths are announced separately. A [participant supplies a higher-rate single-ID trace](https://github.com/hypery11/flipper-tesla-fsd/issues/104#issuecomment-4611246025), and the [maintainer validates consecutive counters while noting a length discrepancy](https://github.com/hypery11/flipper-tesla-fsd/issues/104#issuecomment-4614833145). Later the [maintainer acknowledges the export lacks loss counters](https://github.com/hypery11/flipper-tesla-fsd/issues/104#issuecomment-5482270983), then [announces controller-missed and stream metadata](https://github.com/hypery11/flipper-tesla-fsd/issues/104#issuecomment-5560847152), distinguishing controller loss from stream-ring loss.

Review covered the issue and 8 visible comments. [Upstream discussion](https://github.com/hypery11/flipper-tesla-fsd/issues/104).
