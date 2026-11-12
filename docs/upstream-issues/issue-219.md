# Issue 219

**The initial Wi-Fi diagnosis was corrected to stack overflow.**

The [first maintainer answer](https://github.com/hypery11/flipper-tesla-fsd/issues/219#issuecomment-6111013409) claims a captive-portal fix. The reporter says the portal improves but dashboard opening still triggers reconnects. The [maintainer correction](https://github.com/hypery11/flipper-tesla-fsd/issues/219#issuecomment-6152243528) attributes the loop to WebSocket-time task-stack overflow, explicitly admitting compiled-frame analysis without board testing, and separates another portal completion issue. The [reporter later confirms both tested clients work](https://github.com/hypery11/flipper-tesla-fsd/issues/219#issuecomment-6159033657).

Review covered the issue and 4 visible comments. [Upstream discussion](https://github.com/hypery11/flipper-tesla-fsd/issues/219).
