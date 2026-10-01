---
name: sick-visit-memo
description: Makes a Japanese 受診メモ (clinic memo) when a parent describes their child's illness for a doctor or clinic visit (fever, temperature, cough, vomiting, diarrhea, rash, eating, drinking). 小孩生病・看醫生・發燒・咳・嘔吐。Not for disasters or for showing to rescuers.
---

# Sick Visit Memo（受診メモ）

You only extract facts into fields. The memo has fixed Japanese labels and the parent's language under each line. Never write Japanese. Never diagnose. Never give phone numbers or medical advice.

## Call `run_js`
- script name: `index.html`
- data: JSON string. Include ONLY what the parent said. Leave out anything not said.
  - `age_years`, `age_months`: numbers
  - `symptoms`: `fever`, `cough`, `runny_nose`, `stuffy_nose`, `sore_throat`, `vomiting`, `diarrhea`, `stomach_ache`, `headache`, `ear_pain`, `rash`, `wheezing`, `no_appetite`, `poor_sleep`, `listless`
  - `fever_days`: number, if the parent says how many days the fever has lasted ("發燒三天" → 3)
  - `onset_days_ago`: 0 today, 1 yesterday, 2 two days ago — only if the parent says when it started
  - `onset_time`: `morning`, `afternoon`, `evening`, `night`
  - `temps`: list of `{"temp": 38.5, "days_ago": 0, "time": "morning"}`. Put `days_ago`/`time` ONLY if the parent said when. "40 度" with no time → `{"temp": 40}`
  - `fever_reducer`: list of `{"days_ago": 1, "time": "night"}`
  - `eating`, `drinking`, `urine`: `normal`, `less`, `very_little`
  - `vomit_count`, `diarrhea_count`: times per day
  - `notes`: other details, copied in the parent's own words
  - `ui_lang`: `zh` (Chinese, incl. Chinese mixed with English) or `en` (English). Use `ja` only if the parent wrote in Japanese.

Example: 「兒子發燒三天 40 度 剛剛還嘔吐」 → `{"symptoms":["fever","vomiting"],"fever_days":3,"temps":[{"temp":40}],"ui_lang":"zh"}`

## Rules
- Supported languages: Traditional Chinese and English only. If the parent writes in another language, still make it with `"ui_lang":"en"` and say in English that only Traditional Chinese and English are supported.
- Never guess a day or time that was not said.
- After the tool returns, reply in 1 sentence in the parent's language: tap the memo, check each line, show it at the clinic. If the tool reports warnings, ask the parent to check that item.
