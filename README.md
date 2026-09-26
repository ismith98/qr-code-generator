# QR Code Generator

A single-page QR code generator that puts your own image (a logo, icon or photo) in the center of the code. It runs entirely in your browser. Nothing is uploaded anywhere.

## Getting started

Open `index.html` in any modern browser. There's nothing to install or build.

> The page loads two small libraries from a CDN, so it needs an internet connection when it opens.

## Features

- **Live preview.** Type a URL or any text and the code updates as you type.
- **Center image.** Drop an image in, browse for a file, or paste one from the clipboard. You can adjust:
  - **Logo size.** How much of the code the image covers.
  - **Logo padding.** Space between the image and the surrounding dots.
  - **Behind the logo.** Clear the dots in a square area, or add a white rounded, circle or square badge, or place the logo directly on the dots.
- **Shapes.** Six dot styles, plus separate styles for the corner squares and their centers.
- **Quiet zone.** Set the width of the blank border around the code.
- **Export.** Download a PNG (512–4096 px) or an SVG, which scales to any size and suits print.
- **Scan check.** Every time the code changes, the page decodes it to confirm it still scans with the logo in place, and warns you if it doesn't.
- **Remembers your settings.** Your last text, logo and style are saved in this browser (`localStorage`).

## Tips for codes that scan reliably

- Keep the logo at **40% or less** on the logo size slider. If the scan check shows ✗, shrink the logo or reduce its padding.
- Leave a **quiet zone of at least 4%**. Some scanners struggle without blank space around the code.
- Shorter text makes a simpler code with bigger dots, which scans more easily from a distance.
- **Always test the downloaded file with a phone before printing.** The built-in check is a strong guide, not a guarantee.

## How it works

A QR code can still be read when part of it is damaged or covered. This app always uses the highest error-correction level, **H**, which can recover roughly 30% of the code. The logo covers part of the center, and the error correction fills in what's missing when the code is scanned.

The three large corner squares are position markers, and they're identical on every QR code. The small dots in between carry the data and change completely whenever the text changes.

## Built with

- [qr-code-styling](https://github.com/kozakdenys/qr-code-styling) renders the code, the shapes and the logo, and handles export.
- [jsQR](https://github.com/cozmo/jsQR) decodes the code in the browser for the scan check.

## Known limitations

- Some SVG viewers show faint hairlines between adjacent dots. This is an anti-aliasing artifact and doesn't affect scanning. Use the PNG export if it bothers you.
- The maximum length is about 1,200 characters, which is the capacity at error-correction level H.
