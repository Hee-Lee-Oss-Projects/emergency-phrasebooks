# PLAN — emergency-phrasebooks

> Status: Draft · Version: 0.2.0 · Last updated: 2026-06-29 · Owner: J. Carter (acting maintainer) · Lane: donated

## Executive summary

`emergency-phrasebooks` is an Elyos good-deed project that produces **pre-translated, expert-reviewed
emergency phrase packs** — short, action-oriented utterances that let a **first responder or
frontline worker** and a **newcomer with limited proficiency in the local language** establish
first contact in a crisis: medical/triage, fire/rescue, natural disaster, and shelter/intake
situations. The output is a set of **offline-first, printable pocket cards** (plus their underlying
open data) in many languages, each phrase reviewed by a **qualified speaker** and — for
medically-sensitive phrases — by a **bilingual clinician or qualified medical interpreter**.

The project's single most important framing constraint is this: **a phrasebook is a first-contact
bridge, never a substitute for a qualified human interpreter.** Over-reliance on phrasebooks in
high-stakes settings (especially healthcare) is a known patient-safety hazard and, in many
jurisdictions, a failure to provide legally required language access. Every pack therefore carries a
prominent, non-removable disclaimer to that effect, and the project is scoped so it **supplements**
professional interpretation at the moment of first contact — buying minutes until a qualified
interpreter, telephonic interpreting line, or bilingual staff member is reached.

The work runs in the **donated lane**: a human runs their own coding agent interactively to draft
phrase translations, pack layouts, and supporting data, then opens PRs; the Elyos CLI only prepares
workspaces and opens PRs (it never runs an agent headless and never authenticates one). The project
is **medium risk tier** under the good-deed definition (it needs domain accuracy and a qualified
reviewer), with **two hardened sub-tiers** layered on top: a **medical-critical** sub-tier requiring
clinician/medical-interpreter review and mandatory back-translation, and an explicitly **excluded
legal/rights sub-tier** that would be **high risk** (police caution, rights-on-arrest, asylum, any
"sign here" / consent-with-legal-effect phrase) and is **out of scope for v0.1** unless and until a
**licensed attorney review gate** and jurisdiction sourcing are stood up.

The defining engineering discipline is **license/provenance rigor** and **criticality-aware review**.
Source phrases and pictograms are admitted only from **open / public-domain / CC-licensed** sources
recorded on a per-source allow-list (or are **originally authored** as plain functional phrases to
minimize IP risk); translated content ships with full provenance, attribution, and the mandatory
disclaimer; and each phrase carries a **criticality tier (A/B/C)** that determines how many
reviewers and whether back-translation and clinician review are required before it can ship.

Honest status note: the program is real but **no responder organization, clinic, shelter, or
language-access partner is yet secured** for field delivery. Until one is, `verifiedNeed` is recorded
as `false` on every task whose value depends on a named beneficiary, and the project's Definition of
Shipped (a pack **adopted and used in the field**) cannot be met. Securing a first partner — and
field-testing usability with real responders/newcomers — is the top open dependency (see Open
questions and Roadmap M2). Partner: **TO BE SECURED**.

## Problem & beneficiaries

**Who is helped.** Two linked groups, at the single most stressful moment of contact:

- **First responders & frontline workers** — paramedics/EMS, ER triage nurses, firefighters,
  search-and-rescue, disaster-shelter intake staff, free-clinic and school-newcomer staff — who must
  communicate urgently with someone who does not share their language and for whom **no interpreter is
  available in the first minutes**.
- **Newcomers & people with limited local-language proficiency** — refugees, recently-arrived
  migrants, travelers, and isolated language-minority residents — who must convey an emergency ("my
  child can't breathe," "I'm allergic to penicillin," "there's a fire on the third floor") or
  understand a critical instruction ("stay still, help is coming," "do you have chest pain?").

The **ultimate beneficiaries** are people making or receiving **life-or-death communication** when
the normal language-access infrastructure has not yet engaged.

**The need.** Qualified interpreters are the correct tool, but they are **not instantly available**
at a roadside, a fire scene, a 2 a.m. ER arrival, or a chaotic shelter intake. In that gap,
responders today improvise with generic machine translation on a phone (often offline/unreliable,
and **unsafe for medical content** — it mistranslates negations, dosages, allergies, and consent),
with untrained ad-hoc bilinguals (including **minor children**, which is a recognized harm), or with
nothing. The gap this project fills is a **vetted, offline, point-and-speak bridge** for the first
minutes — accurate, attributed, reviewed, and explicitly bounded so it is **not** mistaken for, or
used in place of, professional interpretation.

**Verified need: TO BE SECURED.** The *category* need (language barriers in emergencies cause
documented harm and delay) is well-evidenced. But a **specific requesting organization, its
deployment locale(s), its priority languages, and its priority scenarios are not yet secured.** Tasks
are written so the program can build reusable, partner-independent foundations (phrase bank,
authoring rules, criticality rubric, glossary, review machinery, print templates) **without** a
partner, while clearly flagging that **delivery, localization, and field-usability tasks are blocked**
until a partner and deployment context are confirmed.

**Partner org: TO BE SECURED.** Candidate partner types: refugee-resettlement agencies (e.g. IRC,
LIRS, USCRI and national equivalents), hospital/health-system **language-access offices**, EMS/fire
departments and emergency-management agencies, Red Cross / Red Crescent societies, free and migrant
clinics, school-district newcomer programs, and translation NGOs (Translators without Borders /
CLEAR Global — also a reviewer-partner candidate). **None is committed as of this draft.**

## Goals and non-goals

**Goals**
- Stand up a **repeatable pipeline** that turns a canonical, English-pivot **phrase bank** into
  expert-reviewed, openly-licensed **emergency phrase packs** in many languages, with full provenance
  and license compliance.
- Maintain a **criticality-tiered phrase model (A/B/C)** so that medically- and safety-critical
  phrases receive proportionally stronger review (second reviewer, back-translation,
  clinician/medical-interpreter sign-off).
- Ship packs as **offline-first, low-tech, printable pocket cards** that work with no power, no
  network, and no app — usable by responders and by low-literacy readers (pictogram + point-to
  support).
- Make the **"bridge, not a substitute for an interpreter"** framing structurally unavoidable: a
  mandatory disclaimer on every pack and a refusal to ship phrases that would encourage misuse.
- Deliver pack sets that a partner responder org / clinic / shelter **actually adopts and uses in the
  field**, and confirm — through usability feedback — that they help.

**Non-goals**
- **Not** a real-time interpreting app, a chatbot, a translation API, or a replacement for qualified
  human interpreters or telephonic/video interpreting lines. We **explicitly de-position** the
  product against those uses.
- **Not** medical, legal, immigration, or financial **advice**. Medical phrases are for
  **communication only** — eliciting information and giving non-clinical directions ("stay still,"
  "are you allergic to any medicine?"). They **never** instruct a patient on treatment, dosing, or
  diagnosis. (Authoring clinical instructions would be **high** risk and is out of scope.)
- **Not** any phrase with legal effect or in a law-enforcement-adversarial setting — police caution,
  rights-on-arrest, "anything you say can be used…", asylum/immigration claims, or "sign here"
  consent with legal consequence. These are **high-risk legal content** and are **excluded from v0.1**
  pending an attorney-review gate (see Quality gates).
- **Not** relicensing or "freeing" copyrighted source material; each source's terms are honored, and
  we prefer **originally-authored** functional phrases to avoid IP entanglement.
- **Not** surveillance, deception, coercion, or immigration-status elicitation; no phrase that could
  be weaponized against the beneficiary is admitted (see Refusal guardrails / §Security).
- **Not** an audio/voice product in v0.1 (pronunciation audio is a sequenced backlog item, text-first
  for now). **Exception (from competitive review):** minimal **romanized pronunciation for Tier-C
  phrases** is pulled forward to M0 — being understood under stress, including by low-literacy or
  non-Latin-script readers, is core to the responder↔newcomer value, not a backlog nicety.

## Competitive landscape & differentiation

The category is served, but **no existing offering occupies the quadrant this project targets: open
*and* clinically-reviewed *and* criticality-tiered, as an offline pocket point-to card with auditable
provenance.** The landscape (analyst pass, cited in COMPETITIVE-ANALYSIS.md) sorts into *closest
competitors*, *cautionary baseline*, and *partners/complements*.

**Closest open competitor — Refugee Phrasebook (RefugePhrasebook.de).** Volunteer / Open Knowledge
Foundation Germany project; 28+ languages incl. medical and juridical phrases, released **CC0**,
printable, broad reach. *Contrast:* it is **crowd-sourced without a structured clinical/criticality
review gate** — no per-phrase back-translation, no clinician sign-off, no A/B/C tiering, and it
includes the legal/juridical phrases this project deliberately gates out. This is our clearest
contrast point: same openness, without the auditable safety discipline.

**Closest commercial product analog — Kwikpoint Medical/EMS Visual Language Translator.** Patented,
battle-tested point-to-picture laminated cards (military/EMS/hospital), waterproof, with a
language-identification panel to *find an interpreter*. *Contrast:* **proprietary/closed — not
open-licensed, not freely redistributable or locally customizable, no open provenance/review
transparency.** It validates the format we are building; our open + reviewed + tiered + provenance-
bearing angle is the differentiator. (Its language-ID "find an interpreter" panel is worth emulating
— see Adjacent opportunities.)

**Cautionary baseline — Google Translate (what responders improvise with today).** Ubiquitous, free,
offline packs, 100+ languages — and **unsafe for high-stakes emergency content**. A 2025 *Journal of
Emergency Medicine* study of ED discharge instructions found wide language variance (>90% accurate for
Spanish, dropping to ~67% Farsi and ~55% Armenian), and the ATA concluded it "still isn't good enough"
for medical instructions. This is the core evidentiary argument for why **raw machine translation is
never shipped for emergencies** and why reviewed packs exist; the same suspicion applies to any LLM
draft (see §Solution approach — Claude leverage).

**Professional / institutional actors — best treated as partners/complements, not competitors.**
- **Translators without Borders / CLEAR Global** ("Words of Relief" + TWB Glossaries) — the
  professional gold standard for crisis-language infrastructure (diaspora translator network,
  terminology standardization). **Best reviewer-network + glossary partner candidate.**
- **Tarjimly** — on-demand human interpreters (120+ languages, HIPAA-compliant) — exactly the
  qualified interpreter the pack **bridges to**; requires connectivity and availability, so it fails in
  the offline first minutes our cards target. **Complement.**
- **American Red Cross / IFRC first-aid & hazard apps** — trusted, offline-capable first-aid
  *instruction to the responder* (largely EN/ES, app-dependent), not cross-language point-to-speak.
  **Partner / distribution candidate.**
- **MedlinePlus (NLM), UNHCR, Save the Children, CDC, ICRC/Red Crescent** — authoritative multilingual
  health/orientation *documents and emergency-comms guidance*, not first-contact pocket cards. **Ideal
  sources/partners,** not competitors.

**Differentiators to win (priority order):**
1. **The empty quadrant — "open + reviewed + criticality-tiered."** Free/forkable like Refugee
   Phrasebook, with Kwikpoint-grade (or higher) review discipline and an auditable A/B/C safety trail.
   *Single strongest differentiator.*
2. **Auditable safety, not asserted safety** — CI-enforced disclaimer + Tier-C back-translation +
   reviewer/back-translator independence + verified locale data; a partner can **inspect the provenance**
   of every phrase.
3. **Offline-first paper runtime** — works at a roadside/shelter with no power, network, or app, where
   Google Translate / Tarjimly / Red Cross apps degrade.
4. **Honest scope** — explicitly de-positioned against MT and against replacing interpreters; a trust
   and adoption advantage with health-system language-access offices bound by ACA **§1557** (which
   requires *qualified* interpreters and generally bars reliance on ad-hoc/family interpreters except
   true emergencies).
5. **Reusable high-stakes translation pipeline** as a portfolio asset shared with sibling projects
   (see Adjacent opportunities).

## Success metrics (outcomes)

Outcome-based and beneficiary-centric. Baselines are ~0 (new project). **Outcome** targets are for
the first ~6 months after a partner is secured; **interim foundation metrics** are tracked from
M0/M1 so progress is visible *before* a partner exists. We explicitly **do not** count "phrases
translated," "packs produced," or "PRs merged" as success — only **reviewed, correctly-licensed packs
adopted and used by a beneficiary org, with zero post-delivery critical defects.**

**Outcome metrics (post-partner)**

| Metric | Baseline | Target | How measured |
|---|---|---|---|
| Reviewed phrase **packs adopted by a partner** for field use | 0 | ≥ 2 scenario packs in ≥ 1 language | Partner written confirmation in PR/receipt |
| Languages covered **end-to-end** (phrase bank → reviewed → delivered) | 0 | ≥ 2 | Project registry |
| **Critical (Tier C) defects** found *after* delivery (allergy/negation/consent/number errors) | n/a | **0** (hard target) | Post-delivery defect log + partner feedback |
| Packs passing **qualified-speaker sign-off** on first or second pass | n/a | ≥ 90% (**effective from M1**) | Reviewer checklist outcomes |
| **License/attribution/disclaimer compliance** of delivered packs | n/a | 100% (hard gate; automated from M1, manual + structural check in M0) | License-check task / CI per deliverable |
| Partner-confirmed **field usability** (a responder/newcomer found the pack usable in a real or simulated first-contact) | n/a | Positive from ≥ 1 partner | Field-usability feedback (M2) |
| **Navigability** — time-to-find a target phrase / time-to-first-contact in simulated trials (a correct-but-unnavigable card fails in the field) | n/a | Baseline + downward trend; partner-acceptable | Timed simulated first-contact trials (M2 fieldtest) |

**Interim foundation metrics (M0/M1, partner-independent)**

| Metric | Baseline | Target | How measured |
|---|---|---|---|
| Phrase-bank entries authored + tiered (A/B/C) | 0 | ≥ 60 across ≥ 3 scenarios by end of M0 | Phrase-bank file count |
| Sources/pictograms **verified** on the allow-list | 0 | ≥ 3 by end of M0 | Allow-list entries with `verifiedBy`/`verifiedDate` |
| Glossary terms captured (per language pair) | 0 | ≥ 25 per pair | Glossary entry count |
| Qualified reviewers onboarded (incl. ≥ 1 medical-interpreter-capable for Tier C) | 0 | ≥ 2 by end of M1 | Reviewer roster |
| Cycle time per pack (draft → sign-off → license-clear) | n/a | Tracked; trend down | Timestamps on PR/review artifacts |

**Denominator & small-sample rule for the "≥ 90% pass" rate.** The denominator is **every pack
deliverable that reaches qualified review** (counted once per deliverable); a deliverable "passes" if
it earns sign-off on its **first or second** pass. The rate is reported **only once the denominator is
≥ 10** reviewed deliverables; below that we report the raw count (e.g. "7/8 passed") to avoid
small-sample noise. The **Tier-C critical-defect target is absolute (0)** and is **not** subject to a
sample threshold.

## Scope

**Phrase criticality tiers (the core scoping device).** Every phrase is classified, in the phrase
bank, into one of three tiers that drive review intensity (see Quality gates). **The cut is by
*harm-on-mistranslation*, not by topic** (per competitive review): a phrase whose misunderstanding
could plausibly injure or kill is Tier C **even if it reads like an "operational" instruction.** When
in doubt between two tiers, classify **up**.
- **Tier C — critical:** a mistranslation could cause **direct harm**. Allergy/medication-allergy,
  consent-to-treatment (non-legal, clinical), numbers/dosing-context, "do not move / do not give food
  or water," choking/breathing, severe-bleeding, pregnancy/labor — **and time-critical safety/hazard
  instructions whose misunderstanding is life-threatening** ("evacuate now," "is anyone still inside?",
  "do not enter / get out"). *Strongest review.*
- **Tier B — sensitive:** triage questions and operational instructions that are harmful-if-wrong but
  not immediately life-threatening ("where does it hurt?", "how long have you felt this way?").
  *Two-reviewer or one + back-translation; **medical/triage Tier-B phrases additionally inherit the
  medical-capable-reviewer requirement**.*
- **Tier A — routine:** orientation and basic needs ("what is your name?", "where is the toilet?",
  "are you hungry/thirsty/cold?"). *Single qualified reviewer.*

**Where exactly the B/C line falls for operational hazard instructions is set by a clinician + a
responder together** (M1 rubric refinement) — see Open questions.

**Scenario coverage (in scope).** Medical/triage, fire/rescue & evacuation, natural-disaster &
shelter, and basic-needs/intake. A **scenario coverage matrix** (M0) enumerates the exact phrase set
per scenario and marks each phrase's tier and any preserved tokens.

**In scope**
- An English-pivot **phrase bank** (canonical IDs, scenario, tier, context-of-use note, register
  notes, preserved tokens) authored to **cross-cultural-clarity guidelines** (no idioms, no
  yes/no-gesture ambiguity, no negations that invert under translation without flagging).
- **Translation** of specific phrase packs into named target languages, decomposed per
  *scenario-pack × language*.
- Per-language-pair **glossary** of emergency/medical terminology, register/politeness and
  grammatical-gender handling, and transliteration rules.
- **Pictogram / point-to** assets (body diagram, hazard icons) sourced **open-licensed** or authored,
  to support low-literacy and no-shared-script communication.
- **Locale data** per deployment region (correct **emergency number** — 911/112/999/etc. — and any
  region-specific protocol note), because the same language deploys to different locales.
- **Print-first pack templates** (B/W-printable, fold-to-pocket PDF + Markdown source), plus
  **accessibility variants** (large-print / dyslexia-friendly / high-contrast).
- Source/pictogram **allow-list**, **per-deliverable license & attribution check**, and **provenance**
  for every pack.
- **Review artifacts**: qualified-speaker sign-off, back-translation for Tier C, clinician/medical-
  interpreter sign-off for medical-critical phrases, agent uncertainty flags.

**Out of scope**
- Real-time interpreting, voice/chatbot, OCR, or any networked runtime; a hosted website or
  self-serve portal.
- **Legal/rights/consent-with-legal-effect** and **law-enforcement-adversarial** phrases (high risk;
  excluded pending an attorney-review gate — see Quality gates).
- Clinical **advice/instruction** to patients (dosing, treatment, diagnosis) — communication only.
- Audio/voice deliverables (sequenced backlog; text + transliteration first).
- Any phrase sourced from material whose license is unverified or incompatible.
- Languages or locales for which **no qualified reviewer** (and, for Tier C, no medical-capable
  reviewer) can be sourced — we will **not** ship unreviewed emergency phrases.
- Collecting or storing any **end-user / patient data**; the packs are static reference artifacts.

## Solution approach & architecture

This is primarily a **content/data pipeline** project (deliverables are phrase packs + open data),
with light tooling (schema validation, license lint, print rendering). It rides on existing Elyos
donated-lane mechanics (CLI prepares workspace, human runs agent, PR opened, human/expert review
gates "done").

**Pipeline (per scenario-pack × language × locale)**
1. **Author / admit phrases (pivot).** Author or admit (from an allow-listed source) the canonical
   English phrases for the scenario; assign **tier (A/B/C)**, context-of-use note, register note, and
   **preserved tokens** (numbers, units, drug names, body parts, emergency number). Run them through
   the **cross-cultural-clarity checklist**.
2. **Verify source & license.** For any admitted (non-original) phrase or pictogram, confirm it is on
   the allow-list and re-verify terms; record provenance + license snapshot. For original phrases,
   record authorship + chosen output license.
3. **Draft translation.** The agent drafts using the language-pair **glossary**, preserving tokens
   and honoring register/gender notes; emits **`UNCERTAIN:` flags** for anything uncertain.
4. **Self-check.** Agent runs the reviewer checklist as a first pass and lists open flags.
5. **Qualified review.** A qualified speaker verifies accuracy/safety/register, completes the
   checklist (incl. the **pictogram-comprehension check** — does a naive target-language reader read the
   icon, and any gesture/point-to interaction, as intended?), and **records sign-off in the PR**.
   **Tier B/C** add an **independent second reviewer** (medical/triage Tier-B inherits the
   medical-capable reviewer); **Tier C** uses a **true ISO-style two-person flow — independent forward
   translator + independent reviewer + an independent back-translator (not the forward translator) +
   recorded reconciliation** — with a committed **back-translation diff report** (negation/number/
   false-friend divergences surfaced for human adjudication) and, for **medical-critical** phrases, a
   **bilingual clinician or qualified medical-interpreter** sign-off. Back-translation **flags**; it
   never self-certifies — a human reconciles every divergence.
6. **License & attribution check.** Confirm attribution + the **mandatory disclaimer** are present and
   verbatim; confirm output license is compatible with any source.
7. **Localize & assemble.** Inject locale data (emergency number/protocol), render the **print
   template** (+ accessibility variant), attach pictograms, and produce the pocket-card PDF + source.
8. **Deliver & confirm field use.** Hand off to the partner; run a **field-usability check**; record
   adoption and any post-delivery defects.

**Artifacts / data model**
- `phrasebank/<scenario>.yaml` — array of phrase entries:
  `{ id, scenario, tier(A|B|C), english, contextOfUse, register, preserveTokens[], pictogramRef?,
  sourceId?(allow-list)|original:true, notes }`.
- `sources/allow-list.yaml` — per-source entries (phrases + pictograms):
  `{ id, name, url, licenseName, licenseUrl, reuseTerms, derivativesAllowed, requiresDisclaimer,
  disclaimerText, attributionTemplate, snapshotHash, snapshotArchiveUrl, verifiedBy, verifiedDate,
  notes }` (`snapshotHash` = SHA-256 of captured license/source text).
- `glossaries/<src>-<tgt>.yaml` — `{ term, translation|preserve, partOfSpeech, register, genderForms?,
  notes }` plus transliteration/units conventions.
- `locales/<region>.yaml` — `{ region, emergencyNumber, protocolNotes, verifiedBy, verifiedDate }`.
- `packs/<scenario>/<lang>/<locale>/` — `phrases.yaml` (translations keyed by phrase id, with
  **minimal romanized pronunciation for Tier-C phrases** from M0), `pack.pdf` + `pack.md`,
  `provenance.yaml`, `attribution.txt` (incl. mandatory disclaimer), `review.yaml` (checklist +
  sign-offs + `agentFlags` + back-translation record **and the back-translation diff report** for Tier
  C + pictogram-comprehension result + back-translator-independence declaration).
- `templates/` — `reviewer-checklist.md`, `reviewer-handoff.md`, `print-pocketcard.*`,
  `accessibility-variant.*`, `phrase-authoring-guidelines.md`, `criticality-rubric.md`,
  `disclaimer.md` (the canonical, mandatory disclaimer text).

**Formats.** UTF-8 throughout; YAML for data; Markdown as canonical pack source; PDF as the
delivered print artifact (B/W-safe, fold-to-pocket). Complex-script rendering is added per partner
need (backlog).

**Content schemas & CI validation.** The Task JSON schema lives in `packages/schema/src/schemas.ts`
(AJV / JSON Schema **draft-07**). This project's content/data artifacts get their **own JSON Schemas
in the same place** — `phraseBankSchema`, `allowListSchema`, `glossarySchema`, `localeSchema`,
`provenanceSchema`, `reviewSchema` — compiled and exposed via `validate.ts` exactly like
`taskSchema`/`registrySchema`. A structural-check script (`tooling-001`) parses the YAML artifacts to
JSON and validates them, wired into `pnpm test` so **CI fails on any malformed or non-conformant
content file** (e.g. a Tier-C phrase missing a back-translation record, or a pack missing the
disclaimer). The license gate (`license-002`) builds on this. Validation stays **agent-neutral**, in
the core schema package, never in adapters.

**Key decisions**
- **Phrase bank is the single source of truth.** Canonical English phrases with stable IDs; every
  translation maps 1:1 to an ID so updates and errata propagate deterministically across languages.
- **Criticality-as-data.** The A/B/C tier is a structured field that the review workflow and CI key
  off — not prose — so the stronger-review requirement for Tier C is enforceable.
- **License-as-data + prefer original phrases.** Short functional phrases are authored originally
  where practical (minimizing IP risk and enabling permissive output licensing); admitted material is
  allow-listed with recorded terms.
- **Offline-first, print-first.** The runtime is paper. No app, no network, no power dependency — the
  conditions of an actual emergency.
- **Disclaimer is non-removable.** Every rendered pack embeds the canonical "first-contact bridge,
  not a substitute for a qualified interpreter" disclaimer; CI fails any pack lacking it.
- **No data ingestion / no PII.** We pull only from public, allow-listed sources and collect no
  end-user data.

**Claude / agent leverage (donated lane, human-run, draft-only).** The human's own coding agent
accelerates the *drafting and QA-surfacing* steps; **every output is an unverified draft treated with
the same suspicion as raw MT.** Useful work:
- **Phrase-bank & scenario authoring** — draft the English-pivot canonical set, propose A/B/C tier +
  context-of-use + preserved tokens, and run the cross-cultural-clarity checklist (idioms, inverting
  negations, yes/no-gesture ambiguity) as a first pass.
- **Translation drafts + structured back-translation QA** — draft a translation honoring
  glossary/register/gender + preserved tokens, then **independently back-translate and emit the diff/
  semantic-divergence report** (negation inversion, number/unit drift, false friends) for the human
  reviewer to adjudicate. Generating the *QA artifact* is where the agent adds the most safety value.
- **Glossary/termbase bootstrapping**, **license/provenance drafting** (allow-list entries, attribution,
  flagging NC/SA/IGO terms + mandatory disclaimers for human verification), **pictogram matching &
  gap-finding** (+ proposed comprehension-test prompts), and **consistency lint** (phrase-ID drift
  across languages; pre-CI sanity for missing disclaimer/back-translation), with typed
  `UNCERTAIN:` flags into `review.yaml`.

**Hard gates the agent must NOT cross (non-negotiable):**
- **No high-stakes emergency phrase ships on model output alone — ever.** Native/near-native +
  (Tier C, and medical/triage Tier B) clinician/medical-interpreter sign-off + back-translation is
  mandatory. The agent drafts; humans certify.
- **Never machine-translation-only for high-tier phrases** — the Google-Translate ED evidence
  (55–67% accuracy in some languages) applies equally to any LLM.
- The agent **never** adjudicates the "not a substitute for an interpreter" boundary, relaxes the
  disclaimer, authors clinical advice (dosing/treatment/diagnosis) or excluded legal/coercive phrases,
  **clears a license** (`license-000` is signed by the human license reviewer), or **verifies an
  emergency number/locale** (official-source human verification only). Back-translation **flags, never
  passes**.

## Data, licensing & compliance

**This is the project's most important section alongside Quality gates. Be conservative; when terms
are unclear, do not use the source.**

**Sources & licenses (per-source allow-list).** Only **open / public-domain / CC-licensed** sources
may be admitted, each **verified and recorded** before use, OR phrases are **originally authored** for
this project. Categories and their typical (must-verify-per-item) terms:
- **Originally-authored functional phrases** — preferred. Short, utilitarian emergency phrases authored
  for the project; released under the project content license (default **CC-BY-SA 4.0**, with **CC0**
  considered for maximum reuse). Low IP risk; still subject to the full review gates.
- **Government/PD emergency phrase lists** (e.g. US-government works that are **public domain**) — verify
  per item; PD source may still embed third-party material.
- **WHO / health-authority phrasing** — frequently **CC BY-NC-SA 3.0 IGO** (NOT generic CC-BY) and may
  carry a **mandatory translation disclaimer**; NC/derivative terms must be honored. **Verify per
  publication.**
- **Open pictogram / icon sets** (e.g. CC-BY or CC0 humanitarian/wayfinding icon libraries) — verify the
  exact license and required attribution per asset; never assume CC0.
- **NGO/Red Cross/other** — terms vary, often all-rights-reserved; **verify and obtain written
  permission** or do not use.

For **each** admitted source we record: canonical URL, version/date, retrieval date, license name +
URL, a **snapshot of the license text**, derivatives/translation permitted (Y/N), required
attribution string, and any mandatory disclaimer.

**Snapshot integrity (hash/archive-based).** Each allow-list entry stores the captured license/source
text as a committed copy plus a **SHA-256 `snapshotHash`** and, where possible, a web-archive
(Wayback) URL. A **source-change watcher** (minimal manual re-fetch in M0, automated in M1)
periodically re-fetches each source, recomputes the hash, and **flags drift** for re-verification.

**BLOCKING prerequisite before the first translated pack.** Before `translate-001` may start, the
maintainer/license reviewer must have **confirmed in writing on the relevant allow-list entries (or
recorded original authorship)**: (a) every phrase/pictogram in the pack is **clearable for derivative
works/translation and open release**, and (b) the **exact disclaimer wording** required (the canonical
project disclaimer, plus any source-mandated disclaimer such as WHO's). This is a **hard gate**: any
phrase/asset that cannot be cleared is **dropped or replaced** before drafting.

**Locale accuracy is a data-compliance item, not just content.** The **emergency number** and any
protocol note in `locales/<region>.yaml` must be **verified against an official source** with
`verifiedBy`/`verifiedDate`; a wrong emergency number is a safety defect. Locale data is re-verified
on the same cadence as sources.

**Provenance model.** Every pack ships `provenance.yaml` linking it to phrase-bank IDs, source entries
(or original-authorship records), versions, retrieval dates, glossary + locale versions, translator
(agent + human), and **all reviewers + sign-offs** (including back-translation and clinician sign-off
for Tier C). Provenance is non-optional and part of the license gate.

**Output licensing.** Project **code/tooling is MIT**. **Content** (phrase bank, packs, glossaries) is
released under **CC-BY-SA 4.0** by default (CC0 considered for maximum reuse) **except** where an
admitted source requires otherwise — then the derived content **inherits the source-compatible
license + any mandatory disclaimer** (e.g. WHO-derived phrasing → BY-NC-SA-style + WHO disclaimer,
**never** a more permissive relicense). Pictograms retain their own license + attribution. Mixed-
license packs declare every component license in `attribution.txt`.

**Privacy / PII.** Packs are static reference artifacts: **no personal data is ingested or collected**,
and example scenarios use **synthetic placeholders only** (no real names, no real case details).
Partner contact details are handled out-of-band and never committed. There is **no end-user
telemetry**.

**Non-partisan & non-coercive content rule.** Content stays strictly neutral and excludes any
immigration-status, partisan, enforcement, or coercive phrasing (see Refusal guardrails / Security).
This is a compliance gate, not a style preference.

**Attribution requirements.** Every pack includes the source attribution string(s), any mandatory
source disclaimer verbatim, and the **canonical project disclaimer**, in both the target language and
a pivot language.

## Quality, review & risk gates

**Risk tier: medium** (needs domain accuracy + a qualified reviewer), with two layered sub-tiers:

- **Medical-critical sub-tier (hardened medium):** Tier-C medical phrases (allergy, consent-to-
  treatment, breathing/bleeding/labor, numbers-in-clinical-context) **and time-critical safety/hazard
  Tier-C instructions** additionally require a **bilingual clinician or qualified medical-interpreter**
  sign-off and **mandatory back-translation QA**. **Medical/triage Tier-B phrases also inherit the
  medical-capable-reviewer requirement** (a misunderstood triage question carries real harm). This is
  the project's most safety-sensitive content and is the reason the project does not treat all medium
  content identically.
- **Back-translator independence (explicit).** For Tier C the back-translation must be performed by
  someone **other than the forward translator** (parallel to the existing reviewer-independence rule);
  the same person's mental model can mask systematic errors in both forward and check. The flow is
  **independent forward translator → independent reviewer → independent back-translator →
  reconciliation**, all recorded in `review.yaml` with a committed **back-translation diff report**.
- **Excluded high-risk legal sub-tier (gated out):** Any phrase with **legal effect** or in a
  **law-enforcement-adversarial** context (rights-on-arrest, police caution, asylum/immigration claims,
  legal consent/"sign here") is **high risk** and **excluded from v0.1**. It may only ever be
  considered behind a **mandatory licensed-attorney review gate**, with **"informational, not legal
  advice" framing** and **jurisdiction-specific sourcing** — and even then is a separate, deliberately
  deferred workstream (see Open questions). Until that gate exists, such phrases are **refused**.

**Required review before a pack is "done"**
1. **Agent self-check** against the reviewer checklist, including the operationalized **uncertainty
   self-check** (below).
2. **Qualified-speaker sign-off** recorded in the PR (accuracy, safety, preserved tokens, negations,
   numbers, register/gender, cultural appropriateness).
3. **Tier-dependent escalation:** Tier B/C → **independent second reviewer** (medical/triage Tier-B →
   **medical-capable reviewer**); Tier C → **independent back-translation QA by a non-forward
   translator + committed diff report**; medical-critical / safety-critical Tier C →
   **clinician/medical-interpreter sign-off**. A **pictogram-comprehension check** (naive-reader icon +
   gesture interpretation) is part of every checklist, **not deferred to field testing**.
4. **License & attribution verification** (mandatory disclaimer + provenance + compatible output
   license + verified locale data).
5. **CI green** for code/tooling and for structural checks on content files (incl. disclaimer
   presence and Tier-C back-translation presence).
6. **Maintainer approval** of the PR.
7. **(M2+) Field-usability confirmation** before "shipped."

**Reviewer independence & COI.** Reviewers must be **independent of the drafting step** — the human
who ran the drafting agent may **not** be the sole sign-off reviewer — and each records a
**conflict-of-interest declaration** in `review.yaml`.

**Reviewer disagreement / conflict resolution.** If reviewers disagree, the pack **cannot be signed
off** until resolved: (1) reconcile against the phrase bank, glossary, and source; (2) escalate
unresolved safety-critical disagreements to a **third qualified reviewer or the maintainer**; (3)
**when in doubt, the more conservative reading wins** and the disputed phrase is **held back** rather
than shipped. The disagreement + resolution are recorded in `review.yaml`.

**Agent uncertainty self-check (operationalized).** The drafting agent must emit explicit flags of the
form `UNCERTAIN: <phraseId> | <type: term|number|negation|register|gender|cultural|ambiguous-source>
| <note>` for anything it is unsure about. These are copied into `review.yaml` as `agentFlags`. **No
sign-off may be recorded while any flag is unresolved**; each must be `resolved` (with adjudication) or
`accepted-as-is` (with reasoning). Unresolved flags **block** "done".

**Definition of Shipped (project-specific).** A pack is *shipped* only when: acceptance criteria met
**and** qualified-speaker sign-off recorded (plus second-reviewer/back-translation/clinician sign-off
where the tier requires) **and** the **mandatory disclaimer + license + attribution + provenance**
verified **and** **locale data verified** **and** CI green **and** the pack is **delivered to and
adopted by a partner for field use**, with a **field-usability confirmation** **and** a **pre-agreed
recall/notify channel** for post-distribution Tier-C defects (printed cards cannot be patched).
Merged-but-not-used is **not** shipped.

## Roadmap & milestones

**M0 — Foundation & cold-start (no partner required).**
Goal: build the phrase-bank/criticality/license/review machinery and prove the pipeline on **one
scenario pack into one language**, end-to-end except final field adoption.
Exit criteria: phrase-authoring guidelines + **criticality (A/B/C) rubric** + canonical **disclaimer**
text merged; **scenario coverage matrix** with ≥ 3 scenarios and ≥ 60 tiered phrases; allow-list with
≥ 3 verified sources (hash/archive snapshots) **or** recorded original authorship; one language-pair
glossary started (≥ 25 terms); one **locale** file with a **verified emergency number**; reviewer
checklist + handoff template merged; **first pack's BLOCKING license/clearance + disclaimer
prerequisite confirmed** before drafting; **one scenario pack translated into one language with
qualified-speaker sign-off (+ back-translation for any Tier-C phrase) and a passing license/attribution
check**; content JSON schemas + **minimal CI structural check** (incl. disclaimer + Tier-C
back-translation presence) green. Field delivery deferred to M2; `verifiedNeed` honestly `false`.
**Added in v0.2:** **minimal romanized pronunciation for Tier-C phrases** authored with the first pack;
a **pictogram-comprehension check** in the reviewer checklist (decoupled from a field partner); and the
**category-need evidence base cited in-repo** (ad-hoc/family-interpreter harm studies; ACA §1557
qualified-interpreter duty; the Google-Translate ED accuracy study) so cold-start work has an
evidentiary anchor independent of a named partner.
**Metric note:** the 100%/≥90% rates are **effective from M1**; M0 verifies the first deliverable
manually + structural check.

**M1 — Repeatability & reviewer/medical network.**
Goal: make the pipeline repeatable and recruit/qualify reviewers (incl. a medical-capable reviewer for
Tier C).
Exit criteria: documented **reviewer-qualification criteria** (incl. medical-interpreter tier,
independence, two-reviewer rule, back-translation gate, conflict resolution) + ≥ 2 qualified reviewers
(or a translation-NGO partner) engaged, **≥ 1 medical-capable**; glossary + packs generalized to ≥ 2
language pairs; **license-check tooling enforced in CI**; **automated source/locale-change watcher
operating**; **accessibility variant** (large-print/dyslexia-friendly) + **pronunciation/
transliteration** support added (extending the M0 Tier-C romanization to all tiers); a second scenario
pack completed; pipeline **runbook** merged. **Added in v0.2:** the **clinician + responder refine the
B/C line** for operational hazard instructions in the criticality rubric; a **"shared vs forked"
infrastructure table with `vital-info-translations`** published so the two projects converge on **one**
high-stakes translation pipeline (allow-list schema, glossary schema, review workflow) rather than
diverging. Dependency: reviewer sourcing.

**M2 — First partner field delivery (needs partner).**
Goal: deliver an **adopted, field-tested** pack set.
Exit criteria: a partner responder org/clinic/shelter secured (`verifiedNeed = true`); deployment
locale(s), priority languages, and priority scenarios agreed; locale data verified for the deployment
region; **≥ 1 reviewed, correctly-licensed pack delivered, field-usability-tested, and confirmed
adopted** for field use. First true Definition of Shipped event. **Added in v0.2 (delivery
preconditions):** the partner agrees to a **delivered-pack recall/notify channel** (printed offline
cards cannot be patched, so a discovered Tier-C defect needs a pre-agreed notify path, paired with the
QR/short-code-to-latest); field testing records **time-to-find / time-to-first-contact** (navigability),
not just positive feedback; and a **"what this is NOT" partner-onboarding one-pager** (states the §1557
qualified-interpreter duty, positions the pack as supplement-only) is delivered with the pack.
Dependency: M0/M1 + partner.

**M3 — Scale program.**
Goal: scale scenarios/languages with sustained quality + tracked outcomes.
Exit criteria: ≥ 2 packs across ≥ 2 languages adopted; reviewer rotation established; outcome tracking
(post-delivery critical-defect log targeting 0, field feedback) operating; source/locale/glossary
maintenance cadence in effect; sustainability owner named. Optional: pronunciation **audio** track
piloted.

## Work breakdown

The itemized, sized backlog lives in **[TASKS.md](./TASKS.md)**, organized by the milestones above
(M0–M3) plus a Backlog/future section. Each task maps to an Elyos Task JSON (see the schema in
`packages/schema/src/schemas.ts`) with id, type, lane, risk tier, deliverable, acceptance criteria,
and license fields. M0 tasks are partner-independent foundations; M2+ tasks are gated on a secured
partner and marked accordingly (`verifiedNeed: false` until then).

## Governance, roles & stakeholders

- **Maintainer (Owner): J. Carter (acting)** — accepts/sequences tasks, approves PRs, owns the phrase
  bank, criticality rubric, allow-list integrity, the disclaimer, and the license gate. Acts as interim
  license/compliance reviewer until a dedicated one is named.
- **Qualified reviewers (per language pair): TO BE SECURED** — native/near-native target-language
  speakers; sign-off in PRs; rotation defined in M1.
- **Medical-capable reviewers (for Tier-C medical phrases): TO BE SECURED** — bilingual clinicians or
  qualified **medical interpreters**; mandatory sign-off for medical-critical content.
- **Licensed-attorney reviewer (only if the excluded legal sub-tier is ever reconsidered): TO BE
  SECURED** — required gate before any legal/rights phrase could be attempted; default is exclusion.
- **License/compliance reviewer** — may be the maintainer initially; verifies per-source terms,
  per-deliverable attribution/disclaimer, and **locale-data accuracy**.
- **Steward (last-mile owner): TBD — named by end of M1** (acting maintainer holds these duties
  meanwhile). Owns the partner relationship, runs **field-usability testing**, and confirms
  **adoption**. Naming a steward is a **prerequisite for entering M2**.
- **Partner / requestor: TO BE SECURED** — responder org/clinic/shelter/resettlement agency defining
  locale, languages, and scenario priorities and confirming field use.

## Dependencies & integrations

- **Elyos donated lane**: `packages/cli` (workspace prep + PR), `packages/core`, `packages/schema`
  (Task + content schemas). No funded-lane / API-key execution in this project.
- **Public source sites**: government/PD emergency phrase lists, WHO/health-authority phrasing, open
  pictogram libraries, official emergency-number references (read-only; per-item license/accuracy
  verification).
- **Human reviewers / a translation NGO** (e.g. Translators without Borders / CLEAR Global) and
  **medical interpreters/clinicians** for qualified review — **external dependency, not yet secured**.
- **Partner responder org / clinic / shelter** for requirements, locale/scenario priorities, and
  field adoption — **not yet secured**.
- **Sibling Elyos projects** (reuse where licenses permit): `open-pictograms`, `easy-read-plus`
  (accessibility), `open-pronunciation-audio`/`open-transliteration` (pronunciation),
  `multilingual-signage-templates`, `vital-info-translations` (shared allow-list/glossary patterns).

## Adjacent opportunities

Perpendicular outcomes that reuse this project's machinery (from the competitive review §7); none is in
v0.1/v0.2 scope, but each is recorded so the pipeline is built to be shared, not forked.

- **Reusable high-stakes translation pipeline (keystone asset).** The criticality-tiering +
  license-as-data allow-list + back-translation gate + clinician sign-off + provenance + CI structural
  checks generalize to **any** high-stakes multilingual content. Factor them into a shared
  package/process consumed by `vital-info-translations`, the `oncology-glossary-multilingual` /
  termbase work, `multilingual-signage-templates`, and future projects — a portfolio-wide "reviewed
  translation" capability. **Recommendation: one shared pipeline, separate deliverable repos.**
- **`open-pictograms` — point-to-communicate / AAC boards.** An emergency point-to icon set (body
  diagram, hazards, allergy/medication, do-not-move) authored/curated open-licensed and
  **comprehension-tested**, consumed by phrasebooks *and* signage, and extensible to AAC-style
  point-to-communicate boards for non-literate or no-shared-script users. Fills the gap Kwikpoint's
  proprietary icons leave.
- **`multilingual-signage-templates`** — the same phrase bank + locale data rendered at signage size
  (lamination-ready, complex-script) for shelters/clinics/triage tents. Parallel deliverable from one
  source of truth.
- **`first-aid-open`** — pair phrase packs with open first-aid step content (complement to Red Cross /
  IFRC apps) so the responder both *communicates* and *acts*, keeping the communication-only /
  no-clinical-advice boundary intact (first-aid content separately reviewed).
- **`community-resource-maps` + a "find an interpreter" panel** — emulate Kwikpoint's language-ID panel
  by linking a "find a qualified interpreter / telephonic line / clinic" panel to locale-specific
  resource data, reinforcing the bridge-to-interpreter framing.
- **MCP server (tooling spin-off)** — expose the validation/lint/back-translation-diff and allow-list
  checks as an MCP server so any agent run can self-check packs pre-CI; stays agent-neutral and
  draft-only, never a substitute for human certification.
- **Partnerships (high-leverage):** **TWB / CLEAR Global** as reviewer-network + glossary partner;
  **Red Cross/IFRC** and **Tarjimly** as distribution/complement partners; **hospital language-access
  offices** (§1557-bound) as priority adopters; **UNHCR / Save the Children / CDC / ICRC** as sources
  and reach.

## Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Mistranslation of a Tier-C phrase (allergy/negation/number/consent) causes harm | Medium | Critical | Criticality tiering; mandatory second reviewer + back-translation + clinician/medical-interpreter sign-off for Tier C; preserved-token rules; post-delivery critical-defect log (target 0) | Reviewers / Maintainer |
| **Phrasebook used in place of a qualified interpreter**, degrading care or breaching language-access duties | High | High | Non-removable "bridge, not a substitute" disclaimer on every pack; explicit de-positioning; partner onboarding stresses supplement-only use; refuse phrases that encourage standalone reliance | Maintainer / Steward |
| Scope creep into excluded **legal/rights** phrases (high risk) | Medium | High | Explicit exclusion; attorney-gate required before any consideration; refuse such tasks; "informational, not legal advice" + jurisdiction sourcing mandatory if ever attempted | Maintainer |
| Scope creep into **clinical advice** (dosing/treatment) — high risk | Medium | High | Communication-only rule; non-goal; reject; clinician review focuses on *communication* accuracy, not authoring advice | Maintainer / Clinician reviewer |
| License violation — using non-open phrases/pictograms or omitting a required disclaimer | Medium | High | Allow-list with recorded terms + snapshot hash; prefer original phrases; per-deliverable license gate; "if unclear, don't use" | License reviewer |
| **Wrong emergency number / outdated locale protocol** shipped | Medium | High | Locale data verified against official source with date; on source-change-watcher cadence; treated as a safety defect | License reviewer / Maintainer |
| No qualified (esp. medical-capable) reviewer for a language | High | High | Don't ship unreviewed; partner with translation NGO/medical interpreters; scope only languages with reviewer coverage | Steward / Maintainer |
| No partner secured → nothing reaches "shipped" | High | High | M0/M1 build partner-independent value; concrete outreach plan with named target types/owner/timeline; `verifiedNeed=false` until secured; explicit pause/decision point | Acting maintainer → Steward |
| Cultural inappropriateness / gesture/register error (e.g. yes/no nod, T-V form) | Medium | Medium | Cross-cultural-clarity authoring guidelines; native-speaker review; register/gender fields in glossary | Reviewers |
| Pictogram misread across cultures (incl. yes/no head-shake / point-to gesture ambiguity) | Medium | Medium | Prefer tested humanitarian/wayfinding icon sets; **pictogram-comprehension check in the reviewer checklist from M0** (naive-reader icon + gesture interpretation); pair icon with text | Reviewers |
| **Safety/hazard instruction mis-tiered as B** (e.g. "evacuate now" treated as merely operational) | Medium | High | Tier cut by **harm-on-mistranslation, not topic**; classify-up rule; clinician + responder co-set the B/C line (M1); medical-capable reviewer inherited by medical/triage Tier-B | Reviewers / Clinician reviewer |
| **Tier-C defect discovered after printed offline distribution** (cards cannot be patched) | Low | Critical | Pre-agreed partner **recall/notify channel** as M2 delivery precondition; version + QR/short-code-to-latest; withdrawal procedure | Steward / Maintainer |
| **Back-translation masks systematic error** (same person drafts + checks) | Medium | High | **Independent back-translator** (≠ forward translator) for Tier C; committed back-translation diff report; reconciliation recorded in `review.yaml` | Reviewers |
| Agent overconfidence / unflagged uncertainty | Medium | High | Operationalized `UNCERTAIN:` flags into `review.yaml`; unresolved flags block sign-off; agent output is draft only | Reviewers |
| Coercive/immigration-status/partisan phrase admitted | Low | High | Refusal guardrails; excluded-content rule is a compliance gate; reviewer + maintainer screen | Maintainer |

## Security & privacy

- **Threat surface** is small: public source ingestion + static text/PDF artifacts in a public repo.
  Main risks are **integrity** (wrong source, mistranslation, wrong emergency number) and
  **license/compliance**, plus **misuse framing** — not data exfiltration.
- **No secrets**: donated lane uses the human's own agent; no API keys, tokens, or escrow. Per
  CLAUDE.md, never write secrets/tokens into logs, receipts, or committed files.
- **PII**: none ingested or collected; example scenarios use synthetic placeholders; partner contacts
  kept out-of-band and uncommitted; no end-user telemetry.
- **Abuse/misuse prevention (refusal guardrails apply):** refuse any phrase or task that could
  **coerce or deceive** (e.g. "sign here" with legal effect), **elicit immigration status or
  self-incrimination**, **surveil**, **target/discriminate**, inject **misinformation**, give
  **unqualified high-stakes (medical/legal) advice**, or **primarily benefit a for-profit**. Strictly
  **non-partisan**; no enforcement-adversarial content. Source/locale integrity verification is part
  of every task.
- **Supply-chain**: pin source URLs + version/date; **hash/archive license + source snapshots**; run a
  source/locale-change watcher (hash-diff) to detect later changes (M0 minimal, M1 automated).

## Sustainability & maintenance

- **After delivery**, the maintainer + steward keep the phrase bank, glossaries, **locale data**, and
  allow-list current and re-verify sources/emergency-numbers when upstream changes. Reviewer rotation
  (M1/M3) avoids single-person dependence; a **medical-reviewer bench** is maintained for Tier C.
- **Versioning & errata.** Packs are versioned; each pack carries a version + date and (where the
  partner supports it) a short-code/QR pointing to the latest version + errata. A **withdrawal
  procedure** flags and recalls any pack affected by a discovered Tier-C defect or a license change.
  Because **printed offline cards cannot be patched**, every delivery secures a **pre-agreed partner
  recall/notify channel** (an M2 delivery precondition, not just a sustainability nicety) so a defect
  found after distribution reaches the field.
- **Outcome tracking** continues post-delivery: a **critical-defect log** (target 0) and **field
  feedback** per delivered pack, plus periodic partner check-ins. Outcomes (adoption, field usability,
  defects) — not merge counts — are the maintained metrics.
- **Decommissioning**: if a source's license changes to forbid reuse, or a locale's emergency
  number/protocol changes, affected packs are flagged and, if required, withdrawn; provenance makes
  impact assessment possible.

## Open questions

1. **Partner & deployment context** (blocks M2 and `verifiedNeed=true`). Which responder
   org/clinic/shelter/resettlement agency is the first requestor, and what are their **deployment
   locale(s), priority languages, and priority scenarios**? **Partner-sourcing plan (concrete):**
   - **Targets (named):** refugee-resettlement agencies (IRC/LIRS/USCRI + national equivalents),
     hospital **language-access offices**, EMS/fire/emergency-management agencies, Red Cross/Red
     Crescent, free/migrant clinics, school newcomer programs, Translators without Borders / CLEAR
     Global (also reviewer-partner candidate).
   - **Owner:** acting maintainer runs outreach until a Steward is named (end of M1).
   - **Timeline:** begin in parallel with M0; **≥ 3 serious conversations by end of M1**; signed first
     partner during M2 sourcing.
   - **Pause/decision point:** if **no partner by end of M1**, the maintainer makes an explicit **go /
     pivot / hold** decision (e.g. pivot to open self-distribution via a resettlement portal, or hold)
     rather than drafting packs no one has committed to use.
2. **Reviewer sourcing — the true bottleneck.** Individual qualified reviewers vs. a translation-NGO
   partnership? Securing **medical-capable** reviewers across needed languages may be harder than
   securing a partner — should M1 **prioritize a TWB/CLEAR Global reviewer-network partnership as the
   primary path** over recruiting individuals (the plan lists both; pick a primary)? Where do we source
   medical-capable reviewers (medical-interpreter associations, bilingual clinician volunteers)? Formal
   qualification criteria?
3. **Output content license:** **CC-BY-SA 4.0 vs. CC0** for maximum reuse — confirm with likely
   partners (Refugee Phrasebook chose CC0; some agencies prefer it for unrestricted redistribution, and
   SA copyleft may impede a clinic remixing). **Lean CC0 for the phrase bank, reserving SA only where a
   source forces it?** Confirm pictogram-source compatibility.
4. **Legal sub-tier:** do we ever stand up the attorney-review gate to offer (clearly "informational,
   not legal advice," jurisdiction-sourced) rights phrases, or keep it permanently out of scope?
5. **Delivery formats / scripts:** pocket-fold PDF sufficient, or do partners need lamination-ready,
   complex-script, or signage-size variants? Relationship to `multilingual-signage-templates`?
6. **Pronunciation:** transliteration-only, IPA, or audio (overlaps `open-pronunciation-audio`)? Audio
   is medium-effort and sequenced as backlog.
7. **Funded lane?** Proposal implies donated; do we ever want metered drafting under escrow for surge
   demand (sudden displacement event), and how is the budget cap + human-review gate preserved under
   time pressure? Out of scope for v0.1 (would require `fundedBudgetUsd`).
8. **Where exactly is the B/C line for operational hazard instructions?** ("evacuate now," "is anyone
   inside?" — the v0.2 default classifies up to Tier C.) Needs a clinician + a responder to co-set the
   rubric in M1.
9. **Pictogram validation without a field partner:** what is the informal target-community
   comprehension-test protocol that lets us check icons in M0/M1 before M2 field testing?
10. **Delivered-pack recall:** what is the realistic notify channel for a Tier-C defect found after
    printed distribution to offline users (beyond QR-to-latest + the partner channel)?
11. **One pipeline or three repos** with `vital-info-translations` / `multilingual-signage-templates`?
    (Working recommendation: **one shared high-stakes-translation pipeline, separate deliverable
    repos**; confirm via the "shared vs forked" infrastructure table in M1.)

## References

- `C:\code\elyos\CLAUDE.md` — Elyos work rules, lanes, quality bar, refusal guardrails.
- `C:\code\elyos\docs\good-deed-definition.md` — good-deed criteria and risk tiers.
- `C:\code\elyos\packages\schema\src\schemas.ts` — Task (and content) JSON schemas.
- `C:\code\elyos\planning\projects\vital-info-translations\{PLAN,TASKS}.md` — sibling project; shared
  allow-list/glossary/review patterns reused here.
- `C:\code\elyos\planning\ROADMAP.md` — portfolio context (`emergency-phrasebooks`, Track 4).
- Open pictogram / humanitarian-icon libraries; official emergency-number references; WHO permissions
  & translation-disclaimer policy; medical-interpreter standards (verify current terms per source).
- `./COMPETITIVE-ANALYSIS.md` — competitive landscape, optimizations, and spin-offs merged into this
  v0.2 (full citations there).
- **Category-need evidence (to be cited in-repo per M0):** ad-hoc/family/child-interpreter harm vs.
  professional interpreters (Flores et al.; AHRQ PSNet "language barrier"); legal duty under **Title VI
  / ACA §1557 (2024 final rule)** to provide *qualified* interpreters (HHS OCR; National Health Law
  Program); Google-Translate ED-discharge accuracy study (*J. Emergency Medicine*, 2025; ATA analysis).
- **Competitive references:** Refugee Phrasebook (CC0); Kwikpoint Medical/EMS visual translator;
  Translators without Borders / CLEAR Global (Words of Relief, TWB Glossaries); Tarjimly; Red Cross /
  IFRC first-aid apps; MedlinePlus; UNHCR / Save the Children / CDC / ICRC.

---

## Appendix A — Improvements applied

Twenty-five specific improvements were identified against the first draft and **applied** to the text
above (and to TASKS.md). Each is concrete and traceable to a section.

1. **"Bridge, not a substitute for an interpreter" made a structural constraint**, not a footnote —
   elevated to the Executive summary, a non-removable disclaimer, a Definition-of-Shipped item, and a
   top risk row. (§Exec, §Quality, §Risks)
2. **Phrase criticality tiers (A/B/C)** introduced as the core scoping/review device, encoded as a
   data field so review intensity is enforceable. (§Scope, §Architecture, §Quality)
3. **Medical-critical sub-tier** added requiring **clinician / qualified medical-interpreter** sign-off
   beyond a generic native speaker. (§Quality, §Governance)
4. **Legal/rights phrases explicitly excluded** as high-risk, behind a mandatory **licensed-attorney
   gate** + "informational, not legal advice" + jurisdiction sourcing if ever reconsidered. (§Non-
   goals, §Quality, §Open questions)
5. **Communication-only medical rule** — phrases never instruct on dosing/treatment/diagnosis — keeps
   the project medium, not high. (§Non-goals, §Risks)
6. **Mandatory back-translation QA for Tier C** promoted from "nice-to-have" to a required gate, with
   a CI structural check that fails if a Tier-C pack lacks a back-translation record. (§Quality,
   §Architecture, TASKS DoD)
7. **Prefer originally-authored functional phrases** to minimize IP risk and enable permissive output
   licensing. (§Data, key decisions)
8. **License-as-data allow-list with SHA-256 snapshot hash + web-archive** and a source-change
   watcher, reused/adapted from `vital-info-translations`. (§Data, §Security)
9. **Locale data as a first-class, safety-critical artifact** — the **emergency number** (911/112/999)
   must be verified per region, because the same language deploys to multiple locales. (§Scope, §Data,
   §Risks)
10. **Offline-first, print-first delivery** (B/W-printable fold-to-pocket PDF, no app/network/power)
    matching real emergency conditions. (§Goals, §Architecture)
11. **Accessibility variants** (large-print / dyslexia-friendly / high-contrast) and **pictogram +
    point-to** support for low-literacy / no-shared-script readers. (§Scope, M1)
12. **Cross-cultural-clarity authoring guidelines** — no idioms, no yes/no-gesture ambiguity, flag
    inverting negations, handle register (T-V) and grammatical gender. (§Scope, §Architecture, §Risks)
13. **Preserved-token model** (numbers, units, drug names, body parts, emergency number) marked in the
    phrase bank for deterministic, reviewable handling. (§Architecture)
14. **Scenario coverage matrix** with explicit **exclusions** (no enforcement-adversarial, no
    immigration-status content). (§Scope, M0)
15. **Operationalized agent uncertainty flags** (`UNCERTAIN: <phraseId>|<type>|<note>`) copied into
    `review.yaml`; unresolved flags block sign-off. (§Quality, §Architecture)
16. **Reviewer independence + COI declaration**; drafting human cannot be sole reviewer; explicit
    disagreement/conflict-resolution rule (conservative reading wins, held back). (§Quality)
17. **Field-usability confirmation** added to Definition of Shipped — linguistic correctness alone is
    insufficient; a responder/newcomer must find the pack usable. (§Metrics, §Quality, M2)
18. **Non-partisan & non-coercive content rule** as a compliance gate (refuse coercion, deception,
    immigration-status elicitation, "sign here," partisan content). (§Data, §Security, §Non-goals)
19. **No-PII / synthetic-placeholder rule** for example scenarios; no end-user telemetry. (§Data,
    §Security)
20. **Outcome metrics rewritten to beneficiary outcomes** (adopted + used packs, 0 Tier-C field
    defects), with a small-sample reporting rule and an **absolute** 0-defect target exempt from it.
    (§Metrics)
21. **Versioning, errata, and withdrawal procedure** (version + date on every pack; short-code/QR to
    latest; recall path for discovered defects or license changes). (§Sustainability)
22. **BLOCKING license/clearance + disclaimer prerequisite** before the first translated pack, modeled
    as a hard gate task (`license-000`), not an open question. (§Data, M0, TASKS)
23. **Content JSON Schemas in the core package** (`phraseBankSchema`, `localeSchema`, `reviewSchema`,
    etc.) with CI enforcement — keeping validation agent-neutral, never in adapters (CLAUDE.md
    architecture rule). (§Architecture)
24. **Sibling-project reuse** wired in (`open-pictograms`, `easy-read-plus`,
    `open-pronunciation-audio`, `multilingual-signage-templates`, `vital-info-translations`) to avoid
    duplicate effort and license re-work. (§Dependencies)
25. **Honest partner status throughout** — `verifiedNeed=false` until secured, a concrete outreach plan
    with owner/timeline, and an explicit **pause/decision point** if no partner by end of M1. (§Exec,
    §Problem, §Open questions, TASKS)

## Review sign-off

**Reviewer:** senior staff engineer + TPM (drafting author), self-review pass.
**Date:** 2026-06-28 · **Doc version reviewed:** 0.1.0.

**Completeness check.** All 17 required PLAN sections present and in order; TASKS.md present with
schema-field mapping, per-milestone tables, acceptance criteria, DoD, backlog, and a schema-valid
example Task JSON. Appendix A (25 applied improvements) present.

**Correctness check (and fixes folded in during review):**
- Confirmed every example/Task JSON field is within the `packages/schema/src/schemas.ts` enums
  (`type`, `lane`, `priority`, `riskTier`, `deliverable`, `tokenEstimate`, `status`) and that all
  `required` fields are present; `verifiedNeed` is `false` wherever no partner exists. **Fixed** an
  initial draft that set the phrase-bank deliverable to `translation` → corrected to `dataset` (it is
  English-pivot source data, not a translation); translated packs use `translation`.
- Confirmed the risk framing is consistent: project is **medium**, with a hardened medical-critical
  sub-tier and an **excluded** high-risk legal sub-tier — and that no in-scope task carries
  `riskTier: high` (the only high-risk content is explicitly out of scope). **Fixed** wording in
  §Non-goals to state the attorney gate explicitly.
- Confirmed license posture is conservative: open/PD/CC only or original authorship; output
  **CC-BY-SA 4.0** (CC0 under consideration) for content / **MIT** for code; source-incompatible
  material inherits source terms + disclaimer; pictograms keep their own license. **Fixed** the
  example JSON `outputLicense` to `MIT` (the phrase-bank scaffolding task is project tooling/data, not
  derived translated content).
- Confirmed safety gates are concrete and enforceable (Tier-C back-translation + clinician sign-off;
  verified emergency-number/locale data; non-removable disclaimer; CI structural checks). 
- Confirmed alignment with CLAUDE.md (donated lane, agent-neutral core, no secrets, refusal
  guardrails) and the good-deed definition (public benefit, freely available, not for-profit-primary,
  no harm/misinformation, AI-executable with expert review).

**Outstanding (human decisions required):** secure a partner + deployment locale (blocks M2 /
`verifiedNeed`); source medical-capable reviewers; choose CC-BY-SA vs CC0; decide whether the legal
sub-tier is ever stood up. **Verdict: APPROVED to proceed to M0** (partner-independent foundations),
with M2+ blocked on the noted human decisions.

## Changelog — v0.2 (analysis merged)

Merged the findings of `COMPETITIVE-ANALYSIS.md` (analyst pass, 2026-06-29) into the plan. Surgical and
additive; no guardrail weakened, no facts or phrases invented.

**Correctness / safety fixes applied:**
- **Criticality cut re-based on *harm-on-mistranslation*, not topic.** Time-critical safety/hazard
  instructions ("evacuate now," "is anyone still inside?") moved to **Tier C**; classify-up rule added.
  (§Scope, §Quality, §Risks)
- **Medical/triage Tier-B phrases now inherit the medical-capable-reviewer requirement.** (§Scope,
  §Quality)
- **Back-translator independence made explicit** — Tier C uses an ISO-style flow (independent forward
  translator + independent reviewer + **independent back-translator ≠ forward translator** +
  reconciliation), with a committed **back-translation diff report** artifact. (§Architecture, §Quality,
  data model, §Risks)
- **Pictogram-comprehension check added to the reviewer checklist from M0** (naive-reader icon + gesture
  interpretation, incl. yes/no head-shake ambiguity), decoupled from securing a field partner.
  (§Architecture, §Quality, M0)
- **Minimal romanized pronunciation for Tier-C phrases pulled forward to M0** (point-to fallback for
  low-literacy / no-shared-script users; closes a first-contact gap). (§Non-goals, data model, M0)
- **Delivered-pack recall/notify channel** made an **M2 delivery precondition** and a Definition-of-
  Shipped item (printed offline cards cannot be patched). (§Quality, M2, §Sustainability, §Risks)
- **Navigability metric** (time-to-find / time-to-first-contact) and **category-need evidence cited
  in-repo** (ad-hoc-interpreter harm; §1557 qualified-interpreter duty; Google-Translate ED study).
  (§Metrics, M0, §References)
- **Reaffirmed never-MT-only for high-tier phrases** and never machine/LLM output shipped without
  qualified-human + (Tier C / medical Tier B) clinician sign-off. (§Architecture)

**Strategy integrated:**
- New **"Competitive landscape & differentiation"** section (Refugee Phrasebook, Kwikpoint, Google
  Translate baseline, TWB/CLEAR Global, Tarjimly, Red Cross/IFRC, MedlinePlus, UNHCR/CDC/ICRC) with the
  **"open + reviewed + criticality-tiered + provenance-bearing" empty-quadrant** differentiator.
- **Claude/agent leverage folded into Architecture** (draft phrasings + back-translation QA/diff
  surfacing + glossary/termbase + license/pictogram drafting) with **hard human gates** kept
  (qualified-human review; never MT-only for high tier; clinician sign-off; agent never clears licenses
  or verifies emergency numbers).
- **Optimizations folded into the Roadmap** (M0 romanization + pictogram check + evidence anchor; M1
  B/C-line refinement + shared-vs-forked infra table; M2 recall channel + navigability + onboarding
  one-pager).
- New **"Adjacent opportunities"** section (shared high-stakes-translation pipeline with
  `vital-info-translations` / `oncology-glossary-multilingual`; `open-pictograms` point-to-communicate /
  AAC boards; signage templates; first-aid pairing; find-an-interpreter panel; **MCP server**).
- **Open Questions merged** (reviewer-supply primary path; CC0 lean; B/C hazard line; pictogram
  validation without a partner; delivered-pack recall; one-pipeline-vs-three-repos).

**Preserved unchanged:** vision and scope; all refusal guardrails (qualified translation review;
harm-on-mistranslation discipline; license/provenance rigor; "bridge, not a substitute for an
interpreter"); excluded high-risk legal sub-tier; communication-only medical rule; honest
`verifiedNeed=false` partner status. `COMPETITIVE-ANALYSIS.md` left untouched.
