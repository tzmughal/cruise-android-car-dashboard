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

- **Media remote** — album art, track details, progress, and transport controls pulled from
  the live media session.
- **Car mode** — tap the album panel for a big-cover layout; tap art to play/pause, swipe
  left/right to skip, swipe up for the seek bar.
- **Speedometer** — GPS speed in km/h, trip meter, and clear states for permission needed,
  location off, searching, weak accuracy, and live fix. Rejects fixes worse than 20 m.
- **Ambient background** — colours sampled from the current album art.
- **Landscape, hands-free** — full screen, keep-awake, dim overlay, haptics, auto-rotate.
- **SOS and Map shortcuts** on the dashboard.

## How it works

- A `NotificationListenerService` receives media-session callbacks and discovers players.
- `MediaController` instances carry transport actions and the reported playback position,
  which Cruise projects forward between updates.
- When several sessions are playing, the most recently updated one wins.
- UI is pure Jetpack Compose, with custom drawing for the gauge and animated background.

## Tech

Kotlin · Jetpack Compose · Material 3 · MediaSession / MediaController ·
NotificationListenerService · Fused location · Gradle · minSdk 26, targetSdk 35

## Install

1. Download `Cruise.apk` from the **Releases** page.
2. Install it (you'll need to allow installs from unknown sources).
3. Open Cruise → **Open notification access** → enable Cruise.
4. Start playback in any media app and return to Cruise.
5. Optional: go to Settings → **Cruise mode** and allow location for the speedometer.

This is a debug-signed build for personal use, not a Play Store release.

## Notes

Speed comes from GPS and depends on hardware, reception, and fix quality. It is not
calibrated vehicle speed and should not be used as a vehicle instrument.
