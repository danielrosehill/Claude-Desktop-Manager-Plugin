# desktop-manager-plugin

Claude Code plugin for managing a local Linux desktop workstation — system checks, package management, config application, hardware troubleshooting, service inspection, and log review. Ships primitive slash commands plus a provisioning skill to spin up a fresh desktop-management workspace.

Scope is explicitly **local desktop only**. Remote server/homelab admin lives in the separate `sysadmin-homelab` plugin.

Part of the [danielrosehill Claude Code marketplace](https://github.com/danielrosehill/Claude-Code-Plugins).

## What you get

### Primitives (always available once the plugin is installed)

Slash commands under `/desktop-manager:*`:

- `check-system` — snapshot OS, kernel, uptime, CPU/RAM/disk, GPU, desktop environment
- `install-package` — install a package via the detected package manager (apt/dnf/pacman/zypper + flatpak/snap)
- `remove-package` — safely remove a package and surface reverse-dependencies first
- `apply-config` — apply a dotfile or desktop-environment config change with a rollback path
- `troubleshoot-hardware` — diagnose a misbehaving device (audio, display, USB, Bluetooth, network)
- `inspect-service` — check a systemd unit's status, recent logs, and common failure modes
- `review-logs` — pull recent journalctl / dmesg output filtered for errors
- `update-system` — run a full system update through the detected package manager(s)

### Provisioning skill

- `/desktop-manager:new-workspace <name> [<target-path>] [--local-only] [--private]`

Scaffolds a new workspace (CLAUDE.md + context/outputs/logs), personalises it from `~/.claude/CLAUDE.md` (OS, locale, timezone, identity), and by default creates a public GitHub repo for it.

## Pattern

Primitives live in the plugin → globally available from any cwd.
Workspace scaffold is provisioned as **data** → no `.claude/` tree inside provisioned workspaces.
Plugin updates never touch your workspace data.

See [PLAN.md in Claude-Workspace-Reshaping-190426](https://github.com/danielrosehill/Claude-Workspace-Reshaping-190426) for the full pattern spec this plugin follows.

## Install

Via the danielrosehill marketplace:

```
/plugin marketplace add danielrosehill/Claude-Code-Plugins
/plugin install desktop-manager
```

## License

MIT.
