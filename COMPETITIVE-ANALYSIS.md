# Competitive & Improvement Analysis — `emergency-phrasebooks`

> Analyst pass · 2026-06-29 · Source plan v0.1.0 (Draft) · Lane: donated · Risk: medium (hardened medical-critical sub-tier; legal sub-tier excluded)

This analysis reviews the `emergency-phrasebooks` PLAN/TASKS, grounds the competitive landscape in
cited web research, and proposes concrete optimizations and spin-offs. The project produces
pre-translated, expert-reviewed, offline-first **point-to-speak emergency phrase packs** for
first-contact between responders and limited-proficiency newcomers — explicitly framed as a *bridge,
never a substitute for a qualified interpreter*.

---

## 1. Correctness & completeness review of PLAN.md

**Overall:** This is an unusually mature plan. It already internalizes the single most important
safety truth in this domain — that the high-stakes failure mode is not "imperfect translation" but
*confidently-wrong* translation of allergy/negation/number/consent phrases, and *over-reliance* that
displaces a qualified interpreter. The criticality-tier (A/B/C) data model, mandatory back-translation
+ clinician sign-off for Tier C, non-removable disclaimer, verified-emergency-number-as-data, and
license-as-data allow-list are all correct and well above the norm for this category. The review below
is therefore mostly about sharpening, not fixing.

**Strengths (correct and load-bearing):**
- **High-stakes translation accuracy is handled structurally, not by aspiration.** Criticality as a
  *data field* that CI keys off (Tier-C pack lacking a back-translation record fails CI) is the right
  design. Back-translation QA promoted from nice-to-have to a required gate is exactly correct: the
  documented harm is dosage/allergy/negation/consent errors, and back-translation is the standard
  catch for inverted negations and false-friend drift.
- **The "not a substitute for professional interpreters" boundary is structural** — Executive summary,
  non-removable disclaimer, Definition-of-Shipped item, and a top risk row. This matches the legal and
  patient-safety reality (see §2): in covered US health settings, language assistance must be provided
  by *qualified* interpreters and entities generally **must not** rely on ad-hoc/family interpreters
  except true emergencies. The plan's framing of the pack as a *first-minutes bridge until a qualified
  interpreter/telephonic line is reached* is the defensible scope.
- **Excluding the legal/rights sub-tier** (police caution, rights-on-arrest, asylum, "sign here")
  behind a mandatory licensed-attorney gate is the correct call and keeps the project at medium risk.
- **Communication-only medical rule** (never dosing/treatment/diagnosis) correctly keeps clinician
  review focused on *communication accuracy*, not authoring clinical advice.
- **Locale/emergency-number as a verified, watched data artifact** is a subtle but real safety win —
  the same language deploys to 911/112/999/000 locales and a wrong number is a safety defect.

**Gaps / corrections to fold in:**

1. **Back-translation alone is necessary but not sufficient for Tier C; add forward-translation by an
   independent translator (true ISO-style two-person workflow).** Back-translation catches meaning
   inversion but can mask systematic errors when the same person's mental model produces both the
   forward and the implicit check. For Tier C, specify: independent forward translator + independent
   reviewer + back-translation by a *third* party (or at least not the forward translator) +
   reconciliation. The plan implies this but should state the *independence of the back-translator*
   explicitly, parallel to the existing reviewer-independence rule.

2. **"Qualified speaker" is under-specified for the emergency register.** Native/near-native fluency
   is necessary but the domain failure mode is *register and terminology* (medical triage, hazard
   instructions), not general fluency. The medical-interpreter tier handles Tier C, but Tier B
   (triage/evacuation operational instructions — "is anyone still inside?", "evacuate now") also
   carries real harm potential and currently only needs "two reviewers or one + back-translation."
   Recommend: Tier B medical/triage phrases inherit the medical-capable-reviewer requirement, not just
   Tier C. The A/B/C cut is by harm-on-mistranslation, but a mistimed/misunderstood evacuation
   instruction is plausibly Tier C, not B.

3. **Pictogram comprehension is asserted but not validated.** The plan pairs icon+text and "prefers
   tested humanitarian/wayfinding sets," but cross-cultural pictogram misreading is a documented risk
   the plan itself lists. There is no *comprehension-testing* step (even informal target-population
   review) in M0/M1 — it is folded into M2 field-usability. Recommend an explicit pictogram
   comprehension check in the reviewer checklist (does a naive target-language speaker read the icon as
   intended?), decoupled from securing a field partner.

4. **Audio is deferred, which is defensible but creates a real first-contact gap.** A point-to card
   only works if the newcomer is literate in their language *and* the script renders. For low-literacy
   or non-Latin-script users at 2 a.m., text+transliteration may not bridge. Pronunciation
   transliteration is M1 and audio is M3 — but for the *responder-speaks-to-newcomer* direction, even
   minimal romanized pronunciation should arguably be M0 for Tier C phrases, since the whole value
   proposition is being understood under stress. Flagging the sequencing as a correctness risk, not
   just a backlog choice.

5. **Currency/maintenance is strong but the errata/withdrawal path needs a delivered-pack recall
   mechanism that works offline.** Printed pocket cards in the field cannot be patched. The QR/short-
   code-to-latest is good, but a *discovered Tier-C defect in an already-distributed printed pack* has
   no recall channel except the partner. The plan should make "partner agrees to a recall/notify
   channel" a M2 delivery precondition, not just a sustainability note.

6. **Cultural sensitivity / accessibility coverage is good but should name a concrete check for
   yes/no-gesture and head-shake ambiguity in the *pictogram* layer**, not only the phrase layer —
   point-to interaction relies on gesture, which is exactly where cross-cultural ambiguity bites.

7. **Scope vs `vital-info-translations`:** The boundary is mostly clear (this = short action-oriented
   first-contact utterances + point-to cards; sibling = longer vital-information documents), but the
   plan reuses that sibling's allow-list/glossary/review patterns without specifying *which artifacts
   are shared vs forked*. Recommend an explicit "shared infrastructure vs project-specific" table to
   avoid divergence of the two review pipelines (a single high-stakes translation pipeline is a
   portfolio asset — see §7).

8. **`verifiedNeed=false` honesty is correct**, but the category-need evidence (language barriers cause
   documented emergency harm) should be *cited in-repo* (the studies in §2) so the cold-start M0 work
   has an evidentiary anchor independent of a named partner. Currently it is asserted as
   "well-evidenced" without references in the plan body.

**Completeness:** All 17 sections present; TASKS maps cleanly to the schema; the BLOCKING `license-000`
gate-before-drafting is correctly modeled as a hard task, not an open question. No in-scope task is
`riskTier: high`. Minor: success metrics omit a *time-to-first-contact* or *find-time* usability target
beyond "positive feedback" — see §6.

---

## 2. Competitive landscape (cited)

**Open/NGO phrasebooks & crisis-translation**

- **Refugee Phrasebook** — volunteer/Open Knowledge Foundation Germany project; **28+ languages**,
  general + medical + juridical phrases, released **CC0**, reusable by any refugee-aid project; spread
  across Europe, Prix Ars Electronica award. *Strengths:* truly open (CC0), broad language coverage,
  community momentum, printable. *Weaknesses:* **crowd/volunteer-sourced without a structured
  clinical/criticality review gate**, no per-phrase back-translation or clinician sign-off, no
  criticality tiering, includes legal/juridical phrases (which this project deliberately gates out),
  maintenance is uneven. This is the closest open competitor and the clearest contrast point.
  Sources: https://en.wikipedia.org/wiki/Refugee_Phrasebook ·
  https://github.com/refugee-phrasebook/refugee-phrasebook.github.io · https://refugeephrasebook.de/

- **Translators without Borders / CLEAR Global — "Words of Relief" + TWB Glossaries** — the
  professional gold standard for crisis-language infrastructure: a pre-crisis multilingual library of
  location-specific disaster messages, a "spider network" of professional diaspora/community
  translators, and crisis glossaries. *Strengths:* professional translators, terminology
  standardization, deep humanitarian-sector trust, scaled deployments. *Weaknesses:* organization-led
  (not a self-serve open phrase-pack you can fork and print today), focus is on *content translation
  pipelines and live response* more than pocket point-to cards; not primarily a first-responder
  point-to-speak artifact. **Best partner/reviewer candidate, not a true competitor.**
  Sources: https://translatorswithoutborders.org/our-work/crisis-response/ ·
  https://translatorswithoutborders.org/twb-glossaries/ · https://clearglobal.org/

- **Tarjimly** — on-demand human interpreter app matching refugees/aid-workers with volunteer
  interpreters in **120+ languages**, live text/voice/photo/call, **HIPAA-compliant**, ~20% of sessions
  medical/trauma. *Strengths:* real qualified humans, huge language reach, free for refugees. *Weakness
  vs us:* requires connectivity, a smartphone, and interpreter availability/latency — it is exactly the
  "qualified interpreter" the phrasebook bridges *to*, and fails in the offline first-minutes our
  packs target. **Complement, not competitor** (the card buys minutes until Tarjimly/an interpreter
  line connects). Sources: https://www.tarjimly.org/ · https://apps.apple.com/us/app/tarjimly/id1438066201

**Point-to-talk visual cards (the most direct product analog)**

- **Kwikpoint Medical/EMS Visual Language Translator** — patented point-to-picture laminated cards for
  medical/EMS, waterproof, pocket-fold, covers symptoms, allergies, pain, medication, plus a
  language-identification panel to *find an interpreter*. *Strengths:* battle-tested format (military,
  EMS, hospitals), language-independent pictograms, durable, the literal design we are reinventing.
  *Weaknesses:* **proprietary/commercial (purchased product, not open-licensed/forkable), not freely
  redistributable, limited language-pair text, no open provenance/review transparency**, can't be
  localized by a shelter for free. Our open + reviewed + criticality-tiered angle is the differentiator.
  Sources: https://www.kwikpoint.com/ · https://www.amazon.com/Kwikpoint-Medical-Visual-Language-Translator/dp/B00125BKEC

**Big-tech MT (the cautionary baseline — what responders use today)**

- **Google Translate** — ubiquitous, free, offline packs, 100+ languages. *Cautionary evidence:* a 2025
  *Journal of Emergency Medicine* study of 400 ED discharge instructions found ~5% overall error but
  **wide language variance — >90% accurate for Spanish, 80–90% for Tagalog/Korean/Chinese, dropping to
  67% Farsi and 55% Armenian**, with errors capable of confusing patients even in "good" languages; ATA
  concluded it "still isn't good enough" for medical instructions. This is the core argument for why
  raw MT must never be shipped for emergencies and why reviewed packs exist.
  Sources: https://www.jem-journal.com/article/S0736-4679(25)00170-2/abstract ·
  https://www.atanet.org/industry-news/study-finds-google-translate-still-isnt-good-enough-for-medical-instructions/ ·
  https://pmc.ncbi.nlm.nih.gov/articles/PMC8606479/

**Institutional health-info & responder apps**

- **MedlinePlus (NLM) "Health Information in Multiple Languages"** — 2,500+ vetted links, **40+
  languages, ~250 topics**, authoritative/free. *Strength:* trusted, reviewed health content.
  *Weakness vs us:* consumer health *documents/education*, not first-contact point-to-speak emergency
  utterances; not pocket-card/offline-first; not responder↔patient bidirectional.
  Source: https://medlineplus.gov/languages/languages.html

- **American Red Cross / IFRC First Aid & Emergency apps** — offline-capable first-aid guidance, 35+
  hazard alerts; ARC apps EN/ES toggle, **IFRC app improved multilingual capability**. *Strengths:*
  trusted brand, offline content, hazard breadth. *Weaknesses vs us:* first-aid *instruction to the
  responder*, limited languages (largely EN/ES on ARC), app-dependent, not a point-to-speak
  *cross-language communication* tool. **Partner/distribution candidate.**
  Sources: https://www.redcross.org/get-help/how-to-prepare-for-emergencies/mobile-apps.html ·
  https://apps.apple.com/us/app/first-aid-ifrc/id1312876691

- **UNHCR / Save the Children / CDC** — multilingual orientation & refugee-health materials and
  guidance (UNHCR Integration Handbook stresses multilingual written/AV materials; CDC immigrant/
  refugee health communication tools). *Strengths:* authoritative, culturally-aware, distribution
  reach. *Weaknesses vs us:* programmatic information materials, not first-contact pocket cards; ideal
  *partners/sources*, not competitors. Sources:
  https://www.unhcr.org/handbooks/ih/health/health-care ·
  https://www.cdc.gov/immigrant-refugee-health/communication-resources/index.html

**Evidence base for the category need (cite in-repo, per §1.8):**
- Flores et al. and follow-on work: **ad hoc/family/child interpreters commit significantly more errors
  of potential clinical consequence than professional interpreters** (and children should never
  interpret except emergencies). Source:
  https://www.sciencedirect.com/science/article/abs/pii/S0196064412001151 ·
  https://link.springer.com/article/10.1186/s13690-024-01461-8 · https://psnet.ahrq.gov/web-mm/language-barrier
- Legal duty: **Title VI / ACA Section 1557 (2024 final rule)** require *qualified* interpreters,
  timely and free, and generally bar reliance on ad-hoc/family interpreters except emergencies.
  Sources: https://healthlaw.org/resource/what-is-required-under-title-vi-and-section-1557-to-ensure-language-access-for-individuals-with-limited-english-proficiency/ ·
  https://www.hhs.gov/civil-rights/for-individuals/special-topics/limited-english-proficiency/index.html

---

## 3. Gaps we can fill

No existing offering combines all of: **(open + freely forkable/printable) × (criticality-tiered,
clinically-reviewed, back-translated) × (offline pocket point-to card) × (full provenance + license-as-
data) × (verified locale/emergency-number) × (structurally enforced "not a substitute" boundary).**

- **Open *and* clinically-reviewed.** Refugee Phrasebook is open but not clinically gated; Kwikpoint is
  reviewed/professional but closed. We occupy the empty quadrant: open **and** Tier-C-reviewed.
- **Criticality tiering as enforced data** — nobody publishes per-phrase A/B/C tiers with CI that fails
  a Tier-C phrase lacking back-translation. This is an auditable safety claim competitors cannot make.
- **Provenance + license-as-data per pack** — verifiable attribution, snapshot hashes, and a
  source/locale-change watcher. Reusable trust infrastructure that crowd phrasebooks lack.
- **Verified emergency-number/locale as a first-class artifact** — most phrasebooks hardcode or omit
  the local emergency number; treating it as verified, watched data is a differentiated safety feature.
- **Anti-misuse engineering** — non-removable "bridge, not a substitute" disclaimer + refusal of
  standalone-reliance-encouraging phrases + excluded legal/coercive content. Directly answers the
  documented over-reliance harm.
- **Accessibility-native** — large-print/dyslexia-friendly/high-contrast variants + pictograms, which
  commercial and crowd options treat as afterthoughts.

---

## 4. Differentiators to win

1. **"Open + reviewed + criticality-tiered" — the empty quadrant.** Free/forkable like Refugee
   Phrasebook, but with Kwikpoint-grade (or higher) review discipline and an auditable A/B/C safety
   trail. *This is the single strongest differentiator.*
2. **Auditable safety, not asserted safety** — CI-enforced disclaimer + Tier-C back-translation + COI/
   independence + verified locale data. A partner can *inspect the provenance* of every phrase.
3. **Offline-first paper runtime** — works at a roadside/shelter with no power, network, or app, where
   Google Translate / Tarjimly / Red Cross apps degrade.
4. **Honest scope** — explicitly de-positioned against MT and against replacing interpreters; this is a
   trust and adoption advantage with health-system language-access offices bound by §1557.
5. **Reusable high-stakes translation pipeline** as a portfolio asset shared with sibling projects (§7).

---

## 5. Claude API leverage (and hard limits)

**Where Claude (donated-lane agent, human-run) accelerates drafting — all output is draft-only:**
- **Phrase-bank & scenario authoring:** draft the English-pivot canonical phrase set per scenario,
  propose A/B/C tier + context-of-use + preserved tokens, run the cross-cultural-clarity checklist
  (flag idioms, inverting negations, yes/no-gesture ambiguity) as a first pass.
- **Translation drafts + structured back-translation QA:** produce a draft translation honoring
  glossary/register/gender and preserved tokens, then *independently back-translate* and surface a
  diff/semantic-divergence report for the human reviewer (esp. negation inversion, number/unit drift,
  false friends). Claude is excellent at *generating the QA artifact* reviewers adjudicate.
- **Pictogram matching & gap-finding:** suggest candidate open-licensed icons per phrase, flag phrases
  lacking a pictogram, and propose comprehension-test prompts for target-population review.
- **License/provenance drafting:** draft allow-list entries, attribution strings, and provenance YAML
  from a source page; flag NC/SA/IGO terms and mandatory disclaimers (e.g. WHO) for human verification.
- **Glossary bootstrapping & `UNCERTAIN:` flag generation** — operationalized exactly as the plan
  specifies (typed flags into `review.yaml`).
- **Consistency/lint at scale:** detect when the same phrase ID drifts across languages, or a Tier-C
  pack is missing a back-translation/disclaimer (pre-CI sanity pass).

**Where Claude must NOT decide (hard guardrails):**
- **No high-stakes medical/emergency phrase ships on model output alone — ever.** Native/near-native +
  (for Tier C) clinician/medical-interpreter sign-off + back-translation is mandatory. Claude drafts;
  humans certify.
- **Raw machine translation is NEVER shipped for emergencies** — the Google-Translate ED evidence
  (55–67% accuracy in some languages) applies equally to any LLM; treat LLM output as *unverified
  draft* with the same suspicion as MT.
- **Claude never adjudicates the "not a substitute for an interpreter" boundary or relaxes the
  disclaimer**, never authors clinical advice (dosing/treatment/diagnosis), and never authors
  excluded legal/rights/coercive phrases.
- **Claude does not clear licenses** — it drafts the allow-list entry; the human license reviewer
  verifies terms and signs `license-000`.
- **Claude does not verify emergency numbers/locale protocol** — official-source human verification
  only (a wrong number is a safety defect).
- **Back-translation by Claude is a *check that flags*, not a *gate that passes*** — it cannot
  self-certify; a human reconciles divergences.

---

## 6. Ten concrete optimizations

1. **Tighten Tier-C to a true 2-person ISO-style flow:** independent forward translator + independent
   reviewer + independent back-translator + recorded reconciliation. Make back-translator independence
   explicit in `review.yaml` (parallel to existing reviewer-independence rule).
2. **Promote medical/safety Tier-B triage/evacuation phrases to the medical-capable-reviewer
   requirement** (a misunderstood "evacuate now"/"is anyone inside?" is high-harm). Re-test the A/B/C
   cut against *harm-on-mistranslation*, not topic.
3. **Add a pictogram-comprehension check to the reviewer checklist in M0/M1** (naive target-language
   reader interprets icon as intended), decoupled from securing a field partner.
4. **Move minimal romanized pronunciation for Tier-C phrases to M0** (not M1), since the
   responder↔newcomer value depends on being understood under stress and by low-literacy users.
5. **Add a usability target metric:** time-to-find a phrase and time-to-first-contact in simulated
   trials, not just "positive feedback" — a card that's correct but unnavigable fails in the field.
6. **Cite the evidence base in-repo** (Flores/AHRQ on ad-hoc-interpreter harm; §1557 legal duty; the
   Google-Translate ED study) so M0 cold-start work carries an evidentiary anchor and the disclaimer is
   defensible. Pre-empts the for-profit/over-reliance critiques.
7. **Build a back-translation *diff report* artifact** (Claude-generated, human-adjudicated) committed
   per Tier-C phrase — turns back-translation from a checkbox into an auditable QA record.
8. **Define a delivered-pack recall/notify channel as an M2 delivery precondition** (printed cards
   can't be patched; the partner must agree to a defect-notification path; pair with the QR-to-latest).
9. **Publish a "shared vs forked" infrastructure table with `vital-info-translations`** so the two
   projects converge on *one* high-stakes translation pipeline (allow-list schema, glossary schema,
   review workflow) rather than diverging. Reduces duplicate license re-work.
10. **Add a "what this is NOT" partner-onboarding one-pager** that states the §1557 duty to provide
    qualified interpreters and positions the pack as supplement-only — turns the disclaimer into an
    adoption asset for risk-averse language-access offices, and reduces legal-liability friction.

---

## 7. Parallel & perpendicular spin-offs

- **Reusable high-stakes translation pipeline (the keystone asset):** the criticality-tiering + license-
  as-data allow-list + back-translation gate + clinician sign-off + provenance + CI structural checks
  generalize to *any* high-stakes multilingual content. Factor it into a shared package/process that
  `vital-info-translations`, `multilingual-signage-templates`, and future projects consume. This is the
  most valuable perpendicular outcome — a portfolio-wide "reviewed translation" capability.
- **`open-pictograms`** — emergency point-to icon set (body diagram, hazards, allergy/medication,
  do-not-move) authored/curated open-licensed, comprehension-tested; consumed by phrasebooks **and**
  signage. A genuinely reusable open asset that fills a real gap (Kwikpoint's are proprietary).
- **`multilingual-signage-templates`** — same phrase bank + locale data rendered at signage size for
  shelters/clinics/triage tents (lamination-ready, complex-script). Parallel deliverable from one
  source of truth.
- **`vital-info-translations`** — share allow-list/glossary/review machinery; this project = short
  first-contact utterances, sibling = longer vital-information documents. Co-develop the pipeline.
- **`first-aid-open`** — pair phrase packs with open first-aid step content (complement to Red Cross/
  IFRC apps) so the responder both *communicates* and *acts*; keep the communication-only/no-clinical-
  advice boundary intact (first-aid content is separately reviewed).
- **`community-resource-maps`** — link the "find a qualified interpreter / telephonic line / clinic"
  panel (à la Kwikpoint's language-ID panel) to locale-specific resource data, reinforcing the bridge-
  to-interpreter framing.
- **Partnerships (high-leverage):** **TWB/CLEAR Global** as reviewer-network + glossary partner;
  **Red Cross/IFRC** and **Tarjimly** as distribution/complement partners; **hospital language-access
  offices** (§1557-bound) as priority adopters; **UNHCR/Save the Children/CDC** as sources and reach.

---

## 8. Open questions

1. **Reviewer supply is the true bottleneck.** Securing medical-capable reviewers across needed
   languages may be harder than securing a partner. Should M1 prioritize a **TWB/CLEAR Global
   reviewer-network partnership** over recruiting individuals? (The plan lists both; pick a primary.)
2. **CC-BY-SA 4.0 vs CC0 for content.** Refugee Phrasebook chose CC0 for maximum reuse; partners may
   prefer it. Does SA's copyleft impede a clinic/agency from remixing? Lean CC0 for the phrase bank,
   reserve SA only where a source forces it?
3. **Pronunciation/audio sequencing** — given the low-literacy/script-rendering gap (§1.4), is text-
   first defensible for Tier C, or should minimal audio/romanization be pulled forward?
4. **Where exactly is the A/B/C line for operational hazard instructions?** (evacuation/"anyone
   inside?" — Tier B or C?) Needs a clinician + responder to co-set the rubric.
5. **Pictogram validation without a field partner** — how do we comprehension-test icons in M0/M1
   before M2? (informal target-community review protocol?)
6. **Delivered-pack recall** — what is the realistic notify channel for a defect found after printed
   distribution, given offline users?
7. **Surge/funded lane** — for a sudden-displacement event, is metered drafting-under-escrow ever
   warranted, and how is the budget cap + human-review gate preserved under time pressure?
8. **Relationship to `multilingual-signage-templates` / `vital-info-translations`** — one shared
   pipeline+repo or three? (Recommend shared pipeline, separate deliverable repos.)

---

### Summary (top lines)

- **Top 3 competitors/analogs:** (1) **Refugee Phrasebook** (open/CC0, 28+ langs, but crowd-sourced
  without clinical/criticality review); (2) **Kwikpoint** point-to-talk medical cards (proven format
  but proprietary/closed); (3) **Google Translate** as the cautionary baseline responders use today
  (ED study: 55–67% accuracy in some languages — proof raw MT is unsafe). TWB/CLEAR Global, Tarjimly,
  and Red Cross/IFRC are best treated as **partners/complements**, not competitors.
- **Single strongest differentiator:** **the empty quadrant — "open + clinically-reviewed +
  criticality-tiered with auditable provenance."** No one is both freely forkable like Refugee
  Phrasebook *and* Tier-C-reviewed/back-translated like a professional product, with CI-enforced safety
  (disclaimer + back-translation) a partner can inspect.
- **Top 3 Claude-API ideas:** (1) draft phrase bank + tier/preserved-token proposals with the cross-
  cultural-clarity checklist as a first pass; (2) translation drafts **plus an independent back-
  translation diff/QA report** that flags negation/number/false-friend divergence for human
  adjudication; (3) license/provenance + pictogram-match drafting with typed `UNCERTAIN:` flags — all
  draft-only, never shipped without native + (Tier C) clinician sign-off.

**Two most important plan-correctness findings:** (1) **High-stakes translation accuracy is handled
well structurally, but back-translation independence must be made explicit and Tier-B medical/triage
operational phrases should inherit the medical-capable-reviewer requirement** — the A/B/C cut should be
re-tested against *harm-on-mistranslation*, not topic, because a misunderstood "evacuate now" is
plausibly Tier C. (2) **The "not a substitute for an interpreter" boundary is correctly structural, and
should be reinforced by citing the legal/clinical evidence in-repo (§1557 qualified-interpreter duty;
ad-hoc-interpreter harm studies; the Google-Translate ED accuracy collapse) and by securing a
delivered-pack recall channel** — printed offline cards cannot be patched, so a discovered Tier-C
defect needs a pre-agreed partner notify path.
