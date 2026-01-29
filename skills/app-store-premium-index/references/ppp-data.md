# Purchasing Power Parity (PPP) Data

World Bank PPP conversion factors for App Store price localization.

## What is PPP?

Purchasing Power Parity (PPP) measures how much local currency is needed to buy
the same basket of goods that $1 USD buys in the United States. A PPP factor of
0.5 means local prices are roughly half of US prices for equivalent goods.

## Data Source

Primary: World Bank International Comparison Program (ICP)
URL: https://data.worldbank.org/indicator/PA.NUS.PPP

Last updated: 2024 (update annually)

---

## PPP Conversion Factors

Values represent the ratio of local purchasing power to US purchasing power.
**Lower values = lower local prices recommended.**

### Tier 1: High Income (PPP 0.8 - 1.2)

These markets can support US-equivalent or higher prices.

| Territory | PPP Factor | Recommendation |
|-----------|------------|----------------|
| Switzerland | 1.15 | Price 10-15% above US |
| Norway | 1.10 | Price 5-10% above US |
| Iceland | 1.08 | Price 5-10% above US |
| Denmark | 1.05 | Price at US level |
| Luxembourg | 1.02 | Price at US level |
| USA | 1.00 | Baseline |
| Australia | 0.98 | Price at US level |
| Sweden | 0.95 | Price at US level |
| Ireland | 0.94 | Price at US level |
| Netherlands | 0.92 | Price 5% below US |
| Finland | 0.91 | Price 5% below US |
| Germany | 0.89 | Price 10% below US |
| Austria | 0.88 | Price 10% below US |
| Belgium | 0.87 | Price 10% below US |
| Canada | 0.86 | Price 10-15% below US |
| France | 0.85 | Price 10-15% below US |
| United Kingdom | 0.82 | Price 15-20% below US |
| Japan | 0.80 | Price 15-20% below US |

### Tier 2: Upper Middle Income (PPP 0.5 - 0.8)

Moderate price reduction recommended.

| Territory | PPP Factor | Recommendation |
|-----------|------------|----------------|
| New Zealand | 0.78 | Price 20-25% below US |
| Italy | 0.76 | Price 20-25% below US |
| Spain | 0.74 | Price 25% below US |
| South Korea | 0.72 | Price 25-30% below US |
| Israel | 0.70 | Price 30% below US |
| Singapore | 0.68 | Price 30% below US |
| UAE | 0.65 | Price 30-35% below US |
| Portugal | 0.62 | Price 35-40% below US |
| Taiwan | 0.60 | Price 40% below US |
| Czech Republic | 0.58 | Price 40% below US |
| Saudi Arabia | 0.55 | Price 40-45% below US |
| Greece | 0.54 | Price 45% below US |
| Poland | 0.52 | Price 45-50% below US |
| Hungary | 0.50 | Price 50% below US |

### Tier 3: Middle Income (PPP 0.3 - 0.5)

Significant price reduction recommended.

| Territory | PPP Factor | Recommendation |
|-----------|------------|----------------|
| Chile | 0.48 | Price 50% below US |
| Croatia | 0.46 | Price 50-55% below US |
| Romania | 0.44 | Price 55% below US |
| Malaysia | 0.42 | Price 55-60% below US |
| Russia | 0.40 | Price 60% below US |
| Argentina | 0.38 | Price 60% below US |
| Turkey | 0.36 | Price 60-65% below US |
| Mexico | 0.35 | Price 65% below US |
| Brazil | 0.34 | Price 65% below US |
| China | 0.32 | Price 65-70% below US |
| Thailand | 0.31 | Price 70% below US |
| Colombia | 0.30 | Price 70% below US |

### Tier 4: Lower Middle Income (PPP 0.15 - 0.3)

Aggressive price reduction for market access.

| Territory | PPP Factor | Recommendation |
|-----------|------------|----------------|
| South Africa | 0.28 | Price 70-75% below US |
| Peru | 0.26 | Price 75% below US |
| Indonesia | 0.24 | Price 75% below US |
| Philippines | 0.22 | Price 75-80% below US |
| Vietnam | 0.20 | Price 80% below US |
| Morocco | 0.19 | Price 80% below US |
| Egypt | 0.18 | Price 80% below US |
| Ukraine | 0.17 | Price 80-85% below US |
| India | 0.16 | Price 85% below US |
| Nigeria | 0.15 | Price 85% below US |

### Tier 5: Low Income (PPP < 0.15)

Maximum price reduction for accessibility.

| Territory | PPP Factor | Recommendation |
|-----------|------------|----------------|
| Pakistan | 0.14 | Price 85-90% below US |
| Bangladesh | 0.12 | Price 85-90% below US |
| Kenya | 0.11 | Price 90% below US |
| Ghana | 0.10 | Price 90% below US |
| Nepal | 0.09 | Price 90% below US |
| Ethiopia | 0.08 | Price 90%+ below US |

---

## Quick Lookup Table (All Territories)

```javascript
const pppFactors = {
  // Tier 1: High Income
  'CHE': 1.15, 'NOR': 1.10, 'ISL': 1.08, 'DNK': 1.05, 'LUX': 1.02,
  'USA': 1.00, 'AUS': 0.98, 'SWE': 0.95, 'IRL': 0.94, 'NLD': 0.92,
  'FIN': 0.91, 'DEU': 0.89, 'AUT': 0.88, 'BEL': 0.87, 'CAN': 0.86,
  'FRA': 0.85, 'GBR': 0.82, 'JPN': 0.80,

  // Tier 2: Upper Middle Income
  'NZL': 0.78, 'ITA': 0.76, 'ESP': 0.74, 'KOR': 0.72, 'ISR': 0.70,
  'SGP': 0.68, 'ARE': 0.65, 'PRT': 0.62, 'TWN': 0.60, 'CZE': 0.58,
  'SAU': 0.55, 'GRC': 0.54, 'POL': 0.52, 'HUN': 0.50,

  // Tier 3: Middle Income
  'CHL': 0.48, 'HRV': 0.46, 'ROU': 0.44, 'MYS': 0.42, 'RUS': 0.40,
  'ARG': 0.38, 'TUR': 0.36, 'MEX': 0.35, 'BRA': 0.34, 'CHN': 0.32,
  'THA': 0.31, 'COL': 0.30,

  // Tier 4: Lower Middle Income
  'ZAF': 0.28, 'PER': 0.26, 'IDN': 0.24, 'PHL': 0.22, 'VNM': 0.20,
  'MAR': 0.19, 'EGY': 0.18, 'UKR': 0.17, 'IND': 0.16, 'NGA': 0.15,

  // Tier 5: Low Income
  'PAK': 0.14, 'BGD': 0.12, 'KEN': 0.11, 'GHA': 0.10, 'NPL': 0.09,
  'ETH': 0.08
};
```

---

## Spotify Premium Index Comparison

How Spotify prices align with PPP (as of 2024):

| Territory | PPP Factor | Spotify Price | Spotify Index | Alignment |
|-----------|------------|---------------|---------------|-----------|
| USA | 1.00 | $11.99 | 1.00 | Baseline |
| UK | 0.82 | £11.99 (~$15) | 1.25 | Overpriced |
| Germany | 0.89 | €10.99 (~$12) | 1.00 | Good |
| Japan | 0.80 | ¥980 (~$7) | 0.58 | Optimized |
| India | 0.16 | ₹119 (~$1.4) | 0.12 | Aggressive |
| Turkey | 0.36 | ₺57.99 (~$1.8) | 0.15 | Aggressive |
| Brazil | 0.34 | R$21.90 (~$4.4) | 0.37 | Good |
| Nigeria | 0.15 | ₦900 (~$1.1) | 0.09 | Aggressive |

**Key Insight:** Spotify uses aggressive localization in emerging markets,
pricing well below PPP parity to maximize subscriber growth.

---

## Strategy Recommendations by Tier

### Tier 1 (PPP > 0.8): Premium Markets
- **Strategy:** Price at or slightly above US
- **Focus:** Maximize ARPU
- **Risk:** Low — high willingness to pay

### Tier 2 (PPP 0.5-0.8): Developed Markets
- **Strategy:** 10-30% below US
- **Focus:** Balance ARPU and conversion
- **Risk:** Medium — some price sensitivity

### Tier 3 (PPP 0.3-0.5): Emerging Markets
- **Strategy:** 40-60% below US
- **Focus:** Market penetration
- **Risk:** Currency volatility

### Tier 4 (PPP 0.15-0.3): Price-Sensitive Markets
- **Strategy:** 70-80% below US
- **Focus:** Volume growth
- **Risk:** Low ARPU, high support costs

### Tier 5 (PPP < 0.15): Accessibility Markets
- **Strategy:** 85-90% below US or consider freemium
- **Focus:** Market presence, brand building
- **Risk:** Minimal revenue contribution

---

## Updating PPP Data

PPP factors should be updated annually:

1. Visit World Bank Data: https://data.worldbank.org/indicator/PA.NUS.PPP
2. Download latest dataset
3. Calculate factor: `ppp_factor = country_ppp / usa_ppp`
4. Update this reference file

**Note:** PPP changes gradually. Annual updates are sufficient for pricing decisions.

---

## Alternative Data Sources

| Source | URL | Notes |
|--------|-----|-------|
| World Bank PPP | data.worldbank.org | Official, comprehensive |
| IMF WEO | imf.org/external/datamapper | Alternative calculations |
| Big Mac Index | economist.com/big-mac-index | Consumer goods proxy |
| OECD PPP | stats.oecd.org | OECD countries only |
| Numbeo | numbeo.com/cost-of-living | Crowdsourced, real-time |
