# 03 — Clock-Aligned Scheduling Specification

**Status:** Draft for review. Changes to these rules require explicit user approval.

## Time model
- Wall-clock time is authoritative for due-time decisions.
- Monotonic time is only for countdown rendering and measuring waits.
- Display uses Windows local time zone.
- Occurrences are identified internally by absolute instants, not formatted time strings.

## Integer-minute intervals
For whole-minute intervals, align to local clock minute boundaries. Example: 5 minutes → 14:05, 14:10, 14:15. A 60-minute interval aligns to local hour boundaries.

## Custom intervals not divisible by 60 seconds
For intervals such as 90 seconds:
1. Resolve the current local calendar day's midnight.
2. Convert that anchor to an absolute instant.
3. Generate occurrences at anchor + n × interval elapsed seconds.
4. Choose the first occurrence strictly later than now.
5. Re-resolve the anchor at each local date change.

For absolute anchor A, interval I > 0, and current instant N:
`Next = A + (floor((N - A) / I) + 1) × I`
Use overflow-safe absolute-time arithmetic. Do not apply a generic Unix epoch formula that ignores the specified interval anchor.

## DST and time-zone rules
- During a repeated local hour, distinct absolute instants may display the same local time; they remain distinct occurrences.
- Resolve invalid/ambiguous local midnight deterministically: choose the earliest valid instant at or after nominal midnight; if ambiguous, choose the earlier absolute occurrence.
- A DST day may contain 23 or 25 elapsed hours. Custom intervals remain elapsed offsets from that day's resolved midnight.
- A time-zone change invalidates the pending schedule, refreshes local display, and recalculates under the new zone.

## Lifecycle semantics
- Start: first occurrence strictly after current time.
- Pause: cancel active wait; no alert while paused.
- Resume: recalculate first occurrence strictly after current wall-clock time; do not preserve remaining duration.
- Reset: cancel schedule and return to Stopped.
- Change interval while Running: invalidate old generation and recalculate immediately.
- Change interval while Paused: store; apply on Resume.
- Sleep/wake: recalculate; skip all occurrences at or before now.
- Clock moved forward/backward: invalidate pending wait and recalculate.
- App restart: do not automatically resume an active session in v1.

## Missed alerts and duplicate prevention
- Never replay a backlog or generate catch-up bursts.
- At most one dispatch per occurrence identity.
- Serialize state changes and dispatch.
- Before dispatch verify Running state, current generation, due occurrence identity, and not-yet-dispatched status.
- A stale callback after Pause/Reset/interval change must be rejected.

## Accuracy
Target dispatch within 1 second under normal awake conditions. Windows scheduling jitter, CPU saturation, sleep, and process suspension can exceed this target; no hard real-time guarantee is made.

## Required verification
Test time-change/time-zone notification coverage, resume handling, delayed callbacks, duplicate suppression, DST custom intervals, and countdown resynchronization on Windows 10 and 11.