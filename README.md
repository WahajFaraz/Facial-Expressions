# FACESENSE · Real-Time Facial Expression Detector

A client-side React app that performs real-time facial expression analysis using face-api.js. It supports demo/mock mode, camera-based detection, session logging, and simple analytics — all in the browser.

---

## Features

- Real-time face detection using `face-api.js` (Tiny Face Detector + Face Expression model).
- Smooth UI with `framer-motion` and Tailwind utilities.
- Demo/mock mode when camera or models are unavailable.
- Log sessions (dominant emotion + snapshot) to browser `localStorage`.
- Mood Vault with per-session mini charts and trends.

---

## Tech stack

- React (Vite)
- face-api.js
- Tailwind CSS (utility classes)
- framer-motion
- react-icons

---

## Getting started

1. Install dependencies:

```bash
npm install
```

2. Run dev server:

```bash
npm run dev
```

3. Open the app at the address Vite prints (usually `http://localhost:5173`).

---

## Important files

- App entry: [src/main.jsx](src/main.jsx)
- App router / screens: [src/App.jsx](src/App.jsx)
- Emotion engine: [src/components/EmotionEngine.jsx](src/components/EmotionEngine.jsx)
- Mock / helpers: [src/utils/mockData.js](src/utils/mockData.js)
- LocalStorage hook: [src/hooks/useLocalStorage.js](src/hooks/useLocalStorage.js)
- Global styles / custom classes: [src/index.css](src/index.css)
- Models folder (must exist in `public/models`): `public/models/`

---

## Models (face-api.js)

The app expects pre-trained model manifests and shard files in `public/models`. By default the repository includes:

- `tiny_face_detector_model-weights_manifest.json` (+ shard file(s))
- `face_expression_model-weights_manifest.json` (+ shard file(s))

`EmotionEngine.jsx` loads these with:

```js
await faceapi.nets.tinyFaceDetector.loadFromUri('/models')
await faceapi.nets.faceExpressionNet.loadFromUri('/models')
```

Ensure the `public/models` folder is served (it is under `public/`, so Vite will serve it at `/models`). If models fail to load, the app falls back to demo/mock mode.

---

## How detection works (short)

- When camera is enabled and models have loaded, the engine runs `detectAllFaces(video, new faceapi.TinyFaceDetectorOptions({ inputSize: 320 }))` and chains `.withFaceExpressions()` to obtain expression probabilities.
- Expressions are converted to percentages and displayed as gauges. The highest value is treated as the dominant emotion.
- If camera is off or models fail, `generateMockEmotions()` provides dynamic demo values.

---

## Data & privacy

- Sessions are saved locally to `localStorage` under the key `emoto-sessions` (see [src/hooks/useLocalStorage.js](src/hooks/useLocalStorage.js)).
- All detection and data processing happens in the browser — no data is sent to any server by default.

---

## Troubleshooting

- "Models not loading": check browser devtools network tab for requests to `/models/*` and confirm files exist in `public/models`.
- "Camera permission denied": allow camera access in browser settings; the app surfaces helpful error messages in the UI.
- FPS low / detection slow: increase `inputSize` or reduce detection frequency in `EmotionEngine.jsx` (constants `DETECTION_INTERVAL` and TinyFaceDetector `inputSize`).

---

## Customization tips

- Styling: tweak theme tokens and animations in [src/index.css](src/index.css).
- Palette / emotion labels: edit [src/utils/mockData.js](src/utils/mockData.js).
- Persist sessions server-side: replace `useLocalStorage` with API calls to a backend and adjust `MoodVault` accordingly.

---

## Development commands

- `npm run dev` — start development server
- `npm run build` — build production bundle
- `npm run preview` — preview built bundle
- `npm run lint` — run ESLint

---

## Authors

- Wahaj Faraz
- Hamza Islam
- Syed Muhammad Faizan Azfar

(Shown in the app `SplashScreen`.)

---

## License

This repository has no license file. Add a `LICENSE` if you wish to open-source it.
