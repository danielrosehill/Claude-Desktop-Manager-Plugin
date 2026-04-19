---
name: review-logs
description: Pull recent journalctl and dmesg output filtered for errors/warnings. Useful after a crash, after boot, or when something is misbehaving but the user doesn't know which service is at fault.
---

Review recent system logs on the local desktop.

Arguments: `$ARGUMENTS` — optional filters:
- `--since=<time>` (default: `boot`)
- `--priority=<err|warning|notice>` (default: `err`)
- `--unit=<name>` to scope to one unit
- `--kernel` to include kernel ring buffer (`dmesg`)

1. **Gather**:
   - `journalctl -b --priority=<priority> --no-pager` (or `--since=<time>` if given)
   - If `--unit=<name>`: scope with `-u <name>`
   - If `--kernel`: also run `dmesg -T --level=err,warn | tail -n 100`

2. **Deduplicate**: group repeated log lines with a count. Spammy repeated errors are often the real signal.

3. **Categorise** by source (unit name, kernel subsystem).

4. **Summarise** top 5–10 distinct issues with: source, severity, count, first occurrence, representative message.

5. **Write** a full report to `outputs/log-review-YYYY-MM-DD-HHMM.md` and print the summary to the user.

6. **Suggest** which issues warrant running `/desktop-manager:inspect-service` or `/desktop-manager:troubleshoot-hardware`.
