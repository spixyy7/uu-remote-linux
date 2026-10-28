# uu-remote-linux · NetEase UU Remote for Linux (unofficial)

[简体中文](README.md) | **English**

Use **NetEase UU Remote (网易 UU 远程)** on Linux, in both directions:

- **Host** (`uuremote-server`): sign in with your UU account and your Linux computer shows up in the official
  Windows / Mac / phone apps, ready to be controlled: desktop, sound, file transfer, terminal, port mapping, remote assistance.
- **Viewer** (`uuremote-viewer`): control your Windows / Mac / Linux devices from Linux, with a launcher, an in-window
  control center and a network-status panel.

Native Rust + C, no Wine, no NetEase binaries. The UI is in **English and Chinese**.

<p align="center"><img src="docs/images/viewer-window.png" width="720" alt="Linux viewer: device list (names redacted)"></p>

> Unofficial hobby project, **not affiliated with NetEase** and not an official client.
> Contact: **[@spaleon_x](https://x.com/spaleon_x)** on X.

## What's new in this fork

| | |
|---|---|
| 🌐 **English / Chinese UI** | Every window and message in both languages, switchable in settings |
| 🔎 **Sharp viewer picture** | Decoded at the host's native resolution with filtered scaling; text no longer looks blurry or uneven |
| ⏳ **Loading screen** | A smooth animation with the connection stage instead of a black window |
| 🎛 **Control center** | Like the official client's menu: quality (Auto / 20M / 8M / 2M), 30–144 fps, true color 4:4:4, follow-remote or window resolution, fit / 1:1 window, volume, lock remote after session, mute remote, send system shortcuts (Alt+Tab), file transfer, watch mode, full screen, disconnect |
| 📶 **Network status** | Session time, UDP/TCP direct or relay, fps and codec, bitrate, RTT, packet loss, resolution; show or hide with `Ctrl+Alt+G` (remembered) |
| 🖼 **Device previews** | The device card shows the last picture of that computer, like the official app (stored locally only) |
| 🔗 **Instantly controllable** | After you sign in to the viewer, this Linux computer joins your account's device list, so all your UU apps can control it |

## Install

**Release package** (x86_64 / aarch64): download from [Releases](https://github.com/spixyy7/uu-remote-linux/releases/latest).

```sh
# Ubuntu / Debian
sudo apt install ./uu-remote-linux_<version>_amd64.deb

# Other distributions (installs into ~/.local)
tar xzf uu-remote-linux-<version>-x86_64-linux.tar.gz
cd uu-remote-linux-<version>-x86_64-linux && ./install.sh
```

Then start the viewer and sign in by scanning the QR code with the UU Remote phone app:

```sh
uuremote-viewer ui
```

After sign-in the host service (`uuremote-native.service`) starts automatically and this computer appears in
your other UU apps. To run only the host: `uuremote-server login`, then
`systemctl --user enable --now uuremote-native.service`.

FFmpeg is required (the installers check for it). Building from source, desktop support (X11, GNOME/KDE Wayland,
Hyprland/Sway), headless servers and Docker/NAS: see the [technical reference](docs/reference.en.md).

## Viewer shortcuts

| Keys | Action |
|---|---|
| `Ctrl+Alt+M` or the **Control center** tab at the top | Open / close the control center |
| `Ctrl+Alt+G` | Show / hide network status |
| `Ctrl+Alt+Enter` | Full screen |
| `Ctrl+Alt+F` | File transfer |

## Language

Order: `UU_LANG=en|zh` → **Settings → General → 语言 / Language** (viewer or host window; saved in
`~/.config/uuremote/language`, applies after a restart) → system locale (`zh*` = Chinese, otherwise English).

## Useful settings

| Variable | Effect |
|---|---|
| `UU_VIEWER_CHROMA=444` | Request true color (sharpest text) from the start |
| `UU_VIEWER_SIZE=WxH` | Decode into a fixed canvas instead of the native resolution |
| `UU_VIEWER_THUMBNAILS=0` | Don't save device previews |
| `UU_VIEWER_HOST=0` | Don't start the host service after viewer sign-in |
| `UU_VIEWER_UI_SCALE=1.5` | Scale the in-window chrome (HiDPI) |
| `UU_VIEWER_FONT=/path/font.ttf` | Font for the in-window chrome (default: fontconfig sans-serif) |

All host options are listed in the [technical reference](docs/reference.en.md#configuration).

## Not implemented yet

HDR, super screen / virtual display, privacy screen on the host, microphone (viewer audio on the host), plugins,
and the Linux login screen before anyone signs in. Details in the [technical reference](docs/reference.en.md#not-implemented--not-supported).

## Privacy

Logs contain sizes, counts and field numbers only: never keystrokes, clipboard, files, passwords or tokens.
Credentials stay in `~/.config/uuremote/config.json` (0600). Device previews stay in `~/.cache/uuremote/thumbs` (0600).

## License and credits

GPL-3.0, see [LICENSE](LICENSE). Based on [VallierDev/uu-remote-linux](https://github.com/VallierDev/uu-remote-linux).
"NetEase" and "UU Remote" are trademarks of their owners; this project ships no NetEase code or assets.
Rights holders with concerns: please contact [@spaleon_x](https://x.com/spaleon_x).
