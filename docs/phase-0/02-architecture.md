# 02 — Architecture Design

**Status:** Draft for review

## Layers
- **Presentation:** WPF Main Window/ViewModels, notification window, tray icon/menu, validation, formatting, UI Dispatcher. Must not calculate schedule boundaries.
- **Application:** Timer orchestration, Start/Pause/Resume/Reset, interval changes, coordination of scheduler/audio/notification/configuration, single-instance activation.
- **Core/Domain:** Interval validation, alignment rules, next-occurrence calculation, timer state, transitions, occurrence identity and duplicate policy. Must be testable without WPF.
- **Infrastructure:** Wall and monotonic clocks, Windows time/power notifications, one-shot wait, WAV playback, JSON persistence, logging, single-instance OS integration.

## Proposed layout
```text
src/ChronoAlert.App/             # WPF UI
src/ChronoAlert.Core/            # Scheduling and domain rules
src/ChronoAlert.Infrastructure/  # Audio, configuration, Windows integration
tests/ChronoAlert.UnitTests/
tests/ChronoAlert.IntegrationTests/
docs/phase-0/
```
Logical proposal only; do not create project files in Phase 0.

## Design rules
- One scheduler is the authority for due times.
- Wall clock decides due occurrences; monotonic time supports countdown display and wait measurement only.
- Timer callbacks are wake-up hints. Re-read wall clock and validate state before dispatch.
- Serialize state-changing commands and dispatch decisions.
- Use a schedule generation/version token to reject stale callbacks.
- Marshal UI changes through WPF Dispatcher.
- Infrastructure failures must not corrupt timer state.

## Scheduler and Windows events
Use a cancellable one-shot wait followed by wall-clock re-evaluation. Investigate and test Windows time-change, time-zone-change, and resume notification coverage on Windows 10/11. Candidate APIs include Microsoft.Win32.SystemEvents.TimeChanged and PowerModeChanged, plus any required Windows time-zone notification. Do not assume one event covers every change.

## Configuration
Versioned JSON; load safe defaults if missing; validate fields independently; save atomically via temporary file and replace/move. Persistence failure must not terminate scheduling.

## Audio
WAV-only v1 behind an abstraction. Verify selected Windows playback API, volume behavior, device failure handling, and preview/alert mutual exclusion.

## Packaging
win-x64, self-contained, single-file, WPF, no console, trimming disabled initially. Verify native dependencies and launch behavior on clean Windows 10/11 systems. Single-file does not remove the need for clean-machine validation.

## Proposed dependencies
Keep dependencies minimal. Microsoft.Extensions.DependencyInjection and System.Text.Json are candidates, not mandatory commitments. xUnit is a candidate test framework. Final selection belongs in Phase 0 compatibility review.