---
name: install-package
description: Install one or more packages using the detected package manager (apt, dnf, pacman, zypper, flatpak, snap). Confirms with the user before running sudo, and logs the action.
---

Install a package on the local desktop.

Arguments: `$ARGUMENTS` is the package name(s), optionally followed by `--source=<apt|dnf|pacman|zypper|flatpak|snap>` to force a specific backend.

1. **Detect package manager**: if `--source` not given, pick the first available in this order — native distro PM (apt/dnf/pacman/zypper) then flatpak, then snap. If multiple natives coexist, ask the user.

2. **Resolve the package**: query the PM for the exact package name. If there are near-matches, show them and confirm.

3. **Show the plan**: print the full install command (with `sudo` where required) and a one-line summary of what will change. Wait for user confirmation before executing.

4. **Execute** the install. Stream output.

5. **Log**: append an entry to `logs/install-log.md` in the workspace with timestamp, package, source, and the command run. If no workspace is detected (skill not provisioned), skip logging and just report to the user.

6. **Post-install check**: if the package provides a binary, confirm `which <binary>` resolves.
