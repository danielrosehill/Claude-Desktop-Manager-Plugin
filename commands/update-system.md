---
name: update-system
description: Run a full system update using the detected native package manager plus any present supplementary sources (flatpak, snap). Shows what will change before applying.
---

Update the local desktop.

Arguments: `$ARGUMENTS` — optional flags:
- `--dry-run` — show what would be updated without applying
- `--native-only` — skip flatpak/snap
- `--yes` — skip the confirmation prompt (still shows the plan)

1. **Detect package managers present**: apt, dnf, pacman, zypper (pick the native one for this distro); then flatpak, snap if installed.

2. **Refresh metadata**:
   - apt: `sudo apt update`
   - dnf: `sudo dnf check-update` (exit code 100 means updates available — treat as non-error)
   - pacman: `sudo pacman -Sy`
   - zypper: `sudo zypper refresh`

3. **List pending updates** per source and show a combined summary (package count, total download size where available).

4. **If `--dry-run`**: stop here.

5. **Confirm** with the user (skip if `--yes`).

6. **Apply**:
   - Native PM upgrade (`apt upgrade`, `dnf upgrade`, `pacman -Su`, `zypper update`)
   - `flatpak update -y` if flatpak present and not `--native-only`
   - `sudo snap refresh` if snap present and not `--native-only`

7. **Post-update checks**: `needrestart` or equivalent if available; flag whether a reboot is needed (`/var/run/reboot-required` on Debian-family, `dnf needs-restarting -r` on RPM-family).

8. **Log** to `logs/update-log.md`: timestamp, counts per source, reboot-required flag.
