# Project: Voice Assistant Skills Localization (Universal Rules)

**⚠️ IMPORTANT: This document contains universal localization rules applicable to ANY language, not just a specific language.**

This document describes the goals, rules, structure, and standards of the localization project for skills to any target language, plus practices for synchronizing intents and tests so CI remains green.

## Project Goals

- Translate and adapt skills to the target language with natural language constructions.
- Ensure 100% alignment between tests (`test/test_intents.yaml`) and intents (`locale/<target-lang>/intents/`) and vocab (`locale/<target-lang>/vocab/`).
- Maintain a consistent style and project structure across all skills.

## Repository Structure

Each skill lives in its own `<skill-name>_<lang>_translation` folder (e.g., `skill-name_ua_translation`, `skill-name_de_translation`).

```
<repo-root>/
├─ skill-*_<lang>_translation/
│  ├─ README.md                # optional per-skill readme
│  ├─ skill_<name>/
│  │  ├─ locale/
│  │  │  ├─ en-us/             # originals
│  │  │  └─ <target-lang>/     # translations (uk-ua, de-de, fr-fr, etc.)
│  │  │     ├─ dialog/         # assistant responses
│  │  │     ├─ intents/        # intent templates
│  │  │     └─ vocab/          # vocabularies and entities
│  │  └─ ...
│  └─ test/
│     ├─ test_intents.yaml     # test utterances
│     ├─ test_resources.yaml   # test resources/config
│     └─ ...
└─ docx/README.md              # project overview
└─ docx/README.en.md           # this file (English)
```

## Universal Localization Rules

### 1. Preserving Technical Elements

- **Preserve placeholders and variables** in dialogs and intents: `{service}`, `{dataset}`, `{time}`, etc.
- **Preserve formatting** of special symbols and data structures.
- **Do not translate** technical terms if they are part of an API or standard (e.g., protocol names).

### 2. Respecting Target Language Grammar

Each language has its own grammatical features that must be considered:

- **Gender of nouns** (masculine, feminine, neuter) — affects agreement of adjectives and verbs.
- **Cases** (nominative, genitive, dative, accusative, etc.) — affects endings and prepositions.
- **Number** (singular, plural) — affects word forms.
- **Verb tenses and moods** — affects sentence structure.
- **Articles** (for languages that have them: the, a, der, die, das, etc.).

### 3. Semantic Load of Sentences

**Critical:** Translation must convey not only the literal meaning but also the pragmatics, context, and intention of the original utterance.

#### Rules for preserving semantic load:

- **Preserve illocutionary force** of the utterance (request, command, question, statement).
  - Example: "Could you please..." → should be translated as a polite request, not as a question about possibility.
  
- **Consider context of use**:
  - Formal vs. informal address
  - Level of politeness (you/thou, formal/informal forms)
  - Register of communication (technical, everyday, official)

- **Preserve emotional tone**:
  - Neutral tone
  - Polite/official tone
  - Friendly/informal tone

- **Avoid literal translation** if it breaks naturalness:
  - Bad: literal translation of idioms and fixed expressions
  - Good: use equivalent idioms and constructions in the target language

### 4. Word Positioning in Sentences

**Word order** is critical for naturalness and correct understanding in each language.

#### Typical word orders by language:

- **SVO (Subject-Verb-Object)** — English, Spanish, French, Russian (basic order)
  - Example: "I turned on the device" / "Я включил устройство"

- **SOV (Subject-Object-Verb)** — Japanese, Korean, Turkish, Hindi
  - Example (Japanese): "私はデバイスをオンにしました" (I device on turned)

- **VSO (Verb-Subject-Object)** — Arabic, Irish, Welsh
  - Example (Arabic): "قمت بتشغيل الجهاز" (Turned I the device on)

- **V2 (Verb Second)** — German, Dutch (verb in second position in main clause)
  - Example (German): "Ich habe das Gerät eingeschaltet" (I have the device turned on)

#### Positioning rules:

- **Determiners and adjectives** must be in the correct position relative to the noun:
  - English: "the red car" (determiner + adjective + noun)
  - French: "la voiture rouge" (determiner + noun + adjective)
  - German: "das rote Auto" (determiner + adjective + noun)

- **Verb particles and prepositions** must be in the correct position:
  - English: "turn on" (separable phrasal verb)
  - German: "einschalten" (inseparable) vs. "anmachen" (separable: "mach...an")

- **Negation** must be in the correct position:
  - English: "I don't want" (negation before verb)
  - French: "Je ne veux pas" (double negation: ne...pas)
  - Russian: "Я не хочу" (negation before verb)

### 5. Sentence Structure

#### Types of sentences and their structure:

- **Declarative sentences**:
  - Must follow the basic word order of the target language
  - Must have correct punctuation (if applicable)

- **Interrogative sentences**:
  - **Questions with question words**: word order may differ from declarative
    - English: "What do you want?" (question word + auxiliary verb + subject)
    - Russian: "Что ты хочешь?" (question word + subject + predicate)
    - German: "Was willst du?" (question word + verb + subject)
  
  - **Yes/no questions**:
    - English: inversion ("Do you want?")
    - Russian: intonation or particle "ли" ("Ты хочешь?" / "Хочешь ли ты?")
    - German: inversion ("Willst du?")

- **Imperative sentences (commands)**:
  - English: base form of verb ("Turn on the device")
  - Russian: imperative mood ("Включи устройство")
  - German: base form of verb ("Schalte das Gerät ein")
  - French: imperative mood ("Allume l'appareil")

- **Complex sentences**:
  - Correct use of conjunctions and relative pronouns
  - Correct order of main and subordinate clauses
  - Tense agreement (if applicable)

#### Structural features by language:

- **Inflected languages** (Russian, German, Polish, etc.):
  - Word order is more flexible due to cases
  - Emphasis can be conveyed by word order
  - Example (Russian): "Я включил устройство" = "Устройство я включил" (emphasis on device)

- **Non-inflected languages** (English, French, etc.):
  - Word order is more fixed
  - Emphasis is conveyed by intonation or special constructions

- **Agglutinative languages** (Turkish, Finnish, Hungarian, etc.):
  - Much information is conveyed by suffixes
  - Word order can be more flexible

### 6. Avoiding Mixed Forms

- Avoid mixed forms (e.g., English-Ukrainian, English-German) in the base variant.
- If such forms are necessary for recognition (e.g., technical terms), add them to intent templates as alternatives.

## Critical: Synchronizing Tests and Intents

- Every phrase in `test/test_intents.yaml` must **literally** match at least one pattern in `locale/<target-lang>/intents/*.intent`.
- If a test fails, the root cause is usually a mismatch between the test phrase and an intent pattern.
- You may extend intents with additional lines/alternatives to cover all test phrases.

**Example:** if the test contains
```
rename device airplay
```
then the intent must include a pattern like
```
rename (my|) device airplay
```
not just
```
rename airplay
```

### Spacing Rule in Groups (important)

- **Do not place spaces inside optional groups**: bad — `(my |)`, good — `(my|)`
- **Put spaces outside groups** to avoid double or missing spaces.
- **Example fix:**
  - Before: `I want (you to |)clear ...`
  - After:  `(I want|) (you to|) clear ...`

## test_resources.yaml Requirements

- **Explicitly list languages** in every skill:
```yaml
languages: ["en-us", "<target-lang>"]
```
- Ensure skill resource paths are correct and match the project layout.

## Workflow for a New or Updated Skill

1. **Verify target language directories/files exist:**
   - `locale/<target-lang>/dialog/`
   - `locale/<target-lang>/intents/`
   - `locale/<target-lang>/vocab/`

2. **Translate missing dialogs, intents, vocab:**
   - Keep placeholders intact
   - Consider target language grammar
   - Check semantic load
   - Check word order
   - Check sentence structure

3. **Compare `test/test_intents.yaml` with intents:**
   - Every test phrase must match a concrete line in `.intent`
   - Add missing variants to `.intent` if needed

4. **Ensure all dialogs are present:**
   - All files from `en-us/dialog/` must have translations in `<target-lang>/dialog/`

5. **Run tests locally** (if environment is ready) or rely on CI.

## Common Errors and Fixes

- **"Phrase not recognized / ConfidenceTooLow":**
  - Add the exact phrase variant to the corresponding `.intent`
  - Check word order in the pattern
  - Check grammatical forms (gender, case, number)

- **"Wrong intent triggered (conflict)":**
  - Narrow the conflicting intent (add distinctive keywords/constructions)
  - Narrow the general pattern
  - Check word order — it might be too general

- **"Dialog file not found":**
  - Copy/translate missing files from `en-us/dialog/` to `<target-lang>/dialog/`

- **"Unnatural translation":**
  - Check semantic load — might be using literal translation
  - Check word order — might not match target language rules
  - Check sentence structure — might not match sentence type

## Intent Pattern Examples (Universal Principles)

### Extended Pattern with `{dataset}` Entity

When creating patterns, consider:
- **Synonym variants** in the target language
- **Grammatical forms** (gender, case, number)
- **Word order** of the target language
- **Natural constructions** of the target language

Example for SVO languages:
```
(clear|erase|delete|reset|remove) (all|) (my|) {dataset}
(I want|) (you to|) (clear|erase|delete|reset|remove) (all|) (my|) {dataset}
```

### Example for Pairing Connections

Consider prepositions and their position in the target language:
- English: "pair with bluetooth" (preposition after verb)
- German: "mit bluetooth koppeln" (preposition before object)
- Russian: "спарить с bluetooth" (preposition after verb)

### Example for Renaming Devices

Include all words that may appear in tests:
```
rename (my|) device airplay
rename (my|) device spotify
```

## Pre-PR / CI Checklist

- [ ] All files from `en-us/dialog/` have translations in `<target-lang>/dialog/`
- [ ] `test/test_intents.yaml` is fully covered by `<target-lang>/intents/`
- [ ] `test_resources.yaml` includes `languages: ["en-us", "<target-lang>"]`
- [ ] Conflicts between similar intents (enable/disable/rename, etc.) are checked
- [ ] All placeholders `{var}` are preserved in translations
- [ ] Target language grammar is checked (gender, cases, number, tenses)
- [ ] Semantic load of translations is checked
- [ ] Word order is checked according to target language rules
- [ ] Sentence structure is checked (questions, commands, statements)

## Translation Style Tips

### General Principles:

- **Use natural constructions** of the target language
- **Avoid literal translation** if it sounds unnatural
- **Consider context** of use (formal/informal, technical/everyday)
- **Preserve pragmatics** of the original utterance

### For Specific Languages:

- **Slavic languages** (Russian, Ukrainian, Polish, etc.):
  - Consider cases and declensions
  - Consider gender of nouns
  - Word order is more flexible, but there are preferred constructions

- **Germanic languages** (English, German, Dutch, etc.):
  - Word order is more fixed
  - In German — consider separable verbs and V2 word order

- **Romance languages** (French, Spanish, Italian, etc.):
  - Consider articles and their agreement
  - Consider position of adjectives (before/after noun)
  - Consider verb conjugations

- **Other language families:**
  - Study basic grammatical rules of the target language
  - Consult native speakers if necessary

---

**Important:** If you modify tests — be sure to extend the corresponding `.intent` files so tests don't fail due to mismatches.

**Remember:** Each language is unique. These rules are general principles that should be adapted to the specific features of the target language.
