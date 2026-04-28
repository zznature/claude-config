# Monitor Background Tasks

Check background-task checks reasonably to save tokens and keep tasks on track.

## Check Intervals

- Estimate runtime before launching. Tell the expected wait to the user.
- First check after full launch (e.g. 2 minutes) to make sure the task is running.
- **Subsequent checks**: exponential backoff — 2 min, 5 min, 10 min, then stop and wait for the task-notification.

## Checking Method

**Prefer `Monitor` over repeated `Bash` polls.** A single `Monitor` call with `tail -f | grep --line-buffered` replaces dozens of manual `ps` + `wc` rounds. Use it whenever the task writes to a log file.
