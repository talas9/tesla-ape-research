# Issue 33

**A crash fix was followed by an unresolved CAN-reception failure.**

The initial explanation blamed a firmware fork, but the reporter also reproduced the failure on official firmware. A claimed working older build was then withdrawn because it contained custom changes. The [maintainer later identified a pointer-size bug](https://github.com/hypery11/flipper-tesla-fsd/issues/33#issuecomment-4304211209); the [reporter confirmed the crash disappeared](https://github.com/hypery11/flipper-tesla-fsd/issues/33#issuecomment-4305887813), while packet reception still failed. Subsequent clock, mode and tap explanations did not produce a confirmed final resolution in this thread.

Review covered the issue and 15 visible comments. [Upstream discussion](https://github.com/hypery11/flipper-tesla-fsd/issues/33).
