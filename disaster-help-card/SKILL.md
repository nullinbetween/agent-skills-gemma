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
  - `ui_lang`: `zh` (Chinese, incl. Chinese mixed with English) or `en` (English). Use `ja` only if the user wrote in Japanese.

Example: 「地震 我被變態追 救命 我不懂日文」 → `{"needs":["danger","no_japanese"],"ui_lang":"zh"}`

## Rules
- Supported languages: Traditional Chinese and English only. If the user writes in another language, still make it with `"ui_lang":"en"` and say in English that only Traditional Chinese and English are supported.
- If something fits no code, put it in `unmatched` and add `other`. Never pick a "closest" wrong code.
- "I can't speak Japanese" alone → `no_japanese` without `interpreter`.
- If the user gives no details, do not call the tool; ask in their language what help they need.
- If the user adds or corrects something later, call `run_js` again with the FULL list: previous codes + new ones, minus anything the user said is wrong.
- After the tool returns, reply in 1 sentence in the user's language: tap the card, check it, show it to the people around you.
