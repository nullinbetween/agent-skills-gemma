---
name: disaster-help-card
description: Makes a Japanese help card to SHOW to shelter staff, rescuers or people nearby during a disaster or emergency in Japan (earthquake, injury, danger, someone following me, need water/food/milk, lost family, cannot speak Japanese). 防災・地震・救命・受傷・被跟蹤・避難所・不懂日文。Not for a planned clinic visit.
---

# Disaster Help Card

You only pick codes. The card already has verified Japanese and the user's language. Never write Japanese. Never give phone numbers.

## Call `run_js`
- script name: `index.html`
- data: JSON string:
  - `needs`: codes that match what the user said:
    `emergency` (life-threatening: not breathing, unconscious, heavy bleeding), `injured`, `family_injured` (someone with me is hurt), `sick`, `danger` (followed, attacked, threatened, unsafe person), `medicine`, `allergy`, `no_pork`, `vegetarian`, `water`, `food`, `with_child`, `formula`, `diapers`, `pregnant`, `mobility`, `lost_family`, `no_japanese`, `shelter`, `toilet`, `charging`, `other`
  - `allergens` (only with `allergy`): `egg`, `milk`, `wheat`, `shrimp`, `crab`, `buckwheat`, `peanut`, `walnut`, `soy`, `sesame`, `fish`
  - `interpreter`: ONLY if the user says which language they speak: `english`, `mandarin`, `cantonese`, `korean`, `vietnamese`, `nepali`, `tagalog`, `thai`, `indonesian`, `spanish`, `portuguese`, `french`
  - `unmatched`: short phrases (user's own words) for anything that fits no code
  - `ui_lang`: `zh`, `en` or `ja` = language the user wrote in

Example: 「地震 我被變態追 救命 我不懂日文」 → `{"needs":["danger","no_japanese"],"ui_lang":"zh"}`

## Rules
- If something fits no code, put it in `unmatched` and add `other`. Never pick a "closest" wrong code.
- "I can't speak Japanese" alone → `no_japanese` without `interpreter`.
- If the user just asks for the card with no details, call with `"needs":[]` — the card opens as a checklist.
- After the tool returns, reply in 1 sentence in the user's language: tap the card, check it, show it to the people around you.
