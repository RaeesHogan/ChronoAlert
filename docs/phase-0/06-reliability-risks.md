# 06 — Reliability, Error Handling, and Risks

**Status:** Draft for review

## Failure handling
| Failure | Expected behavior |
|---|---|
| Settings missing | Load defaults |
| Malformed JSON | Preserve/rename corrupt file if possible; load defaults; log |
| Settings write fails | Keep app running; warn and log |
| Custom WAV missing/invalid | Built-in fallback; warn; timer continues |
| Audio device unavailable | Timer continues; log and warn |
| Notification creation fails | Scheduler continues; log |
| Tray initialization fails | Main window remains usable |
| Delayed callback | Re-read wall clock; skip past occurrences |
| Callback after Pause/Reset | Reject by state/generation check |
| Unexpected exception | Log safely; do not continue with corrupted scheduler state |

## Risks and mitigations
- **Timer jitter:** one-shot wait, re-read wall clock, measure timing.
- **Clock correction:** subscribe to and test OS time-change notifications.
- **DST/time-zone changes:** explicit anchor resolver and deterministic tests.
- **Sleep/resume event gaps:** recalculate on resume and validate occurrence.
- **Callback races:** serialized state machine and generation token.
- **WPF focus behavior:** non-activating notification and fullscreen game tests.
- **Single-file native dependencies:** clean-machine packaging tests.
- **Framework/OS lifecycle:** verify current .NET and Windows support before release.

## Resource and privacy constraints
No busy loops, no administrator rights, no network calls, no telemetry. Idle resource usage should be measured. Memory must remain stable during a 24-hour soak test. Logs must be bounded and contain only necessary operational/error data. Treat custom WAV paths as untrusted input; never execute selected files.

## Verification obligations
Document actual OS builds, timing measurements, memory/CPU observations, and known limitations. Do not claim a Windows event or packaging behavior is supported until tested.