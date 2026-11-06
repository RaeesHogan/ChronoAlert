# 05 — UI/UX Specification

**Status:** Draft for review

## Main window
Show ChronoAlert title, current local time, selected interval, countdown, next alert local time, and state (Stopped/Running/Paused).

Controls:
- Preset selector and custom minutes/seconds inputs.
- Start, Pause, Resume, Reset.
- Sound enabled toggle.
- Browse/select WAV.
- Volume slider and Preview.

Countdown uses MM:SS, switching to HH:MM:SS at one hour or more. It is derived from the next absolute occurrence and refreshed after schedule invalidation. It must not be the scheduling authority.

## Visual notification
- Compact, non-modal window showing ChronoAlert and scheduled alert time.
- Dismiss action; dismissal does not stop timer.
- Must not intentionally steal keyboard focus or activate itself over a full-screen game.
- Prefer non-activating presentation; verify actual WPF/Windows behavior on Windows 10/11.
- Audio and visual notification are independent; failure of one does not prevent the other.

## System Tray
Menu: Open; Start/Pause/Resume as appropriate; Exit.
Closing main window hides to tray. Minimize behavior may be configurable. Exit stops scheduler and playback, saves settings, unsubscribes OS events, disposes resources, and terminates.

## Accessibility
Accessible control names, keyboard navigation, readable contrast, no color-only state indication, and inline validation.

## Visual decisions still open
Exact dimensions, icon, typography, theme, notification auto-dismiss policy, and notification placement require review before implementation.