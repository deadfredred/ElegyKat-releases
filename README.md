# GhoztKat

**[⬇ Download the latest release](https://github.com/deadfredred/GhoztKat-releases/releases/latest)** · [Release notes](https://github.com/deadfredred/GhoztKat-releases/releases)

Linux-first Subsonic/Navidrome player for your desktop



## Features

**Library**
- Full-library browsing: Albums, Artists, Songs, Playlists — paginated so any library size loads
- Home screen with recently played and curated songs
- Library-wide search in every view
- Real album artwork everywhere (album art, artist art, playlists) with letter-tile fallbacks
- Real play counts displayed per song
- **Album grid with inline expansion** — click an album, its tracklist opens right below it in the grid: header with artwork, year, song count, Play and Shuffle, and a playable tracklist with track numbers
- Favorites and Recently Played views (loaded on demand)

**Playback**
- Play, pause, resume, next, previous, seek, volume, mute
- Shuffle, Repeat (track), and a persistent queue
- Album and playlist playback queue the whole set
- **Marquee song title** — long titles scroll in the player bar instead of being clipped, and never overlap the controls
- Discord "Listening to" throttled playback notifications (optional)
- Desktop notification when the next song auto-starts (toggle in Settings)

**Desktop integration (Linux)**
- **Custom themed title bar** — follows the active theme, with Minimize / Maximize-Restore / Close (hides to tray)
- **System tray icon** with right-click menu: media controls, Show / Hide window, Maximize / Restore, Quit; left-click toggles show/hide
- **MPRIS media keys** — keyboard play/pause/next/previous, desktop media widgets, and `playerctl`-style tools all control GhoztKat, even while hidden
- Taskbar/launcher identity: shows under your app launcher with the GhoztKat logo
- Discord Rich Presence built in — no application registration needed (optional custom application ID supported; first line, second line, timer, and artwork all configurable)
- Album tracklist numbers use real metadata track numbers
- Now-playing reports to Navidrome (scrobbles) with play count sync
- **Omarchy theme sync** — an "Omarchy (system)" theme that mirrors your system theme color by color and follows it live

**Settings & server**
- Sidebar-only Settings entry (no top-bar gear); the window is drag-movable by its header and resizable from the corner grip (size remembered)
- **Server info card** from the profile chip in the sidebar — read-only view of the active server (profile, URL, username)
- "Add Navidrome server" dialog for connecting additional or replacement servers
- **Display options** — Text & UI size (85%–150%) for large/high-resolution monitors and a Wide layout for ultrawide screens (removes the 1400px content cap)

**Visualizer**
- Full-screen animated visualizer with three modes: **Mirror**, **Wave**, and **Donut** — a spinning, beat-pulsing ASCII torus in classic terminal style
- Keyboard-friendly mode switcher at the bottom of the visualizer; Esc exits
- Toggle from the player bar, mode and on/off state remembered between launches

**Themes**
- Midnight (default), Light, Violet, **Tokyo Night**, **Catppuccin**, and Omarchy-to-system sync — full-app theming including the title bar, player, and visualizer colors

**Security & data**
- Passwords stored in the OS keyring (freedesktop Secret Service — GNOME Keyring/KWallet); automatic migration of older profiles; graceful plaintext fallback if no Secret Service exists
- Library metadata cached locally; settings and profiles kept outside the app folder

## Requirements

- A Navidrome (or any Subsonic/OpenSubsonic) server reachable locally or over the internet
- The **Discord desktop app** if you want Rich Presence
- Linux with GTK3, WebKitGTK 4.1, GStreamer plugins, and a tray-capable shell for menus

## Install on Arch Linux

Works on Arch, Manjaro, EndeavourOS, and Hyprland/Omarchy-style setups.

### 1. System dependencies

```bash
sudo pacman -S --needed base-devel git npm nodejs \
  webkit2gtk-4.1 gtk3 pango cairo gdk-pixbuf2 \
  gst-plugins-good gst-plugins-base gst-libav \
  libayatana-appindicator gnome-keyring
```

- `gst-libav`/`gst-plugins-good` — audio decoding
- `libayatana-appindicator` — tray icon
- `gnome-keyring` — secure password storage (or any Secret Service implementation)

### 2. Build

```bash
git clone https://github.com/deadfredred/GhoztKat.git
cd GhoztKat
npm install
npm run build && cargo build --release --features custom-protocol --manifest-path src-tauri/Cargo.toml
```

(First build takes a couple of minutes.)

### 3. Install (user-local, no root)

```bash
./install.sh
```

Installs the binary to `~/.local/bin/ghoztkat`, a launcher menu entry ("GhoztKat"), and icons. Launch from your application menu, or run `ghoztkat`.

### 4. Connect your server

Click the **server card** in the top of the sidebar (or, if none is connected, the profile chip) and choose **Add server** — enter your Navidrome URL, username, and password. Everything is stored in your keyring and app profile — not in the repo. On every future start, GhoztKat reconnects automatically.

### Update / Uninstall

```bash
git pull && npm install
npm run build && cargo build --release --features custom-protocol --manifest-path src-tauri/Cargo.toml
./install.sh            # refresh the installed copy
./uninstall.sh          # remove binary, menu entry and icons
```

To wipe personal settings too, delete `~/.local/share/com.ghoztkat.player/` and the "GhoztKat" entry from your keyring (`seahorse` or `secret-tool clear`).

## Building & running from source (any distro)

```bash
npm install
npm run build
cargo build --release --features custom-protocol --manifest-path src-tauri/Cargo.toml
./start.sh       # runs the release build directly
```
or in dev mode: `npm run tauri dev`.

## Publishing updates (AppImage + auto-updater)

Installed apps check GitHub Releases on launch and offer a one-click update.
Builds are signed with the keypair at `~/.tauri/ghoztkat.key` — **keep it safe; losing it breaks the update chain.**

Requirements: `gh` CLI (logged in), `patchelf` (in `~/.local/bin` on Arch without the system package).

```bash
./scripts/release.sh 0.1.1 "What changed"   # bump version everywhere, build, sign, publish
./scripts/release.sh                        # re-release the current version
```

The script creates a release in the public [GhoztKat-releases](https://github.com/deadfredred/GhoztKat-releases) repo
containing the signed AppImage and `latest.json`; running installs pick it up automatically on next launch.
Source stays private here. Bundles also land locally in
`src-tauri/target/release/bundle/` (`deb/`, `rpm/`, `appimage/`).

Note: AppImages are best built on an older base (e.g. Debian 12 / Ubuntu 22.04) so the
bundled glibc doesn't exclude older distros.

## Project layout

```
src/                    React frontend (UI, playback, visualizer, Subsonic API)
src-tauri/              Rust shell: MPRIS media keys, tray menu, Discord IPC, keyring
scripts/release.sh      One-command signed release to GitHub Releases
public/                 Assets bundled with the app
start.sh                Quick launcher for the compiled build
install.sh/uninstall.sh Native user-local install helpers
```

## Roadmap

- Offline downloads
- Playlist editing
- Prebuilt release pipeline (deb/rpm/AppImage + CI)

## License

Personal project — review the code before deploying publicly.
