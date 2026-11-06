# 04 — Timer State Machine

**Status:** Draft for review

## States
- **Stopped:** no active schedule.
- **Running:** one future occurrence is scheduled.
- **Paused:** scheduling suspended; no alert may dispatch.
- **Terminated:** process is exiting; terminal state.

## Transitions
| Current | Event | Next | Behavior |
|---|---|---|---|
| Stopped | Start | Running | Calculate next occurrence strictly after now |
| Running | Pause | Paused | Cancel active wait |
| Paused | Resume | Running | Recalculate from current wall clock |
| Running/Paused | Reset | Stopped | Clear schedule |
| Running | Interval change | Running | Invalidate old generation and recalculate |
| Paused | Interval change | Paused | Store interval for Resume |
| Running | Wake/clock/time-zone change | Running | Recalculate; skip missed occurrences |
| Any | Exit | Terminated | Stop scheduler and release resources |

## Command behavior
Start while Running is a no-op. Pause outside Running is a no-op. Resume is valid only from Paused. Reset is valid from Running or Paused. Invalid interval input leaves the previous valid setting and schedule unchanged.

## Invariants
1. Only Running can dispatch.
2. At most one active scheduler wait.
3. At most one dispatch per occurrence identity.
4. Every wake-up revalidates wall clock and state.
5. Paused and Stopped never dispatch.
6. Dismissing a notification does not change timer state.
7. Audio/notification failure does not change timer state.
8. Exit is terminal.

## Race control
Serialize commands and dispatch decisions. Cancellation alone is insufficient when a callback is already in flight. Use a generation token and re-check state, generation, due instant, and duplicate status immediately before dispatch.