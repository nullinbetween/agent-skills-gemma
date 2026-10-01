---
name: disaster-help-card
description: Makes a Japanese ACTION card to SHOW to people nearby (staff, rescuers, neighbours) in a disaster or emergency in Japan — earthquake, fire, someone trapped, injury, can't breathe, someone following me, lost child, need shelter. Works with short broken phrases. 防災・地震・火災・救命・受傷・被跟蹤・找人・避難所。Not for a planned clinic visit.
---

# Disaster Help Card (action card)

Step 1: translate. Step 2: decide the actions. The card shows pre-written Japanese for actions and notes, plus your translation in a box marked "AI translation". Never give phone numbers.

## Step 1 — translate (`yasashii`)
Translate the user's WHOLE message into easy Japanese:
- Broken phrases stay broken. Do NOT add who, where or what the user did not say. 「地震 狗 屋子裡 救命」 → 地震（じしん）。犬（いぬ）。家（いえ）の 中（なか）。たすけて ください。
- Copy numbers, ages, floors, colours, names exactly.
- Short sentences, です/ます, no keigo, a space between words, reading after every kanji word: 火事（かじ）.

## Step 2 — actions (max 3, most urgent first)
Pick ONLY from clear signals:
- `ambulance`: not breathing, no response, unconscious, heavy bleeding
- `fire`: fire, smoke
- `rescue`: a person is trapped / pinned / can't get out
- `protect`: someone is following, attacking or threatening the user
- `first_aid`: injured, bleeding (not heavy)
- `find_person`: can't find someone, separated
- `shelter`: needs to go to a shelter
- `listen`: anything else or unclear. Better `listen` than a wrong action.

Notes (not actions): `allergy` (+ `allergens`: `egg`,`milk`,`wheat`,`shrimp`,`crab`,`buckwheat`,`peanut`,`walnut`,`soy`,`sesame`,`fish`), `pregnant`, `mobility` (can't walk), `no_japanese` (+ `interpreter` ONLY if the user names their language: `english`,`mandarin`,`cantonese`,`korean`,`vietnamese`,`nepali`,`tagalog`,`thai`,`indonesian`,`spanish`,`portuguese`,`french`).

## Call `run_js`
- script name: `index.html`
- data: JSON string with `actions`, `notes`, `allergens`, `interpreter`, `yasashii`, `original` (the user's message copied exactly), `ui_lang` (`zh` for Chinese incl. Cantonese and Chinese mixed with English; `en` for English; `ja` only if written in Japanese)

Example: 「火 火 三樓 老人 走唔到」 →
`{"actions":["fire","rescue"],"notes":["mobility"],"yasashii":"火事（かじ）。3階（さんがい）。お年寄（としよ）り。歩（ある）けません。","original":"火 火 三樓 老人 走唔到","ui_lang":"zh"}`

## Rules
- Supported languages: Traditional Chinese (incl. Cantonese) and English. Other languages: still make the card with `"ui_lang":"en"` and say only Chinese and English are supported.
- If the user gives no details at all, do not call the tool; ask what happened.
- If the user adds or corrects something, call again with the full updated actions and a translation of everything said so far.
- After the tool returns, reply in 1 sentence in the user's language: tap the card and show it to the people around you.
