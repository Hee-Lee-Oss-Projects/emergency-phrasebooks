# PLAN — emergency-phrasebooks

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: J. Carter (acting maintainer) · Lane: donated

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
  for now).

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
bank, into one of three tiers that drive review intensity (see Quality gates):
- **Tier C — critical:** a mistranslation could cause **direct harm**. Allergy/medication-allergy,
  consent-to-treatment (non-legal, clinical), numbers/dosing-context, "do not move / do not give food
  or water," choking/breathing, severe-bleeding, pregnancy/labor. *Strongest review.*
- **Tier B — sensitive:** triage questions and operational instructions ("where does it hurt?",
  "evacuate now," "is anyone still inside?"). *Two-reviewer or one + back-translation.*
- **Tier A — routine:** orientation and basic needs ("what is your name?", "where is the toilet?",
  "are you hungry/thirsty/cold?"). *Single qualified reviewer.*

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
   checklist, and **records sign-off in the PR**. **Tier B/C** add an **independent second reviewer**;
   **Tier C** add **mandatory back-translation QA** and, for **medical-critical** phrases, a
   **bilingual clinician or qualified medical-interpreter** sign-off.
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
- `packs/<scenario>/<lang>/<locale>/` — `phrases.yaml` (translations keyed by phrase id),
  `pack.pdf` + `pack.md`, `provenance.yaml`, `attribution.txt` (incl. mandatory disclaimer),
  `review.yaml` (checklist + sign-offs + `agentFlags` + back-translation record for Tier C).
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
  treatment, breathing/bleeding/labor, numbers-in-clinical-context) additionally require a **bilingual
  clinician or qualified medical-interpreter** sign-off and **mandatory back-translation QA**. This is
  the project's most safety-sensitive content and is the reason the project does not treat all medium
  content identically.
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
3. **Tier-dependent escalation:** Tier B/C → **independent second reviewer**; Tier C →
   **back-translation QA**; medical-critical Tier C → **clinician/medical-interpreter sign-off**.
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
adopted by a partner for field use**, with a **field-usability confirmation**. Merged-but-not-used is
**not** shipped.

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
transliteration** support added; a second scenario pack completed; pipeline **runbook** merged.
Dependency: reviewer sourcing.

**M2 — First partner field delivery (needs partner).**
Goal: deliver an **adopted, field-tested** pack set.
Exit criteria: a partner responder org/clinic/shelter secured (`verifiedNeed = true`); deployment
locale(s), priority languages, and priority scenarios agreed; locale data verified for the deployment
region; **≥ 1 reviewed, correctly-licensed pack delivered, field-usability-tested, and confirmed
adopted** for field use. First true Definition of Shipped event. Dependency: M0/M1 + partner.

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
| Pictogram misread across cultures | Medium | Medium | Prefer tested humanitarian/wayfinding icon sets; reviewer checks pictogram comprehension; pair icon with text | Reviewers |
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
2. **Reviewer sourcing:** individual qualified reviewers vs. a translation-NGO partnership? Where do we
   source **medical-capable** reviewers (medical-interpreter associations, bilingual clinician
   volunteers)? Formal qualification criteria?
3. **Output content license:** **CC-BY-SA 4.0 vs. CC0** for maximum reuse — confirm with likely
   partners (some prefer CC0 for unrestricted redistribution). Confirm pictogram-source compatibility.
4. **Legal sub-tier:** do we ever stand up the attorney-review gate to offer (clearly "informational,
   not legal advice," jurisdiction-sourced) rights phrases, or keep it permanently out of scope?
5. **Delivery formats / scripts:** pocket-fold PDF sufficient, or do partners need lamination-ready,
   complex-script, or signage-size variants? Relationship to `multilingual-signage-templates`?
6. **Pronunciation:** transliteration-only, IPA, or audio (overlaps `open-pronunciation-audio`)? Audio
   is medium-effort and sequenced as backlog.
7. **Funded lane?** Proposal implies donated; do we ever want metered drafting under escrow for surge
   demand (sudden displacement event)? Out of scope for v0.1 (would require `fundedBudgetUsd`).

## References

- `C:\code\elyos\CLAUDE.md` — Elyos work rules, lanes, quality bar, refusal guardrails.
- `C:\code\elyos\docs\good-deed-definition.md` — good-deed criteria and risk tiers.
- `C:\code\elyos\packages\schema\src\schemas.ts` — Task (and content) JSON schemas.
- `C:\code\elyos\planning\projects\vital-info-translations\{PLAN,TASKS}.md` — sibling project; shared
  allow-list/glossary/review patterns reused here.
- `C:\code\elyos\planning\ROADMAP.md` — portfolio context (`emergency-phrasebooks`, Track 4).
- Open pictogram / humanitarian-icon libraries; official emergency-number references; WHO permissions
  & translation-disclaimer policy; medical-interpreter standards (verify current terms per source).

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
