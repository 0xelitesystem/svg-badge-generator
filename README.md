# SVG Badge Generator

Generate README status badges as pure inline SVG with copy-ready markdown and HTML embeds - no badge service, no rate limits, works offline.

**Live demo:** https://0xelitesystem.github.io/svg-badge-generator/

## Use

1. Type the label text and value text, or start from one of the quick presets.
2. Pick the label and value background colors and a style: flat, flat-square, or for-the-badge.
3. Check the preview on the light and dark backgrounds.
4. Copy the raw SVG, one of the markdown embeds, or the HTML img tag, or click Download .svg.

## Why this exists

A hosted badge service sees every view of your README, can rate-limit you, and shows a broken image when it goes down. This tool builds the SVG locally so you can inline it or commit it, in one HTML file with no tracking, released under the MIT license.

## Features

- Label and value text, label and value background colors, with a preset palette of the classic badge colors (brightgreen, green, yellow, orange, red, blue, lightgrey, brand-dark)
- Text color auto-picked per segment: white or dark, whichever has the higher WCAG contrast ratio against the background
- Three styles: flat (3px radius plus the classic subtle gradient overlay), flat-square (no radius, no gradient), and for-the-badge (taller, uppercase, wider padding)
- Live preview at 1x and 2x on both a light and a dark checker background, so you see how the badge reads on any README theme
- Four copy-ready outputs, each with its own copy button: raw SVG markup, markdown with a base64 data URI (zero hosted files), markdown referencing a committed docs/badge.svg, and an HTML img tag
- Download .svg button, built locally with a Blob and an anchor element
- Quick presets for common badges (build, coverage, version, license, tests)
- Empty label or value degrades gracefully to a single-segment badge
- Dark and light UI themes, persisted to localStorage; keyboard accessible; responsive down to 360px

## How it works

Everything is a single index.html with no external dependencies.

- Text is measured with canvas measureText using the same font stack the SVG declares (Verdana, DejaVu Sans, sans-serif at 11px, or 10px for for-the-badge), with an average-character-width fallback when canvas is unavailable. Each badge segment sizes itself from the measured width plus padding.
- Every SVG text element carries a textLength attribute set to the measured width, so rendering is locked to the layout even when the viewer's system substitutes a different font. Text never clips.
- Text color per segment is chosen by computing WCAG relative luminance of the background and picking white or #333, whichever yields the higher contrast ratio.
- The generated SVG is standalone and self-contained: correct xmlns, role="img", a title element for screen readers, and zero external references.
- User text is XML-escaped (ampersands, angle brackets, quotes), so any input produces valid SVG. Markdown alt text is escaped separately for brackets and backslashes.
- The data URI embed is base64 built from a TextEncoder byte stream, so non-ASCII text in labels survives encoding.

Why not a badge service: a hosted badge is an external request on every README view. That means someone else sees your traffic, can rate-limit you, and a service outage shows a broken image. An inline or committed SVG is permanent, private, and renders offline.

## Privacy

Everything runs in your browser. Nothing you type is uploaded, logged, or sent anywhere. The page makes zero network requests and works offline once loaded.

If you use the theme toggle, your light or dark choice is saved in your browser's localStorage under the key `sbg-theme`. Nothing else is stored.

## Run locally

```
git clone https://github.com/0xelitesystem/svg-badge-generator
cd svg-badge-generator
```

Open `index.html` in a browser, or serve the folder with `python -m http.server` and visit http://localhost:8000.

## Build

No build step. The whole tool is one `index.html` file.

## More

Part of a catalog of single-file browser tools and plain-language references, all MIT licensed and dependency-free: [0xelitesystem.github.io](https://0xelitesystem.github.io/). Built by [elitesystem.ai](https://elitesystem.ai).

## License

MIT
