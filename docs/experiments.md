# Current-car experiments

Stationary controlled captures used a custom C6 board with a CAN transceiver. RX producer attribution and visibility of the board's own TX remain unknown. Raw captures and device identifiers are excluded; these are sanitized derived findings. Layout meanings remain candidates unless separately validated.

## Engine OFF ON OFF

Six approximately 30 s windows: OFF 1=5867 records, ON=5888, OFF 2=5871. The same 26/54 filtered IDs were observed in every phase. The measured per-ID/mux analysis found no ON-only/OFF-only IDs, adjacent-phase constant-bit transitions or reversible stable-bit changes. Engine ON recorded 242 sent requests; OFF recorded 0. Original settings were restored, with final settled ARMED/manual state.

This negative measured-feedback result does not prove the requests were ignored or rejected.

## Isolated features

Eight phases observed the same 29/57 IDs. Visualization OFF/ON produced no observed RX transition; LaneGraph increased scanner TX from approximately 242 to 362. ForceFSD OFF produced 0 Assist modifications/completions; restored Assist completed 121 requests. No stable receiver-candidate changes were found. Exact original configuration content was restored; final revision 24 ARMED/manual, with no physical bus counter degradation.

## Stationary visualization

A separate 120 s read-only capture observed 240 page 0 frames, visualization bit 37 always 1 and 497–503 ms gaps. All 240 sampled engine states were ARMED. Revision 24 remained unchanged. This did not reproduce driving behavior.

A separate 30 s 0x25D window observed 60 DLC 6 frames, with a constant observed payload (excluded from this export). Modern candidate decoding gives traffic-control feature/state disabled and unavailable reason none. This is not an ACK tied to an injected request.

One Assist admission rejection and a later distinct 1 ms `driver_pending` sample occurred in that sidecar window. Their timing was separate; neither proves display-flicker causation.

## Interrupted follow-up and recovery

Partial follow-up: clean 60 s baseline; Restore 61 processed/0 modified because input already matched target; Assist 121 modified/accepted/completed with 0 failed completions and unchanged receiver candidates.

A later isolated LaneGraph configuration was saved/read back at revision 30. Streams continued for approximately 20.8 s after the save, then stopped. The freshness guard aborted when receive age reached approximately 1.003 s against its 0.8 s threshold. Last observed gear was Park and displayed speed valid zero. This does not establish a moving vehicle, reboot, hardware failure or LaneGraph causation. Root cause remains unknown. Review found that both delivered streams stopped; no origin close-packet or device close log established whether the network or server caused it. The recovery harness initially reused a dead socket; a bounded reconnect-and-restore procedure succeeded.

The initial restore failed on a closed WebSocket. Guarded recovery subsequently matched the same device identity/uptime without reboot, acknowledged Stop, freshly verified Park/displayed zero, restored original configuration **content** by CAS 30→31, and verified exact readback. Start returned ARMED/manual and captures closed. No test variants remain. Revision advanced; original settings content was restored. LaneGraph testing was interrupted; remaining individual cases/combinations were not run.

## Interpretation limits

- Backend accepted/completed describes the board's transport lifecycle, not an ECU ACK.
- An unchanged or disabled operational field is not rejection proof.
- Message presence alone does not establish publisher, bus route or matching semantics.
- Stationary results do not explain driving flicker.
- The interrupted experiment is partial; it is not a completed feature-combination sweep.

## Capture-quality correction and future method

The isolation harness subscribed to capture events only. DROP notifications were therefore omitted. Heartbeat, bus, error and fault notifications are always delivered regardless of subscription. Flat physical CAN error/drop counters do **not** establish lossless filtered capture or lossless WebSocket delivery. Earlier descriptions of a clean capture are restricted to the observed counters and delivered records. Counter deltas and absence of reported errors cannot fill this notification gap.

A future run should subscribe to capture/drop/error/fault events, retain connection-close metadata, and implement a bounded identity-checked reconnect followed by Stop and fresh-stationary-guarded restoration. This improvement is proposed here, not implemented in that isolation harness. The Park freshness requirement must not be relaxed. Remaining sweep cases were not run.
