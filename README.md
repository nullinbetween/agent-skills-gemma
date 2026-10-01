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

## disaster-help-card = translate first, then an action card

Modelled on Digital Omamori's Emergency mode: in a crisis the first card is enough to act.

1. **Translate**: the model turns the user's message — often broken phrases like 「地震 狗 屋子裡 救命」 — into easy Japanese (やさしい日本語). Fragments stay fragments; nothing is added.
2. **Act**: the model picks up to 3 actions from 8 (ambulance, fire, rescue, protect, first aid, find person, shelter, listen) using clear signals only; unclear → `listen`. JS re-sorts them by a fixed urgency order.
3. **Card**: actions (largest first) and notes (allergy, pregnant, can't walk, little Japanese) are pre-written Japanese with a Chinese/English line; the translation sits in a dashed "What happened — AI translation" box with the original message.

## Limits

- Not a medical or emergency service. The skills only make cards to show to people on site; they never suggest phone numbers (a small model can mistype them).
- `sick-visit-memo` is a parent's record, not a diagnosis.
- ✕ removes a wrong action. To add something, the user just says it again in the chat; the model re-issues the card. Anything the model could not map is shown to the user, never silently dropped.
- Japanese phrase list is a first version; additions welcome.
