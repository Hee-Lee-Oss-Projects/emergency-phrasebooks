# TASKS — emergency-phrasebooks

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: J. Carter (acting maintainer) · Lane: donated

The backlog for the `emergency-phrasebooks` good-deed project. Read alongside [PLAN.md](./PLAN.md).
Milestones (M0–M3) match the roadmap there.

## How these tasks map to Elyos

Each task below becomes an **Elyos Task JSON** validated against `packages/schema/src/schemas.ts`.
Field mapping:

- **id** — stable slug id, e.g. `emergency-phrasebooks-phrasebank-001` (table column `ID`).
- **title** — the task title.
- **project** — always `emergency-phrasebooks`.
- **type** — one of `code | research | writing | data | design-spec | maintenance` (table `Type`).
- **lane** — `donated` for all current tasks (no funded/API execution). Funded tasks would need
  `fundedBudgetUsd`; none here.
- **priority** — `high | medium | low`.
- **domain** — array, e.g. `["translation","emergency-response","public-health"]`.
- **riskTier** — `low | medium | high`. Phrase-translation, glossary, and review tasks are **medium**
  (qualified reviewer needed); pure tooling/process/data-scaffolding tasks are **low**. **No in-scope
  task is `high`** — legal/rights content is excluded (see PLAN §Quality). (table `Risk`)
- **urgent** — boolean (default `false`; no live emergency deployment yet).
- **deliverable** — `pr | dataset | document | translation` (table `Deliverable`). Phrase **packs** in
  a target language are `translation`; English-pivot phrase bank / glossary / locale / allow-list are
  `dataset`; guidelines/checklists/runbooks are `document`; tooling is `pr`.
- **tokenEstimate** — `small | medium | large` (table `Size`).
- **status** — `open | in-progress | review | delivered | done` (all start `open`).
- **context / objective** — why + what.
- **acceptanceCriteria[]** — checkable bullets (listed below tables for key tasks).
- **resources[]** — links/paths (phrase bank, allow-list, glossary, locale, templates, source URLs).
- **output** — path/description of the produced artifact.
- **requestor** — partner/requestor; `TO BE SECURED` until a partner is confirmed.
- **verifiedNeed** — boolean; **`false`** wherever value depends on an unsecured partner.
- **outputLicense** — **MIT** for code/tooling; **CC-BY-SA 4.0** (CC0 under consideration) for
  project-authored content; **source-compatible license + required disclaimer** where content derives
  from an admitted source (e.g. WHO → CC BY-NC-SA 3.0 IGO + WHO disclaimer, **never** CC-BY).

---

## Milestone M0 — Foundation & cold-start (no partner required)

Goal: build the phrase-bank / criticality / license / review machinery and prove the pipeline on one
scenario pack into one language, end-to-end except final field adoption.

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| emergency-phrasebooks-phrasebank-001 | English-pivot phrase bank + scenario coverage matrix (≥3 scenarios, tiered A/B/C) | data | medium | low | dataset | — | Maintainer |
| emergency-phrasebooks-guidelines-001 | Phrase-authoring + cross-cultural-clarity guidelines | writing | small | low | document | — | Maintainer |
| emergency-phrasebooks-rubric-001 | Criticality (A/B/C) rubric + canonical mandatory disclaimer text | writing | small | low | document | — | Maintainer |
| emergency-phrasebooks-sources-001 | Source/pictogram allow-list (license terms + snapshot hash/archive) | data | small | low | dataset | — | Maintainer / License reviewer |
| emergency-phrasebooks-locale-001 | Locale file with verified emergency number + protocol note (1 region) | data | small | low | dataset | — | License reviewer |
| emergency-phrasebooks-review-001 | Reviewer checklist (tier-aware: 2nd reviewer / back-translation / clinician) | writing | small | low | document | rubric-001 | Maintainer |
| emergency-phrasebooks-review-002 | Reviewer-handoff template (sign-off recorded in PR) | writing | small | low | document | review-001 | Maintainer |
| emergency-phrasebooks-glossary-001 | Emergency/medical glossary, 1 language pair (register/gender) | data | small | medium | dataset | sources-001 | Qualified reviewer |
| emergency-phrasebooks-license-000 | **BLOCKING:** clear all phrases/pictograms in first pack + confirm exact disclaimer wording | research | small | low | document | sources-001, phrasebank-001 | License reviewer / Maintainer |
| emergency-phrasebooks-translate-001 | Translate one scenario pack into one language (qualified-speaker + back-translation for Tier C) | translation | medium | medium | translation | phrasebank-001, glossary-001, review-001, locale-001, **license-000** | Qualified reviewer (+2nd/clinician per tier) |
| emergency-phrasebooks-print-001 | Print-first pocket-card template (B/W, fold-to-pocket PDF + Markdown) | design-spec | small | low | document | rubric-001 | Maintainer |
| emergency-phrasebooks-tooling-001 | Content JSON schemas + CI structural check (incl. disclaimer + Tier-C back-translation presence) | code | medium | low | pr | phrasebank-001, sources-001, locale-001 | Maintainer |

**Acceptance criteria — key M0 tasks**

`phrasebank-001` (phrase bank + coverage matrix)
- `phrasebank/<scenario>.yaml` covers **≥ 3 scenarios** (e.g. medical/triage, fire/evacuation,
  shelter/intake) with **≥ 60 total phrases**, each with: `id`, `scenario`, `tier` (A/B/C),
  `english`, `contextOfUse`, `register`, `preserveTokens[]`, optional `pictogramRef`, and
  `sourceId`(allow-list) **or** `original: true`.
- Every phrase passes the **cross-cultural-clarity guidelines** (no idioms; no yes/no-gesture
  ambiguity; inverting negations flagged; numbers/units listed as preserved tokens).
- Tiering is justified: allergy/consent/breathing/bleeding/numbers-in-clinical-context = **Tier C**;
  triage/evacuation instructions = **Tier B**; orientation/basic-needs = **Tier A**.
- **No excluded content**: no legal/rights/consent-with-legal-effect, enforcement-adversarial,
  immigration-status, coercive ("sign here"), or clinical-advice (dosing/treatment) phrases.
- Validates against `phraseBankSchema`; passes CI structural checks.

`license-000` (BLOCKING prerequisite of `translate-001`)
- For **every phrase and pictogram** in the pack chosen for `translate-001`, confirm and record (on its
  allow-list entry or as an original-authorship record): the item is **clearable for derivative
  works/translation and open release**.
- Confirm the **exact disclaimer wording** required (the canonical project disclaimer + any
  source-mandated disclaimer, verbatim).
- Any item that cannot be cleared is **dropped/replaced** before drafting. `translate-001` **must not
  start** until this passes.

`translate-001` (first scenario pack)
- `license-000` passed **before drafting**; `locale-001` emergency number verified and injected.
- One scenario pack translated in full into one target language, UTF-8; preserved tokens (numbers,
  units, body parts, emergency number) preserved exactly; register/gender per glossary.
- Ships `provenance.yaml` (phrase ids, sources/original records, versions, glossary + locale versions,
  translator, reviewers) and `attribution.txt` (attribution + **mandatory disclaimer verbatim** in
  target + pivot language).
- Agent **`UNCERTAIN:` flags** captured into `review.yaml` as `agentFlags`; **no sign-off while any
  flag is unresolved**.
- **Qualified-speaker sign-off recorded in the PR**, reviewer **independent of drafting** (COI
  declared). **Tier B/C** phrases get an **independent second reviewer**; **every Tier-C phrase** has a
  **back-translation record**; **medical-critical Tier-C** phrases have a **clinician/medical-
  interpreter sign-off**.
- License/attribution check passes; output license correct (CC-BY-SA 4.0 / source-compatible).

`tooling-001` (content schemas + CI)
- Adds `phraseBankSchema`, `allowListSchema`, `glossarySchema`, `localeSchema`, `provenanceSchema`,
  `reviewSchema` to `packages/schema/src/schemas.ts`, compiled via `validate.ts` (AJV + `ajv-formats`).
- Structural-check script parses YAML→JSON and **fails CI** on malformed/non-conformant files, on a
  **pack missing the mandatory disclaimer**, and on a **Tier-C phrase lacking a back-translation
  record**. Wired into `pnpm test`. Agent-neutral (in the core schema package, not adapters).

**M0 Definition of Done:** guidelines + criticality rubric + canonical disclaimer + reviewer checklist
+ handoff template + print template merged; phrase bank (≥3 scenarios, ≥60 tiered phrases) + allow-list
(≥3 verified sources or original-authorship records, with snapshot hash/archive) + 1 glossary (≥25
terms) + 1 locale (verified emergency number) merged; **`license-000` cleared before drafting**; **one
scenario pack translated into one language with qualified-speaker sign-off (+ back-translation for any
Tier-C phrase) and a passing license/attribution check**; content JSON schemas + minimal CI structural
check (incl. disclaimer + Tier-C back-translation presence) green. 100%/≥90% metrics **effective from
M1**. All M0 deliverables carry `verifiedNeed: false` (no partner; field adoption deferred to M2).

---

## Milestone M1 — Repeatability & reviewer/medical network

Goal: make the pipeline repeatable and recruit/qualify reviewers (incl. a medical-capable reviewer for
Tier C).

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| emergency-phrasebooks-reviewers-001 | Reviewer qualification criteria + onboarding (incl. medical-interpreter tier) | writing | small | low | document | review-001 | Maintainer |
| emergency-phrasebooks-reviewers-002 | Recruit/engage ≥2 qualified reviewers (≥1 medical-capable) or a translation NGO | research | medium | low | document | reviewers-001 | Maintainer / Steward |
| emergency-phrasebooks-glossary-002 | Extend glossary + a pack to a second language pair | data | small | medium | dataset | glossary-001 | Qualified reviewer |
| emergency-phrasebooks-license-002 | License-check tooling (lint allow-list + pack metadata + locale verification) | code | medium | low | pr | tooling-001, license-000 | Maintainer / License reviewer |
| emergency-phrasebooks-watcher-001 | Automated source/locale-change watcher (hash-diff vs. snapshots) | code | small | low | pr | sources-001, locale-001, tooling-001 | Maintainer |
| emergency-phrasebooks-a11y-001 | Accessibility variant (large-print / dyslexia-friendly / high-contrast) | design-spec | small | medium | document | print-001 | Qualified reviewer |
| emergency-phrasebooks-pronun-001 | Pronunciation/transliteration support per phrase | data | medium | medium | dataset | translate-001 | Qualified reviewer |
| emergency-phrasebooks-translate-002 | Translate a second scenario pack (same language) | translation | medium | medium | translation | translate-001, reviewers-002 | Qualified reviewer (+2nd/clinician per tier) |
| emergency-phrasebooks-process-001 | End-to-end pipeline runbook | writing | small | low | document | translate-001, license-002 | Maintainer |

**Acceptance criteria — key M1 tasks**

`reviewers-001`
- Objective criteria for a "qualified speaker" (native/near-native target language) and for the
  **medical-capable** tier (bilingual clinician or qualified medical interpreter), plus
  onboarding/sign-off workflow and a **COI declaration**.
- Defines **reviewer independence** (drafting human ≠ sole reviewer), the **mandatory second reviewer**
  for Tier B/C, **back-translation QA** as a required gate for Tier C, **clinician sign-off** for
  medical-critical phrases, and the **disagreement/conflict-resolution** rule (reconcile → escalate →
  conservative reading wins; recorded in `review.yaml`).

`license-002`
- Tooling validates each pack's metadata against its allow-list/original-authorship records and
  **fails** if attribution or the **mandatory disclaimer** is missing, if output license is
  incompatible with a source, or if the **locale's emergency number is unverified/expired**; runs in
  CI. This is the **full enforcement** making the 100% license/disclaimer gate **automated from M1**.

`watcher-001`
- Re-fetches allow-listed sources **and locale references** (emergency numbers/protocols), recomputes
  `snapshotHash`, and **flags drift** against stored snapshots for re-verification; runs on a
  schedule/CI.

**M1 Definition of Done:** qualification criteria published (independence, two-reviewer rule,
back-translation gate, clinician tier, conflict resolution) and ≥ 2 qualified reviewers (≥ 1
medical-capable) or a translation-NGO partner engaged; glossary + packs cover ≥ 2 language pairs;
license + locale checks **enforced in CI** and **source/locale-change watcher operating**;
accessibility variant + pronunciation/transliteration support added; second scenario pack completed;
pipeline runbook merged. **Steward named** (governance prerequisite for M2).

---

## Milestone M2 — First partner field delivery (needs partner)

Goal: deliver an **adopted, field-tested** pack set. **All tasks gated on a secured partner**
(`verifiedNeed` flips to `true` only when the partner is confirmed).

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| emergency-phrasebooks-partner-001 | Secure first partner; agree locale, languages, priority scenarios | research | medium | low | document | reviewers-002 | Steward / Maintainer |
| emergency-phrasebooks-translate-003 | Translate partner-priority pack set into agreed language(s)/locale | translation | large | medium | translation | partner-001, glossary-002, pronun-001 | Qualified reviewers (+clinician for Tier C) |
| emergency-phrasebooks-fieldtest-001 | Field-usability test with a responder/newcomer | research | small | medium | document | translate-003 | Steward |
| emergency-phrasebooks-delivery-001 | Package + deliver pack set; confirm partner adoption for field use | writing | small | medium | document | translate-003, fieldtest-001, license-002 | Steward |

**Acceptance criteria — key M2 tasks**

`partner-001`
- Outreach (owned by acting maintainer → Steward) targets the named candidate types (resettlement
  agencies, hospital language-access offices, EMS/fire, Red Cross/Red Crescent, free clinics,
  Translators without Borders/CLEAR Global), aiming for **≥ 3 serious conversations by end of M1**.
- **Pause/decision point:** if **no partner by end of M1**, the maintainer makes an explicit **go /
  pivot / hold** decision before further translation work.
- A named partner confirmed in writing as requestor; agreed **deployment locale(s)** (emergency number
  verified), **priority languages**, and **priority scenarios**; reviewer coverage (incl. medical-
  capable for any Tier-C content) confirmed. On completion, related tasks update `requestor` and
  `verifiedNeed: true`.

`fieldtest-001`
- At least one **responder and/or newcomer** trials the pack in a real or simulated first-contact;
  usability findings (legibility, find-time, pictogram comprehension, fold/format) recorded and any
  blocking issues fixed before delivery.

`delivery-001`
- Delivered set includes packs, provenance, attribution + **mandatory disclaimer**, all required
  sign-offs (incl. back-translation/clinician for Tier C), verified locale data; license check green;
  **partner confirms adoption for field use in writing** (recorded in PR/receipt). First true
  **Definition of Shipped** event.

**M2 Definition of Done:** partner secured (`verifiedNeed=true`); ≥ 1 reviewed, correctly-licensed,
**field-usability-tested** pack set **delivered and confirmed adopted** for field use.

---

## Milestone M3 — Scale program

Goal: scale scenarios/languages with sustained quality and tracked outcomes.

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| emergency-phrasebooks-scale-001 | Additional packs across ≥2 languages/locales | translation | large | medium | translation | delivery-001 | Qualified reviewers (+clinician for Tier C) |
| emergency-phrasebooks-rotation-001 | Reviewer rotation + medical-reviewer bench | maintenance | small | low | document | reviewers-002 | Maintainer |
| emergency-phrasebooks-outcomes-001 | Outcome tracking: post-delivery critical-defect + field-feedback log | data | small | low | dataset | delivery-001 | Steward |
| emergency-phrasebooks-maint-001 | Source/locale/glossary re-verification cadence + errata/withdrawal procedure | maintenance | small | low | document | sources-001, locale-001 | Maintainer |

**Acceptance criteria — key M3 tasks**

`outcomes-001`
- A maintained log capturing, per delivered pack: adoption status, **post-delivery Tier-C critical
  defects (target 0)**, and field-usability feedback; feeds PLAN.md success metrics.

`maint-001`
- A documented cadence to re-verify allow-list sources **and locale data (emergency numbers/protocols)**
  via stored snapshots and refresh glossaries; defines the **versioning + errata + withdrawal**
  procedure (recall packs affected by a discovered defect or a license/locale change).

**M3 Definition of Done:** ≥ 2 packs across ≥ 2 languages adopted; reviewer rotation + medical bench
operating; outcome tracking live; source/locale/glossary maintenance + errata/withdrawal cadence in
effect; named sustainability owner.

---

## Backlog / future

Sized but unscheduled:
- **(medium) Pronunciation audio track** — recorded + text-aligned narration per phrase (overlaps
  `open-pronunciation-audio`); deferred behind text/transliteration.
- **(medium) Complex-script / signage-size / lamination-ready variants** — partner-driven layout
  (overlaps `multilingual-signage-templates`).
- **(large, gated) Legal/rights phrase sub-tier** — **only** behind a mandatory **licensed-attorney
  review gate**, "informational, not legal advice" framing, and jurisdiction-specific sourcing;
  default **out of scope** (high risk). Requires explicit governance decision before scheduling.
- **(small) Pivot-language attribution bundle** — bilingual attribution/disclaimer packaging.
- **(medium) Phrase-bank coverage expansion** — additional scenarios (search-and-rescue specifics,
  cold/heat exposure, mental-health-crisis de-escalation — the last reviewed for safety framing).
- **(large, funded — needs escrow) Surge drafting under funded lane** — metered drafting against a hard
  per-task `fundedBudgetUsd` for sudden-displacement events; out of scope for v0.1.

---

## Generated task index

Every backlog row above is now materialized as a schema-valid `tasks/<id>.json` (validated against
the Elyos taskSchema; `filename == id`; no duplicates, no extra keys). The seed
`emergency-phrasebooks-phrasebank-001.json` is kept as-is. Field policy follows the "How these tasks
map to Elyos" section: `lane: donated`, `verifiedNeed: false`, `requestor: "TO BE SECURED"` for all
rows (no partner secured); `riskTier` per the table; `outputLicense` = **MIT** for code/tooling and
project scaffolding datasets (allow-list, locale), **CC-BY-SA-4.0** for project-authored
content/documents/datasets, and **source-compatible** for translated packs (never relicensing a
copyrighted source as CC-BY).

**Type note:** rows whose Type/Deliverable is *translation* are emitted as `type: "writing"` +
`deliverable: "translation"` (the taskSchema has no `translation` type — translation is a deliverable).

**Fan-out:** none. The plan does **not** enumerate a concrete language/locale set — target languages,
locales, scenarios, partner, and reviewers are all *TO BE SECURED* / partner-driven (see PLAN §Scope
and the M2 `partner-001` gate). Per the bounded fan-out policy, each `translate-*`/`glossary-*`/`scale-*`
row is therefore emitted as **one representative task** (placeholder `<lang>`/`<locale>`/`<scenario>`
paths) rather than fabricated per-language items. These expand into concrete per-language/per-locale
tasks once a partner confirms the deployment locale(s), priority languages, and scenarios.

**Guardrails:** the excluded-content rules are preserved verbatim in the relevant task `context`/
`acceptanceCriteria` (no legal/rights/consent-with-legal-effect, enforcement-adversarial,
immigration-status, coercive, or clinical-advice phrases). The backlog's gated "Legal/rights phrase
sub-tier" (high-risk, attorney-gate) remains **out of scope** and is intentionally **not** materialized
as a task.

Generated ids (M0 → M3):

- M0: `emergency-phrasebooks-phrasebank-001` (seed), `-guidelines-001`, `-rubric-001`, `-sources-001`,
  `-locale-001`, `-review-001`, `-review-002`, `-glossary-001`, `-license-000`, `-translate-001`,
  `-print-001`, `-tooling-001`
- M1: `-reviewers-001`, `-reviewers-002`, `-glossary-002`, `-license-002`, `-watcher-001`, `-a11y-001`,
  `-pronun-001`, `-translate-002`, `-process-001`
- M2: `-partner-001`, `-translate-003`, `-fieldtest-001`, `-delivery-001`
- M3: `-scale-001`, `-rotation-001`, `-outcomes-001`, `-maint-001`

Total: 29 task files (1 pre-existing seed + 28 generated).

---

## Example task JSON

Schema-valid Task JSON for the first M0 task. `verifiedNeed` is **false** (no partner secured);
`outputLicense` is **MIT** because the phrase-bank scaffolding is project tooling/source data, not
derived translated content (translated packs use CC-BY-SA 4.0 / source-compatible licenses).

```json
{
  "id": "emergency-phrasebooks-phrasebank-001",
  "title": "English-pivot phrase bank + scenario coverage matrix (tiered A/B/C)",
  "project": "emergency-phrasebooks",
  "type": "data",
  "lane": "donated",
  "priority": "high",
  "domain": ["translation", "emergency-response", "public-health", "accessibility"],
  "riskTier": "low",
  "urgent": false,
  "deliverable": "dataset",
  "tokenEstimate": "medium",
  "status": "open",
  "context": "emergency-phrasebooks produces pre-translated, expert-reviewed emergency phrase packs that let first responders and newcomers establish first contact in a crisis. Every downstream translation maps 1:1 to a canonical English-pivot phrase, so the phrase bank is the single source of truth. Each phrase needs a criticality tier (A routine / B sensitive / C critical) that drives review intensity, plus preserved tokens (numbers, units, body parts, emergency number) for deterministic handling. No legal/rights, enforcement-adversarial, immigration-status, coercive, or clinical-advice content is admitted.",
  "objective": "Author the English-pivot phrase bank and scenario coverage matrix covering at least three emergency scenarios with at least sixty phrases, each tiered A/B/C with context-of-use, register, and preserved-token metadata, conforming to the cross-cultural-clarity authoring guidelines.",
  "acceptanceCriteria": [
    "phrasebank/<scenario>.yaml covers >= 3 scenarios with >= 60 total phrases, each with id, scenario, tier (A|B|C), english, contextOfUse, register, preserveTokens[], optional pictogramRef, and sourceId or original:true",
    "Tiering is justified: allergy/consent/breathing/bleeding/numbers-in-clinical-context = Tier C; triage/evacuation instructions = Tier B; orientation/basic-needs = Tier A",
    "Every phrase passes the cross-cultural-clarity guidelines (no idioms; no yes/no-gesture ambiguity; inverting negations flagged; numbers/units as preserved tokens)",
    "No excluded content: no legal/rights/consent-with-legal-effect, enforcement-adversarial, immigration-status, coercive, or clinical-advice (dosing/treatment/diagnosis) phrases",
    "File validates against phraseBankSchema and passes CI structural checks"
  ],
  "resources": [
    "C:/code/elyos/planning/projects/emergency-phrasebooks/PLAN.md",
    "C:/code/elyos/planning/projects/emergency-phrasebooks/TASKS.md",
    "C:/code/elyos/planning/projects/vital-info-translations/PLAN.md",
    "templates/phrase-authoring-guidelines.md",
    "templates/criticality-rubric.md"
  ],
  "output": "phrasebank/*.yaml (English-pivot phrase bank) plus a scenario coverage matrix and a short README documenting the schema, tiers, and exclusions",
  "requestor": "TO BE SECURED",
  "verifiedNeed": false,
  "outputLicense": "MIT"
}
```
