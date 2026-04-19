---
name: remove-package
description: Remove a package from the local desktop, showing reverse-dependencies first so the user knows what else might be affected. Confirms before running sudo.
---

Safely remove a package from the local desktop.

Arguments: `$ARGUMENTS` — the package name, optionally followed by `--source=<backend>` and/or `--purge` to remove config files (apt) where applicable.

1. **Detect the package source**: which PM owns this package? (apt/dnf/pacman/zypper/flatpak/snap)

2. **Reverse-dependency check**: show what else depends on this package.
   - apt: `apt-cache rdepends --installed <pkg>`
   - dnf: `dnf repoquery --installed --whatrequires <pkg>`
   - pacman: `pacman -Qi <pkg>` (Required By)
   - zypper: `zypper search --requires-pattern <pkg>`

3. **Show the plan** and require explicit user confirmation before running the removal.

4. **Execute** with the appropriate flag (e.g. `apt remove`, `apt purge`, `dnf remove`, `pacman -R`).

5. **Log** the action to `logs/remove-log.md` in the workspace with timestamp, package, source, and reverse-deps noted.

Never auto-remove orphaned dependencies without asking.
