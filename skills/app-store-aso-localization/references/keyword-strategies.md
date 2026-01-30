# Keyword Strategies Reference

Advanced techniques for keyword research, optimization, and competitive analysis.

## Table of Contents

- [Keyword Research Methods](#keyword-research-methods)
- [Keyword Scoring Framework](#keyword-scoring-framework)
- [Distribution Strategy](#distribution-strategy)
- [Competitive Keyword Analysis](#competitive-keyword-analysis)
- [Long-Tail Keyword Strategy](#long-tail-keyword-strategy)
- [Seasonal & Trend Keywords](#seasonal--trend-keywords)
- [Measurement & Iteration](#measurement--iteration)

---

## Keyword Research Methods

### 1. Seed Keyword Collection

Start by gathering initial keywords from multiple sources:

#### A. Your App's Core Features

```
List every feature and benefit:
- Photo editing → "photo", "editor", "editing"
- Filters → "filters", "effects", "presets"
- Collage → "collage", "layout", "grid"
- AI enhancement → "ai", "enhance", "automatic"
```

#### B. User Language Mining

Extract keywords from reviews:

```python
def extract_review_keywords(reviews):
    """
    Find frequently mentioned terms in user reviews.
    """
    all_text = " ".join([r["body"] for r in reviews])
    words = tokenize(all_text.lower())
    words = filter_stop_words(words)

    # Count frequency
    frequency = Counter(words)

    # Return top 50 most common
    return frequency.most_common(50)
```

Common patterns in reviews:
- "I love the **filters**" → "filters"
- "Great for **selfies**" → "selfies"
- "Easy **background removal**" → "background", "removal"

#### C. App Store Search Suggestions

Apple's autocomplete reveals popular searches:

```
Type "photo" → Suggestions:
- photo editor
- photo collage
- photo to video
- photo vault
- photo enhancer

Extract: editor, collage, video, vault, enhancer
```

#### D. Competitor Keyword Extraction

Analyze competitor titles, subtitles, and descriptions:

```python
def extract_competitor_keywords(competitor_ids, country="us"):
    keywords = set()

    for app_id in competitor_ids:
        data = itunes_lookup(app_id, country)

        # From title (remove brand)
        title_words = data["trackName"].lower().split()
        keywords.update(title_words)

        # From description (top words)
        desc_keywords = extract_top_words(data["description"], n=30)
        keywords.update(desc_keywords)

    return filter_stop_words(keywords)
```

### 2. Keyword Expansion

Expand seed keywords using these techniques:

#### Synonyms

```
photo → picture, image, pic, snapshot
edit → enhance, adjust, modify, retouch
filter → effect, preset, style, look
```

#### Variations

```
Singular/Plural: filter/filters, effect/effects
Verb/Noun: edit/editing, crop/cropping
Misspellings: collage/colage (if common)
```

#### Related Terms

```
"photo editor" related terms:
- camera, selfie, portrait, landscape
- brightness, contrast, saturation
- crop, resize, rotate
- blur, sharpen, HDR
```

#### Compound Breakdowns

```
"photocollage" → "photo", "collage"
"selfieditor" → "selfie", "editor"
```

---

## Keyword Scoring Framework

### Scoring Formula

```python
def calculate_keyword_score(keyword, metrics):
    """
    Score a keyword on a 0-100 scale.

    Metrics:
    - volume: Search volume (1-100 normalized)
    - difficulty: Competition level (1-100)
    - relevance: How well it fits your app (1-10)
    """
    volume = metrics["volume"]
    difficulty = metrics["difficulty"]
    relevance = metrics["relevance"]

    # Opportunity = high volume, low difficulty
    opportunity = volume * (1 - difficulty / 100)

    # Weight by relevance (0.1 to 1.0)
    relevance_weight = relevance / 10

    score = opportunity * relevance_weight

    return round(score, 2)
```

### Score Interpretation

| Score | Priority | Action |
|-------|----------|--------|
| 80-100 | Highest | Must include in Title/Subtitle |
| 60-79 | High | Include in Keywords field |
| 40-59 | Medium | Include if space permits |
| 20-39 | Low | Consider for specific locales |
| 0-19 | Skip | Not worth pursuing |

### Estimating Metrics Without Tools

#### Volume Estimation

```
High volume indicators:
- Appears in App Store suggestions quickly
- Many competitors use this keyword
- Generic, common term

Low volume indicators:
- Doesn't appear in suggestions
- Very specific or niche term
- Misspelling or uncommon variant
```

#### Difficulty Estimation

```python
def estimate_difficulty(keyword):
    """
    Estimate keyword difficulty based on top-ranking apps.
    """
    top_apps = search_app_store(keyword, limit=10)

    avg_rating = mean([app["userRatingCount"] for app in top_apps])
    avg_stars = mean([app["averageUserRating"] for app in top_apps])

    # Higher ratings = harder to compete
    if avg_rating > 100000 and avg_stars > 4.5:
        return 90  # Very difficult
    elif avg_rating > 50000 and avg_stars > 4.0:
        return 70  # Difficult
    elif avg_rating > 10000:
        return 50  # Moderate
    elif avg_rating > 1000:
        return 30  # Easy
    else:
        return 10  # Very easy
```

---

## Distribution Strategy

### The Golden Rule

**Never repeat keywords across Title, Subtitle, and Keywords field.**

Apple indexes each word once. Duplicating wastes valuable character space.

### Optimal Distribution

```
┌─────────────────────────────────────────────────────────────┐
│ TITLE (30 chars) — Highest Weight                           │
│ • Brand name                                                │
│ • 1-2 primary keywords (highest score)                      │
├─────────────────────────────────────────────────────────────┤
│ SUBTITLE (30 chars) — Medium Weight                         │
│ • Unique value proposition                                  │
│ • 2-3 secondary keywords (not in title)                     │
├─────────────────────────────────────────────────────────────┤
│ KEYWORDS (100 chars) — Standard Weight                      │
│ • All remaining keywords                                    │
│ • No words from title or subtitle                           │
│ • Prioritize by score                                       │
└─────────────────────────────────────────────────────────────┘
```

### Example Distribution

```
App: Photo editing app

SCORED KEYWORDS:
1. photo (95) — PRIMARY
2. editor (90) — PRIMARY
3. filter (75) — SECONDARY
4. enhance (70) — SECONDARY
5. camera (65) — TERTIARY
6. retouch (60) — TERTIARY
7. collage (55) — TERTIARY
...

DISTRIBUTION:
Title: "PhotoMagic - Picture Editor"
→ Uses: photomagic (brand), picture (=photo synonym), editor

Subtitle: "AI Filters & Enhance Tools"
→ Uses: ai, filters, enhance, tools

Keywords: "camera,retouch,collage,crop,adjust,blur,portrait,selfie,beauty,frame,effects,hdr,brightness,contrast"
→ Uses: All remaining (no duplicates from above)

TOTAL INDEXED: 20+ unique keywords
```

### Cross-Locale Distribution

For markets with secondary locale indexing:

```
US MARKET (indexes en-US + es-MX):

en-US Title: "PhotoMagic - Picture Editor"
en-US Subtitle: "AI Filters & Enhance Tools"
en-US Keywords: "camera,retouch,collage,crop,adjust,blur,portrait"

es-MX Keywords: "fotos,editar,camara,filtros,efectos,selfie,belleza,marco"

RESULT: 25+ unique keywords indexed for US users
```

---

## Competitive Keyword Analysis

### Step 1: Identify Top Competitors

```python
def find_competitors(your_keywords, limit=10):
    """Find apps ranking for your target keywords."""
    competitors = {}

    for keyword in your_keywords[:5]:  # Top 5 keywords
        results = search_app_store(keyword, limit=20)

        for app in results:
            app_id = app["trackId"]
            if app_id not in competitors:
                competitors[app_id] = {
                    "name": app["trackName"],
                    "ratings": app["userRatingCount"],
                    "keywords_found": []
                }
            competitors[app_id]["keywords_found"].append(keyword)

    # Sort by relevance (number of shared keywords)
    return sorted(
        competitors.values(),
        key=lambda x: len(x["keywords_found"]),
        reverse=True
    )[:limit]
```

### Step 2: Keyword Gap Analysis

```python
def analyze_keyword_gaps(your_keywords, competitor_keywords):
    """
    Find keywords competitors use that you don't.
    """
    your_set = set(your_keywords)
    comp_set = set(competitor_keywords)

    # Keywords they have that you don't
    gaps = comp_set - your_set

    # Keywords you have that they don't (your advantages)
    unique = your_set - comp_set

    # Shared keywords (high competition)
    shared = your_set & comp_set

    return {
        "gaps": gaps,           # Opportunities
        "unique": unique,       # Your strengths
        "shared": shared        # High competition
    }
```

### Step 3: Opportunity Scoring

```python
def score_keyword_opportunity(keyword, competitor_usage):
    """
    Score opportunity based on competitor usage.

    - Used by many competitors = validated, but competitive
    - Used by few competitors = potential untapped opportunity
    """
    usage_count = competitor_usage[keyword]
    total_competitors = len(competitor_usage)

    if usage_count == 0:
        return "Unknown - needs validation"
    elif usage_count >= total_competitors * 0.7:
        return "High competition - validated keyword"
    elif usage_count >= total_competitors * 0.3:
        return "Medium competition - good opportunity"
    else:
        return "Low competition - potential opportunity"
```

---

## Long-Tail Keyword Strategy

### What Are Long-Tail Keywords?

```
Head keyword: "photo" (high volume, high competition)
Mid-tail: "photo editor" (medium volume, medium competition)
Long-tail: "photo editor for instagram" (low volume, low competition)
```

### Why Long-Tail Matters

1. **Less competition** — Easier to rank
2. **Higher intent** — More specific = closer to download
3. **Compound effect** — Many long-tails add up

### Long-Tail Techniques

#### 1. Feature-Specific

```
Generic: "photo editor"
Long-tail: "background remover", "face retouch", "color correction"
```

#### 2. Use-Case Specific

```
Generic: "camera"
Long-tail: "selfie camera", "food photography", "document scanner"
```

#### 3. Platform-Specific

```
Generic: "video editor"
Long-tail: "tiktok editor", "reels maker", "youtube shorts"
```

#### 4. Audience-Specific

```
Generic: "photo app"
Long-tail: "photo app for beginners", "professional photo editor"
```

### Long-Tail Distribution

Place long-tail keywords in Keywords field (they're too long for Title/Subtitle):

```
Keywords: "background,remover,face,retouch,color,correction,selfie,food,document,tiktok,reels"

Apple combines these into searchable phrases:
- "background remover"
- "face retouch"
- "selfie camera" (if "camera" is in title)
```

---

## Seasonal & Trend Keywords

### Seasonal Patterns

| Season | Keywords | Apps |
|--------|----------|------|
| New Year (Jan) | "goals", "tracker", "resolution", "fitness" | Habit, Fitness |
| Valentine's (Feb) | "love", "romance", "dating", "couple" | Dating, Photo |
| Summer (Jun-Aug) | "vacation", "travel", "beach", "outdoor" | Travel, Camera |
| Back to School (Aug-Sep) | "student", "study", "notes", "college" | Education |
| Halloween (Oct) | "scary", "costume", "spooky", "horror" | Photo, Games |
| Holiday (Nov-Dec) | "gift", "christmas", "holiday", "sale" | Shopping, Photo |

### Trend Monitoring

```python
def identify_trending_keywords(category, timeframe="30d"):
    """
    Identify rising keywords in your category.

    Sources:
    - App Store trending searches
    - Social media trends
    - News/current events
    """
    sources = [
        get_app_store_trends(category),
        get_twitter_trends(category),
        get_google_trends(category)
    ]

    all_trends = merge_and_rank(sources)
    return filter_relevant(all_trends, your_app)
```

### Responding to Trends

1. **Update Promotional Text** — Instant, no review needed
2. **Update Keywords** — Requires new version submission
3. **Update Subtitle** — Requires new version submission

```
Example: AI photo editing becomes trendy

Promotional Text: "Now with AI-powered editing! Try our new smart enhance feature."
Keywords: Add "ai,smart,automatic,intelligent"
Subtitle: Change to "AI Photo Editor & Filters"
```

---

## Measurement & Iteration

### Track These Metrics

| Metric | How to Measure | Goal |
|--------|----------------|------|
| Keyword rankings | ASO tool or manual search | Top 10 for target keywords |
| Organic downloads | App Store Connect analytics | Increasing trend |
| Conversion rate | Impressions → Downloads | >5% for good keywords |
| Keyword discovery | New keywords driving traffic | Expand successful terms |

### A/B Testing Keywords

Since you can't A/B test keywords directly, use:

#### 1. Sequential Testing

```
Week 1-2: Keyword set A
Week 3-4: Keyword set B
Compare: Downloads, rankings, conversion
```

#### 2. Locale Testing

```
en-US: Primary keywords (control)
en-AU: Test keywords (experiment)
Compare: Which keywords perform better?
```

### Iteration Cycle

```
┌─────────────────────────────────────────────────────────────┐
│ 1. RESEARCH (Week 1)                                        │
│    • Analyze current performance                            │
│    • Research new keywords                                  │
│    • Study competitor changes                               │
├─────────────────────────────────────────────────────────────┤
│ 2. OPTIMIZE (Week 2)                                        │
│    • Update keywords in new version                         │
│    • Optimize Title/Subtitle if needed                      │
│    • Submit for review                                      │
├─────────────────────────────────────────────────────────────┤
│ 3. MONITOR (Week 3-4)                                       │
│    • Track ranking changes                                  │
│    • Monitor download trends                                │
│    • Note what's working/not working                        │
├─────────────────────────────────────────────────────────────┤
│ 4. ANALYZE (Week 5)                                         │
│    • Compare before/after metrics                           │
│    • Identify winners and losers                            │
│    • Document learnings                                     │
├─────────────────────────────────────────────────────────────┤
│ 5. REPEAT                                                   │
│    • Apply learnings to next cycle                          │
│    • Test new hypotheses                                    │
│    • Continuous improvement                                 │
└─────────────────────────────────────────────────────────────┘
```

---

## Quick Reference: Keyword Checklist

Before finalizing keywords:

```
Research:
[ ] Analyzed top 10 competitors
[ ] Extracted keywords from user reviews
[ ] Checked App Store search suggestions
[ ] Identified long-tail opportunities
[ ] Researched trending terms

Scoring:
[ ] Estimated volume for each keyword
[ ] Assessed difficulty for each keyword
[ ] Rated relevance for each keyword
[ ] Calculated overall scores
[ ] Prioritized by score

Distribution:
[ ] Primary keywords in Title
[ ] Secondary keywords in Subtitle
[ ] Remaining keywords in Keywords field
[ ] No duplicates across fields
[ ] Cross-localization strategy applied

Validation:
[ ] All character limits respected
[ ] No stop words included
[ ] No competitor names
[ ] Comma-separated, no spaces
[ ] Localized (not just translated)
```
