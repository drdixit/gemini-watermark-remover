# Gemini Watermark Remover

[![License](https://img.shields.io/badge/license-MIT-orange?style=flat-square)](LICENSE)
[![Stack](https://img.shields.io/badge/tech-Vue%20%7C%20Vite%20%7C%20Tailwind-3B82F6?style=flat-square)](#-tech-stack)

A high-performance, **100% client-side** tool for removing visible Google Gemini ✦ AI sparkle watermarks from both **images and videos**. Built with **Vue 3, Vite, and Tailwind CSS**, it utilizes mathematically exact **Reverse Alpha Blending** to restore underlying pixels with zero quality loss.

---

## ✨ Before & After

<table>
  <tr>
    <th align="center">Before (watermarked)</th>
    <th align="center">After (cleaned)</th>
  </tr>
  <tr>
    <td align="center"><img src="public/assets/before.png" alt="Gemini AI image with the visible sparkle watermark" width="100%" /></td>
    <td align="center"><img src="public/assets/after.png" alt="Gemini AI image after the sparkle watermark was removed" width="100%" /></td>
  </tr>
</table>

---

## 🚀 Features

* **Images & Videos:** Remove sparkle watermarks from Gemini images and videos entirely in the browser.
* **100% Client-Side Processing:** No servers involved. Files are processed locally in browser memory for complete privacy.
* **Lossless Image Restoration:** Uses exact mathematical inversion rather than generative AI inpainting, preserving original pixel quality.
* **Audio-Preserving Video Cleaning:** Videos are processed frame-by-frame and re-encoded with original audio preserved.
* **Auto-Detection:** Automatically calculates watermark dimensions (48px, 96px, or compact) based on media resolution.
* **Mobile Responsive:** Fully responsive interface for desktop and mobile devices.

---

## 🧠 How It Works (The Algorithm)

Google Gemini applies visible watermarks using standard **Alpha Compositing** (blending). This tool mathematically reverses that process.

### 1. The Watermark Formula
When Gemini watermarks an image, it blends the logo using:

$$Pixel_{final} = (\alpha \times Pixel_{logo}) + (1 - \alpha) \times Pixel_{original}$$

Where:
* `α` (Alpha): The transparency level of the watermark pixel (0.0 to 1.0).
* `Pixel_logo`: The watermark color (Pure White / 255).
* `Pixel_original`: The original pixel value to recover.

### 2. The Analytical Solution
Because the formula is linear and the logo color is known (White / 255), we solve for the original pixel value:

$$Pixel_{original} = \frac{Pixel_{final} - (\alpha \times 255)}{1 - \alpha}$$

### 3. Alpha Map Calibration
Reference transparency maps (`bg_48.png` and `bg_96.png` in `public/assets/`) provide the isolated watermark alpha values. The engine reads these into a `Float32Array` matrix to perform exact per-pixel subtraction.

---

## 📂 Project Structure

```text
gemini-watermark-remover/
├── index.html              # Vite entry point and SEO head tags
├── package.json            # Dependencies: Vue 3, Mediabunny, Tailwind, Vite
├── vite.config.js          # Vite configuration with @vitejs/plugin-vue
├── tailwind.config.js      # Tailwind CSS configuration
├── postcss.config.js       # PostCSS plugins (Tailwind, Autoprefixer)
├── public/                 # Static assets copied directly to build
│   ├── assets/             # bg_48.png, bg_96.png (reference alpha maps)
│   ├── css/output.css      # Generated Tailwind CSS for static SEO pages
│   └── *.html, blog/       # Static marketing, SEO, and legal pages
├── src/
│   ├── main.js             # Vue application bootstrap
│   ├── App.vue             # Root component & tab switching (Image vs Video)
│   ├── style.css           # Global Tailwind and app styling
│   ├── components/
│   │   ├── ImageRemover.vue    # Image processing UI, drag-and-drop, JSZip export
│   │   ├── VideoRemover.vue    # Video processing UI, timeline preview, WebCodecs pipeline
│   │   ├── WatermarkTuner.vue  # Real-time fine-tuning controls (position, size, gain)
│   │   ├── BeforeAfter.vue     # Interactive comparison slider
│   │   ├── SiteHeader.vue      # Navigation header & theme switcher
│   │   ├── SiteFooter.vue      # Footer links
│   │   ├── SupportPopup.vue    # Support modal
│   │   └── NotificationBanner.vue # Top alert/announcement banner
│   └── engine/
│       ├── blendModes.js       # Core Reverse Alpha Blending math
│       ├── alphaMap.js         # Extraction of Float32Array alpha maps from reference PNGs
│       ├── geometry.js         # Coordinate math for standard & compact watermark bounds
│       ├── tuner.js            # Live parameter adjustments, ROI bounding, canvas cleaning
│       ├── watermarkEngine.js  # Canvas 2D image watermark removal pipeline
│       └── videoEngine.js      # WebCodecs frame-by-frame decoding, cleaning, and MP4 re-muxing
└── docs/                   # Local architectural documentation and agent change logs
```

---

## 🛠️ Tech Stack

* **Vue 3** (Composition API, `<script setup>`)
* **Vite** for local development and optimized production bundling
* **Tailwind CSS** (via PostCSS)
* **MediaBunny** for client-side WebCodecs video processing
* **JSZip** for bulk image download archives
* **Iconify** for interface icons

---

## ⚡ Running Locally

```bash
npm install

# Build static page Tailwind CSS
npm run build:css

# Start Vite dev server
npm run dev
```

---

## ⚠️ Disclaimer

This tool is for **educational and personal use only**.
* It removes visible watermarks only; it **does not** remove invisible SynthID watermarks embedded by Google.
* Respect copyright laws and Google's Terms of Service.

---

## 🤝 Credits & Acknowledgements

This application builds upon the contributions and research of several open-source projects and developers:

* **[Abhin Krishna](https://github.com/dearabhin/gemini-watermark-remover)**: Creator of the original [gemini-watermark-remover](https://github.com/dearabhin/gemini-watermark-remover) web application, UI design, and baseline codebase upon which this project was forked and evolved.
* **[allenk / GeminiWatermarkTool](https://github.com/allenk/GeminiWatermarkTool)**: Pioneer of the mathematical Reverse Alpha Blending algorithm for inverting Gemini watermarks.
* **[GargantuaX / gemini-watermark-remover](https://github.com/GargantuaX/gemini-watermark-remover)**: Video watermark removal pipeline leveraging in-browser WebCodecs.
* **[Vanilagy / mediabunny](https://github.com/Vanilagy/mediabunny)**: WebCodecs demuxing, decoding, and encoding library powering the browser video engine.
