# 08 — Open Decisions and Approval Gate

**Status:** Pending user review

## Current baseline to confirm
1. Pause/Resume: Resume recalculates next future clock boundary; it does not preserve remaining countdown.
2. Custom intervals across DST: each local day anchors at resolved local midnight, then interval steps are elapsed seconds from that instant.
3. Time-zone change: immediately recalculate using the new time zone.
4. Start with Windows: optional, disabled by default.
5. Visual notification: should it auto-dismiss? If yes, after how many seconds? Current behavior leaves duration open.
6. Sound: current proposal plays once per event. Confirm or specify repeat-until-dismissed.
7. Volume: current proposal uses a 0–100% slider.

## Technical compatibility checks before Phase 1
- Verify .NET 10 LTS support status and Windows 10/11 compatibility at implementation time.
- Verify Windows time-change, time-zone-change, and resume event coverage on target OS builds.
- Verify WPF non-activating notification behavior, especially with full-screen games.
- Select and test WAV playback API and volume semantics.
- Verify single-file publishing and native dependencies on clean machines.

## Phase 0 final checklist
- [ ] Requirements approved
- [ ] Architecture approved
- [ ] Scheduling and alignment approved
- [ ] DST/time-zone rules approved
- [ ] Pause/Resume/Reset approved
- [ ] Audio and notification behavior approved
- [ ] Tray/startup behavior approved
- [ ] Test plan and measurable acceptance criteria approved
- [ ] Compatibility checks completed
- [ ] User explicitly authorizes Phase 1

Phase 1 remains blocked until explicit user approval. Draft documentation does not constitute approval.