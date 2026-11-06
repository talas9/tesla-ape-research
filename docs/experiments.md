# Stationary experiments

The experiments used a custom C6 board with a CAN transceiver. Each capture is a separate population. RX producer attribution and visibility of the board's own transmissions are unknown; modern signal meanings remain hardware-validation candidates.

## Engine OFF ON OFF

Six approximately 30-second windows compared the following phases:

| Phase | Records | Filtered IDs observed | Board requests sent |
|---|---:|---:|---:|
| OFF 1 | 5867 | 26/54 | 0 |
| ON | 5888 | 26/54 | 242 |
| OFF 2 | 5871 | 26/54 | 0 |

Per-ID/mux comparison found no phase-only IDs, adjacent-phase constant-bit transitions or reversible stable-bit changes. Original settings were restored; the final runtime state was ARMED/manual.

## Isolated features

Eight phases observed the same 29/57 IDs. Visualization OFF/ON produced no observed RX transition. LaneGraph increased board TX from approximately 242 to 362. ForceFSD OFF produced no Assist modifications/completions; restoring Assist completed 121 requests. No stable receiver-candidate changes were found.

Original configuration content was restored. The final state was revision 24, ARMED/manual, with no observed physical bus counter degradation.

## Stationary visualization

A separate 120-second observation recorded 240 page-0 frames: visualization bit 37 stayed 1, with 497–503 ms gaps. All sampled engine states were ARMED, and settings remained unchanged.

A separate 30-second `0x25D` capture recorded 60 DLC-6 frames. Modern candidate decoding indicated traffic-control feature/state disabled and unavailable reason none. One Assist admission rejection and a distinct later 1 ms `driver_pending` sample occurred in that window; they were not simultaneous.

This parked observation did not reproduce driving flicker.

## Interrupted follow-up and recovery

The follow-up completed a 60-second baseline, Restore with 61 inputs and no modifications because the input already matched, and Assist with 121 modified/accepted/completed requests and no failed completions. Receiver candidates remained unchanged.

After the LaneGraph-only configuration was saved and read back at revision 30, streams continued for approximately 20.8 seconds, then delivery stopped. The freshness guard aborted at receive age 1.003 seconds against its 0.8-second threshold. The last observed gear was Park with valid displayed speed zero. No close-packet or device log established whether the network or server caused the interruption.

The initial restoration reused a closed socket and failed. Recovery reconnected, matched the device identity and uninterrupted uptime, stopped the engine, freshly checked Park/zero, restored the original settings content by compare-and-swap revision 30→31, and verified exact readback. Restart returned ARMED/manual. No test variant remained; LaneGraph testing and the remaining combinations were incomplete.

## What the results establish

No measured feedback change established request acceptance or rejection. Board completion describes transport; an unchanged or disabled operational field does not establish the receiver's decision. Likewise, the interruption does not establish LaneGraph causation.

The isolation harness omitted unsubscribed DROP notifications. Heartbeat, bus, error and fault notifications were always delivered. Flat physical bus counters therefore cannot establish lossless filtered delivery. Future runs need DROP events, close metadata and reconnect-aware restoration; these were not implemented in the interrupted harness. [Methodology](methodology.md#live-work).
