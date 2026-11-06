# Consolidated research knowledge base

This file is a self-contained account of the recovered firmware paths, measurements and remaining gaps. It is intended for technical readers and LLMs interpreting this research. It contains derived findings, not firmware source, a complete decoder, a bypass recipe or a claim that the current vehicle accepted a modified request. AI-assisted reverse engineering can produce mistakes; preserve the evidence qualifications below. See [methods and authorship limits](docs/methodology.md).

## 1. Evidence contract and source profiles

| Label | Meaning |
|---|---|
| Recovered | A bounded static branch, definition, packing operation or producer/consumer relationship was identified in an inspected artifact. |
| Observed | A result was measured in a specific capture or intervention cohort. It does not identify every RX producer. |
| Reported | An attributed contributor/upstream/tool result, with verification limited to the retained evidence. |
| Candidate | A layout or interpretation awaiting matching hardware/firmware validation. |
| Open | A missing causal edge, artifact, semantic label or runtime value. |

Profiles must remain separate:

- **CID-ICE:** a modern ICE-based MCU/CID artifact. It contains GUI, VAPI, configuration, navigation and managed-feature client code. Its build identifies a separate source platform; shared-code equivalence with a different MCU or HW4 is not established.
- **APE-HW25:** an older HW2.5 APE artifact. It provides concrete request decoding, accepted shared state, ASM consumers and CAN emitters. It is neither an HW3 image nor an HW4 image.
- **Modern schema:** a separate MCU-derived signal reference. Its internal confidence labels describe the source extraction, not compatibility with the live test platform or physical network routing.
- **Live-HW4:** stationary captures on the test platform with manual HW4 selection. The capture itself does not independently establish its hardware identity. RX producer attribution and visibility of the board's own TX are unknown.
- **Historical HW3:** prior platform context, not a paired current HW3/HW4 validation experiment.
- **Comparative gateway:** the analyzed newer gateway image belongs to a separate source platform, correcting an earlier platform attribution. An older gateway artifact exists but does not establish matching modern forwarding.

Firmware and APE images acquired from Lunars. The contributor reported these were the latest images available to them; no MCU3/HW4 images or newer HW3/HW2.5 APE images were available to them. This is a source-availability statement, not an independent audit. Source-class years differ: modern ICE work and older APE work must not be collapsed into one release.

Public evidence excludes original Tesla files, bulk databases, raw captures, decompilation exports, identifying hashes and personal vehicle information. Function offsets below are artifact-local coordinates, not portable addresses. Some earlier findings survive as agent tool-output reports without retained raw exports; the newer APE wire edge has retained assembly and ELF-backed checks. No full-image completeness claim follows from a symbol scan.

## 2. Main conclusions

1. The main maps warning comes from a managed **maps-support Boolean**, not a direct finding that an installed navigation package is old.
2. FSD/TLSSC eligibility combines several authorities: gateway configuration, managed features, navigation localization, maps, camera/runtime state and saved requests. “The backend decides everything” is too broad.
3. Confirming a GUI selection, encoding a request, backend TX completion, receiver acceptance, feature availability and active control are different states.
4. Two promising force-map variable names had no consuming reads in the bounded inspected set. Other stock force keys have consumers, but configuration environment and caching determine whether a file could affect them.
5. Older APE evidence proves an accepted TLSSC-dependent state reaches ASM and a diagnostic CAN output. It does not establish a dedicated TLSSC ACK or matching HW4 behavior.
6. Stationary interventions changed board processing/TX counts but did not yield a demonstrated receiver acceptance/rejection indicator. Reported driving flicker and reboot asymmetry remain untested by those stationary comparisons.

## 3. Code ownership and initial warning paths

Installed artifact-relative locations:

| Component | Role |
|---|---|
| `usr/tesla/UI/bin/QtCar` | Primary GUI/feature DataValue definitions and service publisher. |
| `libQtCarGUI.so.1.0.0` | GUI callbacks, capability predicates, FeatureConfigManager, profile/reset logic and map controller. |
| `libQtCarVAPI.so.1.0.0` | Request assembly, vehicle-link sender, packet decoders and replicated values. |
| `libQtCarUtils.so.1.0.0` | DataValue registration/persistence, configuration loading, runtime environment and restart helpers. |
| `libQtCarUIFramework.so.1.0.0` | Generic confirmation-switch executor. |
| `QtCarTMServer`, `QtCarGpsManager`, `libvalhalla.so.0.0.0` | Map-matched administrative country, coordinates and on-map state. |
| APE `canrx`, `autopilot_state_machine`, `cantx` | Older request receiver, accepted-state consumers and output packing. |

### 3.1 Main FSD entry

CID-ICE `TopLevelAPSelectSetting::topLevelApFeaturesDependencyChanged` at `0x38d5b90` selects the warning:

> Full Self-Driving (Supervised) requires updated navigation maps. Connect to WiFi to download.

Recovered logic:

- Offer FSD when `hasAutopilotAndNavData() && hasFSDControl()`.
- The offered FSD entry additionally needs parked state, `hasFSDNavMaps()` and `hasFSDCameraHardware()` to be enabled.
- Offered entry with a false maps predicate selects the maps warning. The selector can disable its tail entries rather than disabling the entire control.
- `hasFSDNavMaps()` at `0x37072e0` directly reads `FEATURE_hasFullSelfDrivingNavMaps`, registered under `fsd_navigation_supported`.
- `hasAutopilotAndNavData()` is `forceHasAutosteer || (hasAutopilot() && navigationDataAvailable())`; not every nested capability leaf is closed.
- `hasFSDCameraHardware()` at `0x3707340` returns false only for a valid `APP_teslaVisionUnsupportedReason` with raw2; invalid input takes its permissive branch. This differs from the separate vision branch inside `hasFSDControl()`.

`hasFSDControl()` at `0x3724d50` first rejects missing steering capability. Then force-allow precedes force-deny. Otherwise it combines configured-country approval, HW3-or-higher, `FEATURE_fullSelfDriving` and `hasFSDAutopilot()`. A validity-sensitive vision branch can clear eligibility: valid supported raw1 with unsupported-reason raw1 clears the hardware result, whereas reason raw0 logs without clearing it. Raw branches are recovered; current-platform enum equivalence is not implied.

### 3.2 TLSSC entry

`FSDStopsControl::fsdStopsControlDependencyChanged` at `0x38ea100` uses separate visibility, enablement and invalidation branches. Its region warning is:

> Traffic Light and Stop Sign Control is not supported in current region.

- `isFSDStopsControlAllowed()` at `0x37258b0` = `hasFSDStopsControl() && isStopsLocationApproved()`.
- `hasFSDStopsControl()` at `0x3725760` rejects legacy DCAS and a valid feature-package raw1; otherwise requires configured-country approval and `hasFSDAutopilot()`.
- Configured-country approval requires HW3-or-higher and the inspected approved group or `isEuropeanVehicle()`. This is not the localized-country test.
- `isStopsLocationApproved()` at `0x371e6c0` requires one of the inspected localized approved groups and HW3-or-higher.
- Localized membership requires valid `GUI_navVehicleCountry`, `GUI_isOnNavigationMap` and country-list membership. The helper named localized-EU includes 51 numeric codes; its name does not mean literal EU membership and is distinct from configured European-vehicle eligibility.
- `hasFSDStopsMaps()` at `0x37088f0` directly reads `FEATURE_dasHasFSDStopsMaps`, registered as `fsd_stops_maps`.
- `isStopsCamCalibrated()` at `0x37079b0` requires main, fisheye and narrow camera calibration values each valid and true; no repeater check occurs in this helper.
- Ordinary non-cruise toggle enablement requires allowed, maps, parked and calibrated. An existing enabled request with cruise active takes another branch disabling the toggle.
- Warning precedence is allowed/region → maps → parked → calibration.
- Failure of allowed/maps/calibration clears `GUI_fsdStopsControlEnable` at `0x38ea19f`. Parked state affects the UI but does not guard that invalidation write.
- Section visibility uses `shouldShowTaccCustomizationSettings()` and remains a separate, incompletely expanded predicate.

[Detailed GUI flow diagrams](docs/trace-details.md#read-the-flows).

## 4. Managed-feature source, policy and persistence

The recovered client path is:

`feature_configuration response OR persisted startup state → GUI_featureConfig → registered FeatureConfigInfo rules → FEATURE_* → VehicleUtils predicates`.

| Key | Destination | Hardware support | UpdateBehavior raw |
|---|---|---|---:|
| `full_self_driving` | `FEATURE_fullSelfDriving` | HW3-or-higher | 1 |
| `fsd_navigation_supported` | `FEATURE_hasFullSelfDrivingNavMaps` | HW3-or-higher | 1 |
| `fsd_stops_maps` | `FEATURE_dasHasFSDStopsMaps` | Autopilot AND HW3-or-higher | 0 |
| `enable_eu_fsd_behavior` | `FEATURE_enableEUFSDBehavior` | HW3-or-higher | 0 |

Main three registrations have FactoryBehavior1, ConfigBehavior1 and LatchBehavior0. Defaults/absent keys resolve false; ConfigBehavior1 preserves the parsed Boolean. Unsupported hardware and the recovered factory policy can produce false. The EU behavior feature additionally uses RestartBehavior1; the main three use RestartBehavior0.

`shouldUpdate` at `0x25ba060` is `(effectiveFactory || updateRaw==1 || (CD_displayState.valid && raw==0) || (support!=1 && currentBoolDV)) && !latched`. `effectiveFactory` combines `GUI_factoryMode` with the runtime in-factory marker. LatchBehavior0 makes the target latch check false. Display raw0's semantic name remains open. These raw policy values should not acquire guessed enum names.

`FeatureConfigManager` constructor `0x25bab20` subscribes feature map, factory mode and display state. `checkForNewConfig` at `0x25bd3b0` requires master enabled and no in-flight request. Force bypasses age only, not the master/VPN/in-flight gates. Request construction `0x25bbfe0` includes `das_hw` and `autopilot`; values derive from gateway-backed VAPI inputs. A disconnected VPN defers sending until its connection value changes. The recovered VPN client reads a Hermes connection-state property; the underlying daemon/external network decision remains beyond this client trace.

Response `0x25bd7c0` requires the recovered reply state, HTTP200 and nonempty body before parsing and writing `GUI_featureConfig` at `0x25bde96`. Failure schedules retry without replacing the map. Success also updates last-update and a successful-fetch latch. Backend regional/entitlement algorithms were not recovered.

Scheduler settings: master default true/environment1; update interval43200/environment0; timeout10, retry1800 and initial interval300 use implicit environment4; update jitter7200 and retry jitter600 use environment0. These are compiled defaults, not measured runtime settings.

Persistence is SQL Settings, separate from `settings.conf`. Constructor persistence sets DataValue flag bit0; `persistNoLock` tests it. The map and three main feature values are persistence-eligible. Filename expression is `baseFolder + /data/ + applicationName + optionalPrefix + Settings.db`; actual rows/base/prefix were not read. Explicit startup calls restore initial values before the explicit FeatureConfigManager startup call; earlier indirect initialization is not exhaustively excluded. A cached value is not proof of a fresh server decision.

Service listeners and FlatBuffers setters replicate feature values. `FEATURE_hasFSDStopsMaps` is also a diagnostic alias of `FEATURE_dasHasFSDStopsMaps`; it is not an independent backend policy source. The bounded file/reference classification found mirrors, definitions and consumers rather than independent regional writers; compressed interiors and arbitrary indirect references remain outside that claim.

## 5. Gateway, navigation and wrong-region inputs

### 5.1 Gateway configuration

CID-ICE internal ETH bus3, `0x7FF`, decoded by GUICanCracker, feeds:

| Page / bits | Registered source | VAPI destination |
|---|---|---|
| p1 `16\|16` | `ETH_GTW_country` | `VAPI_countryCode` |
| p1 `40\|3` | `ETH_GTW_dasHw` | `VAPI_driverAssist` |
| p2 `42\|3` | `ETH_GTW_autopilot` | `VAPI_autopilot` |
| p3 `8\|4` | `ETH_GTW_mapRegion` | `VAPI_navigationMapRegion` |
| p9 `20\|2` | `ETH_GTW_autopilotFeaturePackage` | `VAPI_autopilotFeaturePackage` |

DataValue registration stores a source name and CRC-keyed target. Decoder `storeSignalValue` looks up that target and invokes its setter; a separate source DataValue object is not required. Internal ETH framing and bus3 dispatch do not establish physical Chassis CAN routing or the originating gateway algorithm. A captured hardware raw5 must not be automatically named HW3/HW4 from this work.

### 5.2 Localized country and on-map state

Map-matched country is not gateway factory country. `QtCarTMServer` map matching selects administrative metadata from a matched edge's node admin index. Valhalla obtains `country_iso` from installed graph-tile administrative records, fills the response Admin, and TMLocalizer writes `INAV_vehicleCountry`; publication connects it to `NAV_vehicleCountry`, then `GUI_navVehicleCountry`. Actual installed map tiles were absent from the image, leaving runtime tile content and matched-road state open.

`QtCarGpsManager` parses RMC coordinates from its configured AP/APB UDP sources, publishes location and calls `NavUtils::isOnNavigationMap()`. That helper has delivery/transport/factory and navigation guards, then checks coordinates against region-selected map bounds. `LOC_isOnNavigationMap` reaches `GUI_isOnNavigationMap` by IPC/publication. The GNSS internals behind the external peers were not recovered.

### 5.3 Wrong-region lifecycle

`GUI_navigationMapWrongRegion` is written by map-controller `updateMapStatus` at `0x3ef58e0`, separately from managed FSD maps support:

1. Without `nav/forcePremiumNav`, invalid map-region input returns without writing the flag.
2. Region enum2 sets mount-status raw2 and returns without writing the flag.
3. Continue only for valid mount-status raw4 OR an existing maps VERSION file; otherwise return retaining the previous flag. Mount raw4 bypasses the VERSION guard.
4. An expected region folder clears wrong-region; a missing folder sets it to `!shouldSuppressMapAlerts()`.
5. Initial/update-alert calls refresh it; mount changes schedule a refresh after5000ms, and an unavailable initialization branch after30000ms. The global flag's original default and every mount producer remain open.

The stock string `nav/forceMapRegion` defaults empty, environment4. A trimmed nonempty string uses `EnumForName`; an unknown nonempty value resolves enum2 rather than falling back to VAPI. Accepted enum names: `USCanada, EU, None, CN, AU, JP, TW, KR, ME, HK, MO, SEAsia, IN, LA` for raw0..13. `NA` is a folder label, not the raw0 override name. These are source enum names, not the test platform's location.

## 6. Stock configuration and restart branches

A force-looking key is not automatically an active or usable override.

| Key | Default / env | Recovered consumer |
|---|---|---|
| `vapi/forceHasFSDNavMaps` | false / 0 | No consuming read in bounded 15-ELF/30-object search. |
| `vapi/forceHasFSDStopsMaps` | false / 0 | Same bounded negative result; not an active path in the recovered maps helpers. |
| `vapi/forceHasFSDControl` | false / 0 | Force-allow after steering rejection, before force-deny. |
| `vapi/forceDisableFSDControl` | false / 0 | Force-deny after force-allow. |
| `vapi/force_fsd_autopilot` | false / 0 | Otherwise `hasFSDAutopilot` needs valid tier raw3. |
| `vapi/forceAP3` | false / 0 | Hardware helper branch; not an authoritative hardware detector. |
| `vapi/forceHasAutosteer` | false / 4 | Overrides `hasAutopilotAndNavData`. |
| `vapi/enable_DCAS_behavior` | false / 3 | Cached DCAS force-enable. |
| `vapi/force_disable_DCAS_behavior` | false / 1 | Cached DCAS inhibition before managed EU feature. |
| `vapi/enable_DCAS_behavior_legacy` | false / 1 | Config AND NOT (`hasFSDControl && GUI_fsdControlEnabled`). |
| `vapi/force_allow_auto_enable_fsd` | false / 0 | Auto-enable capability override. |

`ConfigValue::variant` environment0 returns current/default without ordinary QSettings loading. Raw1/raw4 skip file loading when `/var/run/is-fused` exists; raw3 takes the inspected lazy-loading path without that check. The two-argument string/int constructors explicitly use raw4. Normal fallback expression is `QDir::homePath() + /.Tesla/car/settings.conf`, unless base-folder configuration changes it. These are stock artifact paths, not disclosed host paths or verified live file contents.

The runtime fused byte comes from marker existence. Startup creates/removes the marker according to the inspected fuse-query script and sentinel chain; actual target hardware values and query internals remain open. Factory-related markers separately affect factory/config behavior. No file, fuse or force value was changed in this investigation.

`shouldApplyDCASBehavior()` = cached enable-DCAS OR (`!cached force-disable-DCAS && FEATURE_enableEUFSDBehavior`). The EU feature is registered from `enable_eu_fsd_behavior`, defaults false, is persistence-eligible, and is distributed to VAPI through service listeners. Its source is not unresolved merely because an older summary stopped before registration.

RestartBehavior1 registers a conditional restart monitor against `VAPI_autonomyBehavior` and expected QVariant(false). Its internal ETH source is `0x221`, mux1, bits34\|2. Names recovered for raw0/1/2 are Driver/DriverlessTakeover/DriverlessNoTakeover; raw3 lacks a name although the decoder accepts two bits. A valid unequal condition delays; invalid condition with the supplied true registration option does not delay. The restarter persists values then quits the process unless suppressed. It is not a car power reset. Concrete GUI restarter instantiation, full suppression policy and external ETH sender remain open.

Generic debug DataValue setters also exist. A listing whitelist is not write authorization. The inspected receiver applies missing-target/flag/optional-regex checks and DBus policy; mutating a server-side mirror has not been shown to update the authoritative GUI publisher. No setter was invoked and no viable stock override was demonstrated.

## 7. GUI request writers, lifecycle and encoder

Primary `GUI_fsdControlEnabled` and `GUI_fsdStopsControlEnable` default false and are persistence-eligible. Their saved state is user intent, not receiver acceptance.

| Writer/lifecycle | Recovered effect |
|---|---|
| FSD confirmation `0x2a6f080` | Writes confirmed Boolean to `GUI_fsdControlEnabled` at `0x2a6f0af`. |
| TLSSC confirmation `0x2a6f790` | Writes confirmed Boolean to `GUI_fsdStopsControlEnable` at `0x2a6f7be`. |
| Selector off | Clears FSD and `GUI_autosteerOnRequest`. |
| Autosteer branch | Clears FSD; Autosteer popup `0x2a6f410` writes confirmation to autosteer at `0x2a6f43e`. |
| Selector paint/update | Reads saved values; does not report acceptance. |
| Generic confirmation switch | `confirmPosition(true)` accepts the mapped current position, not unconditional Boolean true; position/inversion/protected-write policy matters. |
| Profile restore `0x2709d00` | Capability-checked enum0/1/2 writes off, Autosteer or FSD requests. FSD's transmitted bit retains later maps gates. |
| Startup persisted-load reset | `profiles/reset_fsd_control` defaults true/persistent; clears matching true profile FSD settings and current FSD, then clears the reset flag. |
| Default/test-drive paths | Default reset can clear TLSSC; test-drive defaults set it true only after allowed/maps/calibration checks. Invocation guards remain partial. |
| Drive-rail callback | Recovered rail-on/legacy-DCAS branch clears FSD/autosteer. It was incorrectly described earlier as a generic rail-off reset. |
| AutopilotManager dependency | Can clear/preserve active requests and conditionally auto-enable; complete path-sensitive lifecycle remains partial. |

Local profile saving serializes records into persistent `ProfileDeviceStorage`; cloud saving has a separate gate/path. Actual rows, all serializers and cloud policy were not recovered. Pending and acknowledgement-pending GUI auto-enable values default false/persistent. Their names do not turn them into ECU request acknowledgements.

AutopilotManager preserves an existing active request on a recovered failure path, otherwise can clear FSD. Pending auto-enable additionally depends on parked/display, delivery/engineering, capability, camera/autosteer and manager state. A separate branch-specific software-release membership check exists; it is compiled policy, not a discovered universal bypass. `gui/auto_reset_to_tacc` defaults true/persistent and can clear requests/profile settings under its own guards. Remaining display enum meanings, manager transitions, indirect/table/profile writers and runtime values remain open.

VAPI `AutopilotControlMessage::assembleMessage` at `0x7270e0` assembles ID `0x3FD`, DLC8; constructor defines500ms interval. Equations in CID-ICE (page0 except the explicitly page1 LaneGraph row):

| Bit | Equation |
|---|---|
| 37 | `isFSDVisualizationEnabled()`; helper rechecks localization/capability, not a startup latch. |
| 38 | `isFSDStopsControlAllowed() && GUI_fsdStopsControlEnable`. |
| page1 bit45 | LaneGraph request from `GUI_showLaneGraph`. |
| 46 | `hasFullSelfDriving() && GUI_fsdControlEnabled`. |
| 47 | `hasFullSelfDriving()`. |
| 60 | `shouldApplyDCASBehavior() && GUI_fsdControlEnabled`. |

`hasFullSelfDriving()` = `hasFSDControl() && hasFSDNavMaps() && !GUI_navigationMapWrongRegion`. Direct bit38 has no maps term: maps/calibration can clear its saved request upstream. Do not merge those two stages into a different equation.

The sender packages BE16(DLC/ID header) plus eight payload bytes into a ten-byte UDP record and sends to the configured gateway peer. Primary transport is proved; physical forwarding and matching HW4 consumer/acceptance remain open. Comparative gateway parsing/routing does not establish forwarding on the current platform. Stock bit60 semantics from this ICE profile cannot alone establish a bug in another hardware-specific implementation.

## 8. Custom-board/upstream modifiers

The custom C6 board adds modified frames; the inspected dispatcher/capture path also retains original received frames. It does not replace or suppress every original message. Workers process fresh RX, with a20ms age/echo budget, bounded queue, single transport lease and admission/pending checks. No periodic generated retry stream or proved boot-only latch occurs in this path.

- In the retained comparisons, TLSSC Restore input already matched its target and made no change; a no-op is not evidence of a failed sender.
- ForceFSD/Assist is a predicate-controlled bit38 modification, distinct from GUI FSD selection and the separate unlock preset.
- Disabling Assist stops its processing; disabling ForceFSD can leave Assist examining frames while making zero modifications.
- LaneGraph and visualization named overrides operate per frame. In the retained comparison, observed RX page0 visualization bit37 was already1, so setting it on was a predicted no-op; this is not actual submitted-payload proof. Unchanged outputs can be skipped. Configuration/session boundaries reset readiness and require new RX.
- A journal of actual backend-submitted bytes was built/reviewed locally but not deployed. It records backend events/completion, not pre-admission skips or ECU acceptance.

Inspected upstream [predicate](https://github.com/hypery11/flipper-tesla-fsd/blob/main/fsd_logic/fsd_can_ops.h#L59) has no universal HW4 exclusion. [ESP32 handler](https://github.com/hypery11/flipper-tesla-fsd/blob/main/esp32/.firmware/fsd_handler.cpp#L393) places Assist before the hardware split; unlock writes46/60 on HW4 and46 on HW3. [README description](https://github.com/hypery11/flipper-tesla-fsd/blob/main/README.md#L102) distinguishes bypassing UI selection. A [version-specific regression report](https://github.com/hypery11/flipper-tesla-fsd/blob/main/README.md#L243) is not universal failure. The [Flipper C Nav/Lane branch](https://github.com/hypery11/flipper-tesla-fsd/blob/main/fsd_logic/fsd_handler.c#L280) is separate from ESP32 coverage.

[Discussion163](https://github.com/hypery11/flipper-tesla-fsd/discussions/163) reports modified-frame preflight aborts, tolerated exact duplicates and no effect/abort for Party-only. The author corrected turn-signal/intent versus attempted lane change. [Discussion168](https://github.com/hypery11/flipper-tesla-fsd/discussions/168) attributes newer failures to an off-CAN regional gate. These are upstream conclusions, not the recovered live root cause. Main-branch links may change; the inspected revision is retained privately.

## 9. Older APE accepted request and status outputs

### 9.1 Receiver and ASM consumer

APE-HW25 `canrx` decodes **0x3EE**, not the modern CID's0x3FD. Raw bit38 becomes packed bit126, then guarded accepted state at shared object `+0x4cc`. A receiver-state input comes from DI state; the2..4 suppression guard is not a recovered map/region policy. Accepted-state changes reach timestamped `RoadRunnerSignals` snapshot `+0x4dc`.

The old LaneGraph path is also closed:0x3EE mux1 bit45 → packed object+0x294e bit8 → callback0x2b2df4 → RoadRunner payload+0x3c0 / snapshot+0x3d0. A231-instruction receiver/callback check establishes this path, not rendering behavior. Old mux0 bit46 maps to packed object+0x2946 bit55; the normalized full-FSD consumer remains open. Payload+0x3a0/+0x3a1 are not a shortcut to that bit: they derive from packed bits30/31, corresponding to mux1 bits19/52.

ASM opens the same RoadRunner shared-memory identity. At `0x2b32cc`, selected RoadRunner state must equal2; a second input, `ArbiterTrajectoryOutput +0xdf0`, must meet its threshold. The conditional path can return numeric12 after earlier helper/status guards, accepted state2, Arbiter value≥2 and a rounded deceleration ratio>3. Failing this branch continues common checks rather than returning a dedicated request-disabled result. No direct Park predicate was established. Exact +0xdf0 member name/enum and earlier helper meanings remain open.

The earlier timer helper0x2b3a00 compares three common-state timestamps with a350ms threshold, including future timestamps as recent; its priority results are3/1/2, or0 otherwise. Caller0x2b3220 maps any nonzero result to48 before the TLSSC-specific branch. Numeric writers are identified in0x2b7880 on the same common object: common+0x68 stores0x2b7a24/0x2b7a90, +0x88 store0x2b7a60, +0x78 store0x2b7bd0. Conditions include raw RoadRunner+0x2c4==2, and for+0x88, (+0x128==2 OR (+0x4f0!=1 AND byte+0x396==1)) plus helper0x2b5c30 bit0. These event names and prior helper0x2b35b0 semantics are unresolved; they must not be labelled Park/brake/cancel. **Common-state timestamp+0x68 is a different object from ASM snapshot HOLD Boolean+0x68.**

HOLD handling at `0x2c0418` uses enabled TLSSC plus second-input value5. Cached toggle loss can produce internal reason8 `STOPS_BUTTON_TOGGLE`; a HOLD substate additionally requires RoadRunner `+0x104 == 3`. Parent HOLD substate `+0x554` is published as ASM snapshot Boolean `+0x68` through getter `0x29d2d0` and publisher `0x267a58`. **No wire mapping for this +0x68 Boolean is established.** Internal HOLD reason `+0x558` must not be conflated with ASM abort `+0x84`.

A bounded cantx follow-up inventoried13 textual TLS-slot0x158 materializations (11 emitter accesses and two pointer-maintenance sites). The full-ASM helper0x2b5e20, called via0x26d204/0x26d20c, reads+0x70/+0x94/+0x1e0/+0x74/+0xd8; the subobject call0x26f370/0x26f374 to getter0x2b6720 reads+0x34. Neither closes HOLD+0x68. The0x399 direct reads inspected and0x5D9 paired+0x70/+0x74 load also do not overlap it. Alternate address construction, remaining aliases, other helper paths and other tasks remain outside this bounded check; this is not proof the Boolean has no wire consumer.

### 9.2 Newly closed diagnostic wire edge

The accepted-request-dependent numeric result has a separate established wire consumer:

`ASM conditional result → common +0x150 → getter0x2ad660 → publisher0x267894/0x2678a4 → ASM snapshot+0x74 → cantx → old0x5D9 mux2 bits46\|7`.

| Proof stage | APE-HW25 artifact-local evidence |
|---|---|
| Consumer identity | `DasCarlogMessageEmitter`, constructor0x26c780, populate0x26c810, registered0x5D9. |
| Selected snapshot | TLS offset0x158 loaded at0x26c878. |
| Hidden paired read | `ldp` at0x26c884 reads+0x70 and+0x74; second word saved stack+4 at0x26c890. Literal-only loads from+0x74 missed this alias. |
| Translation | Stack+4 reloaded0x26caf4; helper0x2b5f90 called0x26cb0c. Inputs0..52 index a53-entry uint32 table atVA0x2302ec; larger unsigned inputs map45. Input12 maps12. |
| Mux and packing | Mux2 selected0x26ca60; pointer advances two bytes0x26ca68. Split packing0x26cb20/0x26cb28–34 therefore targets global bits46..52. |
| Cache | Comparison0x26c978–84; cached result updated0x26cb4c. This is multiplexed diagnostic output, not an immediate response for every request. |
| Validation | Translation read by ELF segment mapping; all53 table entries checked against split-field reconstruction. No target execution. |

Modern schema calls the same position `DAS_accUnavailableReason`, with12 labelled `ACC_UNAVAILABLE_REASON_CONTROLLER_NOT_ALLOWED`. This is a cross-version candidate meaning, not proof of the old internal enum identity. It is not a dedicated TLSSC ACK and does not close the HW4 path. It supersedes the earlier “+0x74 wire consumer unresolved” edge only; +0x68 remains unresolved.

### 9.3 Other outputs and queues

Older `0x399` state emitter reads ASM `+0x70`, ASM `+0x10` and MissionPlanner `+0x16dc8` through its helper and packs low4bits. The recovered helper does not emit6; modern `ACTIVE_FSD=6` must not be imported into it.

Older `0x247` abort output reads ASM `+0x84`, translates enum numerically, then packs bits32..38, width7. Modern width8 is a real discrepancy. Bit8's older source has no established modern map-availability semantic identity.

The identified cantx dispatcher has52entries/48uniqueIDs. It is a potential-emission registry, not proof every ID is present at runtime. Primary/backup registrations differ. Two Simple queue readers use fixed emitted IDs: alerts0x5E9 and gateway0x6F9; they copy queued payload after an11-byte record/DLC check and do not pass through arbitrary queued IDs. A controller alert writer uses POSIX message queue backing. Warning-matrix packing copies snapshot byte lowbits into64payloadbits; bank/index names remain partly unresolved. Dynamic transport capabilities outside the identified registry remain a coverage gap.

### 9.4 Visualization

Older0x3EE bit37 decodes each received request, enters accepted callback and reaches RoadRunner payload+0x3d6 / snapshot+0x3e6. Both valid0 and1 overwrite; the proved receiver branch is not a boot-only latch. LaneGraph reaches payload+0x3c0 / snapshot+0x3d0. Downstream byte consumers/rendering/timeouts remain open. CID visualization capability also depends on non-Tegra, HW3-or-higher and configured/localized groups. No proved latch explains the reported reboot/removal asymmetry.

## 10. Selected modern candidate signals

Notation is `start|width`, LSB-first Intel bits. The listed fields are unsigned, scale1/offset0. Require correct ID, source profile, sufficient DLC and matching mux before extraction. Unknown/SNA enum values stay separate from numeric range. For other source fields, signed extension precedes scale/offset; Motorola sawtooth positions require their own algorithm. ID equality, line comments or enum labels do not establish live compatibility.

| ID / DLC | Selected exact signal names and positions | Interpretation |
|---|---|---|
| 0x25D /6 | `APP_tcFeatureState`0\|2; `APP_tcStateMachine`2\|3; `APP_tcUnavailableReason`38\|4 | Traffic-control feature/state/reason, not general APP status. |
| 0x399 /8 | `DAS_autopilotState`0\|4; `DAS_autoparkReady`24\|1; `DAS_autoparkActive`26\|1; `DAS_autopilotHandsOnState`42\|4; `DAS_summonAvailable`51\|1 | Operational/availability context. |
| 0x259 /8 | `APP_summonUnavailableReason`14\|5; `APP_fsdSuspendState`19\|2; `APP_teslaVisionSupported`24\|2; `APP_teslaVisionUnsupportedReason`26\|3; `APP_autonomySystemHealth`49\|1 | General health/suspension/reasons; not0x25D. |
| 0x191 /2 | `DAS_autonomyUnavailableReason`2\|9 | General diagnostic enum, not a per-feature acknowledgement. |
| 0x247 /8 | `DAS_mapAvailability`8\|1; `DAS_autopilotAbortReason`32\|8; `DAS_rqstedBehaviorStatus`40\|5; `DAS_lastWatchdogCode`56\|8 | Abort/behavior validity; zero is not acceptance. |
| 0x24A /8 | `DAS_autosteerNavigationUsage`8\|2; `DAS_plannerState`28\|4; `DAS_navAvailable`39\|1; `DAS_trafficAwareSetSpeedInUse`53\|1 | Operational navigation, not managed FSD maps support. |
| 0x439 /8 | `APP_w235_fsdRegionNotAllowed`42\|1 | Warning candidate; clear does not exclude every regional gate. |
| 0x47D /8 | `APP_w392_fsdSuspended`7\|1; `APP_w397_apFeatDisabledPermanent`12\|1; availability system/visibility/AP-system/user30/32/34/36\|1 | Warning context. |
| 0x5D9 /8 | selector `DAS_carLogMultiplexer`0\|4; mux2: `DAS_driveOnNavUnavailableReason`40\|5, `DAS_accUnavailableReason`46\|7, `DAS_pmmNotActivatingReason`53\|5 | Distinct diagnostic fields; PMM→TLSSC meaning unproved. |
| 0x5E9 /8 | mux `APP_alertID`407: `APP_sw407_fsdDisabled`20\|1; `APP_sw407_outsideGeofence`21\|1 | Alert-context candidates, unobserved in selected comparisons. |
| 0x30A /8 | `DAS_roadSignControlActive`, object mux4,24\|1 | Object-scoped road-sign activity; not parked configuration acceptance. |

Selected enum maps:

- `APP_tcFeatureState`:0TC_DISABLED,1TC_UNAVAILABLE,2TC_AVAILABLE,3TC_ACTIVE.
- `APP_tcStateMachine`:0DISABLED,1STANDBY,2AWARE,3WARNING,4STOPPING,5STOPPED,6CONTINUING, with canonical `TCSM_` prefixes.
- `APP_tcUnavailableReason`:0none,1MCU_DISABLED,2LOST_MAP_LOCALIZATION,3ROAD_CLASS,4CAMERA_BLINDED,5SUN_GLARE,6ROAD_ESTIMATOR_UNHEALTHY,7GPS_DEGRADED,8CAMERA_BLINDED_ON_ROAD_RESTRICTED,9SUN_GLARE_ON_ROAD_RESTRICTED. Canonical spellings retain the source `AP_UNAVAILBLE_` typo.
- Modern `DAS_autopilotState`:0DISABLED,1UNAVAILABLE,2AVAILABLE,3ACTIVE_NOMINAL,4ACTIVE_RESTRICTED,5ACTIVE_NAV,6ACTIVE_FSD,8ABORTING,9ABORTED,14FAULT;15SNA metadata is distinct from an ordinary state.
- `DAS_autonomyUnavailableReason` raw4 labelDELIVERED is not a demonstrated root-cause explanation.
- Modern abort examples include62FSD_GEOFENCE_DISABLED and70FSD_REGION_NOT_ALLOWED; observed zero does not prove a request reached that path.
- `DAS_rqstedBehaviorStatus` raw0VALID is lane/path behavior validity, not a TLSSC ACK.
- `DAS_driveOnNavUnavailableReason` raw15 means feature not enabled in this reference; it does not identify the upstream policy deciding it.

Additional semantic exclusions:0x248 is APE→UI control request rather than UI request ACK;0x445 is control output;0x286 DI cruise feedback is distinct from APE internal state. `DAS_devAppInterface` is not a proved acknowledgement of the GUI developer setting. `0x39B` has no definition in the reviewed canonical/comparison schemas and cannot replace0x399; an unobserved filtered interval is not whole-bus absence.

## 11. Inventory counts and validation

Keep these populations distinct:

| Population | Recovered count / limitation |
|---|---|
| Earlier modern54-ID selection | 36definitions,2686fields,18missing;2151mux fields,27signed;266bitmaps. Bounded extraction/review completed. |
| Consolidated55-ID selection | Older48registry IDs + six external candidates +0x25D;37definitions,2700signals,18missing. Three tests, strict parsing and source-pointer verification/review passed. |
| Full modern reference | 676messages/42132signals privately retained; not redistributed here. |
| Older APE registry | 52entries/48uniqueIDs;30have modern matches,18modern gaps. |
| Older missing-modern recovery | 18layouts/291packing groups, reviewed; packing is not complete semantics. |
| Two CH profiles | Identical26messages/39unique signals represented as78profile rows; source hardware/release unknown. One source declares the other model label. |
| Bounded raw-file feature scan | 29785regular files, zero read errors; compressed interiors not expanded and indirect completeness not claimed. |

Eighteen missing modern IDs:0x260,0x299,0x2BF,0x379,0x38A,0x3BF,0x3EF,0x3F7,0x409,0x419,0x469,0x479,0x488,0x548,0x5F9,0x6F9,0x721,0x722. Missing definitions do not mean unsupported/nonexistent messages.

Older→modern DLC discrepancies include0x219:3→7,0x249:8→4,0x3F2:6→8,0x3F6:1→8,0x3FC:8→7,0x720:8→5. Other ID collisions are substantial. Network labels remain attached; not all CAN is CH and an ETH definition does not prove a physical route.

## 12. Live experiments, chronological cohorts

All results below are separate populations. No row establishes a causal request-to-accepted-state mapping. Original settings were restored after completed interventions; the interrupted attempt required a later verified recovery. Exact configuration revisions, identity and raw payloads are intentionally omitted.

| Phase | Measured result | Limit |
|---|---|---|
| Short Stop/Start | Operational0x399 raw0 unchanged; stopped TX0; restarted transport completed24total/Assist12; Restore processed without change. | Transport completion only. |
| Passive registry scan | Two30s windows,5535records,24/48IDs observed. | Potential old registry differs from live presence; DLC collision observed. |
| Expanded candidate scan | Three30s windows,5867records,26/54IDs observed. | No positive acceptance indicator. |
| Engine OFF→ON→OFF | Six30s windows;5867/5888/5871records;26/54IDs each phase;ON sent242,OFF0. No phase-only IDs or reversible stable-bit transitions in compared groups. | Does not prove ignored requests. |
| Isolated eight phases |29/57IDs every phase; visualization no observed RX transition; LaneGraph TX approximately242→362; ForceFSD-off Assist modifications/completions0; restoration completed121Assist requests. | No stable receiver-candidate change. |
| Parked visualization |120s,240page0frames,bit37always1,gaps497–503ms. Separate0x25D window60DLC6frames with candidate feature/state/reason0/0/0. | Driving flicker/reboot asymmetry not reproduced. |
| Historical five-phase comparison | Baseline,Restore-on/off,Assist-on/off;all108canonical fields in six diagnostic IDs reviewed:104singleton unchanged;only fourcounter/checksum fields varied, with identical sets between phases. | This108-field cohort is not the newer56-field cohort. |
| Interrupted follow-up | Baseline,Restore61no-op inputs,Assist121modified/completed;later stream loss aborted freshness guard. | LaneGraph phase incomplete; planned combinations not run; cause unknown. |
| Guarded nine-window comparison | Nine15s windows;137050ms summed device bounds,27721bounded records from30778total including transitions. Baseline and restored steps interleave LaneGraph,visualization,Assist-off,ForceFSD-off. | Stable-bit analysis required≥10records/group; not exhaustive transient correlation. |
| Awake passive comparison | Approximately152s,30721records,23IDs requested/15observed;56selected candidate raw sets equal the preceding nine-window capture, matching ID/mux/DLC. | Continuous traffic; no observed awake-to-quiescent transition, no sleep test. |

### 12.1 Newer nine-window details

| Window | Bounded records | Engine sent | Assist examined / completed |
|---|---:|---:|---:|
| Baseline |3078|60|30/30|
| LaneGraph |3072|90|30/30|
| Restored |3082|61|31/31|
| Visualization |3088|62|31/31|
| Restored |3063|62|31/31|
| Assist off |3080|30|0/0|
| Restored |3083|60|30/30|
| ForceFSD off |3090|30|30/0|
| Restored |3085|62|31/31|

Candidate modern outputs stayed0x25D feature/state/reason0/0/0;0x399state0;0x191reason4;0x247map/abort0/0;0x439regional warning0. No constant-bit differences in eligible per-ID/mux groups were detected. Restore input already matched and required no change. An existing speed-profile setting was preserved and explains other ongoing board work; it was not independently tested. Source-to-RX comparison predicts Assist changes page0 bit38, LaneGraph changes page1 bit45, and the retained forced speed-profile setting changes page2 bits60..62. That last setting accounts for continued profile work when Assist or ForceFSD is off. Visualization forced-on matches the already-set observed RX page0 bit37, so those windows did not test a0→1 visualization transition. These are source-derived expected transformations, not retained backend-submitted payloads. No0x3FD checksum recomputation occurs in this modifier path; unrelated bits are copied.

A targeted comparison of the newly linked diagnostic position, modern0x5D9 mux2 bits46\|7, found155awake samples all58 and15–16samples in each of the nine bounded windows all58. This does not contradict the older conditional result12: its additional state/ratio/helper guards and current-hardware equivalence remain unresolved. No claim that every emitted field stayed unchanged follows from these selected comparisons.

The56selected fields are a curated multi-ID set, not all108fields from the historical six-ID review. In that historical selected-field table,53nonselector observed fields were unchanged, the0x5D9selector cycled0..4 and two0x5E9fields were unobserved. The later awake comparison uses the56selected fields against the newer nine-window capture; it must not inherit historical field values merely because names match.

Independent final read-only checks verified exact original configuration content, ARMED/manual and no blockers. No new reported bus error/drop counter increments or generation/stationary-guard failures occurred. These measurements neither test engagement nor independently establish ECU sleep or receiver identity. Unobserved0x39B is limited to the filtered interval.

### 12.2 Historical interruption and capture quality

Streams continued about20.8s after a saved LaneGraph-only change, then delivery stopped. Last stationary evidence was Park/valid displayed zero. No close-packet/device logs identified network versus server cause. The first restore reused a dead connection and failed; a later identity-checked reconnect/Stop/fresh stationary check/CAS restore/readback succeeded, followed by ARMED/manual. No test variant remained. This does not establish LaneGraph causation.

That isolation harness omitted unsubscribed DROP events. Heartbeat,bus,error,fault notifications were always delivered. Flat physical RX/drop counters do not prove lossless filtered capture. A future reconnect-aware harness must retain DROP/close metadata; improvements were not retroactively present in that run. Newer guarded-run recovery was tested offline but not fault-injected on the car.

[Experiment narrative](docs/experiments.md) · [methodology](docs/methodology.md#live-work).

## 13. Local tooling and deployment state

| Tool | Verification snapshot | State |
|---|---|---|
| Host WebSocket CLI |42targeted offline tests;reviewer35;read-only live status/capture checked. | Host use checked; not the historical interrupted harness. |
| Actual backend TX journal |16-event payload/result/terminal lifecycle with boot cursor;53host cases/8022assertions;firmware build passed;review clean. | Not deployed; current device command unavailable, handled gracefully. |
| Passive feedback UI/firmware |5IDs/11fields;58APE cases/8302assertions;42UI tests;checks/build passed;review no blockers. | Not deployed. |
| Modern parsing inventory/consolidation | Extraction,source pointers and strict parsing reviewed. | Specification evidence, not validated live hardware semantics. |

Counts describe different snapshots and must not be added together. Build success is not deployment or device validation. The journal would prove actual driver-submitted bytes/results, still not ECU accepted/enabled/active state. Automatic hardware selection from raw5 was not implemented; manual selection remains preserved.

### 13.1 Silence, restart and sleep boundary

A source audit with10targeted cases/1543assertions verified scanner lifecycle: per-source freshness expires after750/1500ms; all accepted RX silent for>1500ms clears readiness while preserving intent/configuration. Fresh native traffic can rebuild readiness without Start. Explicit Stop latches stopped intent for that boot; reboot reconstructs intent from persisted permission and boot policy rather than preserving the old Manual/Stop state. A pending backend lease needs exact completion or retirement; silence alone does not release it. Detector down does not prove node destruction or ECU sleep. These source tests do not turn the continuously awake observation into a physical sleep experiment.

## 14. Route closure index

Each row retains a distinct route; documented or parked does not mean abandoned or fully closed.

| Route | Established | Concrete remaining edge |
|---|---|---|
| R01 GUI warnings | Strings/callers/immediate predicates. | Matching live code and failed gate. |
| R02 Localization/maps | Gateway region, map-country and on-map sources separated. | Actual tiles/matched state and remaining metadata producers. |
| R03 Managed features | Client request/response/update. | Backend algorithm/runtime response. |
| R04 Persistence | Constructors,SQL eligibility,startup order. | Actual rows/base and full profile serialization. |
| R05 Force-map clues | Bounded no-consumer result. | Indirect/other-build uses not excluded. |
| R06 Config/fuse | Environment gates and marker source. | Actual fuse/query/runtime settings. |
| R07 GUI writers | Confirm/off/Autosteer/profile/reset/auto-enable paths. | Complete indirect/table/lifecycle guards. |
| R08 Child predicates | Camera/calibration/park/cruise partly traced. | Remaining producers/timeouts/full enums. |
| R09 Encoder | Eligibility equations and UDP sender. | Matching physical forwarding. |
| R10 Gateway | Comparative receiver/parser. | Matching target gateway artifact/consumer. |
| R11 APE receiver | Old0x3EE→accepted shared state. | Modern0x3FD/HW4 receiver. |
| R12 Acceptance/status | OldASMreads;new+0x74→5D9diagnostic edge. | +0x68wire/HW4positive acceptance oracle. |
| R13 Emitters |52registry entries/48IDs. | Runtime variants,dynamic transports,full semantics. |
| R14 Passive presence | Observed subsets. | Presence→proven producer/layout. |
| R15 Stop/Start | Operational candidate unchanged. | Acceptance/rejection inference unavailable. |
| R16 Restore | Already-target no-op understood. | Receiver outcome for a changed request. |
| R17 Assist | Predicate/bit38/TX processing differentiated. | Accepted state and policy result. |
| R18 Hardware | Profiles explicitly separated. | Paired modernHW3/HW4 evidence. |
| R19 Passive diagnostics | Locally built/reviewed. | Deployment/device validation. |
| R20 Deployment | No firmware deployment in record. | Authorized future deployment/validation. |
| R21 Debug setter | Generic receiver/access partly classified. | Authoritative propagation/exact target conversion. |
| R22 EU/DCAS/restart | Key,distribution,conditional restart. | External sender/concrete restarter/runtime. |
| R23 Modern spec | Bounded extraction/review. | Matching hardware semantic validation. |
| R24 Oldmissing layouts |18layouts/291groups. | Remaining semantic labels. |
| R25 Signal semantics | Request/warning/activity distinguished. | Request-correlated acceptance. |
| R26 Visualization | Per-frame source/callback. | Downstream renderer/timeout/HW4path. |
| R27 Isolated phases | No stable candidate changes. | Not engagement/driving validation. |
| R28 CLI | Local tests/read-only use. | Full guarded-harness integration distinct. |
| R29 TX journal | Backend events implemented/reviewed. | Deployment and submitted-byte capture. |
| R30 Parked/flicker | Stable measured parked requests. | Driving/reboot asymmetry. |
| R31 Feature feedback | Candidate mapping/implementation. | Matching producer/acceptance validation. |
| R32 Consolidation |55/37/2700/18review complete. | Source hardware/network applicability. |
| R33 Hardware inference | Raw5left unresolved. | Authoritative enum/source. |
| R34 Follow-up | Interrupted run recovered;later nine windows completed/restored. | Unrun presets/combinations/motion cases. |
| R35 Upstream | Predicate/handler/report distinctions reviewed. | Upstream explanations not live-root-cause proof. |

## 15. Corrections and interpretation rules

- A maps warning does not identify an outdated package; a clear regional-warning bit does not rule out all regional policy.
- Neither gateway country nor the backend alone determines every branch. Configured and map-localized countries differ.
- ForceFSD/Assist, stock GUI FSD selection and unlock are different actions. No universal upstream HW4 predicate exclusion was found.
- Conflicting overrides were rejected configuration, not valid last-write-wins evidence.
- OlderHW2.5 is not HW3/HW4. Old0x3EE/new0x3FD and old/new399/247 differences are substantive.
- 0x25D is traffic control;0x259 is general APP status.0x39B cannot substitute for0x399.
- TX accepted/completed means backend lifecycle, not vehicle ACK. ARMED/READY,zero abort,DELIVERED and disabled activity do not establish rejection/acceptance.
- HOLD Boolean+0x68,HOLDreasonparent+0x558,ASMdiagnostic+0x74 andASMabort+0x84 are different values.
- The new0x5D9edge closes a numeric older diagnostic output, not current hardware semantics or a dedicated TLSSC acknowledgement.
- Stable parked visualization does not explain driving flicker; stream interruption does not identify the feature as cause.
- Source scans and missing definitions are bounded coverage, not proof of absence. Capture cohorts and counters are not interchangeable.

## 16. Proof references and next work

The two recovered CH profile files record repository origin [tesla_can_signals](https://github.com/talas9/tesla_can_signals). They contain identical26-message/39-signal definitions represented as78profile rows; hardware/firmware identity and checkout-commit file membership remain unverified. This is distinct from the separately recovered canonical modern signal database, which is not attributed to that repository. [Source attribution](docs/sources.md#recovered-ch-signal-profiles).

Public references: [GUI/request diagrams](docs/trace-details.md), [source profiles](docs/signal-profiles.md), [experiments](docs/experiments.md), [all routes](docs/route-index.md), [corrections](docs/corrections.md), [implementation/open work](docs/evidence-and-open-work.md), [sources](docs/sources.md), [methodology](docs/methodology.md), [main narrative](docs/research-paper.md).

Privately retained proof roles: recursive CID node/edge ledger; source-location and ELF disassembly reports; older receiver/ASM shared-memory notes; emitter registry/metadata and packing exports; newest paired-load wire-consumer report plus machine-readable table/packing checks; canonical candidate-field matrix; separate per-cohort summaries/raw captures; guarded nine-window and awake comparison reports; local implementation test/review/build reports. These are roles, not public links or promises that original files may be redistributed. Obsidian tools/destination were unavailable; local artifacts carried persistence.

Priority next work is a matching modernHW4receiver/publisher and independent accepted/enabled-state oracle. In the older profile, next bounded targets are remaining ASM+0x68aliases/shared-reader consumers, raw RoadRunner event names and the original result12schema; in the live path, exact submitted payload/cadence would distinguish modifier execution from RX visibility. Backend policy, actual maps/runtime values, remaining GUI lifecycle sources, driving flicker and ECU-sleep behavior remain separate open questions. None should be closed by a suggestive enum name or an unchanged parked field.
