# Analysis Prompts Reference

Structured approaches for analyzing App Store reviews. Use these patterns
to extract actionable insights from review data.

## Table of Contents

- [Review Classification](#review-classification)
- [Theme Extraction](#theme-extraction)
- [Sentiment Scoring](#sentiment-scoring)
- [Competitor Report Format](#competitor-report)
- [Trend Detection](#trend-detection)
- [Version Impact Analysis](#version-impact-analysis)

---

## Review Classification

Classify each review into exactly one primary category and zero or more
secondary categories.

### Categories

| Category | Signal Words / Patterns |
|----------|------------------------|
| `bug` | crash, freeze, stuck, error, broken, doesn't work, won't load, glitch, bug |
| `feature_request` | wish, would be nice, please add, should have, missing, need, want |
| `ux_complaint` | confusing, hard to find, unintuitive, complicated, cluttered, ugly |
| `performance` | slow, laggy, battery, storage, memory, takes forever, heats up |
| `pricing` | expensive, subscription, price, cost, pay, free, refund, worth |
| `praise` | love, amazing, great, best, excellent, perfect, awesome, fantastic |
| `support` | help, support, contact, response, customer service, no reply |
| `data_loss` | lost, deleted, disappeared, gone, missing data, reset |
| `auth_issue` | login, sign in, password, account, locked out, verification |
| `update_regression` | after update, since update, new version broke, worked before |

### Classification Output Format

```json
{
  "review_id": "...",
  "primary_category": "bug",
  "secondary_categories": ["update_regression"],
  "confidence": 0.92,
  "key_phrases": ["app crashes when I open settings", "started after last update"]
}
```

---

## Theme Extraction

Group classified reviews into higher-level themes to identify patterns.

### Process

1. Collect all classified reviews
2. Group by `primary_category`
3. Within each category, cluster by semantic similarity of `key_phrases`
4. Name each cluster with a descriptive theme label
5. Rank themes by: frequency (primary), average rating (secondary)

### Output Format

```json
{
  "theme": "Settings page crash on iOS 18",
  "category": "bug",
  "review_count": 47,
  "avg_rating": 1.3,
  "date_range": "2025-02-01 to 2025-03-15",
  "trend": "increasing",
  "representative_quotes": [
    "Every time I open settings the app crashes instantly",
    "Settings crash started with version 3.2.0"
  ],
  "affected_versions": ["3.2.0", "3.2.1"],
  "territories": ["USA", "GBR", "CAN"]
}
```

---

## Sentiment Scoring

Assign a sentiment score to each review on a scale of -1.0 to +1.0.

### Scoring Rules

| Rating | Base Score | Adjust by Body |
|--------|-----------|----------------|
| 5 | +0.9 | ±0.1 based on text sentiment |
| 4 | +0.6 | ±0.2 based on text sentiment |
| 3 | +0.0 | ±0.4 based on text sentiment |
| 2 | -0.6 | ±0.2 based on text sentiment |
| 1 | -0.9 | ±0.1 based on text sentiment |

### Text Adjustments

- **Upgrade signal:** Body is more positive than rating suggests (e.g., accidental low rating, rating about different app)
- **Downgrade signal:** Body is more negative than rating suggests (e.g., sarcasm, "5 stars so devs see this complaint")
- **Mixed signal:** Both positive and negative content → lean toward body sentiment

### Aggregate Sentiment

```
App Sentiment Score = mean(all review scores)
Period Sentiment    = mean(reviews in time window)
Category Sentiment  = mean(reviews in category)
```

---

## Competitor Report

### Structure

```markdown
# Competitive Analysis: {Your App} vs {Competitor 1}, {Competitor 2}
## Generated: {date}

### Rating Overview

| App | Avg Rating | Total Reviews | Review Velocity |
|-----|-----------|---------------|-----------------|
| {Your App} | {x.x} | {n} | {n}/month |
| {Comp 1}   | {x.x} | {n} | {n}/month |
| {Comp 2}   | {x.x} | {n} | {n}/month |

### Rating Distribution

| App | ★5 | ★4 | ★3 | ★2 | ★1 |
|-----|----|----|----|----|-----|
| ... | % | % | % | % | % |

### Sentiment Comparison

| App | Positive | Neutral | Negative | Score |
|-----|----------|---------|----------|-------|
| ... | % | % | % | ±x.xx |

### Your Strengths (praised by your users, criticized in competitors)

1. **{Feature/quality}** — mentioned {n} times positively by your users,
   mentioned {n} times negatively by {Competitor} users
   - Your users say: "{quote}"
   - Their users say: "{quote}"

### Your Gaps (praised in competitors, criticized in your reviews)

1. **{Feature/quality}** — mentioned {n} times positively by {Competitor} users,
   mentioned {n} times negatively by your users
   - Their users say: "{quote}"
   - Your users say: "{quote}"

### Shared Pain Points

1. **{Issue}** — negative across all apps ({n} total mentions)

### Feature Demand Across Market

1. **{Feature}** — requested by users of {n} apps, {total mentions} total mentions

### Strategic Recommendations

1. **Quick win:** {low-effort improvement based on gap analysis}
2. **Differentiate:** {leverage your strengths}
3. **Address:** {fix shared pain points before competitors do}
4. **Build:** {high-demand features not yet in any app}
```

---

## Trend Detection

Identify how themes evolve over time.

### Method

1. Divide reviews into time buckets (weekly or monthly)
2. For each bucket, count reviews per theme
3. Compute moving average over 3 periods
4. Flag trends:
   - **Rising:** current period > 1.5x moving average
   - **Falling:** current period < 0.5x moving average
   - **Stable:** within 0.5x–1.5x of moving average
   - **New:** theme absent in previous 3 periods, now appearing

### Output Format

```json
{
  "theme": "Login failures",
  "trend": "rising",
  "current_period_count": 23,
  "moving_average": 8,
  "change_pct": "+188%",
  "first_seen": "2025-01-15",
  "peak": "2025-03-01"
}
```

---

## Version Impact Analysis

Measure how a new app version affects reviews.

### Method

1. Identify release date of the version
2. Collect reviews tagged with the new version (or dated after release)
3. Compare against the previous version's reviews:
   - Average rating change
   - Sentiment score change
   - New themes that appeared
   - Themes that disappeared or decreased
4. Compute a "version health score": `new_avg_rating - previous_avg_rating`

### Output Format

```markdown
### Version {X.Y.Z} Impact Report

- **Released:** {date}
- **Reviews analyzed:** {n} (vs {n} for previous version)
- **Avg rating:** {x.x} (previous: {x.x}) → {↑/↓} {delta}
- **Sentiment:** {score} (previous: {score}) → {↑/↓} {delta}

#### New Issues Introduced
- {theme} — {n} mentions

#### Issues Resolved
- {theme} — dropped from {n} to {n} mentions

#### Unchanged Issues
- {theme} — {n} mentions (was {n})
```
