# PostScript to PDF in the browser

This is a tool for converting PostScript (`.ps` or `.ps.gz`) files to PDF entirely in the browser.

Serving at {TODO: url here}.

My preferred way when not on a phone is to use [this browser extension](https://chromewebstore.google.com/detail/postscript-viewer/ebpiondkhkldijolgmhfenknngkkjola) ([GitHub](https://github.com/ochachacha/ps-wasm)), but this tool is for other times, or others who don't have it installed.

Mostly written by Claude (Sonnet 4.5) and then by ChatGPT (GPT-5 Thinking). Uses Ghostscript compiled to WebAssembly, from [this repo](https://github.com/laurentmmeyer/ghostscript-pdf-compress.wasm).

TODO: Tested on {Chrome, Firefox} on {macOS, Android}, and Safari on macOS. Not tested on iPhones.

## Quick Start

Visit <url> and use it.

Alternatively, put these three files:

- gs.js
- gs.wasm
- index.html

and start a server, for example

```bash
python3 -m http.server 8000
# Then open in browser: http://localhost:8000/index.html
```

## How It Works

1. User uploads PostScript file or provides URL
2. File is loaded into browser (stays local)
3. Ghostscript WebAssembly converts PS → PDF
4. PDF is displayed in browser and can be downloaded

The ~14MB `gs.wasm` file is downloaded once and cached by your browser.

## Customization

### Modify Ghostscript Options

In the JavaScript, find the `arguments` array:

```javascript
arguments: [
    '-sDEVICE=pdfwrite',
    '-dNOPAUSE',
    '-dBATCH',
    '-dSAFER',
    '-dPDFSETTINGS=/prepress',  // Quality: /screen, /ebook, /printer, /prepress
    '-dEmbedAllFonts=true',      // Embed all fonts
    '-sOutputFile=output.pdf',
    'input.ps'
]
```

## Troubleshooting

**"Failed to load Ghostscript"**
- Make sure `gs.js` and `gs.wasm` are in the same directory as the HTML (Run `setup.sh` if they are missing.)
- Use a web server, not `file://`

## License

**AGPL-3.0** (same as Ghostscript)

See [LICENSE](LICENSE) file for details.

## Privacy

All processing happens in your browser. Your files never leave your device. The converter:
- Doesn't upload files to any server
- Doesn't require an internet connection (after initial load of the HTML, JS and WASM files)
