# ASO Best Practices Reference

Comprehensive guidelines for App Store Optimization on iOS.

## Table of Contents

- [Character Limits](#character-limits)
- [Keyword Rules](#keyword-rules)
- [Title Optimization](#title-optimization)
- [Subtitle Optimization](#subtitle-optimization)
- [Keywords Field](#keywords-field)
- [Description Best Practices](#description-best-practices)
- [Common Mistakes](#common-mistakes)
- [Algorithm Factors (2025)](#algorithm-factors-2025)

---

## Character Limits

| Field | Limit | Indexed | Editable Without Review |
|-------|-------|---------|------------------------|
| App Name | 30 characters | Yes | No |
| Subtitle | 30 characters | Yes | No |
| Keywords | 100 characters | Yes | No |
| Promotional Text | 170 characters | No | Yes |
| Description | 4,000 characters | No | No |
| What's New | 4,000 characters | No | No |

### Utilization Targets

| Utilization | Status | Action |
|-------------|--------|--------|
| 95-100% | Excellent | Maintain |
| 85-94% | Good | Minor optimization |
| 70-84% | Underutilized | Add more keywords |
| <70% | Poor | Major optimization needed |

---

## Keyword Rules

### Do's

1. **Use all 100 characters** — Every unused character is wasted opportunity
2. **Comma-separate without spaces** — `keyword1,keyword2,keyword3`
3. **Single words only** — Break phrases: "photo editor" → `photo,editor`
4. **Include variations** — Singular/plural, common misspellings
5. **Research local terms** — Don't just translate, localize
6. **Update regularly** — Refresh keywords with each update

### Don'ts

1. **No duplicates** — Words in Title/Subtitle are already indexed
2. **No spaces after commas** — Wastes characters
3. **No stop words** — "the", "and", "a", "an", "for", "app"
4. **No competitor names** — Violates guidelines, risks rejection
5. **No category names** — Already associated with your app
6. **No repeated words** — Across any locale in the same field

### Stop Words to Avoid

```
a, an, and, are, as, at, be, by, for, from, has, he, in, is, it,
its, of, on, or, that, the, to, was, were, will, with, app, apps,
application, free, new, best
```

---

## Title Optimization

### Formula

```
[Brand Name] - [Primary Keyword] [Secondary Keyword]
```

Or if brand is unknown:

```
[Primary Keyword] [Secondary Keyword] - [Brand]
```

### Examples

| Good | Why |
|------|-----|
| "Calm - Sleep & Meditation" | Brand + 2 high-value keywords |
| "Duolingo - Language Lessons" | Clear brand + category keyword |
| "Scanner Pro: PDF Scanner App" | Function-first for utility apps |

| Bad | Why |
|-----|-----|
| "My Amazing App!!!" | No keywords, excessive punctuation |
| "Photo Editor Camera Filter Collage Beauty" | Keyword stuffing |
| "The Best Photo Editing App" | Stop words, generic claims |

### Title Weight

- Title keywords have **highest ranking weight**
- Place most important keyword after brand name
- Don't waste characters on "app" or "the"

---

## Subtitle Optimization

### Formula

```
[Unique Value Proposition] + [Keyword 1] & [Keyword 2]
```

### Examples

| Good | Why |
|------|-----|
| "AI Photo Enhancer & Filters" | Clear value + 2 keywords |
| "Budget Tracker & Bill Reminder" | Features as keywords |
| "Learn Spanish with AI Tutors" | Benefit + keyword |

| Bad | Why |
|-----|-----|
| "The #1 App for Photos" | Unverifiable claim, generic |
| "Photo Editor" | Duplicate of title, wastes space |
| "Download Now - It's Free!" | No keywords, spammy |

### Subtitle Rules

- **Never duplicate title keywords** — Already indexed
- **Focus on secondary features** — What title doesn't cover
- **Include a benefit** — Why should users download?

---

## Keywords Field

### Optimization Checklist

```
[ ] All 100 characters used (or close)
[ ] No spaces after commas
[ ] No words from title included
[ ] No words from subtitle included
[ ] No stop words
[ ] No competitor brand names
[ ] Includes singular/plural variations
[ ] Includes common misspellings (if relevant)
[ ] Localized (not just translated) for each market
```

### Keyword Prioritization

Order keywords by:

1. **Relevance** — Must describe your app accurately
2. **Volume** — Higher search volume = more opportunity
3. **Difficulty** — Lower competition = easier to rank

```
High relevance + High volume + Low difficulty = Perfect keyword
High relevance + Low volume + Low difficulty = Niche opportunity
High relevance + High volume + High difficulty = Long-term goal
```

### Example Keyword Distribution

```
Title (30 chars):
"PhotoMagic - Picture Editor"
→ indexed: photomagic, picture, editor

Subtitle (30 chars):
"AI Filters & Background Erase"
→ indexed: ai, filters, background, erase

Keywords (100 chars):
"camera,enhance,retouch,beauty,portrait,collage,crop,adjust,brightness,contrast,blur,frame,effects,selfie"
→ indexed: all above (14 additional keywords)

Total unique indexed terms: 21
```

---

## Description Best Practices

### iOS Description is NOT Indexed

Unlike Google Play, iOS descriptions don't affect search ranking.
Focus on **conversion**, not keywords.

### Structure

```markdown
[Hook - First 1-2 sentences visible without "more"]

[Key Features - Bullet points]
• Feature 1 — Benefit
• Feature 2 — Benefit
• Feature 3 — Benefit

[Social Proof]
"Quote from press or user review"
— Source

[Subscription Info - If applicable]
Pricing, free trial, terms

[Call to Action]
Download now and start...
```

### First 170 Characters

**Critical** — This is all users see before tapping "more".

```
✓ Good: "Transform your photos with AI-powered filters. Remove backgrounds
in one tap. Join 10M+ users who love PhotoMagic."

✗ Bad: "PhotoMagic is an application developed by ABC Corp that allows
users to edit their photographs using various tools and filters..."
```

### Formatting Tips

- Use bullet points (• or -)
- Short paragraphs (2-3 lines max)
- Emojis sparingly (if appropriate for brand)
- Highlight key features
- Include social proof

---

## Common Mistakes

### 1. Keyword Duplication

```
❌ Title: "Photo Editor Pro"
❌ Keywords: "photo,editor,pro,camera..."

✓ Title: "Photo Editor Pro"
✓ Keywords: "camera,filters,retouch,beauty..."
(No duplication — Title words already indexed)
```

### 2. Space After Commas

```
❌ "photo, editor, camera, filters"
   (Wastes 3 characters on spaces)

✓ "photo,editor,camera,filters"
   (3 extra characters for more keywords)
```

### 3. Keyword Stuffing in Title

```
❌ "Photo Editor Camera Filter Collage Maker"
   (Unreadable, looks spammy, may be rejected)

✓ "Snapseed - Photo Editor"
   (Clean, brandable, professional)
```

### 4. Stop Words in Keywords

```
❌ "best,photo,app,for,editing,the,photos"
   (Wasted: "best", "app", "for", "the")

✓ "portrait,landscape,enhance,sharpen,hdr"
   (All valuable keywords)
```

### 5. Ignoring Localization

```
❌ Same English keywords for Germany:
   "photo,editor,camera"

✓ Localized German keywords:
   "bildbearbeitung,fotos,bilder,kamera"
```

### 6. Competitor Names in Keywords

```
❌ "photoshop,lightroom,snapseed,vsco"
   (Violates guidelines — risk of rejection)

✓ "professional,filters,presets,editing"
   (Generic terms that capture similar searches)
```

---

## Algorithm Factors (2025)

### Primary Ranking Factors

| Factor | Weight | Notes |
|--------|--------|-------|
| **Keyword relevance** | High | Title > Subtitle > Keywords |
| **Download velocity** | High | Recent downloads matter more |
| **Ratings & reviews** | High | Both quantity and quality |
| **Retention** | Medium-High | Users who keep and use the app |
| **Update frequency** | Medium | Regular updates signal active development |
| **Engagement** | Medium | Session length, frequency |

### Secondary Factors

| Factor | Weight | Notes |
|--------|--------|-------|
| **App stability** | Medium | Crash rate affects ranking |
| **Conversion rate** | Medium | Impression-to-download ratio |
| **Keyword density** | Low | Natural usage, not stuffing |
| **Backlinks** | Low | External links to App Store page |

### What Changed in 2024-2025

1. **Quality matters more** — Stability, retention weighted higher
2. **Engagement signals** — Session time, return visits
3. **Contextual relevance** — Apple better understands intent
4. **Personalization** — Results vary by user history
5. **Fraud detection** — Fake reviews/downloads penalized

### Optimization Priority

1. Focus on **conversion** first (great screenshots, description)
2. Then **keyword optimization** (title, subtitle, keywords)
3. Then **retention** (great app experience)
4. Then **reviews** (prompt happy users to review)

---

## Quick Reference Card

```
┌─────────────────────────────────────────────────────────────┐
│                    ASO QUICK REFERENCE                       │
├─────────────────────────────────────────────────────────────┤
│ TITLE (30 chars)                                            │
│ • Brand + Primary Keyword                                   │
│ • Highest ranking weight                                    │
│ • No stop words                                             │
├─────────────────────────────────────────────────────────────┤
│ SUBTITLE (30 chars)                                         │
│ • USP + Secondary Keywords                                  │
│ • Don't duplicate title                                     │
│ • Medium ranking weight                                     │
├─────────────────────────────────────────────────────────────┤
│ KEYWORDS (100 chars)                                        │
│ • Comma-separated, NO SPACES                                │
│ • No title/subtitle words                                   │
│ • No stop words or competitors                              │
├─────────────────────────────────────────────────────────────┤
│ DESCRIPTION (4000 chars)                                    │
│ • NOT indexed on iOS                                        │
│ • Focus on conversion                                       │
│ • First 170 chars critical                                  │
├─────────────────────────────────────────────────────────────┤
│ PROMOTIONAL TEXT (170 chars)                                │
│ • Can update without new version                            │
│ • Use for timely messaging                                  │
│ • NOT indexed                                               │
└─────────────────────────────────────────────────────────────┘
```
