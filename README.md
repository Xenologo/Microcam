# Micro Motion Tracker — Android Browser/PWA

A no-Android-Studio live camera detector/tracker for tiny moving candidates: insects, pollen, dust, drifting seed material, small debris, and similar micro-motion events.

It runs fully in the browser using `navigator.mediaDevices.getUserMedia()`, `<canvas>`, frame differencing, connected components, and centroid tracking. No cloud service, no OpenCV, no app store, no native build step.

## Fastest ways to run on Android

### Option A — Host on HTTPS

Upload this folder to any HTTPS static host such as GitHub Pages, Netlify, Cloudflare Pages, or your own HTTPS server. Open the URL in Android Chrome and tap **Start camera**.

#### GitHub Pages setup

1. Push the repository to GitHub.
2. In **Settings → Pages**, set **Source** to **GitHub Actions**.
3. Push to `main` or run the **Deploy static site to Pages** workflow manually.
4. Open the published Pages URL over HTTPS and tap **Start camera**.

### Option B — Run locally on the phone with Termux

1. Install Termux from F-Droid.
2. Copy this folder into phone storage, for example `Download/micro_motion_webapp`.
3. In Termux:

```bash
pkg update
pkg install python
cd /sdcard/Download/micro_motion_webapp
python3 -m http.server 8080 --bind 127.0.0.1
```

4. Open Android Chrome to:

```text
http://127.0.0.1:8080
```

5. Press **Start camera** and grant camera permission.

## Why not just open index.html directly?

Browser camera access normally requires a secure context such as HTTPS or localhost. `file://` may fail on Android. `http://127.0.0.1` is the most reliable local no-hosting route.

## Modes

- **Strict micro**: cleaner bounding boxes, fewer false positives.
- **Broad catch-all**: catches more but marks more leaf shimmer/camera residual.
- **Dust / pollen**: tiny low-area candidates.
- **Insect-like**: slightly larger, more persistent moving candidates.

## Reading the overlay

- Green boxes: persistent tracks across enough frames.
- Yellow/orange boxes: transient candidates.
- Red wash: high global motion/shake gate active.

This is a motion-candidate detector, not a species classifier.

## Outputs

- Screenshot: saves a PNG of the annotated view.
- Record overlay: records the annotated canvas as WebM where supported.
- Export CSV: exports frame/time/track/bounding-box rows.
