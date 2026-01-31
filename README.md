# Skills

Reusable AI agent skills for Claude Code.

## Setup

### Prerequisites

- [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code) installed
- [GitHub CLI](https://cli.github.com/) (`gh`) installed and authenticated

### Install a skill

```bash
npx skills add onatcipli/skills --skill <skill-name>
```

### Install all skills

```bash
npx skills add onatcipli/skills
```

## Available Skills

| Skill | Description | Install |
|-------|-------------|---------|
| `app-store-reviews` | Review analysis, sentiment, intelligent replies | `npx skills add onatcipli/skills --skill app-store-reviews` |
| `app-store-premium-index` | Price localization via purchasing power parity | `npx skills add onatcipli/skills --skill app-store-premium-index` |
| `app-store-aso-localization` | ASO optimization and metadata localization | `npx skills add onatcipli/skills --skill app-store-aso-localization` |
| `app-store-screenshots` | Screenshot research, competitor analysis, planning | `npx skills add onatcipli/skills --skill app-store-screenshots` |

---

## App Store Reviews

App Store review analysis, competitor intelligence, and automated response generation for iOS/macOS apps.

```bash
npx skills add onatcipli/skills --skill app-store-reviews
```

| Feature | Description |
|---------|-------------|
| Review Fetching | Fetch reviews via App Store Connect API (own apps) or public RSS feed (competitors) |
| Sentiment Analysis | Classify reviews, extract themes, and score sentiment |
| Reply Generation | Context-aware replies with brand voice configuration |
| Onboarding | Auto-detect reply patterns and configure brand preferences |
| Competitor Analysis | Compare ratings, sentiment, and feature gaps across competing apps |
| Trend Monitoring | Track rating changes over time, detect anomalies |
| Executive Reports | Generate stakeholder-ready summaries with recommendations |

---

## App Store Premium Index

Price localization intelligence using purchasing power parity analysis — like the Big Mac Index for your iOS app pricing.

```bash
npx skills add onatcipli/skills --skill app-store-premium-index
```

| Feature | Description |
|---------|-------------|
| Build Premium Index | Analyze reference app prices across 175 territories |
| Pricing Analysis | Compare your prices against PPP-adjusted benchmarks |
| Recommendations | Generate localized prices (conservative/balanced/aggressive) |
| API Automation | Apply price updates via App Store Connect API |
| Competitor Intel | Analyze competitor pricing strategies globally |

---

## App Store ASO Localization

App Store Optimization and metadata localization across 38 languages.

```bash
npx skills add onatcipli/skills --skill app-store-aso-localization
```

| Feature | Description |
|---------|-------------|
| Metadata Health | Audit character usage, missing locales, ASO violations |
| Keyword Research | Research, score, and optimize keyword distribution |
| Localization | AI-translate + localize (not just translate) to 38 languages |
| Cross-Localization | Double keyword coverage using Apple's multi-locale indexing |
| Bulk Updates | Programmatically update metadata via App Store Connect API |
| Competitor ASO | Analyze competitor keywords and find gaps |

---

## App Store Screenshots

Screenshot research, competitor analysis, and planning tool with local preview website.

```bash
npx skills add onatcipli/skills --skill app-store-screenshots
```

| Feature | Description |
|---------|-------------|
| Find Competitors | Discover competitors via iTunes Search and RSS feeds |
| Download Screenshots | Download competitor screenshots locally for analysis |
| Strategy Analysis | Analyze screenshot styles, captions, and patterns |
| Plan Screenshots | Generate detailed screenshot plan based on research |
| Preview Website | Lightweight local HTML tool to view and compare |
| Export Guide | Checklist and specs for App Store Connect upload |

---

## Repository Structure

```
skills/
├── skills/
│   └── <skill-name>/
│       ├── SKILL.md           # Skill definition (required)
│       ├── scripts/           # Executable scripts (optional)
│       ├── references/        # Reference documentation (optional)
│       └── assets/            # Output resources (optional)
├── .gitignore
└── README.md
```

## Creating a New Skill

Use the [skill-creator](https://skills.sh/anthropics/skills/skill-creator) skill to scaffold and package new skills:

```bash
# Initialize a new skill
python scripts/init_skill.py <skill-name> --path skills/

# Package for distribution
python scripts/package_skill.py skills/<skill-name>
```
