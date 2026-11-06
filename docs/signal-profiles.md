# Signal candidates and profiles

This export deliberately contains no bulk DBC/database copies. The following counts describe retained private source extraction, not independently verified current-HW4 semantic coverage.

## Consolidated selection

The latest selection has 55 IDs:48 unique older registry IDs, six external modern candidates and 0x25D. It contains 37 modern message definitions/2700 signals;18 selected IDs lack definitions. The full canonical reference contains 676 messages/42132 signals and remains separately hash-pinned. An earlier 31-definition subset omitted the six external candidates; the consolidated selection corrects that omission. The older 54-ID inventory remains immutable.

Older APE evidence is separate:52 registry entries/48 unique IDs;18 older layouts/291 packing groups. Those are potential registered emissions, not messages necessarily observed on the current car.

Two recovered CH source profiles contain identical 26 messages/39 unique signals, represented as 78 profile rows. Hardware/firmware applicability is unknown; folder labels were not treated as authoritative product identity. Source networks are preserved. Numerical ID equality does not create an ETH-to-CH route, and not all CAN is CH.

I completed verification and independent review: 3 tests, strict parsing and source-pointer checks passed with no blocking findings. No claim is made that every Tesla CAN signal repository was imported.

## Small feedback shortlist

Minimal numeric summaries below are research candidates, not a released decoder specification or acceptance oracle.

| Feature | Modern candidate | Limitation |
|---|---|---|
| TLSSC/traffic control |0x25DDLC 6: feature 0\|2, state 2\|3, unavailable 38\|4 | Additional to prior 54/57 filters; actual-HW4 applicability unproved. |
| FSD operation |0x399 state 0\|4;0x259 suspension 19\|2/health 49\|1;0x191 unavailable 2\|9 DLC 2 | General operational/diagnostic feedback, not request ACK. |
| Summon |0x399available 51\|1;0x259 reason 14\|5 | Availability differs from configuration acceptance. |
| Navigation |0x24A navigation 39;0x399 state | Navigation available is not equivalent to managed FSD maps support. |
| Hands |0x399 state 42\|4 | Matching version/hardware remains unverified. |
| Visualization/LaneGraph | No independent feedback established in the bounded search | Do not substitute request bits as ACKs. |

Old 0x247abort width 7 versus modern width 8 is a real version discrepancy. Older/current 0x399 states also differ; a modern enum name cannot be applied silently to an older publisher.
