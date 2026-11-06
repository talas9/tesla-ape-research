# Methodology and evidence limits

AI-assisted analysis and REA helped with the investigation. Independent branch/capture checks were used where retained evidence allowed; unresolved interpretations are labelled rather than presented as results.

## Static work

The trace began at visible warning strings, followed their callers, then recursively followed the defining input variables. GUI saved-request writers were traced forward separately from eligibility decisions. Functions, DataValue registrations, response parsing, persistence and packet encoding were compared without assuming that identical names or IDs mean identical behavior across hardware.

The older APE chain used decoder/callback/shared-memory identity/consumer evidence. Emitter catalogue entries were recovered from constructors and dispatch, while message semantics were retained as unknown where only packing was established. Coverage counts describe the bounded inspected populations, not all firmware.

The public export intentionally omits original binaries, bulk definitions, source/decompilation excerpts, raw captures, screenshots, identifying hashes and private evidence paths. It provides independent summaries and public upstream links. Reproduction against matching images requires private source provenance and permission; this export does not promise full standalone reproducibility.

## Live work

I used a stationary freshness guard and bounded filtered captures, preserved original settings and separated intervention phases. Backend counters, delivered RX, operational candidates and receiver acceptance were treated separately. Engine off/on and isolated feature experiments were distinct populations, not merged as one capture.

The interrupted sweep had a known quality gap: it subscribed only to capture events, omitting DROP notifications. Heartbeat, bus, error and fault notifications are always delivered regardless of subscription. Flat physical bus counters cannot establish lossless filtered delivery. Stream loss correctly triggered the freshness abort; the later guarded reconnect/restore was verified. No moving-state cause or feature-induced failure was established.

## Future improvement

A future harness should retain all capture/drop/error/fault events, connection-close metadata and a bounded identity-checked reconnect/Stop/restore sequence with fresh stationary prerequisites. This is a proposed method improvement, not an implemented capability of the interrupted harness. The freshness guard should remain strict.

Request-correlated acknowledgement requires a proved receiver/status relationship. General availability, warnings, activity states and TX completion are insufficient individually. Matching current hardware remains the highest-value missing evidence.

Firmware and APE images acquired from Lunars. The contributor reports that these were the latest images available to them; no MCU3/HW4 images or newer HW3/HW2.5 APE images were available to them. This availability statement is contributor-reported, not an independent source audit. Modern ICE-based MCU analysis and the older HW2.5 APE artifact remain separate evidence profiles; neither establishes current HW3/HW4 equivalence.
