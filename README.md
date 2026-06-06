# lottie-tools

> A collection of open-source Lottie and SVG conversion tools by [KodeKing](https://github.com/kodeking)

All tools are extracted from [IconKing](https://iconking.net) — a Lottie animation platform — and published as standalone, dependency-light packages. Every tool has a live web version you can try without installing anything.

---

## Packages

### Browser (client-side) — no server required

| Package | Description | Live tool |
|---|---|---|
| [@kodeking/lottie-to-svg](https://github.com/kodeking/lottie-to-svg) | Extract SVG frames from a Lottie animation | [iconking.net/tools/lottie-to-svg](https://iconking.net/tools/lottie-to-svg) |
| [@kodeking/svg-to-lottie](https://github.com/kodeking/svg-to-lottie) | Wrap an SVG file as a Lottie JSON animation | [iconking.net/tools/svg-to-lottie](https://iconking.net/tools/svg-to-lottie) |
| [@kodeking/lottie-to-dotlottie](https://github.com/kodeking/lottie-to-dotlottie) | Convert Lottie JSON to .lottie binary format | [iconking.net/tools/lottie-json-to-dotlottie](https://iconking.net/tools/lottie-json-to-dotlottie) |

### Node.js (server-side) — requires ffmpeg / gifski

| Package | Description | Live tool |
|---|---|---|
| [@kodeking/lottie-to-gif](https://github.com/kodeking/lottie-to-gif) | Render Lottie to animated GIF (gifski or ffmpeg) | [iconking.net/tools/lottie-to-gif](https://iconking.net/tools/lottie-to-gif) |
| [@kodeking/lottie-to-mp4](https://github.com/kodeking/lottie-to-mp4) | Render Lottie to MP4 video (H.264) | [iconking.net/tools/lottie-to-mp4](https://iconking.net/tools/lottie-to-mp4) |
| [@kodeking/lottie-to-webm](https://github.com/kodeking/lottie-to-webm) | Render Lottie to WebM (VP9 with transparency) | [iconking.net/tools/lottie-to-webm](https://iconking.net/tools/lottie-to-webm) |
| [@kodeking/lottie-to-webp](https://github.com/kodeking/lottie-to-webp) | Render Lottie to animated WebP | [iconking.net/tools/lottie-to-webp](https://iconking.net/tools/lottie-to-webp) |
| [@kodeking/lottie-to-apng](https://github.com/kodeking/lottie-to-apng) | Render Lottie to animated PNG (APNG) | [iconking.net/tools/lottie-to-apng](https://iconking.net/tools/lottie-to-apng) |

### Python + Node.js wrapper

| Package | Description | Live tool |
|---|---|---|
| [@kodeking/ai-to-svg](https://github.com/kodeking/ai-to-svg) | Convert Adobe Illustrator (.ai) files to SVG (MuPDF) | [iconking.net/tools/ai-to-svg](https://iconking.net/tools/ai-to-svg) |

---

## Quick start

### Browser packages

```bash
npm install @kodeking/lottie-to-svg lottie-web
```

```ts
import { extractSvgFrame } from '@kodeking/lottie-to-svg';

const json = await fetch('/animation.json').then(r => r.json());
const svg  = await extractSvgFrame(json, { frame: 0 });
document.body.innerHTML = svg;
```

### Server-side packages

```bash
# Install dependencies
brew install ffmpeg gifski   # macOS
# or: apt install ffmpeg (Ubuntu)

npm install -g @kodeking/lottie-to-gif
lottie-to-gif animation.json animation.gif --fps 15 --width 480
```

### AI to SVG

```bash
pip3 install pymupdf
npm install -g @kodeking/ai-to-svg
ai-to-svg logo.ai logo.svg
```

---

## Format comparison

| Format | Transparency | Colors | Size | Browser support | Use case |
|---|---|---|---|---|---|
| GIF | 1-bit | 256 | Large | Universal | Email, Slack, legacy |
| MP4 | ❌ | Full RGB | Small | All | Presentations, social media |
| WebM | Full RGBA | Full RGB | Small | All modern (not Safari) | Transparent video overlays |
| WebP | Full RGBA | Full RGB | Medium | All modern | Drop-in GIF replacement |
| APNG | Full RGBA | Full RGB (lossless) | Larger | All modern | Pixel-perfect, no quality loss |
| SVG | Full RGBA | Full (vector) | Tiny | All | Static export, icons |
| .lottie | N/A | N/A | Smallest | Lottie players | Smaller Lottie distribution |

---

## How the server-side tools work

All Node.js packages share the same two-step pipeline:

1. **Render** — Puppeteer launches a headless Chromium, loads the Lottie JSON with `lottie-web` in canvas mode, seeks to each frame, and saves PNG screenshots
2. **Encode** — The PNGs are piped into the format-specific encoder (gifski, ffmpeg, img2webp)

This approach is accurate and requires no native Lottie runtime binary — just Node.js and the encoder tool.

---

## Contributing

Issues and PRs are welcome on each individual package repo. For bugs affecting multiple tools, open an issue here.

---

## License

MIT © [KodeKing](https://github.com/kodeking) — built with ❤️ for the Lottie community.
