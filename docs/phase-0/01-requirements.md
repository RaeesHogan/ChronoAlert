# 01 — Product Requirements

**Status:** Draft for review

## Product goal
ChronoAlert is a lightweight Windows utility that plays a WAV sound and displays a compact visual notification at recurring real-clock boundaries. Scheduling is not measured from the time Start is pressed.

## Platform
- Windows 10 22H2 (build 19045) and supported Windows 11 x64 versions; verify current Microsoft lifecycle before release.
- C# / .NET 10 LTS / WPF.
- Self-contained, single-file x64 executable; no console.
- Offline operation; no telemetry or network dependency.
- Trimming disabled initially; reassess only after WPF/dependency tests.

## Functional requirements
1. Presets: 1, 2, 3, 5, 10, 15, 30, 60 minutes.
2. Custom interval: minutes + seconds, greater than zero and at most 24 hours.
3. Clock alignment: 5-minute interval started at 14:02:15 alerts at 14:05:00, 14:10:00, 14:15:00. Never use Start time + interval.
4. Display current local time, next alert time, and countdown. Format MM:SS or HH:MM:SS.
5. Controls: Start, Pause, Resume, Reset. Changing interval while Running recalculates immediately.
6. Resume calculates the next future clock boundary; it does not preserve remaining countdown. Reset returns to Stopped.
7. Built-in WAV, user-selected WAV, enable/disable, volume, preview, and no overlapping playback.
8. Compact non-modal visual notification showing scheduled time. Dismissing it does not stop the timer. It must not intentionally steal focus or activate over full-screen games.
9. System Tray: close-to-tray; Open, Start/Pause/Resume, Exit. Explicit Exit stops scheduling and closes the process.
10. Optional Start with Windows, disabled by default.
11. JSON settings at %AppData%\ChronoAlert\settings.json. Persist interval, audio settings, WAV path, volume, notification/tray preferences, and startup preference.
12. No persistent alert history in v1. Keep bounded operational/error logs only.
13. Single active instance; a second launch should activate the existing instance when possible.

## Non-functional requirements
- Low idle CPU and stable memory.
- No busy-wait loop or high-frequency schedule polling.
- Recalculate after sleep/wake, system-clock changes, and time-zone changes.
- Skip missed occurrences; never replay a backlog.
- Prevent duplicate alerts and stale callback dispatch.
- No administrator rights or internet connection required.
- Graceful handling of malformed settings, unavailable audio, and missing WAV files.

## Out of scope
Cloud sync, mobile app, multiple concurrent timers, alert-history database, calendar integration, network time sync, updater service, and hard real-time guarantees.

## Timing target
At least 99% of measured alerts within ±1 second under normal awake conditions on reference Windows systems. Windows is not a hard real-time OS; this is a target, not a guarantee.