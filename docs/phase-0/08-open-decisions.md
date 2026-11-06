# 08 — Decisions and Phase 1 Authorization

**Status:** Approved by user for implementation
**Approval scope:** Accept the proposed baseline decisions and proceed with Phase 1 implementation and verification. Keep all work on the dedicated implementation branch until release review.

## Approved product decisions

1. **Pause/Resume:** Resume recalculates the next future clock boundary; it does not preserve the remaining countdown.
2. **Custom intervals across DST:** Each local day anchors at resolved local midnight, then interval steps are elapsed seconds from that instant.
3. **Time-zone change:** Immediately recalculate using the new time zone.
4. **Start with Windows:** Optional, disabled by default.
5. **Visual notification:** Non-modal and non-activating; auto-dismiss after 5 seconds. The user may dismiss it sooner. It must not stop the timer.
6. **Sound:** Play once per alert occurrence. Never overlap alert and preview playback.
7. **Volume:** 0–100% slider, persisted in settings.

## Compatibility findings

- .NET 10 is an active LTS release. The current official support policy lists end of support as November 14, 2028. Always build with the latest available .NET 10 servicing patch.
- Windows 10 22H2 Home/Pro reached end of support on October 14, 2025. The application may retain a best-effort Windows 10 22H2 compatibility target, but Windows 10 Home/Pro must be identified as an out-of-support OS unless the device is covered by an applicable ESU/LTSC lifecycle.
- WPF supports showing a window without activating it via ShowActivated=false before Show(). This is the baseline for the alert window; actual full-screen-game behavior still requires runtime testing.
- System-time changes can be observed through WM_TIMECHANGE. Time-zone changes and resume must be handled separately and verified on target Windows builds; no single event is assumed to cover all cases.
- WAV playback, volume semantics, single-file publish, and clean-machine behavior remain runtime verification items.

## Implementation gates

- Phase 1 is authorized.
- Implement on `phase-1/implementation`; do not write directly to `main`.
- Keep the build reproducible through Windows CI.
- Do not claim timing, full-screen, DST, or clean-machine acceptance until corresponding tests have run and evidence is recorded.
- Do not merge or publish a release without a final verified build and release review.
