# APP_w462 diagnostic recordings

These are two sanitized recordings extracted from delivered scanner reports, not reconstructed CAN traces. Each retains the complete recorded 11-sample warning/bus window and three selected receiver checkpoints. The JSON files contain numeric diagnostic codes and their ages; the source reports did **not** retain the original CAN payloads or DLCs.

| Recording | Scanner / UI | Warning-relative receiver checkpoints |
|---|---|---|
| [November 18 evidence](2026-11-18-diagnostic-window.json) | 3.0 / 0.16.19 | −666 ms, onset, +1,000 ms |
| [November 19 evidence](2026-11-19-diagnostic-window.json) | 3.0 / 0.16.20 | −698 ms, onset, +1,000 ms |

The dates identify the evidence/readback records. The November 18 source had no absolute event time. The November 19 event had an absolute time, omitted here for privacy. Neither upload time nor a separate saved bus incident is used to date the warning.

## What the recordings show

Autosteer reason **91**, ACC reason **69** and autonomy reason **4** are present and fresh at all three checkpoints in both recordings. The reference descriptions for 91 and 69 concern unsupported user settings; they do not identify which setting failed. Both codes already exist before the warning onset. All retained bus samples show running with no bus-off and no increase in the error or drop counters. Delta-zero does not mean that the original cumulative counter was zero or that earlier capture was lossless.

November 18 clears the warning at its last sample (+5,002 ms); November 19 still shows it at +5,000 ms. The original alert history was partial in both reports. “Complete” applies to this recorder's bounded warning window, not to all vehicle events or CAN traffic.

## Fields and provenance

`offset_ms` is relative to the scanner's observed warning onset, not the driver's engagement input. `receiver_checkpoints[].sample_index` points into `samples`. The checkpoint contains the latest cached diagnostic observation at that time: `age_ms` gives its age, `available` records presence and `fresh` uses the deployed 1,500 ms threshold. An observation at a checkpoint with offset −666 ms and age 527 ms was sampled approximately 1,193 ms before warning onset; it is not a newly received frame at that checkpoint.

| Published checkpoint field | Decoder source | Meaning of stored value |
|---|---|---|
| `autosteer_unavailable_reason` | `0x5D9`, mux 0, little-endian start bit 4, width 7 | Extracted numeric code, with independent source age. |
| `acc_unavailable_reason` | `0x5D9`, mux 2, little-endian start bit 46, width 7 | Extracted numeric code, with mux-2 source age. |
| `autonomy_unavailable_reason` | `0x191`, little-endian start bit 2, width 9 | A distinct reason field; its code 4 must not be confused with Autosteer reason 4. |

Source reports store `receiver_context_v2` as a 34-byte packed diagnostic structure. It combines selected signal bits, availability/freshness masks and ages; it is **not** an original CAN payload. These published files decode only the three fields above, preserve their numeric values/ages and the warning-relative timing, and omit the other receiver fields. Bus counters are converted to deltas from the first retained sample; running, bus-off and warning states are unchanged. Decoding was checked against scanner firmware `fw-v3.0` packing and the separately retained decoded exports.

The scanner identifies the decoder as `modern_2026_dbc_candidate`; the reference release is 2026.26.6.5. Archived HW3 producer and service-UI evidence corroborate the reason-field layout, but matching current HW4 producer and rejection-predicate validation remain open. The reference names are diagnostic leads, not current-hardware causal proof. [Research context](../../../LLM.md#november-18-21-evidence-update) · [Reason-layout evidence](../../../LLM.md#november-13-eligibility-and-rejection-evidence)

## What is not in these files

No original CAN frames, driver-input timestamp, vehicle motion, feature configuration, request/TX journal, boot identity, network address, receipt, absolute event timestamp, or vehicle identity is included. The source reports themselves did not include the first six of those items. Separate older bus incidents and unrelated history entries are excluded. No exact run-mode transition or APE reboot can be established from these recordings.

The numerical source/output request comparison documented elsewhere is a separate observation, not synchronized with these warnings. These recordings do not establish acceptance or the precise current HW4 refusal condition, and they are not a complete raw-frame capture of the engagement attempt.
