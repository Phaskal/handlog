# HandLog

A browser-based app for recording handwriting as video — word by word, played back simultaneously.

[한국어](README.ko.md)

![Platform](https://img.shields.io/badge/platform-web-lightgray) ![No build](https://img.shields.io/badge/build-none-green)

---

## What it does

HandLog lets you write each word of a sentence separately, then replays all of them at once so the full sentence appears to be written simultaneously. Recording starts the moment your first stroke touches the canvas and ends when you click **Done**. The result exports as a `.webm` video.

## Features

- **Word-by-word capture** — record each word as an independent track
- **Simultaneous playback** — all tracks replay in sync, frame-perfect
- **5 pen types** — Basic, Pressure, Brush, Calligraphy, Pencil
- **Stylus support** — reads pressure and tilt from Apple Pencil and other styluses via the Pointer Events API
- **Configurable canvas** — set background color, pen color, and stroke width
- **Background image** — drop in a custom image and the canvas resizes to match it
- **Retina rendering** — `devicePixelRatio`-scaled canvas for sharp output on high-DPI displays
- **Download as video** — exports `.webm` (VP9/VP8) via `MediaRecorder`
- **KO / EN** — language toggle in the header

## How to use

1. Open `index.html` in a browser
2. Set your background, pen color, and stroke width
3. Click **Add Word** — draw the first word on the canvas, then click **Done**
4. Repeat for each word in your sentence
5. Click **Play** to preview all words animating simultaneously
6. Click **Download** to save the recording as a `.webm` file

> Tip: the canvas starts recording on your **first stroke**, so you control the timing of each word naturally.

## Pen types

| Pen | Behavior |
|-----|----------|
| Basic | Constant-width smooth stroke |
| Pressure | Width varies with stylus pressure (mouse: fixed 0.5) |
| Brush | Soft, tapered edges with pressure sensitivity |
| Calligraphy | Flat nib — width varies with tilt angle |
| Pencil | Textured, low-opacity strokes that layer up |

## Technical notes

**Smoothing** — strokes use quadratic Bézier curves with the midpoint algorithm (`moveTo` midpoint → `quadraticCurveTo` control point → next midpoint), eliminating the jagged segments that straight-line segments produce.

**Playback** — each word stores `{type, x, y, t, color, width, pressure, tiltX, tiltY, pen}` per pointer event. During export, a `requestAnimationFrame` loop replays all tracks simultaneously while `MediaRecorder` captures the canvas stream at 30 fps.

**Canvas sizing** — logical dimensions (`logicalW` / `logicalH`) are independent of CSS size. Coordinates are stored in logical pixels and rescaled proportionally if the canvas is resized. Physical canvas dimensions are `logicalW * devicePixelRatio` for retina sharpness.

## Requirements

Any modern browser that supports `MediaRecorder` and `canvas.captureStream`. No install, no build step — just open the file.
