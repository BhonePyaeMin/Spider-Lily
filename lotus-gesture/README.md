# Lotus Bloom Gesture

A 3D lotus on a glowing pond that you control with your hands. Built with
Three.js (rendering + bloom glow) and MediaPipe HandLandmarker (hand tracking).
Single file, no build step, nothing to install.

## Run

```bash
cd lotus-gesture
python -m http.server 8642
```

Open http://localhost:8642 in Chrome and allow the camera.
(Camera access needs `http://localhost` or `https`, not a `file://` path.)

## Controls

| Input | Effect |
|-------|--------|
| Right hand open / fist | bloom / close the lotus |
| Left hand move left-right | spin the flower |
| Left hand move up-down | change petal colour (pink to purple to blue) |
| Only one hand visible | it controls the bloom |
| `D` | auto demo (no camera needed) |
| `H` | hide the panel |
| Slider | manual bloom |
| Mouse drag / wheel | orbit / zoom |

## How it works

1. `HandLandmarker` returns 21 landmarks per hand every video frame.
2. Openness = average fingertip-to-wrist distance divided by palm size (wrist to
   middle knuckle), remapped to 0..1 (`OPEN_MIN` / `OPEN_MAX` at the top of the file).
3. The two hands are told apart by position in the camera image (smaller x = your right hand).
4. Every petal is a procedural curved ribbon rebuilt each frame: its angle from vertical
   and tip curl are interpolated between a closed bud pose and an open pose.
   Outer rings open first, inner rings later, then stamens extend.
5. Smoothed bloom value also drives light intensity, bloom post-processing,
   fireflies and water ripples.

## Tweaking

- `RINGS` — petal count, length and open/closed angles per ring.
- `OPEN_MIN` / `OPEN_MAX` — calibrate to your hand and camera distance.
- `SMOOTH` — how fast the flower follows your hand.
