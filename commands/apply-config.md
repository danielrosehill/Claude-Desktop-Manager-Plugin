---
name: apply-config
description: Apply a configuration change (dotfile edit, desktop-environment setting, systemd unit, etc.) with a rollback path. Backs up the original, makes the change, and logs both.
---

Apply a configuration change to the local desktop and keep a rollback path.

Arguments: `$ARGUMENTS` should describe the config change, either as a free-form request or as `<target-file> <change-description>`.

1. **Identify the target**:
   - User dotfile (e.g. `~/.bashrc`, `~/.config/foo/config.toml`)
   - System config (e.g. `/etc/systemd/...`, `/etc/default/...`) — requires sudo
   - Desktop-environment setting (gsettings, kwriteconfig5/6, dconf)

2. **Back up** the original before modifying:
   - File: copy to `<workspace>/logs/backups/<basename>.<timestamp>.bak` (or `~/.config/backups/` if no workspace).
   - gsettings/dconf: capture current value and record it in the log.

3. **Show the diff** of the intended change and wait for user confirmation.

4. **Apply** the change.

5. **Verify** the change took effect (re-read the file, or `gsettings get ...`).

6. **Log** to `logs/config-log.md`: timestamp, target, old value (or backup path), new value, verification result, and the exact rollback command.

If the change requires restarting a service or re-logging into the session to take effect, flag that clearly.
