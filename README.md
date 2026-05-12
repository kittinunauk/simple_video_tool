# Video Frame Trimmer

A single-page, browser-based tool for frame-accurate video trimming and frame capture. No install, no upload — everything runs locally in your browser using the WebCodecs API.

## Usage

Open [index.html](index.html) in Chrome or Edge (WebCodecs is required).

1. Drop a video onto the page (or click to choose a file).
2. Scrub to the in-point and press **I** (or click **Mark Start**).
3. Scrub to the out-point and press **O** (or click **Mark End**).
4. Press **E** (or click **Export MP4 + JSON**) to download the trimmed clip plus a sidecar JSON with timing metadata.

Press **C** at any time to capture the current frame as a PNG.

## Keyboard shortcuts

| Key | Action |
| --- | --- |
| `Space` | Play / pause |
| `←` / `→` | Skip 5 s |
| `Shift` + `←/→` | Skip 1 s |
| `Alt` + `←/→` | Skip 10 s |
| `,` / `.` | Step 1 frame |
| `I` / `O` | Mark in / out |
| `C` | Capture current frame |
| `E` | Export trimmed MP4 + JSON |

## Requirements

- A Chromium-based browser (Chrome, Edge, Arc, Brave) with WebCodecs `VideoEncoder`/`VideoDecoder` support.
- Source video must be an MP4 with H.264 (AVC) or H.265 (HEVC) video. Output is always H.264.

## How it works

- **FPS detection** uses `requestVideoFrameCallback` to sample real frame intervals.
- **Trimming** demuxes the source MP4 with [mp4box.js](https://github.com/gpac/mp4box.js), decodes the relevant GOP via `VideoDecoder`, re-encodes the in-range frames with `VideoEncoder`, and remuxes with [mp4-muxer](https://github.com/Vanilagy/mp4-muxer).
- Everything stays on your machine — no network upload.
