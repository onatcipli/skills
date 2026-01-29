# Premium Index Methodology

How to calculate and interpret the App Store Premium Index.

## Overview

The Premium Index measures how app prices compare across territories, normalized
to a baseline (typically USA). It combines actual market prices with purchasing
power parity (PPP) data to identify pricing optimization opportunities.

---

## Core Concepts

### Price Index

The ratio of a territory's price to the baseline price:

```
price_index = local_price_in_usd / baseline_price_usd
```

**Example:**
- US price: $9.99
- UK price: £7.99 (≈ $10.50 at current exchange rate)
- Price Index: 10.50 / 9.99 = 1.05

### PPP Factor

The purchasing power parity factor indicates relative local purchasing power:

```
ppp_factor = local_ppp / baseline_ppp
```

A PPP factor of 0.5 means local purchasing power is half of the baseline.

### Index Deviation

How far actual pricing deviates from PPP-optimal pricing:

```
deviation = (price_index - ppp_factor) / ppp_factor × 100
```

- **Positive deviation:** Price higher than PPP suggests (may hurt conversions)
- **Negative deviation:** Price lower than PPP suggests (may leave revenue on table)

---

## Calculation Steps

### Step 1: Collect Reference Prices

Fetch prices from 3-5 reference apps across all territories:

```python
reference_apps = [
    {"name": "Spotify", "id": 324684580},
    {"name": "Netflix", "id": 363590051},
    {"name": "YouTube Premium", "id": 544007664}
]

prices = {}
for app in reference_apps:
    for territory in territories:
        price = fetch_itunes_price(app["id"], territory)
        prices[(app["id"], territory)] = price
```

### Step 2: Convert to USD

Use current exchange rates to normalize all prices to USD:

```python
def to_usd(price, currency):
    exchange_rates = get_current_rates()  # From forex API
    return price / exchange_rates[currency]
```

### Step 3: Calculate Per-App Index

For each reference app:

```python
def calculate_app_index(app_id, prices, baseline="USA"):
    baseline_price = prices[(app_id, baseline)]

    index = {}
    for territory in territories:
        local_price_usd = to_usd(prices[(app_id, territory)])
        index[territory] = local_price_usd / baseline_price

    return index
```

### Step 4: Calculate Average Premium Index

Average across all reference apps:

```python
def calculate_premium_index(all_indices):
    premium_index = {}
    for territory in territories:
        values = [idx[territory] for idx in all_indices.values()]
        premium_index[territory] = sum(values) / len(values)

    return premium_index
```

### Step 5: Compare to PPP

```python
def calculate_alignment(premium_index, ppp_factors):
    alignment = {}
    for territory in territories:
        index = premium_index[territory]
        ppp = ppp_factors[territory]
        deviation = (index - ppp) / ppp * 100

        alignment[territory] = {
            "premium_index": index,
            "ppp_factor": ppp,
            "deviation": deviation,
            "status": classify_deviation(deviation)
        }

    return alignment

def classify_deviation(deviation):
    if abs(deviation) <= 20:
        return "Optimized"
    elif deviation > 50:
        return "Significantly High"
    elif deviation > 20:
        return "High"
    elif deviation < -50:
        return "Significantly Low"
    else:
        return "Low"
```

---

## Index Interpretation

### Status Classifications

| Status | Deviation Range | Meaning | Action |
|--------|-----------------|---------|--------|
| Optimized | ±20% | Good alignment with PPP | Maintain |
| High | +20% to +50% | Above PPP expectations | Consider reducing |
| Significantly High | >+50% | Well above PPP | Likely hurting conversions |
| Low | -20% to -50% | Below PPP expectations | May increase |
| Significantly Low | <-50% | Well below PPP | Leaving revenue on table |

### Example Analysis

```markdown
| Territory | Price Index | PPP Factor | Deviation | Status |
|-----------|-------------|------------|-----------|--------|
| USA       | 1.00        | 1.00       | 0%        | Baseline |
| Germany   | 0.95        | 0.89       | +7%       | Optimized |
| UK        | 1.10        | 0.82       | +34%      | High |
| India     | 0.15        | 0.16       | -6%       | Optimized |
| Brazil    | 0.45        | 0.34       | +32%      | High |
| Turkey    | 0.18        | 0.36       | -50%      | Significantly Low |
```

**Insights:**
- **UK:** Price 34% higher than PPP — may be hurting conversions
- **Brazil:** Price 32% higher than PPP — emerging market, price-sensitive
- **Turkey:** Price 50% below PPP — could potentially raise prices

---

## Generating Recommendations

### Price Recommendation Formula

```python
def recommend_price(base_price, territory, ppp_factor, strategy="balanced"):
    weights = {
        "conservative": 0.3,
        "balanced": 0.6,
        "aggressive": 1.0
    }
    weight = weights[strategy]

    # Calculate PPP-adjusted target
    adjustment = (1 - ppp_factor) * weight
    target_price = base_price * (1 - adjustment)

    # Apply floor (don't go below viable minimum)
    min_price = get_minimum_price(territory)
    target_price = max(target_price, min_price)

    return target_price
```

### Strategy Examples

For a $9.99 base price in India (PPP: 0.16):

| Strategy | Calculation | Target Price |
|----------|-------------|--------------|
| Conservative | 9.99 × (1 - 0.84 × 0.3) = 9.99 × 0.75 | $7.49 |
| Balanced | 9.99 × (1 - 0.84 × 0.6) = 9.99 × 0.50 | $4.99 |
| Aggressive | 9.99 × (1 - 0.84 × 1.0) = 9.99 × 0.16 | $1.59 |

---

## Matching to Apple Price Points

Apple has ~800 fixed price points. Match recommendations to nearest available tier:

```python
def find_closest_price_point(target_price, available_points):
    """Find the Apple price point closest to target."""
    closest = min(
        available_points,
        key=lambda p: abs(p["customer_price"] - target_price)
    )
    return closest

# Example: Target $4.99 in India
# Available points: ₹159 ($1.91), ₹299 ($3.59), ₹449 ($5.39), ₹649 ($7.79)
# Closest: ₹449 ($5.39)
```

---

## Weighting Reference Apps

Not all reference apps should be weighted equally:

| App Type | Weight | Rationale |
|----------|--------|-----------|
| Direct competitor | 1.5x | Most relevant benchmark |
| Same category | 1.2x | Similar user expectations |
| General subscription | 1.0x | Baseline reference |
| Premium/luxury | 0.8x | Higher price tolerance |
| Budget/utility | 0.8x | Lower price expectations |

```python
def weighted_premium_index(indices, weights):
    weighted_index = {}
    for territory in territories:
        total_weight = sum(weights.values())
        weighted_sum = sum(
            indices[app][territory] * weights[app]
            for app in indices
        )
        weighted_index[territory] = weighted_sum / total_weight

    return weighted_index
```

---

## Temporal Considerations

### Exchange Rate Volatility

For volatile currencies, use trailing averages:

```python
def stable_exchange_rate(currency, days=30):
    """Use 30-day average to smooth volatility."""
    rates = fetch_historical_rates(currency, days)
    return sum(rates) / len(rates)
```

### Seasonal Adjustments

Some markets have seasonal price sensitivity:
- **Q4 (Holiday):** Higher willingness to pay in US/EU
- **Diwali (India):** Higher spending in October/November
- **Chinese New Year:** Peak in China

---

## Validation Checks

Before finalizing recommendations:

### 1. Sanity Check
```python
def sanity_check(recommendations):
    for territory, price in recommendations.items():
        # No price should be higher than US (except premium markets)
        if price > us_price and territory not in premium_markets:
            flag_for_review(territory, "Higher than US")

        # No price should be less than $0.49 (Apple minimum)
        if price < 0.49:
            flag_for_review(territory, "Below Apple minimum")
```

### 2. Revenue Impact Modeling
```python
def estimate_revenue_impact(current, recommended, conversion_lift=0.1):
    """
    Estimate revenue impact of price change.
    Assumes 10% conversion lift per 10% price reduction.
    """
    price_change = (recommended - current) / current
    conversion_change = -price_change * conversion_lift

    # Revenue = Price × Volume
    # New Revenue = New Price × (Old Volume × (1 + conversion_change))
    revenue_change = (1 + price_change) * (1 + conversion_change) - 1

    return revenue_change
```

### 3. Competitive Position Check
```python
def check_competitive_position(recommendations, competitor_prices):
    for territory in recommendations:
        my_price = recommendations[territory]
        comp_avg = average(competitor_prices[territory])

        if my_price > comp_avg * 1.3:
            flag_for_review(territory, "30%+ above competitor average")
        elif my_price < comp_avg * 0.5:
            flag_for_review(territory, "50%+ below competitor average")
```

---

## Output Format

### Premium Index Report

```markdown
## App Store Premium Index Report
Generated: {date}
Reference Apps: Spotify, Netflix, YouTube Premium

### Global Overview
- Territories analyzed: 175
- Average index: 0.72
- Median index: 0.65
- Index range: 0.08 (Nigeria) — 1.15 (Switzerland)

### By Region
| Region | Avg Index | Avg PPP | Avg Deviation |
|--------|-----------|---------|---------------|
| North America | 0.98 | 0.93 | +5% |
| Western Europe | 0.88 | 0.85 | +4% |
| Eastern Europe | 0.52 | 0.48 | +8% |
| Asia Pacific | 0.45 | 0.42 | +7% |
| Latin America | 0.38 | 0.35 | +9% |
| Middle East | 0.55 | 0.50 | +10% |
| Africa | 0.18 | 0.15 | +20% |

### Territory Details
[Full table with all 175 territories...]
```

---

## Limitations

1. **Exchange rate lag:** Market rates change faster than prices update
2. **PPP limitations:** Doesn't capture all local economic factors
3. **Category variance:** Index may not apply equally to all app categories
4. **Minimum prices:** Apple's minimum price tiers limit downward adjustment
5. **User expectations:** Some markets expect premium apps to cost more
