# Locale Reference

Complete reference for App Store locales, language codes, and cross-localization mapping.

## Table of Contents

- [All 38 Supported Locales](#all-38-supported-locales)
- [Cross-Localization Map](#cross-localization-map)
- [Market Priority Tiers](#market-priority-tiers)
- [Locale-Specific Guidelines](#locale-specific-guidelines)

---

## All 38 Supported Locales

| Locale Code | Language | Region | API Code |
|-------------|----------|--------|----------|
| `ar-SA` | Arabic | Saudi Arabia | ar-SA |
| `ca` | Catalan | Spain | ca |
| `cs` | Czech | Czech Republic | cs |
| `da` | Danish | Denmark | da |
| `de-DE` | German | Germany | de-DE |
| `el` | Greek | Greece | el |
| `en-AU` | English | Australia | en-AU |
| `en-CA` | English | Canada | en-CA |
| `en-GB` | English | United Kingdom | en-GB |
| `en-US` | English | United States | en-US |
| `es-ES` | Spanish | Spain | es-ES |
| `es-MX` | Spanish | Mexico | es-MX |
| `fi` | Finnish | Finland | fi |
| `fr-CA` | French | Canada | fr-CA |
| `fr-FR` | French | France | fr-FR |
| `he` | Hebrew | Israel | he |
| `hi` | Hindi | India | hi |
| `hr` | Croatian | Croatia | hr |
| `hu` | Hungarian | Hungary | hu |
| `id` | Indonesian | Indonesia | id |
| `it` | Italian | Italy | it |
| `ja` | Japanese | Japan | ja |
| `ko` | Korean | South Korea | ko |
| `ms` | Malay | Malaysia | ms |
| `nl-NL` | Dutch | Netherlands | nl-NL |
| `no` | Norwegian | Norway | no |
| `pl` | Polish | Poland | pl |
| `pt-BR` | Portuguese | Brazil | pt-BR |
| `pt-PT` | Portuguese | Portugal | pt-PT |
| `ro` | Romanian | Romania | ro |
| `ru` | Russian | Russia | ru |
| `sk` | Slovak | Slovakia | sk |
| `sv` | Swedish | Sweden | sv |
| `th` | Thai | Thailand | th |
| `tr` | Turkish | Turkey | tr |
| `uk` | Ukrainian | Ukraine | uk |
| `vi` | Vietnamese | Vietnam | vi |
| `zh-Hans` | Chinese (Simplified) | China | zh-Hans |
| `zh-Hant` | Chinese (Traditional) | Taiwan/HK | zh-Hant |

---

## Cross-Localization Map

Apple indexes keywords from multiple locales for each storefront. Use this to
maximize keyword coverage.

### How Cross-Localization Works

When a user searches on a storefront, Apple indexes keywords from:
1. The **primary locale** for that storefront
2. One or more **secondary locales**

This means you can target **more keywords per market** by strategically
distributing keywords across paired locales.

### Complete Cross-Indexing Reference

| Storefront | Country | Primary Locale | Secondary Locale(s) |
|------------|---------|---------------|---------------------|
| United States | USA | en-US | es-MX |
| United Kingdom | GBR | en-GB | en-US |
| Canada | CAN | en-CA | fr-CA, en-US |
| Australia | AUS | en-AU | en-US |
| Germany | DEU | de-DE | en-US |
| France | FRA | fr-FR | en-US |
| Spain | ESP | es-ES | en-US |
| Italy | ITA | it | en-US |
| Netherlands | NLD | nl-NL | en-US |
| Belgium | BEL | fr-FR, nl-NL | en-US |
| Switzerland | CHE | de-DE, fr-FR, it | en-US |
| Austria | AUT | de-DE | en-US |
| Japan | JPN | ja | en-US |
| South Korea | KOR | ko | en-US |
| China | CHN | zh-Hans | en-US |
| Taiwan | TWN | zh-Hant | en-US |
| Hong Kong | HKG | zh-Hant | en-US |
| Brazil | BRA | pt-BR | en-US |
| Mexico | MEX | es-MX | en-US |
| Russia | RUS | ru | en-US |
| India | IND | en-GB, hi | en-US |
| Indonesia | IDN | id | en-US |
| Thailand | THA | th | en-US |
| Vietnam | VNM | vi | en-US |
| Turkey | TUR | tr | en-US |
| Saudi Arabia | SAU | ar-SA | en-US |
| UAE | ARE | ar-SA, en-GB | en-US |
| Israel | ISR | he | en-US |
| Poland | POL | pl | en-US |
| Sweden | SWE | sv | en-US |
| Norway | NOR | no | en-US |
| Denmark | DNK | da | en-US |
| Finland | FIN | fi | en-US |
| Portugal | PRT | pt-PT | en-US |
| Greece | GRC | el | en-US |
| Czech Republic | CZE | cs | en-US |
| Romania | ROU | ro | en-US |
| Hungary | HUN | hu | en-US |
| Ukraine | UKR | uk | en-US |

### Strategic Cross-Localization Examples

#### USA Market (en-US + es-MX)

```
en-US Keywords (100 chars):
"photo,editor,camera,filter,retouch,enhance,crop,adjust,portrait,beauty"

es-MX Keywords (100 chars):
"fotos,editar,imagen,efectos,marcos,selfie,blur,hdr,collage,stickers"

Result: 20 unique keywords indexed for US users
```

#### Canada Market (en-CA + fr-CA + en-US)

```
en-CA Keywords:
"photo,editor,camera,filter" (+ unique Canadian terms)

fr-CA Keywords:
"photos,editeur,appareil,filtre" (French Canadian terms)

en-US Keywords:
(Also indexed — put unique terms here too)

Result: Up to 3x keyword coverage for Canada
```

#### Germany Market (de-DE + en-US)

```
de-DE Keywords:
"bildbearbeitung,fotos,bilder,kamera,filter,bearbeiten,effekte"

en-US Keywords:
"photo,editor,enhance,retouch,crop,adjust"

Result: German users find you via both German AND English searches
```

---

## Market Priority Tiers

### Tier 1: Must-Have Localizations

| Market | Locale | Why |
|--------|--------|-----|
| United States | en-US | Largest App Store revenue |
| China | zh-Hans | Largest user base |
| Japan | ja | Highest ARPU |
| United Kingdom | en-GB | Major English market |
| Germany | de-DE | Largest EU market |

### Tier 2: High-Value Markets

| Market | Locale | Why |
|--------|--------|-----|
| France | fr-FR | Major EU market |
| South Korea | ko | High smartphone adoption |
| Canada | en-CA, fr-CA | High spending, bilingual |
| Australia | en-AU | High ARPU market |
| Brazil | pt-BR | Largest LATAM market |

### Tier 3: Growth Markets

| Market | Locale | Why |
|--------|--------|-----|
| Spain | es-ES | Growing market |
| Italy | it | Engaged user base |
| Mexico | es-MX | Large population, growing |
| Russia | ru | Large market |
| India | hi, en-GB | Huge growth potential |
| Indonesia | id | Fastest growing |

### Tier 4: Additional Markets

| Market | Locale | Why |
|--------|--------|-----|
| Netherlands | nl-NL | High ARPU |
| Sweden | sv | Early adopters |
| Turkey | tr | Large young population |
| Taiwan | zh-Hant | High spending |
| Thailand | th | Growing market |
| Vietnam | vi | Fast growing |
| Poland | pl | Largest CEE market |

---

## Locale-Specific Guidelines

### Chinese (zh-Hans, zh-Hant)

```
Characters: 30 char limit ≈ 10-15 Chinese characters
Keywords: Simplified/Traditional have different search terms
Note: China (zh-Hans) and Taiwan (zh-Hant) are DIFFERENT markets

Example:
zh-Hans: 照片编辑器,美颜,滤镜,相机
zh-Hant: 照片編輯器,美顏,濾鏡,相機
```

### Japanese (ja)

```
Characters: 30 char limit ≈ 10-15 Japanese characters
Keywords: Mix of kanji, hiragana, katakana
Note: Include both Japanese and common English loanwords

Example:
写真,編集,カメラ,フィルター,加工,エフェクト
(Include: 写真編集 AND フォトエディター)
```

### Korean (ko)

```
Characters: 30 char limit ≈ 10-15 Korean characters
Keywords: Hangul primarily, some English terms popular
Note: Include Korean AND Konglish terms

Example:
사진,편집,카메라,필터,보정,셀카
```

### Arabic (ar-SA)

```
Direction: Right-to-left (RTL)
Characters: 30 char limit ≈ 15-20 Arabic characters
Note: Consider RTL in screenshots and UI

Example:
محرر,صور,كاميرا,فلتر,تحرير
```

### German (de-DE)

```
Characters: German words tend to be LONG (compound words)
Note: 30 chars may only fit 2-3 German words

Example:
Bildbearbeitung (15 chars) = "image editing"
Fotobearbeitungs-App (20 chars) = "photo editing app"

Strategy: Use shorter word forms or break compounds
```

### Spanish (es-ES vs es-MX)

```
Note: Spain and Mexico Spanish differ!

es-ES (Spain): "ordenador" (computer), "móvil" (mobile)
es-MX (Mexico): "computadora" (computer), "celular" (mobile)

Strategy: Use locale-appropriate terms, not generic Spanish
```

### Portuguese (pt-BR vs pt-PT)

```
Note: Brazil and Portugal Portuguese differ significantly!

pt-BR: More informal, different vocabulary
pt-PT: More formal, European terms

Example:
pt-BR: "celular" (cell phone), "legal" (cool)
pt-PT: "telemóvel" (cell phone), "fixe" (cool)
```

### Hindi (hi)

```
Characters: 30 char limit ≈ 10-15 Devanagari characters
Note: Many Indians search in English — consider en-GB too
Strategy: Mix Hindi AND English keywords

Example:
फोटो,एडिटर,कैमरा,फ़िल्टर (+ English keywords in en-GB)
```

---

## iTunes Lookup Country Codes

For fetching competitor data, use these 2-letter codes:

```python
COUNTRY_CODES = {
    "us": "United States",
    "gb": "United Kingdom",
    "ca": "Canada",
    "au": "Australia",
    "de": "Germany",
    "fr": "France",
    "es": "Spain",
    "it": "Italy",
    "nl": "Netherlands",
    "be": "Belgium",
    "ch": "Switzerland",
    "at": "Austria",
    "jp": "Japan",
    "kr": "South Korea",
    "cn": "China",
    "tw": "Taiwan",
    "hk": "Hong Kong",
    "br": "Brazil",
    "mx": "Mexico",
    "ru": "Russia",
    "in": "India",
    "id": "Indonesia",
    "th": "Thailand",
    "vn": "Vietnam",
    "tr": "Turkey",
    "sa": "Saudi Arabia",
    "ae": "UAE",
    "il": "Israel",
    "pl": "Poland",
    "se": "Sweden",
    "no": "Norway",
    "dk": "Denmark",
    "fi": "Finland",
    "pt": "Portugal",
    "gr": "Greece",
    "cz": "Czech Republic",
    "ro": "Romania",
    "hu": "Hungary",
    "ua": "Ukraine"
}
```

---

## Localization Checklist

Before launching in a new locale:

```
[ ] Keywords researched (not just translated)
[ ] Title fits 30 character limit
[ ] Subtitle fits 30 character limit
[ ] Keywords use full 100 characters
[ ] No duplicate keywords from title/subtitle
[ ] Screenshots localized
[ ] Description converted (focus on conversion)
[ ] Cross-localization strategy applied
[ ] Cultural review completed
[ ] RTL considered (Arabic, Hebrew)
```
