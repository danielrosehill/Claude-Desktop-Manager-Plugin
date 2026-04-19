---
name: troubleshoot-hardware
description: Diagnose a misbehaving hardware device — audio, display, USB, Bluetooth, network, input. Runs targeted probes based on the device class and produces a triage report.
---

Troubleshoot a hardware device on the local desktop.

Arguments: `$ARGUMENTS` — the device class or symptom, e.g. `audio`, `display`, `usb`, `bluetooth`, `network`, `keyboard`, `mouse`, or a free-form description.

1. **Classify the issue**. If ambiguous, ask the user one clarifying question.

2. **Run the appropriate probe set**:
   - **Audio**: `pactl info`, `pactl list short sinks`, `pactl list short sources`, `wpctl status` (if pipewire), relevant `journalctl -b` lines for `pulseaudio`/`pipewire`/`wireplumber`/`alsa`.
   - **Display**: `xrandr` or `wlr-randr` (depending on session type), GPU driver via `lspci -k | grep -A2 VGA`, `journalctl -b | grep -iE 'drm|gpu|nvidia|amdgpu|i915'`.
   - **USB**: `lsusb`, `dmesg | tail -n 50`, recent `journalctl -k` entries.
   - **Bluetooth**: `bluetoothctl show`, `systemctl status bluetooth`, recent journal for `bluetoothd`.
   - **Network**: `ip -br addr`, `ip route`, `nmcli device status`, `systemctl status NetworkManager`, `journalctl -u NetworkManager -b --no-pager | tail -n 50`.
   - **Input**: `libinput list-devices` (may require sudo), `udevadm info` for the device, journal for kernel input events.

3. **Summarise findings**: what is the device, is the kernel seeing it, is the daemon managing it, any error signatures in the log?

4. **Propose next steps**: likely fixes ranked by invasiveness (restart daemon → reinstall driver → kernel module reload → reboot). Require confirmation before any destructive action.

5. **Write report** to `outputs/hw-triage-<class>-YYYY-MM-DD-HHMM.md`.
