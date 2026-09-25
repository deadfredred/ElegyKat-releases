
# ElegyKat

**[⬇ Download the latest release](https://github.com/deadfredred/ElegyKat-releases/releases/latest)** · [Release notes](https://github.com/deadfredred/ElegyKat-releases/releases)

Linux-first Subsonic/Navidrome player for your desktop

<img width="2518" height="1368" alt="screenshot-2026-09-23_14-50-00" src="https://github.com/user-attachments/assets/155f37f4-fbb2-48fe-a9ce-a01b9b031eeb" />

## Features

**Library**
- Full-library browsing: Albums, Artists, Songs, Playlists — paginated so any library size loads smoothly (long lists render progressively and keep filling as you scroll)
- Home screen with recently played and curated songs
- Library-wide search in every view (150ms-debounced for typing comfort)
- Real album artwork everywhere (album art, artist art, playlists) with letter-tile fallbacks — artwork requests are cached so scrolling a big library no longer re-downloads covers
- Real play counts displayed per song
- **Album grid with inline expansion** — click an album, its tracklist opens right below it in the grid: header with artwork, year, song count, Play and Shuffle, and a playable tracklist with track numbers
- Favorites and Recently Played views (loaded on demand)
- Working top-bar Back / Forward buttons with real navigation history

**Playback**
- Play, pause, resume, next, previous, seek, volume, mute
- Shuffle, Repeat (track), and a persistent queue
- Album and playlist playback queue the whole set
- Smooth gapless track transitions with next-track preloading
- **Non-scratching seekbar** — scrubbing previews; the seek applies once on release, so dragging never chops the audio
- Render pipeline built for large libraries: the playback clock updates only the seekbar, so the whole app stops repainting 4x per second during playback
- **Marquee song title** — long titles scroll in the player bar instead of being clipped, and never overlap the controls
- Discord "Listening to" throttled playback notifications (optional)
- Desktop notification when the next song auto-starts (toggle in Settings)
- Keyboard shortcuts: Space play/pause, ←/→ seek, Shift+←/→ skip, ↑/↓ volume, `/` or Ctrl+K search, Esc closes overlays

**Desktop integration (Linux)**
- **Custom themed title bar** — follows the active theme, with Minimize / Maximize-Restore / Close (hides to tray)
- **System tray icon** with right-click menu: media controls, Show / Hide window, Maximize / Restore, Quit; left-click toggles show/hide
- **MPRIS media keys** — keyboard play/pause/next/previous, desktop media widgets, and `playerctl`-style tools all control ElegyKat, even while hidden
- Taskbar/launcher identity: shows under your app launcher with the ElegyKat logo
- Discord Rich Presence built in — no application registration needed (optional custom application ID supported; first line, second line, timer, and artwork all configurable)
- Album tracklist numbers use real metadata track numbers
- Now-playing reports to Navidrome (scrobbles) with play count sync
- **Omarchy theme sync** — an "Omarchy (system)" theme that mirrors your system theme color by color and follows it live

**Settings & server**
- Sidebar-only Settings entry (no top-bar gear); the window is drag-movable by its header and resizable from the corner grip (size remembered)
- **Server info card** from the profile chip in the sidebar — read-only view of the active server (profile, URL, username)
- "Add Navidrome server" dialog for connecting additional or replacement servers
- **Display options** — Text & UI size (85%–150%) for large/high-resolution monitors, a Wide layout for ultrawide screens (removes the 1400px content cap), and **Tiling window manager mode** — compact always-on layout for half/quarter tiles: icon-only sidebar, tighter player bar, and grids/hero that reflow to any width (narrow windows adapt automatically too)

**Performance & startup**
- Fonts are bundled locally — no Google Fonts network request, faster and more private startup
- Cover-art URLs are cached per image, so webview image caching actually works across re-renders
- Empty-start render trimmed: pass on rendering, immediate first paint

**Visualizer**
- Full-screen animated visualizer with three modes: **Mirror**, **Wave**, and **Donut** — a spinning, beat-pulsing ASCII torus in classic terminal style
- Keyboard-friendly mode switcher at the bottom of the visualizer; Esc exits
- Toggle from the player bar, mode and on/off state remembered between launches


https://github.com/user-attachments/assets/f27b6b13-f315-4411-a765-8f14321ffc77


**Themes**
- Midnight (default), Light, Violet, **Tokyo Night**, **Catppuccin**, and Omarchy-to-system sync — full-app theming including the title bar, player, and visualizer colors



https://github.com/user-attachments/assets/28df4e33-972c-4aa5-8c96-912af957e3c5


**Security & data**
- Passwords stored in the OS keyring (freedesktop Secret Service — GNOME Keyring/KWallet); automatic migration of older profiles; graceful plaintext fallback if no Secret Service exists
- Library metadata cached locally; settings and profiles kept outside the app folder

## Requirements

- A Navidrome (or any Subsonic/OpenSubsonic) server reachable locally or over the internet
- The **Discord desktop app** if you want Rich Presence
- Linux with GTK3, WebKitGTK 4.1, GStreamer plugins, and a tray-capable shell for menus

## Install

Works on Arch, Manjaro, EndeavourOS, Hyprland/Omarchy-style setups, **and** Mint/Ubuntu/Debian-family desktops (Cinnamon, Xfce, …).

### 1. System dependencies

**Arch-family:**

```bash
sudo pacman -S --needed \
  webkit2gtk-4.1 gtk3 pango cairo gdk-pixbuf2 \
  gst-plugins-good gst-plugins-base gst-libav \
  libayatana-appindicator gnome-keyring
```

- `webkit2gtk-4.1` — the web view the app is built on
- `gst-libav`/`gst-plugins-good` — audio decoding
- `libayatana-appindicator` — tray icon
- `gnome-keyring` — secure password storage (or any Secret Service implementation)

**Mint 22 / Ubuntu 24.04+ / Debian-family:**

```bash
sudo apt install -y webkit2gtk-4.1 gstreamer1.0-plugins-good gstreamer1.0-plugins-base gstreamer1.0-libav
```

- Preinstalled on Mint 22.3 — this section matters on trimmed installs only.

### 2. Download & install

Grab the AppImage **or** the `.deb` from the [latest release](https://github.com/deadfredred/ElegyKat-releases/releases/latest):

**AppImage** (`ElegyKat_<version>_amd64.AppImage`) — portable, for Arch/Manjaro/EndeavourOS/Hyprland-style setups:

```bash
mkdir -p ~/Applications
mv ~/Downloads/ElegyKat_*.AppImage ~/Applications/elegykat.AppImage
chmod +x ~/Applications/elegykat.AppImage
~/Applications/elegykat.AppImage
```

**.deb** (`ElegyKat_<version>_amd64.deb`) — for Mint/Ubuntu/Debian-family (Cinnamon and friends):

```bash
sudo dpkg -i ~/Downloads/ElegyKat_*.deb   # or double-click it in the file manager
```

- The AppImage updates itself: launching it checks GitHub Releases and offers a one-click update signed against the app's key — no manual re-download needed.
- **Note:** builds older than v0.1.10 (September 2026) were signed with a now-retired key and no longer offer automatic updates. Those installs need one manual download of the latest release; after that, self-updating works normally. Related releases carry a notice on their release page.
- To add the AppImage to your launcher menu, use any launcher that builds entries from AppImages (e.g. [AppImageLauncher](https://github.com/TheAssassin/AppImageLauncher)) or create a `.desktop` file pointing at `~/Applications/elegykat.AppImage`.

### 3. Connect your server

Click the **profile chip** at the top of the sidebar and choose **Add server** — enter your Navidrome URL, username, and password. Everything is stored in your keyring and app profile — not in the repo. On every future start, ElegyKat reconnects automatically.

<img width="655" height="568" alt="screenshot-2026-09-23_14-51-18" src="https://github.com/user-attachments/assets/ba0c7f90-16a7-42c4-b3a6-3c8299cfaf98" />


### Update / Uninstall (AppImage)

```bash
# update: the app offers a one-click update on launch, or:
mv ~/Downloads/ElegyKat_*.AppImage ~/Applications/elegykat.AppImage

# remove the app data except for your server credentials, to start from scratch:
rm -rf ~/.local/share/com.elegykat.player

# uninstall entirely: delete the AppImage, plus the data folder above and the
# "ElegyKat" keyring entry (seahorse or `secret-tool clear`).
```

### Update / Uninstall (.deb)

```bash
sudo dpkg -i ~/Downloads/ElegyKat_*.deb          # update in place
sudo apt remove --purge elegykat && sudo apt autoremove   # uninstall
rm -rf ~/.local/share/com.elegykat.player                # remove app data
```

## Roadmap

- Offline downloads
- Playlist editing
- Fedora/RPM packaging
- Debian-family AppImage — webkit-inside-AppImage locks up on some Mint/Cinnamon hosts; use the `.deb` there (done) and split a debian-base AppImage if demand appears
- Prebuilt release pipeline (deb/rpm/AppImage + CI)

## Support ElegyKat

ElegyKat is free and ad-free by design — features will never be gated behind donations. If it saves you clicks and clicks of library-scrolling, buy me a coffee:

<p align="center">
  <a href="https://ko-fi.com/deadfredred"><img alt="Support ElegyKat on Ko-fi" height="36" src="https://cdn.ko-fi.com/cdn/kofi3.png?v=6"></a> ·
  <a href="https://ko-fi.com/deadfredred">ko-fi.com/deadfredred</a>
</p>

Donations go toward release infrastructure (build and signing machines, test setups) and keep the project independent. Bugs and feature requests are always welcome for free on [GitHub](https://github.com/deadfredred/ElegyKat-releases/issues).
