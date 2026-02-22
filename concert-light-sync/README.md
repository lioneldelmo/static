# Concert Light Sync

![Version](https://img.shields.io/badge/version-2.0.0-FF6B00?style=flat-square)
![Platform](https://img.shields.io/badge/platform-iOS%20%7C%20Android-blue?style=flat-square)
![Expo](https://img.shields.io/badge/Expo-SDK%2054-4C97FB?style=flat-square)
![React Native](https://img.shields.io/badge/React%20Native-0.81-61DAFB?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)

> **Turn your smartphone into a real-time concert light controller.**

Concert Light Sync transforms your phone's screen into an audio-reactive concert light with six stunning visual modes, gesture-based controls designed for dark venues, and 60fps animations powered by React Native Reanimated.

---

## Overview

Concert Light Sync was built for one purpose: making live music more immersive. Whether you're at a stadium show, a club night, or a festival, the app listens to the music around you and responds with synchronized light effects you control with a swipe.

**Who is this for?**
- Concert attendees who want to be part of the visual experience
- Festival-goers creating crowd light shows
- Anyone who wants a beautiful, responsive flashlight for live events

---

## Features

### Lighting Modes
| Mode | Description | Audio Reactive |
|------|-------------|:--------------:|
| ⚡ Strobe | Rapid on/off flash, safety-capped at 6Hz | — |
| 🌊 Flow | Smooth hue rotation through the full spectrum | — |
| 💓 Pulse | Breathing baseline that explodes on bass hits | ✓ |
| ● Solid | Steady, consistent color — no animation | — |
| 〰 Wave | Sweeping beam from left to right on a 2s loop | — |
| 🌈 Rainbow | Six concentric arcs cycling through all colors | — |

### Controls
- **Swipe left/right** — cycle through modes instantly
- **Long-press (0.6s)** — toggle screen lock to prevent accidental taps
- **Color swatch** — opens a full HSV color picker with 7 presets
- **Brightness slider** — independent brightness control
- **Dim UI** — fades the control panel to 7% opacity, keeping the light on without glare
- **Lock mode** — minimal overlay shows only the lock icon

### Audio Reactivity
- Live microphone input via `expo-audio`
- 20Hz polling rate balancing responsiveness and battery
- Exponential moving average smoothing (α=0.3) prevents jitter
- Graceful fallback to ambient level 0.25 if mic permission is denied

### UX & Safety
- Glassmorphism dark UI (#121212 base) designed for dark venues
- WCAG-compliant text contrast for readability under stage lights
- Photosensitivity warning modal before Strobe mode activates
- Haptic feedback on mode changes, color selection, and brightness notches
- Portrait-locked for one-handed use in crowds

---

## Tech Stack

| Category | Library | Version |
|----------|---------|---------|
| Framework | React Native | 0.81.5 |
| Runtime | Expo | ~54.0.0 |
| UI | React | 19.1.0 |
| Animations | react-native-reanimated | ~4.1.1 |
| Gestures | react-native-gesture-handler | ~2.28.0 |
| Audio | expo-audio | ~1.1.1 |
| Haptics | expo-haptics | ~15.0.8 |
| Gradients | expo-linear-gradient | ~15.0.8 |
| Worklets | react-native-worklets | ^0.5.1 |

---

## Project Structure

```
concert-light-sync/
├── App.js                   # Root component — state, gestures, lifecycle
├── index.js                 # Expo entry point
├── package.json
├── app.json                 # Expo config (permissions, icons, orientation)
├── babel.config.js          # Babel preset for Expo + Reanimated plugin
├── metro.config.js          # Metro bundler config
├── index.html               # Static marketing landing page
├── CLAUDE_PROMPT.md         # Original design specification
└── components/
    ├── LightCanvas.js       # Reanimated rendering engine (all 6 modes)
    ├── ControlPanel.js      # Bento grid UI, mode selection, color/brightness
    ├── ColorWheel.js        # HSV color picker with presets and haptics
    └── AudioReactivity.js   # Microphone polling and signal smoothing
```

### Component Responsibilities

**`App.js`** — Orchestrates the app. Holds top-level state (mode, color, brightness, lock, dim). Creates the composed pan + long-press gesture for swiping and locking. Manages splash screen timing and Reanimated runtime initialization.

**`LightCanvas.js`** — The visual engine. Each mode has dedicated Reanimated `withRepeat`/`withTiming`/`withSequence` animation sequences running on the native thread. Polls `audioLevelRef` at 20Hz for Pulse mode reactivity.

**`ControlPanel.js`** — The 3×2 bento grid of mode tiles, brightness slider, color swatch, and top-bar toggles (Dim, Lock). Uses React Native `Animated` for the Dim UI fade since it doesn't need native-thread precision.

**`ColorWheel.js`** — HSV color picker. Two `PanResponder` handlers drive the hue and brightness sliders. Fires haptic feedback at primary hue nodes (0°, 60°, 120°, 180°, 240°, 300°). Converts HSV → Hex for the rest of the app.

**`AudioReactivity.js`** — Invisible component (returns `null`). Requests mic permission, starts `expo-audio` recording with metering enabled, and polls every 50ms. Writes the smoothed audio level to `audioLevelRef` — a ref, not state, so it never triggers re-renders.

---

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org) 18+
- [Expo CLI](https://docs.expo.dev/get-started/installation/) or `npx expo`
- iOS Simulator / Android Emulator **or** the [Expo Go](https://expo.dev/client) app on a physical device

### Installation

```bash
git clone https://github.com/your-username/concert-light-sync.git
cd concert-light-sync
npm install
```

### Run

```bash
# Start the Expo dev server
npx expo start

# Run directly on iOS simulator
npx expo run:ios

# Run directly on Android emulator
npx expo run:android
```

Scan the QR code with **Expo Go** (iOS/Android) to run on a physical device.

### Permissions

The app requests **microphone access** on first launch. Audio reactivity requires this permission. If denied, Pulse mode falls back to a constant ambient level of 0.25.

---

## Usage

| Action | Result |
|--------|--------|
| Swipe left / right | Previous / next lighting mode |
| Long-press (0.6s) | Toggle screen lock |
| Tap color swatch | Open HSV color picker |
| Drag brightness bar | Adjust brightness 0–100% |
| Tap **Dim** button | Fade control panel to 7% opacity |
| Tap **Lock** button | Toggle screen lock overlay |

### Strobe Mode
Selecting Strobe shows a photosensitivity warning. You must confirm before the mode activates. Frequency is hard-capped at 6Hz for safety.

---

## Configuration

Key settings in `app.json`:

```json
{
  "expo": {
    "name": "Concert Light Sync",
    "slug": "concert-light-sync",
    "version": "2.0.0",
    "orientation": "portrait",
    "newArchEnabled": true,
    "ios": {
      "infoPlist": {
        "NSMicrophoneUsageDescription": "Used to react to music and ambient sound."
      },
      "supportsTablet": false
    },
    "android": {
      "permissions": ["android.permission.RECORD_AUDIO"]
    }
  }
}
```

### Babel
The Reanimated Babel plugin **must** be listed last in the `plugins` array in `babel.config.js`:

```js
module.exports = {
  presets: ['babel-preset-expo'],
  plugins: [
    'react-native-worklets/plugin',
    'react-native-reanimated/plugin', // must be last
  ],
};
```

---

## Architecture Notes

### Why Reanimated?
React Native's default `Animated` API runs on the JS thread. At 60fps, any JS work (state updates, event handlers) can cause dropped frames. Reanimated runs animation worklets on the native thread — the screen flicker is guaranteed smooth even on lower-end devices.

### Why a ref for audio level?
Audio polls at 20Hz. If the level were stored in React state, every update would trigger a full re-render of the component tree. Using a ref (`audioLevelRef`) means `LightCanvas` can read the latest value inside a Reanimated worklet with zero render overhead.

### Gesture composition
The root gesture is a `Composed` gesture wrapping a `LongPress` and a `Pan`. This lets both gestures coexist: a slow pan triggers swipe mode cycling, while a long stationary press triggers screen lock — without conflict.

---

## Contributing

Contributions are welcome. Please:

1. Fork the repo and create a feature branch (`git checkout -b feature/my-feature`)
2. Keep changes focused — one feature or fix per PR
3. Test on both iOS and Android if possible
4. Open a pull request with a clear description

### Ideas for Contribution
- Persistent settings via `AsyncStorage` (last mode, color, brightness)
- FFT-based spectrum analysis for frequency-specific reactivity
- Preset scene saving (party, concert, festival)
- BPM detection and tempo-locked strobing
- Accessibility improvements (reduced motion support)

---

## License

MIT License — see [LICENSE](LICENSE) for details.

---

*Concert Light Sync — Built for concert-goers, by concert-goers.*
