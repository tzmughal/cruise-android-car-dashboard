# Cruise

A landscape Android app that turns an old phone into a car dashboard: a media remote for
whatever is playing on the device, plus a GPS speedometer mode.

| Media remote | Car mode |
| --- | --- |
| ![Home screen](screenshots/home.png) | ![Car mode](screenshots/car-mode.png) |
| **Speedometer** | **Settings** |
| ![Speedometer](screenshots/speedometer.png) | ![Settings](screenshots/settings.png) |

Cruise does not play or stream audio. It finds the active media session using Android's
notification listener, then sends transport commands (previous / play-pause / next / seek)
to that app — Spotify, YouTube Music, a local player, anything that publishes a session.

## Features

### Media remote

- Live album art, track title, artist, and playback progress from the active session.
- Previous, play/pause, and next controls, plus seeking when the player supports it.
- Position is projected from the session's reported position and playback speed, so the
  progress bar keeps moving between updates.
- A five-second refresh recovers from players that don't push session updates reliably.
- A playing session is preferred. If several are playing, the most recently updated one
  wins; when playback pauses, the current session is kept while it stays active.
- **Media player** launches the app that owns the active session, when it exposes one.

### Car mode

- Tap the album panel to open a big-cover layout with title and artist.
- Background colours are sampled from the current album art, animating as tracks change.
- Optional artist line (`Artist in car mode`), close button returns to the remote.

### GPS speedometer

- Speed in km/h drawn on a custom gauge, with a trip meter and a live signal state.
- Long-press the trip meter to reset it.
- Explicit states for permission required, location services off, searching, weak accuracy,
  unavailable, and locked: **"GPS LOCKED · LIVE"**.
- Fixes without accuracy data, or worse than 20 m, are never used for speed.
- **SOS** opens the dialer (it never places the call). **Map** opens a map app at the last fix.
- Clock, date, and session time on the dashboard.
- Track controls keep working without GPS; only the speed reading needs location.

### Gestures

| Screen | Gesture | Action |
| --- | --- | --- |
| Home | Swipe left | Open Settings |
| Settings | Swipe right | Back to Home |
| Home | Tap album panel | Enter car mode |
| Car mode | Tap artwork | Play / pause |
| Car mode | Swipe left / right | Next / previous track |
| Car mode | Swipe up from the lower area | Reveal the seek bar |
| Car mode | Swipe down | Hide the seek bar |
| Car mode | Drag the seek bar | Seek in the current track |
| Speedometer | Tap the gauge | Play / pause |
| Speedometer | Swipe left / right | Next / previous track |
| Speedometer | Long-press the trip meter | Reset trip distance |
| Settings | Long-press and drag a tile | Reorder tiles |

Reordering is also available to screen readers through each tile's *move earlier* and
*move later* actions.

### Settings

| Tile | Behaviour |
| --- | --- |
| Cruise mode | Opens the GPS speedometer dashboard |
| Ambient lights | Enables album-art colours in the animated background |
| Full screen | Hides system bars; an edge swipe reveals them |
| Keep display awake | Stops the screen from timing out while Cruise is open |
| Artist in car mode | Shows or hides the artist line in car mode |
| Touch haptics | Haptic feedback on supported controls |
| Auto rotate | Toggles Android's system auto-rotate setting |
| Media player | Opens the app behind the active media session |
| Dim display | Dims Cruise's window without changing device brightness |

## How it works

- A `NotificationListenerService` receives media-session callbacks and discovers players.
- `MediaController` instances carry transport actions and the reported playback position,
  which Cruise projects forward between updates.
- When several sessions are playing, the most recently updated one wins.
- UI is pure Jetpack Compose, with custom drawing for the gauge and animated background.

## Tech

Kotlin · Jetpack Compose · Material 3 · MediaSession / MediaController ·
NotificationListenerService · Fused location · Palette · Gradle · minSdk 26, targetSdk 35

## Install

1. Download `Cruise.apk` from the **Releases** page.
2. Install it (you'll need to allow installs from unknown sources).
3. Open Cruise → **Open notification access** → enable Cruise.
4. Start playback in any media app and return to Cruise.
5. Optional: go to Settings → **Cruise mode** and allow location for the speedometer.

This is a debug-signed build for personal use, not a Play Store release.

## Permissions

- **Notification access** — required; used to discover and control media sessions.
- **Location (fine or coarse)** — optional, requested only when the speedometer is opened.
  Cruise does not ask for background location.
- **Write system settings** — optional, only for the Auto rotate tile.
- **Vibrate** — optional, only for haptic feedback.

## Notes

Speed comes from GPS and depends on hardware, reception, and fix quality. It is not
calibrated vehicle speed and should not be used as a vehicle instrument.
