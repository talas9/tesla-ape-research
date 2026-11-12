# Upstream issue evidence index

This index records reported outcomes, maintainer corrections and unresolved questions. Hardware, software and test setup limit how far each result applies.

## Coverage

The source snapshot included 123 issues and 1,287 visible comment bodies. This index contains 122 briefs. One thread was omitted to protect privacy. Review covers the visible discussion for each indexed issue. Linked discussions and attachments have separate coverage.

The November 12 REST and GraphQL recheck found four visible comments against five advertised on [#19](issue-19.md), and ten against fourteen on [#103](issue-103.md). GraphQL reported no next page in either case. The cause of the difference is unknown.

## Related discussions

The November 12 inventory listed 124 visible issues and 1,305 advertised comments. There are 123 public briefs including [new thread #276](issue-276.md), plus one thread withheld for privacy. The targeted recheck updates selected briefs and counts in the original table.

| Question | What the comments add |
|---|---|
| Does `APP_w462` identify a preflight check? | [#73](issue-73.md) reports the same alert after the FSD option appeared on a reported Intel 2026.14-series setup. The maintainer inferred rejection by a newer check. Tesla documents the alert as a generic engagement refusal. |
| What supports a vehicle-side check? | [Discussion #163](https://github.com/hypery11/flipper-tesla-fsd/discussions/163#discussioncomment-18156673) gives the maintainer's interpretation of a comparative HW4 2026.14.6 capture. The reporter corrected a lane-change observation. A later question about whether anyone had located the receiver code remained unanswered. [Correction](https://github.com/hypery11/flipper-tesla-fsd/discussions/163#discussioncomment-18159098), [receiver question](https://github.com/hypery11/flipper-tesla-fsd/discussions/163#discussioncomment-19498958) |
| Do the display and status tell us what engaged? | [#153](issue-153.md) reports visualization with engagement refusal on 2026.26.1 HW3 Model S Plaid. [#142](issue-142.md) reports restored status readout followed by continued refusal. [#129](issue-129.md) concerns a flickering availability icon and an RX-only timing gap. |
| What can scanner logs miss? | [#100](issue-100.md) leaves frame absence and capture loss indistinguishable in an erroring capture. [#191](issue-191.md) and [#270](issue-270.md) document recorder memory and dashboard scheduling limits on other hardware. |
| Do the tools install software or prove settings changed? | [#271](issue-271.md) separates Tesla OTA delivery from the project's scope. [#276](issue-276.md) separates observed DAS status from a vehicle setting change. |

| Thread | Visible comments | Main finding |
|---|---:|---|
| [1](issue-1.md) | 29 | Legacy naming mixed hardware generations; later reports show partial behavior and residual nagging |
| [2](issue-2.md) | 44 | Compatibility claims were revised; preflight remains an upstream attribution |
| [3](issue-3.md) | 6 | Entitlement claims change within the thread |
| [4](issue-4.md) | 2 | Broad regional compatibility claim without a result |
| [5](issue-5.md) | 3 | Termination advice needs board evidence |
| [7](issue-7.md) | 6 | HW3 predictions versus missing activation results |
| [8](issue-8.md) | 3 | Where the claimed confirmations came from |
| [9](issue-9.md) | 3 | Branch and hardware assumptions remain disputed |
| [10](issue-10.md) | 2 | HW2.5 support prediction later withdrawn |
| [12](issue-12.md) | 5 | Modified frames do not establish activation or its cause |
| [13](issue-13.md) | 7 | A structured HW3 success report is still self-reported |
| [14](issue-14.md) | 49 | Maintainer retracts no-subscription wording; competing architecture claims remain unverified |
| [15](issue-15.md) | 5 | An allegation is not a technical refutation |
| [17](issue-17.md) | 78 | Crash fixes lead to a reported refreshed-platform HW3 success, followed by software regression |
| [18](issue-18.md) | 169 | Reported TLSSC restoration, configuration-path successes and major retractions |
| [19](issue-19.md) | 4 | OTA false-positive diagnosis and claimed fix |
| [21](issue-21.md) | 14 | Connector pins were confirmed; the bus label was withdrawn |
| [22](issue-22.md) | 1 | Maintainer identifies inferred parsing and gateway gaps |
| [24](issue-24.md) | 1 | High-beam override remains an experiment proposal |
| [25](issue-25.md) | 2 | A lost toggle report conflicts with the closure description |
| [29](issue-29.md) | 19 | A reported HW3 success followed a corrected connection, with residual braking problems |
| [30](issue-30.md) | 2 | Startup crash discussion moved to another issue |
| [33](issue-33.md) | 15 | A crash fix was followed by an unresolved CAN-reception failure |
| [34](issue-34.md) | 2 | Availability and entitlement explanations remain assertions |
| [38](issue-38.md) | 24 | Correcting a hardware label did not establish FSD activation |
| [39](issue-39.md) | 12 | Nag suppression reports vary by tap and release; later explanations overgeneralize |
| [41](issue-41.md) | 4 | Speed-profile packing corrected to preserve validity |
| [42](issue-42.md) | 1 | A populated success report has a mismatched closure |
| [43](issue-43.md) | 31 | Scroll-button emulation has a reported HW4 success and later regression; state is corrected to feedback |
| [44](issue-44.md) | 2 | Bus visibility and error counts remain unresolved |
| [45](issue-45.md) | 1 | Reported operation despite a wrong hardware label |
| [48](issue-48.md) | 5 | Unknown hardware and connection diagnoses are hypotheses |
| [49](issue-49.md) | 4 | Historical refreshed-HW4 TLSSC result |
| [52](issue-52.md) | 34 | AP-First rollout does not resolve reported HW3 disengagement; connector advice is corrected |
| [53](issue-53.md) | 1 | Displayed release identity can differ from installed code |
| [54](issue-54.md) | 1 | Experimental toggles were explicitly untested |
| [55](issue-55.md) | 1 | Activation failure and errors versus a blank-template closure |
| [56](issue-56.md) | 2 | About scrolling is not resolved by a version-label fix |
| [58](issue-58.md) | 3 | Legacy-mode success report does not equate MCU and AP hardware |
| [59](issue-59.md) | 1 | A reported packing A/B exposes a validity-bit bug |
| [60](issue-60.md) | 13 | The maintainer retracts ban-prevention claims for configuration replay |
| [63](issue-63.md) | 2 | Pedal response is explicitly separated from power unlock |
| [64](issue-64.md) | 8 | An exact-build compatibility gap was explicitly acknowledged |
| [65](issue-65.md) | 0 | No substantive report |
| [66](issue-66.md) | 9 | A wrong signal recommendation was corrected; a driving-side override remains unvalidated |
| [67](issue-67.md) | 17 | A proposed removal became a rename without new standalone-benefit evidence |
| [68](issue-68.md) | 1 | Strategic TX-sunset proposal is not a universal receiver proof |
| [69](issue-69.md) | 1 | Persistent manual hardware override requested |
| [70](issue-70.md) | 11 | The maintainer retracts a guessed display-starvation diagnosis after a failed Nag test |
| [72](issue-72.md) | 0 | Translations acknowledged to contain obsolete claims |
| [73](issue-73.md) | 14 | A protection warning is manually configured; implementation changes did not establish a bypass |
| [75](issue-75.md) | 2 | Board-family support needs a matching target |
| [77](issue-77.md) | 0 | A filed-on-behalf success needs its source context |
| [79](issue-79.md) | 5 | A question was repeatedly mistaken for a compatibility result |
| [80](issue-80.md) | 21 | A genuine initial HW4 success report later became a failure report |
| [95](issue-95.md) | 23 | A recovered stalk checksum did not establish a working shared-bus engagement path |
| [96](issue-96.md) | 3 | Dual-bus architecture is distinct from feature acceptance |
| [100](issue-100.md) | 61 | Nag suppression: partial successes, changing preflight theories and corrected bus attribution |
| [101](issue-101.md) | 5 | Two modifications were distinguished in source |
| [103](issue-103.md) | 10 | Capture and status layout observations apply to the tested setup |
| [104](issue-104.md) | 8 | A single-ID filter improved a trace; later exports added explicit loss metadata |
| [106](issue-106.md) | 6 | A target-specific configuration helped, but the pin explanation contradicts its example |
| [108](issue-108.md) | 94 | HW3 activation successes, steering faults and the correction from RX-only to actual TX evidence |
| [109](issue-109.md) | 22 | Sleep explanations changed as passive-mode and connection tests arrived |
| [110](issue-110.md) | 25 | Hardware identity and working parser choice diverged; forcing HW4 could break nagging |
| [111](issue-111.md) | 5 | A feature report and later maintainer interpretation diverge |
| [112](issue-112.md) | 10 | The reporter corrects a false sequence: turning continued while the warning was active |
| [115](issue-115.md) | 10 | An empty capture was followed by real traffic, without a demonstrated activation result |
| [116](issue-116.md) | 1 | A reported status-layout variant affects tool gating |
| [117](issue-117.md) | 6 | An isolated tier rewrite had no UI effect, while the genuine broadcast remained present |
| [119](issue-119.md) | 7 | Purchased FSD does not settle the asserted region gate |
| [121](issue-121.md) | 10 | Capture-based explanations change, and a passive escalation is not an injection experiment |
| [122](issue-122.md) | 73 | Tracker records limited nag positives, failed activation and repeated parser/diagnostic corrections |
| [123](issue-123.md) | 2 | Shared event detector implementation reported |
| [124](issue-124.md) | 2 | Recorder specification changed at implementation |
| [125](issue-125.md) | 2 | Tap diagnostics distinguish presence from bus naming |
| [126](issue-126.md) | 2 | Profile suggestions explicitly require confirmation |
| [127](issue-127.md) | 0 | Field-readiness umbrella records scope, not a new test |
| [128](issue-128.md) | 1 | Body-message visibility does not prove actuation |
| [129](issue-129.md) | 9 | An engagement-timing fix evolved into experiments; RX-only logs could not verify injection timing |
| [132](issue-132.md) | 2 | No-activation report awaits tap evidence |
| [135](issue-135.md) | 4 | No confirmed Nag-only restriction is not a safety guarantee |
| [138](issue-138.md) | 5 | Continuous AP is not EAP entitlement |
| [139](issue-139.md) | 14 | The maintainer withdraws a claim that Summon limits cannot be changed by injection |
| [140](issue-140.md) | 3 | A capture offer becomes an attributed passive result |
| [141](issue-141.md) | 3 | Tool compatibility and network update access are separate |
| [142](issue-142.md) | 15 | Status reception improved, while Summon success and Restore failure remained distinct |
| [143](issue-143.md) | 8 | A template failure field contradicted the actual test |
| [153](issue-153.md) | 5 | A reported HW3 Force-on failure does not itself locate the preflight check |
| [154](issue-154.md) | 4 | Third-bus receive load can disturb shared processing |
| [158](issue-158.md) | 1 | ESP32 and Flipper feature parity was incomplete |
| [160](issue-160.md) | 8 | A Summon safety guard depends on a signal the tester lacks |
| [161](issue-161.md) | 1 | Regional Summon unlock is not missing-feature installation |
| [164](issue-164.md) | 3 | A status ID was initially mistaken for hardware identity |
| [173](issue-173.md) | 6 | A contributor-tested Summon recovery model is not the merged default |
| [177](issue-177.md) | 3 | A manual status map worked on a reported HW4 setup; a standard-parser update followed |
| [180](issue-180.md) | 1 | An Autopark-specific transmission guard followed a contributor’s warning report |
| [182](issue-182.md) | 1 | Preflight and remaining features are maintainer assertions |
| [183](issue-183.md) | 3 | Automatic hardware and OTA detection both misclassified input |
| [191](issue-191.md) | 18 | A reported nag reduction was followed by intermittent faults and an unresolved recording regression |
| [192](issue-192.md) | 3 | A compiled handler and an exposed toggle were different milestones |
| [193](issue-193.md) | 6 | HW3 interaction report and a corrected diagnosis |
| [206](issue-206.md) | 15 | An FD-board build existed, but its second controller remained unverified and failed probing |
| [209](issue-209.md) | 5 | Default profile rewriting is acknowledged and then corrected |
| [211](issue-211.md) | 8 | A pedal-map rewrite showed no reported effect; two absence-based diagnoses were corrected |
| [218](issue-218.md) | 7 | Read-only diagnostics shipped, with remaining timing and validation questions |
| [219](issue-219.md) | 4 | The initial Wi-Fi diagnosis was corrected to stack overflow |
| [223](issue-223.md) | 5 | Board labels and driver targets must be separate |
| [224](issue-224.md) | 1 | A dependency complaint is partly corrected |
| [225](issue-225.md) | 6 | Configuration saving and hostname each receive confirmation |
| [229](issue-229.md) | 2 | Broad compatibility claims include an explicit build gap |
| [237](issue-237.md) | 6 | A varied-pressure comparison resolves a source-specific wheel order |
| [250](issue-250.md) | 1 | A board-port request is not delivered support |
| [251](issue-251.md) | 2 | Battery decode is deliberately provisional |
| [252](issue-252.md) | 2 | Resetting displayed counters is reported working |
| [253](issue-253.md) | 2 | Per-bus counters receive a positive UI report |
| [254](issue-254.md) | 2 | Command status was explicitly separated from heating |
| [264](issue-264.md) | 1 | Follow-distance display is not the raw profile selector |
| [265](issue-265.md) | 1 | An entitlement question remains unanswered |
| [270](issue-270.md) | 3 | Maintainer confirms a scheduling failure and reports a heartbeat fix |
| [271](issue-271.md) | 3 | Activation flags do not install a new software image |
| [272](issue-272.md) | 0 | A compatibility question is not a test |

[Machine-readable claim ledger](claims.json). The three main references for this research are [#49 and its historical HW4 TLSSC report](issue-49.md), [#193 and the revised HW3 diagnosis](issue-193.md), and [#2 and the maintainer’s preflight explanation](issue-2.md).
