# Raw frames across APP_w462 — November 25

[Download the raw CAN excerpt (JSONL)](raw-can.jsonl).

This is a new raw-frame recording containing an observed `APP_w462_selfDrivingUnavailable` transition. It is separate from the earlier decoded warning reports and the November 18 no-attempt baseline. Original payloads are retained; no frames were reconstructed from reason values.

The uploaded browser CSV contains 20,000 rows spanning **6,636 ms**, with 187 distinct IDs. This privacy-scoped excerpt retains **all 45 rows** matching `0x4D1`, `0x191`, or `0x5D9` low-nibble mux 0/2, in original order. Other IDs and mux pages are excluded. The source file remains private.

## Observed transition

Times below are milliseconds from the first frame in the original CSV, not the driver's input.

| Observation | Relative time | Original payload |
|---|---:|---|
| Last clear `0x4D1` before the rise | 2,261 ms | `00 00 00 00 00 00 00 00` |
| First set `0x4D1` | 3,262 ms | `00 20 00 00 00 00 00 00` |
| Last captured `0x4D1`, still set | 6,261 ms | `00 20 00 00 00 00 00 00` |

Using the 2026.26.6.5 reference layout, bit 13 rises between the last-clear and first-set samples: **(2,261, 3,262] ms**. There are three clear and four set samples, with no captured falling edge. File coverage extends 3,374 ms beyond the first set sample. Bit 29, the reference `APP_w478_runModeSwitch` flag, is clear in all seven samples; this does not establish which run mode was active.

## Reason values before and after

| Candidate field | Samples | Values throughout | Closest before / after first warning frame |
|---|---:|---|---|
| `0x5D9` mux 0, bits 4–10: Autosteer reason | 10 | 91 | −429 / +552 ms |
| `0x5D9` mux 2, bits 46–52: ACC reason | 21 | 69 | −299 / +425 ms |
| `0x191`, bits 2–10: autonomy reason | 7 | 4 | −979 / +21 ms |

The reason pair **91/69 already existed before the warning and remains unchanged after it**. Reference descriptions concern unsupported user settings; they do not identify the rejected setting or establish the current HW4 predicate. Autonomy code 4 belongs to a separate enum (reference label `DELIVERED`), not Autosteer reason 4. The recorded `0x5D9` payloads contain changes in other bits even though these extracted reason values stay constant.

The mux-0 layout is documented in the [qualified older-HW3 reason supplement](https://github.com/talas9/tesla-can-signal-db/blob/main/research/hw3-carlog-unavailability.md); it is absent from the modern canonical signal table. Other cited layouts come from the [2026.26.6.5 reference table](https://github.com/talas9/tesla-can-signal-db/blob/main/data/2026.26.6.5/signals.csv). Applying these references to the current vehicle remains a candidate interpretation.

## Format, provenance and limits

- `offset_us` is the original CSV `Time Stamp` minus the first original row's timestamp, in microseconds. The browser exporter converts device uptime milliseconds to microseconds; these are not UTC timestamps. The November 25 date identifies the supplied export, not independently validated vehicle wall-clock time.
- ID, DLC, payload bytes, row order and the CSV `extended` value are preserved. `extended` remains the original string `"false"`. The source has no RTR column. Absolute uptime, bus column and unrelated traffic are omitted.
- The exporter hardcodes `Dir=Rx`; that label is omitted rather than presented as proof of producer identity or absence of scanner transmissions.
- The browser raw-log export retains at most the latest 20,000 frames. This file reaches that cap and is a bounded tail, not the full recording session. Source timestamps never decrease; the largest adjacent-row gap is 33 ms. No exported drop counters or sequence numbers establish lossless capture.
- The CSV contains no scanner firmware/UI version metadata or driver-input marker. No version, feature configuration, ECU identity, input time, vehicle motion, acknowledgement or causal rejection predicate is inferred here.

This supplies actual frames around an observed warning rise. It does not explain why the internal eligibility check failed. [Earlier decoded warning windows](../README.md) · [Older raw baseline](../../passive-baseline/README.md).
