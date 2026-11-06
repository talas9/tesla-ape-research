# Signal profiles and feedback candidates

Source profiles stay separate because matching IDs do not guarantee matching payloads, networks or hardware.

## Consolidated selection

| Source set | Coverage |
|---|---|
| Selected modern reference | 55 IDs: 48 older-registry IDs, six additional candidates and `0x25D`; 37 definitions / 2700 signals; 18 IDs lack definitions. |
| Full modern reference | 676 messages / 42132 signals, retained privately. |
| Older APE | 52 registry entries / 48 unique IDs; 18 recovered layouts / 291 packing groups. |
| Two CH source profiles | Identical 26 messages / 39 unique signals, represented as 78 profile rows. Hardware/firmware applicability remains unknown. |

The consolidated selection corrects an earlier 31-definition subset that omitted six external candidates; the older 54-ID inventory remains unchanged. Verification and independent review passed: 3 tests, strict parsing and source-pointer checks, with no blocking findings.

The older registry lists potential emissions, not necessarily observed messages. Source network labels remain attached: equal IDs do not establish an ETH-to-CH route, and not all CAN is CH. Bulk databases are not published here.

## Feedback shortlist

Positions use `start bit | width`. These are modern reference candidates, not a validated HW4 decoder.

| Feature | Candidate fields | Interpretation |
|---|---|---|
| Traffic control | `0x25D`, DLC 6: feature `0\|2`, state `2\|3`, unavailable `38\|4` | Traffic-control state, distinct from general APP status. |
| FSD operation | `0x399` state `0\|4`; `0x259` suspension `19\|2`, health `49\|1`; `0x191` unavailable `2\|9`, DLC 2 | Operational and diagnostic context. |
| Summon | `0x399` available `51\|1`; `0x259` reason `14\|5` | Availability, not configuration acceptance. |
| Navigation | `0x24A` navigation bit 39; `0x399` state | Navigation availability differs from managed FSD maps support. |
| Hands | `0x399` state `42\|4` | Hands-on state candidate. |
| Visualization/LaneGraph | No independent feedback established | Request bits cannot stand in for receiver feedback. |

The older `0x247` abort field is 7 bits wide; the modern field is 8. Older and modern `0x399` states also differ. Applying a modern enum to an older publisher would change the interpretation without evidence. [Producer and consumer trace](trace-details.md#older-ape-chain).
