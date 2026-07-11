# Selina Martin — Official Site

Single-page artist website for singer/songwriter/guitarist Selina Martin. Built as one self-contained `index.html` — no build step, no framework, no server required.

**Live structure:** Home → About → Music → Live → Press → Contact, presented as a horizontally-scrolling pinned track on desktop (GSAP ScrollTrigger) that falls back to a normal stacked vertical layout on mobile/narrow screens.

## Running it locally

There's nothing to install or build. Either:

- Open `index.html` directly in a browser, or
- Serve the folder with any static file server (recommended, since some browsers restrict local video/`file://` playback):

```bash
python -m http.server 8000
# then visit http://localhost:8000
```

## Structure

- `index.html` — the entire site: markup, CSS, and JS in one file
- `IMG_*.jpeg`, `_A7R*.jpg`, `_A749*.jpg` — photography used across the hero, about, and live gallery sections
- `Untitled.png`, `selina article *.png` — press clipping images
- `como-live-web.mp4`, `fabrica-live-web.mp4` — compressed live-performance video clips used in the hero and Live section video players

## Video assets

The two `.mp4` files are re-encoded, web-sized versions of much larger camera-original footage (H.264, two-pass, ~5 Mbps for the 1080p clip and ~2.9 Mbps for the 720p clip), kept under GitHub's 100MB per-file limit while staying visually close to full quality. The raw masters aren't part of this repo — if new footage needs to be added, compress it first:

```bash
ffmpeg -y -i "source.mp4" -c:v libx264 -preset slow -b:v <bitrate> -pass 1 -an -f mp4 NUL
ffmpeg -y -i "source.mp4" -c:v libx264 -preset slow -b:v <bitrate> -pass 2 -pix_fmt yuv420p -c:a aac -b:a 128k -movflags +faststart "output-web.mp4"
```

Pick `<bitrate>` so `bitrate × duration ⁄ 8 ≈` target file size (aim comfortably under 100MB).

## Notes on the video player

Video playback happens in a full-size modal (`#video-modal`), not inline in the small thumbnail cards. This is intentional: any CSS `transform` on an ancestor of a `<video>` element — including the horizontal-scroll track this site uses — silently breaks the native Fullscreen API. Keeping the actual `<video>` element at the top level of the page (outside the transformed track) is what makes fullscreen playback work correctly.

## Dependencies

Loaded via CDN, no local install needed:
- [GSAP](https://gsap.com/) + ScrollTrigger — scroll-driven animation and the pinned horizontal track
- Google Fonts — Cormorant Garamond (display) and Outfit (body/UI)
