# CLAUDE.md

Single-file app: everything (markup, styles, script) lives in `index.html`. No build step, no package.json. Libraries come from jsDelivr (`qr-code-styling@1.9.2`, `jsqr@1.4.0`). Keep it single-file unless the user asks otherwise.

## Non-obvious decisions — don't undo these

- **UTF-8 pre-encoding (`toUtf8ByteString`)**: qr-code-styling's default byte encoder truncates each char code to 8 bits (Latin-1), which corrupts accents/emoji/CJK. We pass a UTF-8 byte string instead. Removing this breaks non-ASCII text.
- **Error correction is always `H`**: it's what lets the logo cover the center. `imageOptions.imageSize` is a fraction of that capacity (hidden area ≈ `imageSize × 30%` of modules), not a fraction of the image width.
- **Logo backdrop is composed on a canvas (`composeLogo`)**: `hideBackgroundDots` always clears a *square* area, so rounded/circle badges only show when it's off. Padding and badge are baked into the image before handing it to the library.
- **Preview renders to canvas, not SVG**: SVG rendering shows hairline seams between adjacent dots. The canvas is rendered at 900px and downscaled by CSS for retina sharpness.
- **Scan verification pads the image with white** before decoding with jsQR, so it tests logo damage, not the quiet zone (the thin-border case gets its own warning).
- **`generation` counter** guards async renders; any new async step in `render()` needs a `if (gen !== generation) return;` after it.
- `localStorage` access is wrapped in try/catch; logos are downscaled to 512px before storing to stay within quota.

## Testing

No test suite. Verify changes in a real browser: `puppeteer-core` pointed at the system Chrome (`/Applications/Google Chrome.app/Contents/MacOS/Google Chrome`) loading `file://.../index.html` works well. Cover at least: no logo, logo with each backdrop, an oversized logo (should show ✗), over-long text (overflow error), non-ASCII text (should scan ✓), reload persistence, PNG/SVG download, and 390px-wide layout with no horizontal overflow. Put test scripts outside the repo unless the user wants them committed.
