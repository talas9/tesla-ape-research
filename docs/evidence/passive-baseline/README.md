# Raw CAN diagnostic baseline — November 18

[Download the raw CAN recording (JSONL)](2026-11-18-raw-can.jsonl).

This file contains **all 122 retained CAN frames with original payload bytes**, from a separate passive baseline on scanner firmware **2.0**. No engagement attempt was triggered in this recording. The existing scanner feature engine remained ARMED; the read-only acquisition introduced no new CAN command. It is not the raw frame trace for either of the later firmware 3.0 `APP_w462` warning reports.

The original narrow capture retained 122 RX frames during a requested 15-second observation; its first-to-last frame span is 14,825 ms. This publication preserves every retained frame in order, with its CAN ID, DLC, payload string and extended/RTR flags unchanged; `offset_ms` replaces scanner uptime with elapsed milliseconds from the original capture's first frame. Device uptime and bus/generation metadata are omitted; no captured payload or page is omitted. No frames have been reconstructed from decoded values.

| Captured ID/page | Frames | Observation under the candidate decoder |
|---|---:|---|
| `0x5D9`, low-nibble mux 0 | 16 | Autosteer reason 48 throughout. |
| `0x5D9`, low-nibble mux 2 | 16 | ACC reason 42 throughout. |
| `0x191` | 15 | Autonomy reason 4 throughout. |
| `0x4D1` | 15 | `APP_w462` bit 13 clear throughout. |
| `0x5D9`, mux 1, 3 and 4 | 15 each | Retained unchanged; no event-cause interpretation assigned here. |
| `0x3F8` | 15 | Retained unchanged; separate route-related message. |

The retained narrow recording contains `0x5D9`, `0x4D1`, `0x191` and `0x3F8`; all observed `0x5D9` pages are retained. This is the full retained narrow capture, not a full-bus recording or a claim that other messages were absent on the bus.

The source capture reported no observed drop events, but its transport had no end marker; tail completion and producer attribution remain unverified. No claim of lossless capture or ECU acceptance follows. The source note identifies installed firmware 2.0 and a passive baseline with no triggered attempt. Relative timestamps come directly from the captured device times, not inferred from upload times or warning reports.

The reason descriptions use the 2026.26.6.5 candidate signal/reference layout. The numeric codes do not independently validate the current HW4 producer or establish the cause of a different warning event. [Reason layout and source limits](https://github.com/talas9/tesla-can-signal-db/blob/main/research/hw3-carlog-unavailability.md).

## Comparison with the warning recordings

The separate [firmware 3.0 warning recordings](../w462/README.md) preserve diagnostic codes and ages around `APP_w462`, including Autosteer 91 and ACC 69. Those reports do not contain original CAN frames. The baseline and warning recordings differ in timing/context and are not a controlled experiment; their reason-code difference does not establish which setting caused the refusal or an exact run-mode transition.
