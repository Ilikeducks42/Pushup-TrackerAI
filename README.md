## Pushup Tracker

A browser-based pushup counter powered by [Teachable Machine](https://teachablemachine.withgoogle.com/) pose detection. Train your own model, paste the URL, and let the app count your reps in real time.

---

## Requirements

- A modern browser (Chrome recommended)
- A webcam
- A [Teachable Machine](https://teachablemachine.withgoogle.com/train/pose) pose model with at least two classes (e.g. `up` and `down`)
- A local web server — e.g. the [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) extension for VS Code

> **Important:** The file must be served over `http://localhost` or `https://`. Opening it directly as `file:///` will block the scripts from loading.

---

## Setup

1. Download `pushup-tracker.html` and open the folder in VS Code
2. Click **Go Live** in the bottom right corner of VS Code
3. The app opens at `http://127.0.0.1:5500/pushup-tracker.html`

---

## Training Your Model

1. Go to [teachablemachine.withgoogle.com/train/pose](https://teachablemachine.withgoogle.com/train/pose)
2. Create two classes:
   - **up** — arms fully extended (top of a pushup)
   - **down** — chest close to the floor (bottom of a pushup)
3. Record ~50–100 samples per class
4. Click **Train Model**
5. Click **Export Model** → choose **TensorFlow.js** → **Upload (shareable link)**
6. Copy the URL (looks like `https://teachablemachine.withgoogle.com/models/xxxxxxxx/`)
You can also use this Pre-Made model: https://teachablemachine.withgoogle.com/models/jJi-ZhvH0/

---

## Usage

1. Paste your model URL into the input field
2. Click **Start**
3. The status panel confirms each step:
   - TensorFlow.js loaded
   - Teachable Machine connected
   - Camera active
   - Pose detection running
   - Confidence signal received
4. Use the **Up Class** / **Down Class** dropdowns to map your class names to the correct positions
5. Do pushups — the counter increments on every `down → up` transition
6. Click **Reset** to start a new session

You could also use This to track any other Rep-Based exercise with clear ''up'' and ''down positions.

---

## How Counting Works

A rep is counted when the model detects a **down → up** transition with a confidence above **72%**. This prevents false positives from partial movements.

```
down (confidence > 72%)  →  up (confidence > 72%)  =  +1 rep
```

---

## Dependency Versions

The app uses exact versions that are compatible with Teachable Machine:

| Package | Version |
|---|---|
| `@tensorflow/tfjs` | `1.3.1` |
| `@teachablemachine/pose` | `0.8` |

> Using newer versions of TensorFlow.js (2.x, 3.x, 4.x) breaks the app with a `Pt.fromPixels is not a function` error. Do not change these versions.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| Scripts not loading | Serve via Live Server, not `file:///` |
| `Pt.fromPixels is not a function` | Wrong TF version — use exactly `tfjs@1.3.1` |
| Model fails to load | Check that the URL ends with `/` and the model is set to public |
| Camera shows black | Allow camera access in browser permissions |
| Reps not counting | Adjust your training data or lower the confidence threshold in the code (`CONFIDENCE = 0.72`) |
