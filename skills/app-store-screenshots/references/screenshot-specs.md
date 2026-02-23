# Screenshot Specifications Reference

Complete technical requirements for App Store screenshots.
Verify latest specs at: https://developer.apple.com/help/app-store-connect/reference/screenshot-specifications/

## Mandatory Sizes

Apple requires these two screenshot sizes (all others auto-scale):

| Device | Dimensions (Portrait) | Dimensions (Landscape) | Required |
|--------|----------------------|------------------------|----------|
| **iPhone 6.9"** | 1260 x 2736 | 2736 x 1260 | **Yes** |
| **iPad 13"** | 2064 x 2752 | 2752 x 2064 | **Yes** |

Apple automatically scales these to fit smaller devices.

---

## All iPhone Sizes

| Display | Dimensions (Portrait) | Dimensions (Landscape) | Devices | API Display Type |
|---------|----------------------|------------------------|---------|-----------------|
| 6.9" | 1260 x 2736 | 2736 x 1260 | iPhone 16 Pro Max | `APP_IPHONE_67` |
| 6.7" | 1290 x 2796 | 2796 x 1290 | iPhone 16 Plus, 15 Pro Max | `APP_IPHONE_67` |
| 6.5" | 1284 x 2778 or 1242 x 2688 | 2778 x 1284 or 2688 x 1242 | iPhone 14 Plus, 13 Pro Max | `APP_IPHONE_65` |
| 6.1" | 1179 x 2556 or 1170 x 2532 | 2556 x 1179 or 2532 x 1170 | iPhone 16, 15, 14 | `APP_IPHONE_61` |
| 5.8" | 1125 x 2436 | 2436 x 1125 | iPhone X, XS, 11 Pro | `APP_IPHONE_58` |
| 5.5" | 1242 x 2208 | 2208 x 1242 | iPhone 8 Plus (legacy) | `APP_IPHONE_55` |
| 4.7" | 750 x 1334 | 1334 x 750 | iPhone SE (legacy) | `APP_IPHONE_47` |

### Important: Display Type Mapping

```
⚠️ APP_IPHONE_69 does NOT exist in the App Store Connect API.
   Both 6.9" and 6.7" screenshots use APP_IPHONE_67.

When uploading 1260x2736 (6.9") screenshots, use display type: APP_IPHONE_67
When uploading 1290x2796 (6.7") screenshots, use display type: APP_IPHONE_67
```

---

## All iPad Sizes

| Display | Dimensions (Portrait) | Dimensions (Landscape) | Devices | API Display Type |
|---------|----------------------|------------------------|---------|-----------------|
| 13" | 2064 x 2752 | 2752 x 2064 | iPad Pro 13" (M4) | `APP_IPAD_PRO_3GEN_129` |
| 12.9" | 2048 x 2732 | 2732 x 2048 | iPad Pro 12.9" (legacy) | `APP_IPAD_PRO_3GEN_129` |
| 11" | 1668 x 2388 | 2388 x 1668 | iPad Pro 11", iPad Air | `APP_IPAD_PRO_3GEN_11` |
| 10.9" | 1640 x 2360 | 2360 x 1640 | iPad (10th gen) | `APP_IPAD_109` |
| 10.5" | 1668 x 2224 | 2224 x 1668 | iPad Air 3, iPad Pro 10.5" | `APP_IPAD_PRO_129` |
| 9.7" | 1536 x 2048 | 2048 x 1536 | iPad (legacy) | `APP_IPAD_97` |

---

## Technical Requirements

| Requirement | Specification |
|-------------|---------------|
| **File Format** | JPEG or PNG (24-bit, no transparency) |
| **Maximum File Size** | 8 MB per screenshot |
| **Color Space** | sRGB |
| **Resolution** | 72 dpi |
| **Minimum Count** | 1 screenshot |
| **Maximum Count** | 10 screenshots per localization |

---

## Safe Zones

Keep critical content away from these areas:

```
┌─────────────────────────────────────┐
│         160px (Dynamic Island)       │
│  ┌─────────────────────────────┐    │
│  │                             │    │
│1 │                             │ 1  │
│2 │      SAFE CONTENT AREA      │ 2  │
│0 │                             │ 0  │
│p │                             │ p  │
│x │                             │ x  │
│  │                             │    │
│  └─────────────────────────────┘    │
│          34px (Home Indicator)       │
└─────────────────────────────────────┘
```

| Zone | Clearance | Reason |
|------|-----------|--------|
| Top center | 160px | Dynamic Island on newer iPhones |
| All edges | 120px | May be cropped in some views |
| Bottom | 34px | Home indicator area |

---

## Text Readability

| Minimum Size | Context |
|--------------|---------|
| 28pt | Body text (must be readable at thumbnail size) |
| 36pt | Headlines (recommended) |
| 48pt+ | Primary captions |

**Note:** Screenshots appear as small thumbnails in search results. Test readability by viewing at 25% zoom.

---

## App Preview Videos

If including video previews:

| Device | Resolution | Duration |
|--------|------------|----------|
| iPhone 6.9" | 1260 x 2736 | 15-30 seconds |
| iPad 13" | 2064 x 2752 | 15-30 seconds |

- Maximum 3 videos per localization
- Must match screenshot dimensions for device

---

## Localization

Screenshots can be localized for each supported locale:

- Different images per language/region
- Same technical requirements apply
- Maximum 10 screenshots per locale

### Priority Locales for Screenshots

| Tier | Locales |
|------|---------|
| 1 | en-US, en-GB, de-DE, ja, zh-Hans |
| 2 | fr-FR, es-ES, ko, pt-BR, it |
| 3 | nl, ru, tr, ar, zh-Hant |

---

## CDN Dimension Warning

```
⚠️ The Apple CDN (mzstatic.com) does NOT upscale images.

If a developer uploaded 5.5" screenshots (1242x2208), requesting a URL
ending in 1320x2868bb.png silently returns the original 1242x2208 image.

ALWAYS verify actual downloaded dimensions:
  macOS:  sips -g pixelWidth -g pixelHeight screenshot.png
  Linux:  identify screenshot.png  (ImageMagick)
  Python: from PIL import Image; Image.open("screenshot.png").size
```

---

## Common Mistakes to Avoid

| Mistake | Why It's a Problem |
|---------|-------------------|
| Using 1290x2796 as 6.9" dimension | Wrong — 6.9" is 1260x2736 |
| Using APP_IPHONE_69 display type | Doesn't exist — use APP_IPHONE_67 |
| Text too small | Unreadable in search results |
| Content in Dynamic Island area | Gets obscured on newer devices |
| Wrong aspect ratio | Will be rejected |
| Transparency in PNG | Not allowed |
| Over 8 MB file size | Will be rejected |
| Less than 1 screenshot | Submission blocked |
| Competitor logos visible | Policy violation |
| Misleading content | Policy violation |
| Trusting CDN URL dimensions | CDN returns original size, not requested size |

---

## Export Checklist

```markdown
## Pre-Upload Checklist

### Technical
- [ ] iPhone: 1260 x 2736 (6.9" — required)
- [ ] iPad: 2064 x 2752 (13" — required)
- [ ] Format: PNG or JPEG
- [ ] No transparency
- [ ] Under 8 MB each
- [ ] sRGB color space
- [ ] Verified actual pixel dimensions (not just filename)

### Content
- [ ] Text readable at thumbnail size
- [ ] No content in Dynamic Island zone
- [ ] No competitor names/logos
- [ ] Accurate representation of app
- [ ] Consistent visual style
- [ ] Screenshots tell a story
- [ ] ASO keywords in captions

### Quantity
- [ ] Minimum 1 screenshot
- [ ] Maximum 10 screenshots
- [ ] Same count for iPhone and iPad (recommended)
```

---

## Quick Dimension Reference

```
iPhone 6.9" (REQUIRED):
├── Portrait:  1260 x 2736
└── Landscape: 2736 x 1260
    Display Type: APP_IPHONE_67 (not APP_IPHONE_69!)

iPad 13" (REQUIRED):
├── Portrait:  2064 x 2752
└── Landscape: 2752 x 2064
    Display Type: APP_IPAD_PRO_3GEN_129
```
