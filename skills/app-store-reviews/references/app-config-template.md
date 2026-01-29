# App Configuration Template

This template defines the structure for app-specific review reply configurations.
After onboarding, a personalized version of this file is generated for each app.

---

## Template Structure

```markdown
# Review Reply Configuration — {APP_NAME}

> Generated: {DATE}
> Last Updated: {DATE}
> App ID: {APP_STORE_ID}

---

## App Identity

| Field | Value |
|-------|-------|
| **App Name** | {app_name} |
| **App Description** | {one_line_description} |
| **Support Email** | {support_email} |
| **Help Center URL** | {help_center_url} |
| **Feedback URL** | {feedback_url} |

---

## Brand Voice

### Tone Profile

- **Primary Tone:** {tone}
  <!-- Options: formal, friendly-professional, casual, playful -->
- **Warmth Level:** {warmth}
  <!-- Options: reserved, balanced, warm, enthusiastic -->

### Writing Style

| Setting | Value |
|---------|-------|
| Use contractions | {yes/no} |
| Use exclamations | {yes/no/sparingly} |
| Emoji usage | {none/minimal/moderate/frequent} |
| Preferred length | {short/medium/detailed} |

### Sign-off

```
{sign_off_text}
```
<!-- Examples: "— The {App} Team", "Best, {Name}", leave empty for none -->

---

## Language Preferences

### Preferred Phrases

Use these phrases when appropriate:

- {phrase_1}
- {phrase_2}
- {phrase_3}

### Phrases to Avoid

Never use these words/phrases:

- {avoid_1}
- {avoid_2}
- {avoid_3}

### Greeting Patterns

| Scenario | Greeting |
|----------|----------|
| Known name | "Hi {nickname}," |
| Unknown name | "Hello!" / "Thanks for your review!" |
| Positive review | "Thank you so much for the kind words!" |
| Negative review | "Thank you for taking the time to share your feedback." |

---

## Reply Policies

### By Rating

| Rating | Tone | Include Support Email | Max Length |
|--------|------|----------------------|------------|
| 5 stars | {tone_5} | {yes/no} | {short/medium} |
| 4 stars | {tone_4} | {yes/no} | {short/medium} |
| 3 stars | {tone_3} | {yes/no} | {medium} |
| 2 stars | {tone_2} | {yes/no} | {medium/detailed} |
| 1 star | {tone_1} | {yes/no} | {detailed} |

### By Issue Type

| Issue Type | Response Strategy |
|------------|-------------------|
| **Bug/Crash** | {strategy} |
| **Feature Request** | {strategy} |
| **Performance** | {strategy} |
| **Pricing/Subscription** | {strategy} |
| **UX/Design** | {strategy} |
| **Accidental Review** | {strategy} |

### Escalation Rules

- **Always escalate to support:** {conditions}
- **Flag for manual review:** {conditions}
- **Auto-skip:** {conditions}

---

## Restricted Topics

### Never Discuss Publicly

These topics should redirect to support email:

- [ ] Pricing details or changes
- [ ] Refund requests or policies
- [ ] Legal matters
- [ ] Account-specific issues
- [ ] {custom_topic_1}
- [ ] {custom_topic_2}

### Never Mention

- [ ] Competitor names
- [ ] Unreleased features
- [ ] Specific timelines or ETAs
- [ ] {custom_restriction_1}

---

## Language Support

### Supported Languages

| Language | Quality | Notes |
|----------|---------|-------|
| English | Native | Primary |
| {lang_2} | {quality} | {notes} |
| {lang_3} | {quality} | {notes} |

### Unsupported Language Handling

```
{handling_strategy}
```
<!-- Options:
  - Reply in English with apology
  - Skip and flag for manual review
  - Use translation service
-->

---

## Custom Templates

### Override: 5-Star Reply

```
{custom_template_or_empty}
```

### Override: Bug Report Reply

```
{custom_template_or_empty}
```

### Override: Feature Request Reply

```
{custom_template_or_empty}
```

---

## Analysis Preferences

### Theme Priorities

Rank themes by importance for your app (1 = highest):

1. {priority_theme_1}
2. {priority_theme_2}
3. {priority_theme_3}
4. {priority_theme_4}
5. {priority_theme_5}

### Alert Triggers

Generate alerts when:

- [ ] Rating drops below {threshold} stars in {timeframe}
- [ ] More than {count} reviews mention "{keyword}"
- [ ] Negative review velocity exceeds {count} per {timeframe}
- [ ] {custom_trigger}

---

## Notes

{free_form_notes}

---

## Version History

| Date | Change | By |
|------|--------|-----|
| {date} | Initial configuration | {user} |
```

---

## Field Reference

### Tone Options

| Value | Description | Best For |
|-------|-------------|----------|
| `formal` | Professional, corporate language | Enterprise, B2B, finance apps |
| `friendly-professional` | Warm but polished | Most consumer apps |
| `casual` | Relaxed, conversational | Lifestyle, social apps |
| `playful` | Fun, energetic, may use humor | Games, entertainment |

### Emoji Usage Levels

| Level | Description | Example |
|-------|-------------|---------|
| `none` | No emojis ever | "Thank you for your feedback." |
| `minimal` | 1-2 on positive reviews only | "Thanks for the 5 stars! 🙌" |
| `moderate` | 2-3 per reply, context-appropriate | "We're so glad you love it! 🎉 Let us know if you need anything 💬" |
| `frequent` | Emoji-rich responses | "OMG thank you!! 😍🙏✨ You made our day! 🎊" |

### Length Guidelines

| Setting | Characters | Sentences | Use When |
|---------|------------|-----------|----------|
| `short` | <200 | 1-2 | Positive reviews, quick acknowledgments |
| `medium` | 200-500 | 2-4 | Most reviews, balanced response |
| `detailed` | 500-1000 | 4-6 | Complex issues, negative reviews |

---

## Example Configurations

### Example 1: Enterprise B2B App

```markdown
## Brand Voice
- **Primary Tone:** formal
- **Use contractions:** no
- **Emoji usage:** none
- **Sign-off:** "Best regards, The ProductName Support Team"

## Phrases to Avoid
- "Hey", "Awesome", "Cool"
- Exclamation points (use sparingly)
- Casual greetings
```

### Example 2: Casual Lifestyle App

```markdown
## Brand Voice
- **Primary Tone:** casual
- **Use contractions:** yes
- **Emoji usage:** moderate
- **Sign-off:** "— Team AppName 💜"

## Preferred Phrases
- "We hear you!"
- "Thanks for keeping it real"
- "You're the best!"
```

### Example 3: Gaming App

```markdown
## Brand Voice
- **Primary Tone:** playful
- **Use contractions:** yes
- **Emoji usage:** frequent
- **Sign-off:** "Game on! 🎮 — The GameName Crew"

## Preferred Phrases
- "GG!"
- "Level up your experience"
- "We've got your back, player!"
```
