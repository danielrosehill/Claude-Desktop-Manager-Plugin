---
name: check-system
description: Snapshot the local desktop — OS, kernel, uptime, CPU/RAM/disk, GPU, desktop environment, display server. Writes a timestamped report to the workspace outputs/ directory.
---

Gather a broad health/identity snapshot of the local Linux desktop.

1. **Identify the system**:
   - `/etc/os-release` (distro + version)
   - `uname -r` (kernel)
   - `uptime -p` (uptime)
   - Desktop environment via `$XDG_CURRENT_DESKTOP`; display server via `$XDG_SESSION_TYPE` (x11/wayland)

2. **Hardware summary**:
   - CPU: `lscpu | grep -E 'Model name|Socket|Core|Thread'`
   - RAM: `free -h`
   - Disk: `df -h -x tmpfs -x devtmpfs`
   - GPU: `lspci | grep -iE 'vga|3d|display'` (fall back gracefully if no discrete GPU)

3. **Package manager(s) present**: detect apt, dnf, pacman, zypper, flatpak, snap.

4. **Report**: Write to `outputs/system-check-YYYY-MM-DD-HHMM.md` and present a concise summary to the user. Flag anything unusual (disk > 85% full, kernel older than distro's current, RAM pressure, missing expected tools).

If `$ARGUMENTS` includes `--quiet`, skip the file write and only print the summary.
