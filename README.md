# Gemma Edge Skills

Two agent skills for **Google AI Edge Gallery** (tested target: Gemma 4 E4B-it / E2B-it on phone), built for foreign residents in Japan.

**Supported languages: Traditional Chinese (繁體中文) and English only.** Users type in Chinese or English; every Japanese line on the card has a pre-written Traditional Chinese or English line under it so the user can check it. Other languages are not supported yet (the card falls back to English).

| Skill | What it does | Load URL |
|---|---|---|
| `disaster-help-card` | Say your situation in Chinese or English → a large Japanese help card to show shelter staff | `https://nullinbetween.github.io/agent-skills-gemma/disaster-help-card` |
| `sick-visit-memo` | Describe your child's illness → a Japanese 受診メモ for the pediatrician | `https://nullinbetween.github.io/agent-skills-gemma/sick-visit-memo` |

In the app: Agent Skills → Skills chip → (+) → load from URL → paste the URL above.

## Design idea: the small model is a router, not a writer

On-device 2B/4B models are fast and private but can hallucinate, especially in a second language. So:

1. **The model only picks codes and copies numbers** (`["water","allergy"]`, `temp: 38.5`).
2. **JavaScript validates** them: unknown codes are rejected and reported, impossible values (e.g. 385 °C) are flagged back to the model.
3. **All Japanese is pre-written** in `assets/*.html` (single source of truth, human-proofread). The model never generates Japanese text.

Everything runs offline in the app's webview — no network, no API key. Offline matters most in a disaster.

## Structure

```
<skill>/
├── SKILL.md            # instructions + code list for the model
├── scripts/index.html  # ai_edge_gallery_get_result(): validate → webview URL
└── assets/<card>.html  # renders the card; all Japanese text lives here
```

`.nojekyll` is required: without it GitHub Pages runs Jekyll and turns `SKILL.md` (it has front matter) into HTML.

## Limits

- Not a medical or emergency service. The skills only make cards to show to people on site; they never suggest phone numbers (a small model can mistype them).
- `sick-visit-memo` is a parent's record, not a diagnosis.
- ✕ removes an item from the card. To add something, the user just says it again in the chat, in their own words; the model re-issues the card with the full list. Anything the model could not map is shown to the user, never silently dropped.
- Japanese phrase list is a first version; additions welcome.
