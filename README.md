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

| Skill | Description |
|-------|-------------|
| [review-cat](skills/review-cat/) | App Store review analysis, competitor intelligence, and automated reply generation |

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
