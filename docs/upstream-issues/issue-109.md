# Issue 109

**Sleep explanations changed as passive-mode and connection tests arrived.**

The initial report concerns a custom board remaining connected overnight. The [maintainer initially attributes it to Active-mode traffic and board-specific sleep support](https://github.com/hypery11/flipper-tesla-fsd/issues/109#issuecomment-4634489552). Later the [reporter describes periodic waking even in Listen-Only](https://github.com/hypery11/flipper-tesla-fsd/issues/109#issuecomment-4795782429), followed by a [connected-versus-detached observation](https://github.com/hypery11/flipper-tesla-fsd/issues/109#issuecomment-4808244283). The maintainer first calls the traffic vehicle-originated, then [accepts the physical connection as a remaining suspect](https://github.com/hypery11/flipper-tesla-fsd/issues/109#issuecomment-4846625160). A [later participant reports connection-dependent behavior on two boards](https://github.com/hypery11/flipper-tesla-fsd/issues/109#issuecomment-4946025228). The final [exchange distinguishes switched accessory power from an always-on supply](https://github.com/hypery11/flipper-tesla-fsd/issues/109#issuecomment-5172922283), and still describes idle-sleep support as board-specific.

Review covered the issue and 22 visible comments. [Upstream discussion](https://github.com/hypery11/flipper-tesla-fsd/issues/109).
