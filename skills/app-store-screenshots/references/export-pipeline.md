# Export Pipeline Reference

Complete guide for exporting screenshots from the HTML/CSS builder at correct
dimensions using html2canvas (browser) or Puppeteer (headless automation).

---

## Critical Requirements

```
⚠️ NEVER open export-tool.html as a file:// URL
   Canvas export WILL fail with: "SecurityError: Tainted canvases may not be exported"

✅ ALWAYS serve via HTTP:
   python3 -m http.server 8080
   Then visit: http://localhost:8080/export-tool.html

✅ Add crossorigin="anonymous" to ALL <img> tags
✅ In html2canvas config, use: { useCORS: true }
```

---

## Method A: Browser Export (html2canvas)

Best for quick exports during design iteration.

### Setup

Include html2canvas via CDN in your export tool:

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
```

### Export Function

```javascript
// Detect file:// and warn immediately
if (window.location.protocol === 'file:') {
    alert('Export requires HTTP server.\nRun: python3 -m http.server 8080');
}

async function exportOne(elementId, filename, format = 'png') {
    const el = document.getElementById(elementId);

    // Temporarily remove preview scale transform for full-res capture
    const origTransform = el.style.transform;
    el.style.transform = 'none';

    const canvas = await html2canvas(el, {
        width: 1260,
        height: 2736,
        scale: 1,             // Already at target size — don't upscale
        useCORS: true,        // Required for cross-origin images
        allowTaint: false,    // Must be false for toDataURL() to work
        backgroundColor: null,
        logging: false,
    });

    // Restore preview transform
    el.style.transform = origTransform;

    // Download
    const link = document.createElement('a');
    link.download = `${filename}.${format === 'png' ? 'png' : 'jpg'}`;
    link.href = canvas.toDataURL(
        format === 'png' ? 'image/png' : 'image/jpeg',
        format === 'png' ? undefined : 0.95
    );
    link.click();
}
```

### Batch Export

```javascript
async function exportAll(locale = 'en-US') {
    const screenshots = [
        { id: 'ss1', name: '01_hook' },
        { id: 'ss2', name: '02_feature1' },
        { id: 'ss3', name: '03_feature2' },
        { id: 'ss4', name: '04_feature3' },
        { id: 'ss5', name: '05_usecase' },
        { id: 'ss6', name: '06_trust' },
    ];

    for (const { id, name } of screenshots) {
        await exportOne(id, `${name}_${locale}`, 'png');
        await new Promise(r => setTimeout(r, 500)); // Brief delay between exports
    }
}
```

### html2canvas Limitations

| Issue | Workaround |
|-------|------------|
| Some CSS properties not supported | Use simpler CSS (no `backdrop-filter`, limited `filter`) |
| Fonts may not render correctly | Use web-safe fonts or include @font-face |
| SVGs may not render | Convert SVGs to PNGs before export |
| Large images may cause memory issues | Optimize source images before using |

---

## Method B: Puppeteer Headless Capture (Recommended)

Best for automated batch exports. More reliable font and image rendering.

### Setup

```bash
npm install puppeteer
```

### Why Naive Approaches Fail

```
FAILED APPROACH 1: element.screenshot() after removing transform
  Problem: Parent .screenshot-container has overflow:hidden at the
  scaled size. Even with transform:none, the container clips the content.

FAILED APPROACH 2: page.screenshot() of full page
  Problem: Captures the entire page layout including toolbars,
  labels, and multiple screenshots side by side.

WORKING APPROACH: Clone target element into a fixed full-size wrapper
```

### Complete Capture Script

```javascript
const puppeteer = require('puppeteer');

async function captureScreenshot(page, elementId, outputPath) {
    // Step 1: Clone element into full-size fixed wrapper
    await page.evaluate((id) => {
        const canvas = document.getElementById(id);
        if (!canvas) throw new Error(`Element #${id} not found`);

        // Hide all page content
        document.body.style.margin = '0';
        document.body.style.padding = '0';
        document.body.style.overflow = 'hidden';
        for (const child of document.body.children) {
            child.style.display = 'none';
        }

        // Create fixed wrapper at exact viewport size
        let wrapper = document.getElementById('__capture_wrapper');
        if (!wrapper) {
            wrapper = document.createElement('div');
            wrapper.id = '__capture_wrapper';
            document.body.prepend(wrapper);
        }

        wrapper.style.cssText = `
            display: block;
            position: fixed;
            top: 0; left: 0;
            width: 1260px; height: 2736px;
            overflow: hidden;
            z-index: 99999;
        `;

        // Clone the canvas at full size (no transform)
        const clone = canvas.cloneNode(true);
        clone.id = '__capture_target';
        clone.style.cssText = `
            transform: none;
            width: 1260px; height: 2736px;
            position: absolute;
            top: 0; left: 0;
        `;

        wrapper.innerHTML = '';
        wrapper.appendChild(clone);
    }, elementId);

    // Step 2: Wait for cloned images to load
    await new Promise(r => setTimeout(r, 1000));

    // Step 3: Capture at exact dimensions
    await page.screenshot({
        path: outputPath,
        type: 'png',
        clip: { x: 0, y: 0, width: 1260, height: 2736 },
    });

    // Step 4: Clean up — restore page
    await page.evaluate(() => {
        const wrapper = document.getElementById('__capture_wrapper');
        if (wrapper) wrapper.remove();
        for (const child of document.body.children) {
            child.style.display = '';
        }
        document.body.style.margin = '';
        document.body.style.padding = '';
        document.body.style.overflow = '';
    });
}

// Main capture loop
async function captureAll(exportToolUrl, screenshots, outputDir) {
    const browser = await puppeteer.launch({ headless: 'new' });
    const page = await browser.newPage();

    // Viewport MUST match export dimensions
    await page.setViewport({
        width: 1260,
        height: 2736,
        deviceScaleFactor: 1,
    });

    await page.goto(exportToolUrl, {
        waitUntil: 'networkidle0',
        timeout: 30000,
    });

    // Wait for fonts to load
    await page.waitForFunction(() => document.fonts.ready);
    await new Promise(r => setTimeout(r, 3000));

    for (const { id, filename } of screenshots) {
        const outputPath = `${outputDir}/${filename}`;
        await captureScreenshot(page, id, outputPath);
        console.log(`Captured: ${outputPath}`);
    }

    await browser.close();
}

// Usage
captureAll('http://localhost:8080/export-tool.html', [
    { id: 'ss1', filename: '01_hook_en-US.png' },
    { id: 'ss2', filename: '02_feature1_en-US.png' },
    { id: 'ss3', filename: '03_feature2_en-US.png' },
    { id: 'ss4', filename: '04_feature3_en-US.png' },
    { id: 'ss5', filename: '05_usecase_en-US.png' },
    { id: 'ss6', filename: '06_trust_en-US.png' },
], './exports/en-US');
```

---

## Verify Exported Dimensions

Always verify after export — never trust the filename or process:

```bash
# macOS
sips -g pixelWidth -g pixelHeight screenshot.png
# Expected output:
#   pixelWidth: 1260
#   pixelHeight: 2736

# Linux (ImageMagick)
identify screenshot.png
# Expected: screenshot.png PNG 1260x2736 ...

# Python
python3 -c "from PIL import Image; print(Image.open('screenshot.png').size)"
# Expected: (1260, 2736)
```

---

## Export Naming Convention

```
{nn}_{name}_{locale}.{ext}

Examples:
  01_hook_en-US.png
  02_scan_food_en-US.png
  03_ai_tracker_en-US.png
  01_hook_tr.png
  02_yemek_tara_tr.png
  03_ai_takip_tr.png
```

### Folder Structure

```
export/
└── locales/
    ├── en-US/
    │   ├── 01_hook.png
    │   ├── 02_feature1.png
    │   └── ...
    ├── tr/
    │   ├── 01_hook.png
    │   └── ...
    └── de-DE/
        ├── 01_hook.png
        └── ...
```

---

## Troubleshooting

| Problem | Cause | Solution |
|---------|-------|----------|
| "Tainted canvases may not be exported" | Opened as `file://` URL | Serve via `python3 -m http.server 8080` |
| Exported image is tiny | Captured the CSS-scaled preview | Use DOM cloning approach (see Puppeteer section) |
| Fonts look different in export | Font not loaded before capture | Add `await page.waitForFunction(() => document.fonts.ready)` |
| Images missing in export | Cross-origin images blocked | Add `crossorigin="anonymous"` to img tags |
| Blurry export | html2canvas `scale` set > 1 | Use `scale: 1` (element is already at target size) |
| Puppeteer clips content | Parent overflow:hidden at scaled size | Use clone-to-wrapper approach |
| Colors slightly off | Browser color profile conversion | Use sRGB color space in CSS, verify exported PNG |
