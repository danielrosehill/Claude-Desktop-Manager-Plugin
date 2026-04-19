---
name: inspect-service
description: Inspect a systemd service or user unit — status, recent logs, failure history, config path. Use when a service won't start, keeps crashing, or is behaving oddly.
---

Inspect a systemd unit on the local desktop.

Arguments: `$ARGUMENTS` — the unit name (e.g. `NetworkManager`, `bluetooth`, `pipewire.service`). If the unit is a user service, append `--user`.

1. **Unit type detection**: system vs user. If the user didn't specify and the unit doesn't exist at system scope, try user scope (`systemctl --user`).

2. **Core checks**:
   - `systemctl [--user] status <unit> --no-pager`
   - `systemctl [--user] is-enabled <unit>`
   - `systemctl [--user] is-failed <unit>`

3. **Logs**: `journalctl [--user] -u <unit> -b --no-pager | tail -n 100`. Highlight the first and last error lines.

4. **Config path**: `systemctl [--user] show <unit> -p FragmentPath`. Offer to open/print it.

5. **Common failure signatures**: look for permission errors, missing binaries (ExecStart pointing at a nonexistent path), port-already-in-use, dependency failures. If detected, suggest the corresponding fix.

6. **Report** to the user with: status, enabled state, restart count, top error signatures, and recommended next action.
