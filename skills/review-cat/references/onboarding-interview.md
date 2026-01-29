# Onboarding Interview Reference

Guide for conducting the app setup interview and auto-detecting reply patterns from
existing responses. The goal is to create a personalized app configuration file that
ensures consistent, on-brand replies.

## Table of Contents

- [Auto-Detection Phase](#auto-detection-phase)
- [Interview Questions](#interview-questions)
- [Configuration Output](#configuration-output)
- [Interview Flow](#interview-flow)

---

## Auto-Detection Phase

Before interviewing the user, fetch existing developer responses to auto-detect patterns.
These detected patterns become smart defaults during the interview.

### Step 1: Fetch Existing Responses

```
GET /v1/apps/{appId}/customerReviews?include=response&filter[rating]=1,2,3,4,5&limit=100
```

Extract reviews where `relationships.response.data` is not null.

### Step 2: Analyze Response Patterns

For each existing response, extract:

| Pattern | Detection Method |
|---------|------------------|
| **Greeting style** | First sentence pattern: "Hi {name}", "Hello!", "Thanks for...", etc. |
| **Sign-off style** | Last line: "— The {App} Team", "Best, {Name}", none, etc. |
| **Tone** | Formal ("We apologize...") vs Casual ("Sorry about that!") |
| **Emoji usage** | Count emojis per response: none, minimal (1-2), frequent (3+) |
| **Support channel** | Extract email/URL patterns: support@..., help center links |
| **Average length** | Character count: short (<200), medium (200-500), long (500+) |
| **Language style** | "We" vs "I", contractions (don't vs do not), exclamation points |
| **Common phrases** | Frequently used phrases (e.g., "We appreciate your feedback") |

### Step 3: Generate Smart Defaults

```yaml
detected_defaults:
  greeting_style: "Hi {nickname},"          # or "Hello!", "Thanks for reaching out!"
  sign_off: "— The {app_name} Team"         # or "Best regards,", none
  tone: "friendly-professional"              # formal, casual, friendly-professional
  emoji_usage: "minimal"                     # none, minimal, frequent
  support_email: "support@example.com"       # extracted from responses
  avg_response_length: "medium"              # short, medium, long
  uses_contractions: true                    # don't vs do not
  uses_exclamations: true                    # enthusiasm markers
  common_phrases:
    - "We appreciate your feedback"
    - "Please don't hesitate to reach out"
```

---

## Interview Questions

Present detected defaults where available. User can accept or override.

### Section 1: App Identity

```
Q1: What is your app's name?
    [Auto-filled from App Store Connect if available]

Q2: What does your app do in one sentence?
    Example: "A meditation app that helps users build daily mindfulness habits"

Q3: What is your support email?
    [Default: {detected_support_email}]

Q4: Do you have a help center or FAQ URL?
    [Optional]
```

### Section 2: Brand Voice

```
Q5: How would you describe your brand's personality? (select one)
    [ ] Professional & Formal — Corporate, enterprise apps
    [ ] Friendly & Professional — Most consumer apps (DEFAULT if detected)
    [ ] Casual & Warm — Lifestyle, social apps
    [ ] Playful & Fun — Games, entertainment apps
    [Default: {detected_tone}]

Q6: Do you want to use emojis in replies?
    [ ] Never
    [ ] Occasionally (1-2 per reply, for positive reviews only)
    [ ] Frequently
    [Default: {detected_emoji_usage}]

Q7: How should replies be signed off?
    [ ] No signature
    [ ] "— The {App Name} Team"
    [ ] "— {Custom Team Name}"
    [ ] "Best regards, {App Name}"
    [Default: {detected_sign_off}]
```

### Section 3: Reply Policies

```
Q8: What is the preferred reply length?
    [ ] Short (1-2 sentences) — Quick acknowledgments
    [ ] Medium (2-4 sentences) — Balanced responses (RECOMMENDED)
    [ ] Detailed (4-6 sentences) — Thorough explanations
    [Default: {detected_avg_length}]

Q9: Are there topics you should NEVER discuss in public replies?
    Examples: pricing details, refund policies, legal matters, competitor names
    [Free text, comma-separated]

Q10: Are there any phrases or words you always want to avoid?
     Examples: "unfortunately", "can't", "won't", negative language
     [Free text, comma-separated]

Q11: Are there key phrases you'd like to consistently use?
     Examples: "We'd love to hear more", "Your feedback matters"
     [Default: {detected_common_phrases}]
```

### Section 4: Escalation & Special Cases

```
Q12: At what star rating should replies include direct support contact?
     [ ] All ratings (1-5 stars)
     [ ] Negative reviews only (1-2 stars) — RECOMMENDED
     [ ] Critical only (1 star)
     [ ] Never include in public reply

Q13: How should you handle reviews mentioning bugs or crashes?
     [ ] Apologize and ask for details via support email
     [ ] Acknowledge and mention it's being investigated
     [ ] Both — apologize, acknowledge, and request details
     [Default: Both]

Q14: How should you handle feature requests?
     [ ] Thank and add to roadmap (no commitment)
     [ ] Thank and explain why it may/may not be added
     [ ] Redirect to a feature request channel/URL
     [Free text for URL if applicable]

Q15: Should replies ever mention competitors by name?
     [ ] Never (RECOMMENDED)
     [ ] Only if the reviewer mentioned them first
```

### Section 5: Language & Localization

```
Q16: What languages can you support for replies?
     [ ] English only
     [ ] English + major languages (Spanish, French, German, Japanese)
     [ ] Match any language the reviewer uses
     [Default: Match reviewer's language]

Q17: For reviews in languages you can't support, what should happen?
     [ ] Reply in English with apology for language limitation
     [ ] Skip the review (flag for manual handling)
     [ ] Use translation (note: may have quality issues)
```

---

## Configuration Output

After the interview, generate a markdown configuration file saved to the user's
preferred location (default: `{app-name}-review-config.md`).

### Output Template

See [app-config-template.md](app-config-template.md) for the full template structure.

### Example Output

```markdown
# Review Reply Configuration — MindfulMe

## App Identity
- **App Name:** MindfulMe
- **Description:** A meditation app that helps users build daily mindfulness habits
- **Support Email:** help@mindfulme.app
- **Help Center:** https://mindfulme.app/support

## Brand Voice
- **Tone:** Friendly & Professional
- **Emoji Usage:** Occasional (positive reviews only)
- **Sign-off:** — The MindfulMe Team
- **Preferred Length:** Medium (2-4 sentences)

## Language Preferences
- **Contractions:** Yes (use "we're" not "we are")
- **Exclamations:** Yes, sparingly
- **Avoid Words:** unfortunately, can't, won't, issue
- **Preferred Phrases:**
  - "We appreciate you sharing this"
  - "Your feedback helps us improve"
  - "We'd love to help"

## Reply Policies
- **Include Support Email:** 1-2 star reviews only
- **Bug Reports:** Apologize, acknowledge, request details
- **Feature Requests:** Thank, add to roadmap, no commitment
- **Mention Competitors:** Never
- **Never Discuss:** Pricing, refunds, legal matters

## Language Support
- **Supported Languages:** English, Spanish, French, German, Japanese
- **Unsupported Language Handling:** Reply in English with apology

## Custom Notes
{Any additional notes from the user}
```

---

## Interview Flow

### Recommended Flow

```
┌─────────────────────────────────────────────────────────────┐
│  1. FETCH EXISTING RESPONSES                                │
│     └─ GET /v1/apps/{appId}/customerReviews?include=response│
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│  2. ANALYZE PATTERNS                                        │
│     └─ Extract tone, greetings, sign-offs, common phrases   │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│  3. PRESENT FINDINGS                                        │
│     "I analyzed 47 existing replies. Here's what I found:   │
│      - Tone: Friendly-professional                          │
│      - Sign-off: '— The App Team'                           │
│      - Emoji usage: Minimal                                 │
│      - Support email: help@app.com                          │
│      Would you like to use these as defaults?"              │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│  4. CONDUCT INTERVIEW                                       │
│     └─ Walk through each section                            │
│     └─ Pre-fill with detected defaults                      │
│     └─ Allow user to override any setting                   │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│  5. GENERATE CONFIG FILE                                    │
│     └─ Create {app-name}-review-config.md                   │
│     └─ Save to user's specified location                    │
│     └─ Confirm file created and location                    │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│  6. SUMMARY                                                 │
│     "Configuration saved! When generating replies, I'll     │
│      reference this file to match your brand voice."        │
└─────────────────────────────────────────────────────────────┘
```

### Quick Setup Option

For users who want to skip the full interview:

```
"Would you like to:
 [A] Quick Setup — Use auto-detected settings + fill in basics (2 min)
 [B] Full Setup — Customize all settings in detail (5-10 min)
 [C] Minimal Setup — Just provide app name and support email"
```

### Re-running Setup

Users can re-run the setup at any time to update their configuration:

```
"I found an existing configuration for {App Name}.
 Would you like to:
 [A] Update specific settings
 [B] Start fresh with a new configuration
 [C] Keep existing and continue"
```
