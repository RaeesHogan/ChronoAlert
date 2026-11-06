# 07 — Test Strategy and Acceptance Criteria

**Status:** Draft for review

## Unit tests
Interval validation/bounds; preset alignment; custom anchor calculation; strictly-future occurrence; DST resolver; state transitions; duplicate suppression; generation invalidation. Use fake wall-clock and monotonic-clock abstractions; tests must not depend on actual current time.

## Application tests
Start/Pause/Resume/Reset; interval changes in Running/Paused; stale callback rejection; audio failure isolation; notification dismissal; configuration load/save/recovery.

## Windows integration tests
Clock forward/backward; time-zone change; sleep/wake; tray and close-to-tray; single instance; WAV preview/volume/playback; notification focus behavior.

## Packaging tests
Self-contained single-file win-x64; no console; clean Windows 10 22H2 and supported Windows 11; offline launch; settings persistence.

## Required scenarios
| ID | Scenario | Expected result |
|---|---|---|
| SCH-01 | 5 min started 14:02:15 | First alert 14:05:00 |
| SCH-02 | 5 min started exactly 14:05:00 | First alert 14:10:00 |
| SCH-03 | 90 sec custom | Local-midnight anchor sequence |
| SCH-04 | Pause before boundary | No alert while paused |
| SCH-05 | Resume after missed boundaries | Next future boundary only |
| SCH-06 | Wake after missed boundaries | No catch-up burst |
| SCH-07 | Clock forward/backward | Recalculate; no duplicate |
| SCH-08 | Time-zone change | Refresh display and custom anchor |
| SCH-09 | Change interval while Running | Old schedule invalidated |
| SCH-10 | Callback after Reset | Rejected |
| SCH-11 | Two launches | One active instance |
| AUD-01 | Missing custom WAV | Fallback/error; timer remains active |
| UI-01 | Close main window | Remains in tray |
| UI-02 | Dismiss notification | Timer continues |
| PKG-01 | Network disconnected | Core functions still work |

## Acceptance criteria
- Timing: at least 99% of 100 measured alerts within ±1 second under normal awake conditions on each reference OS; record actual measurements and maximum deviation.
- Duplicates: zero duplicate dispatches per occurrence identity in automated race/time-change tests.
- Catch-up: zero replayed occurrences at or before post-resume/current time.
- Stability: 24-hour run without crash, unbounded memory growth, or duplicate schedule.
- Resources: no busy loop; record CPU/memory on reference hardware.
- Offline: core functions pass with network disconnected.
- Packaging: executable launches on each supported clean OS test machine.

## Evidence required
Automated test results, OS/build details, timing measurement log, packaging verification, and known limitations. No release candidate until critical scheduling, state, sleep/wake, clock-change, and packaging tests pass.