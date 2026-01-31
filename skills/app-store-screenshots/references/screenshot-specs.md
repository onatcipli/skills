# Screenshot Specifications Reference

Complete technical requirements for App Store screenshots (2025).

## Mandatory Sizes

As of 2025, Apple requires these two screenshot sizes:

| Device | Dimensions (Portrait) | Dimensions (Landscape) | Required |
|--------|----------------------|------------------------|----------|
| **iPhone 6.9"** | 1290 x 2796 or 1320 x 2868 | 2796 x 1290 or 2868 x 1320 | **Yes** |
| **iPad 13"** | 2064 x 2752 | 2752 x 2064 | **Yes** |

Apple automatically scales these to fit smaller devices.

---

## All iPhone Sizes

| Display | Dimensions (Portrait) | Dimensions (Landscape) | Notes |
|---------|----------------------|------------------------|-------|
| 6.9" | 1320 x 2868 | 2868 x 1320 | iPhone 16 Pro Max |
| 6.7" | 1290 x 2796 | 2796 x 1290 | iPhone 16 Plus, 15 Pro Max |
| 6.5" | 1284 x 2778 or 1242 x 2688 | 2778 x 1284 or 2688 x 1242 | iPhone 14 Plus, 13 Pro Max |
| 6.1" | 1179 x 2556 or 1170 x 2532 | 2556 x 1179 or 2532 x 1170 | iPhone 16, 15, 14 |
| 5.8" | 1125 x 2436 | 2436 x 1125 | iPhone X, XS, 11 Pro |
| 5.5" | 1242 x 2208 | 2208 x 1242 | iPhone 8 Plus (legacy) |
| 4.7" | 750 x 1334 | 1334 x 750 | iPhone SE (legacy) |

---

## All iPad Sizes

| Display | Dimensions (Portrait) | Dimensions (Landscape) | Notes |
|---------|----------------------|------------------------|-------|
| 13" | 2064 x 2752 | 2752 x 2064 | iPad Pro 13" (M4) |
| 12.9" | 2048 x 2732 | 2732 x 2048 | iPad Pro 12.9" (legacy) |
| 11" | 1668 x 2388 | 2388 x 1668 | iPad Pro 11", iPad Air |
| 10.9" | 1640 x 2360 | 2360 x 1640 | iPad (10th gen) |
| 10.5" | 1668 x 2224 | 2224 x 1668 | iPad Air 3, iPad Pro 10.5" |
| 9.7" | 1536 x 2048 | 2048 x 1536 | iPad (legacy) |

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
| iPhone 6.9" | 1290 x 2796 or 1320 x 2868 | 15-30 seconds |
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

## Common Mistakes to Avoid

| Mistake | Why It's a Problem |
|---------|-------------------|
| Text too small | Unreadable in search results |
| Content in Dynamic Island area | Gets obscured on newer devices |
| Wrong aspect ratio | Will be rejected |
| Transparency in PNG | Not allowed |
| Over 8 MB file size | Will be rejected |
| Less than 1 screenshot | Submission blocked |
| Competitor logos visible | Policy violation |
| Misleading content | Policy violation |

---

## Export Checklist

```markdown
## Pre-Upload Checklist

### Technical
- [ ] iPhone: 1290 x 2796 or 1320 x 2868
- [ ] iPad: 2064 x 2752
- [ ] Format: PNG or JPEG
- [ ] No transparency
- [ ] Under 8 MB each
- [ ] sRGB color space

### Content
- [ ] Text readable at thumbnail size
- [ ] No content in Dynamic Island zone
- [ ] No competitor names/logos
- [ ] Accurate representation of app
- [ ] Consistent visual style
- [ ] Screenshots tell a story

### Quantity
- [ ] Minimum 1 screenshot
- [ ] Maximum 10 screenshots
- [ ] Same count for iPhone and iPad (recommended)
```

---

## Quick Dimension Reference

```
iPhone 6.9" (REQUIRED):
├── Portrait:  1290 x 2796  or  1320 x 2868
└── Landscape: 2796 x 1290  or  2868 x 1320

iPad 13" (REQUIRED):
├── Portrait:  2064 x 2752
└── Landscape: 2752 x 2064
```
