# From region warnings to vehicle feedback

This research and its documentation were written entirely by an LLM, using firmware analysis, code inspection and recorded tests. Findings are linked to evidence; unresolved interpretations are marked.

The investigation began with a mismatch: the main FSD control asked for updated navigation maps, while Traffic Light and Stop Sign Control (TLSSC) said the region was unsupported. Following the code showed that these messages come from different checks. Neither message alone identifies the failed condition on the live platform.

## Following the warning back

String references led to the GUI's eligibility functions. The main maps check reads `FEATURE_hasFullSelfDrivingNavMaps`; TLSSC combines capability with localized approval. Gateway configuration, managed feature values and navigation-derived country are separate inputs. Managed values may also be restored from a cache rather than fetched during the current session.

That distinction matters: a “maps supported” flag is not a direct inspection of whether an installed map package needs an update. The recovered client shows how feature values are consumed, but not the server's decision algorithm. [Functions and value producers](trace-details.md#gui-warnings).

## Following the switch forward

Confirmation saves a GUI request. The stock encoder then checks eligibility again before assembling the outgoing message. The custom board takes a different route: it adds modified request frames without suppressing the originals.

Comparing the handlers also corrected an early assumption. ForceFSD Assist is not equivalent to selecting FSD in the GUI, and its inspected upstream predicate has no HW4 exclusion. Unlock has separate hardware branches. Restore can make no change when its input already matches the target. [Request path](trace-details.md#requests-and-transport) · [upstream code and reports](sources.md#inspected-upstream-source).

## Finding a receiver and feedback

The available older APE artifact provided a concrete TLSSC chain: request decoding, accepted state in shared memory, then state-machine reads with additional conditions. A toggle-loss branch produces `STOPS_BUTTON_TOGGLE` and a conditional HOLD state. Its published internal Boolean has not been mapped to CAN feedback.

The older operational-state emitter was recovered separately. Modern signal definitions supplied further feedback candidates, but IDs, widths and enums differ between sources. These profiles cannot be merged solely because a field name looks familiar. [Older APE chain](trace-details.md#older-ape-chain) · [signal profiles](signal-profiles.md).

## What the live comparisons showed

Stationary engine OFF→ON→OFF and isolated-feature comparisons showed no reversible changes establishing request acceptance in the measured feedback candidates. The parked visualization request remained stable during a separate observation; this did not reproduce reported driving flicker. A later sweep ended after connection loss, and recovery restored the original settings. The remaining combinations were not tested. [Measurements and recovery](experiments.md).

Board transmission completion is not vehicle acceptance. A matching HW4 receiver-to-status trace remains the missing link between these interventions and their outcome. A submitted-transmission journal and passive diagnostics have been built and reviewed, but are not deployed. [Implementation and next work](evidence-and-open-work.md).

## Hardware and applicability matrix

| Evidence source | What it establishes |
|---|---|
| ICE-based CID artifact | GUI, feature-policy and request code in the inspected build. |
| Older HW2.5 APE artifact | Older request decoder, shared-state consumers and emitters. |
| Live HW4 platform | Stationary observations described in the experiments. |
| Historical HW3 platform | Reported region-availability symptoms; no paired comparison. |
| Nonmatching gateway artifact | Comparative routing, not target-platform forwarding. |

No matching MCU3/HW4 image was available for this investigation. Older APE behavior therefore does not establish HW4 equivalence. Gateway hardware raw value 5 also remains unresolved rather than being assigned a guessed hardware label.

[Route index](route-index.md) · [corrections](corrections.md) · [methods and source limits](methodology.md)

<sub>Research purposes only. Do not act on these findings or use them to modify or operate a vehicle. The authors accept no responsibility for how this information is used. Do not use it for unlawful activities.</sub>
