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

## ReviewCat

App Store review analysis, competitor intelligence, and automated response generation for iOS/macOS apps.

```bash
npx skills add onatcipli/skills --skill review-cat
```

| Feature | Description |
|---------|-------------|
| Review Fetching | Fetch reviews via App Store Connect API (own apps) or public RSS feed (competitors) |
| Sentiment Analysis | Classify reviews, extract themes, and score sentiment (-1.0 to +1.0) |
| Reply Generation | Context-aware developer replies with templates by rating and issue type |
| Competitor Analysis | Compare ratings, sentiment, and feature gaps across competing apps |
| Trend Monitoring | Track rating changes over time, detect anomalies, correlate with releases |
| Executive Reports | Generate stakeholder-ready summaries with actionable recommendations |

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
