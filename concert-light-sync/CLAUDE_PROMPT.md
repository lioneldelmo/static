Role: You are a Senior React Native Developer specializing in high-performance UI and real-time sensor/audio integration.

Task: Architect and code a mobile application called "Concert Light Sync" using React Native (JavaScript only, no TypeScript) and Expo.
UI/UX Directives:

Visual Style: Implement a Modern Dark Aesthetic. Use deep charcoal (#121212) instead of pure black to avoid "ghosting" during fast movement. Use Glassmorphism for control panels (frosted glass effect with subtle borders).
Add a very nice looking Splash screen

Intuitive Controls: >     * Bento Grid Layout: Organize modes (Strobe, Flow, Pulse) into large, tactile tiles that are easy to tap in a dark venue.

Gesture-First: Allow swiping left/right to change lighting modes and a long-press on the center screen to "lock" the light (to prevent accidental taps while holding the phone up).

Micro-Interactions: Use react-native-reanimated to create a "breathing" effect on buttons that matches the detected audio tempo.

Color Selection: Create a Haptic Color Wheel. As the user drags their finger to pick a custom color, provide subtle haptic feedback (using expo-haptics) when they pass primary color nodes.

Concert Safety: Include a "Dim UI" toggle that keeps the main light bright but makes the control buttons almost invisible to reduce glare for the user.

Core Functionality:

Audio Reactivity: Use the device microphone to detect rhythm and volume. The screen should pulse or change intensity based on the decibel level.

Lighting Modes:    
Strobe: Rapid flashing (adjustable frequency).
Flow: Smooth transitions between colors.
Pulse: Brightness peaks on bass hits.
Solid: Steady, consistent light.
Wave: Animated wave pattern
Rainbow: Cycling rainbow gradient
Customization: A color picker (using react-native-color-picker or similar) to allow users to set a "signature" concert color.

Dark Mode UI: High-contrast, large-hit-area buttons optimized for one-handed use in a dark, crowded environment.

Technical Requirements:

Framework: Expo SDK (Latest Version).

Language: Pure JavaScript (.js files). Avoid all TS syntax.

Libraries: Suggest and use the most stable, compatible libraries for audio analysis (e.g., expo-av or react-native-audio-engine equivalents compatible with Expo Go) and animations (e.g., react-native-reanimated).

Performance: Ensure the frame rate remains high during audio processing to avoid "jank."

Output: Provide a clean folder structure, the package.json with compatible versions, and the primary logic for the App.js and a Visualizer.js component. Include brief comments explaining the audio-to-visual mapping logic.