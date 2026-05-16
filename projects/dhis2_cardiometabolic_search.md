# DHIS2 for cardiometabolic diseases: a global evidence search

May 16, 2026

## Introduction

Noncommunicable diseases (NCDs) killed at least 43 million people in 2021, equivalent to 75% of non-pandemic-related deaths globally, [according to the WHO](https://www.who.int/news-room/fact-sheets/detail/noncommunicable-diseases). The largest category of these, *cardiometabolic NCDs*, refers to the interconnected cluster of conditions that includes insulin resistance, type 2 diabetes (T2DM), hypertension, and cardiovascular disease (CVD), accounting for roughly half of all global NCD deaths.

As countries launch new programs to prevent and monitor NCDs, many need to adapt their routine health information systems to better plan and target scarce public health resources. The data gap is particularly pressing for many LMICs, which are facing the epidemiological transition to higher NCD burden with a constrained health workforce, limited digital infrastructure for patient-level data systems, outdated surveys of population-level NCD prevalence, all while record decreases in foreign assistance for health exacerbate endemic infectious diseases such as HIV, TB, and malaria.

DHIS2 is a digital public good used in over 120 countries and by 80+ national health authorities to manage routine health data. Most national DHIS2 implementations are in LMICs, where over 73% of deaths due to NCDs occur.

While DHIS2 core software is maintained by the HISP Centre at the University of Oslo, DHIS2 implementers can define the contents of their own DHIS2 database without alerting DHIS2 developers or the wider HISP network. With a global and diverse user base, understanding the scope, depth, and complexity of specific use cases in the health domain presents a critical challenge for the HISP Centre’s global support team. The HISP Centre's intel on DHIS2 use cases typically relies on academic research, Google Alerts, the DHIS2 Community of Practice, and the collective wisdom of 23 HISP groups providing country and regional DHIS2 support in the field.

This blog post describes an approach to identify use of **DHIS2 for cardiometabolic NCDS by governmental health authorities**, through an AI-assisted targeted websearch pipeline built with Claude Code's Opus 4.7 model.


> __Results identified 42 countries with direct evidence of some data capture of hypertension, diabetes, *or* CVD in a government-owned DHIS2 system.__ A further 32 countries showed inconclusive or indirect evidence, and require further investigation.

This work builds on my previous reports for more targeted searches with Claude for NCD-relevant DHIS2 usage: [heat health early warning systems](https://github.com/iambodo/iambodo.github.io/blob/master/projects/hhews_country_search.md) and [cancer registries](https://github.com/iambodo/iambodo.github.io/blob/master/projects/dhis2_cancer_search.md).

## Methodology

### Country and language coverage
A search term list was developed, tailored for every country with a known government-owned DHIS2 implementation: 116 unique countries x 56 languages x 236 country-language search rows. The country list is drawn from `country_langs.csv` (reused from the cancer pipeline, broadly aligned with WHO Member States in LMIC regions). National HMIS aliases (KHIS, DHIMS2, SNIGS, SISMA, SNIS, ENDOS-BF, MISPAS, BHMIS, SISPRO, etc.) are sourced from `hmis_aliases.csv` and appended to each country's query so that local DHIS2 brand names are matched alongside the generic "DHIS2" terms.

### English search terms (24)

The following 24 English keywords were used for every country. Each was translated once per language and reused across all countries sharing that language.

1. Diabetes
2. Hypertension
3. Diabetes registry
4. Hypertension registry
5. NCD clinic register
6. PEN protocol
7. HEARTS technical package
8. Cardiovascular disease surveillance
9. Blood pressure screening program
10. Hypertension control program
11. Diabetes screening program
12. HbA1c monitoring
13. Glycemic control surveillance
14. Stroke registry
15. Acute myocardial infarction registry
16. Cardiac rehabilitation tracking
17. Statin therapy monitoring
18. NCD risk factor surveillance
19. STEPS survey
20. Hypertension monitoring
21. Diabetes monitoring
22. Community-based hypertension screening
23. Type 2 diabetes management
24. Chronic disease management system

### Translation strategy

Each keyword was translated **once per language** (56 languages total) and reused across every country speaking that language. A French translation, for example, was reused for every Francophone country. Acronyms (HbA1c, PEN, HEARTS, STEPS, NCD, CVD, PSA, MI) were kept in Latin script across all languages, with the surrounding noun translated. Less-widely used African creoles and local languages (Bislama, Tok Pisin, Pidgin, Mende, Wolof, Tongan, etc.) retain clinical English terms with the surrounding words translated, which mirrors how those terms may actually appear in country HMIS documentation.

### Query template

For each (country, language) row, the search query is built as:

```
("<Localised country name>") & ("<keyword 1>" OR "<keyword 2>" OR ... OR "<keyword 24>") & ("DHIS2" OR "DHIS-2" OR "DHIS 2" OR "DHIS II" OR "<HMIS alias 1>" OR "<HMIS alias 2>" ...)
```

Example - Kenya, English (HMIS alias = KHIS):

```
("Kenya") & ("Diabetes" OR "Hypertension" OR "Diabetes registry" OR ... OR "Chronic disease management system") & ("DHIS2" OR "DHIS-2" OR "DHIS 2" OR "DHIS II" OR "KHIS")
```

Example - Benin, French (HMIS alias = SNIGS):

```
("Benin") & ("Diabete" OR "Hypertension" OR "Registre du diabete" OR ... OR "Systeme de gestion des maladies chroniques") & ("DHIS2" OR "DHIS-2" OR "DHIS 2" OR "DHIS II" OR "SNIGS")
```

### Phase 2 execution

Each of the 236 country-language rows ran through Google WebSearch with `user_location` set to the country (so results are localised to that country's Google index). Up to 10 unique hits were retained per country across all languages, with continuous numbering across the per-language subsections.

### Result filtering rules

1. **Drop any URL containing `dhis2.org`** - self-referential, inflates the DHIS2-use signal.
2. **Drop pages primarily about gestational diabetes or hypertension** - these are ANC-related rather than chronic-NCD care.
3. **Drop WHO / PAHO / Resolve-to-Save-Lives / linkscommunity.org links unless the country name appears in the URL or the page title.** A "relevant to <country>" mention only in the description does not qualify - that is boilerplate, not a country-specific source.
4. **Prefer government / MoH / Public Health Institute / WHO / Lancet / PubMed / SciELO / `.gov` sources from the last 5 years.**

### Verdict rubric

- **CONFIRMED** - explicit named DHIS2 (or local alias) capture of at least one of diabetes, hypertension, or CVD data. Aggregate counts qualify; patient-level Tracker / case-based registry is noted in the country's one-line hook when present.
- **MODERATE** - DHIS2 is the country's HMIS and cardiometabolic activity exists in the system landscape, but the search did not surface explicit DHIS2 capture of diabetes / hypertension / CVD indicators.
- **LIKELY** - DHIS2 is the country's HMIS and cardiometabolic care exists, but no direct evidence linking the two was found.
- **UNCLEAR** - mixed or ambiguous signals (e.g. DHIS2 piloted but national HMIS still paper-based at facility level).
- **NONE** - country uses a non-DHIS2 system (SISPRO, SNVS, RIPSA, etc.) or no DHIS2 deployment found.
- **UNKNOWN** - insufficient information to assess.

MODERATE outranks LIKELY because MODERATE has direct evidence cardiometabolic indicators are in the national HMIS landscape, whereas LIKELY is inferential.

### Targeted HMIS-angle re-search

After the initial pass, a follow-up round of searches was run against every country initially scored LIKELY or MODERATE (38 countries, split into 5 parallel batches). The goal was to find Ministry of Health / Public Health Institute / national HMIS reports that explicitly cite DHIS2 (or the local alias) as the source for diabetes, hypertension, NCD risk-factor, or CVD data. Each country received three additional targeted queries:

1. `"<Country>" "DHIS2" (hypertension OR diabetes OR "non-communicable")`
2. `"<Country>" "<HMIS_alias>" (hypertension OR diabetes OR NCD) report`
3. `"<Country>" (MoH OR "Ministry of Health" OR "Public Health Institute") (hypertension OR diabetes) (HMIS OR routine OR aggregate)`

Where applicable, terms were translated into French, Spanish, or Portuguese. Promotions to CONFIRMED were only made when the new source explicitly stated that DHIS2 / the alias / the national HMIS routine data system contains or was used for cardiometabolic data for that country. The targeted re-search promoted six countries to CONFIRMED: **Bhutan, Comores, Dominican Republic, Gambia, Panama, Tanzania**. Uganda was promoted separately based on the UNIPH 2024 hypertension-trends report drawn from DHIS2 OPD data.

### Link verification

All 860 cited URLs across the 116 profiles were checked with a HEAD-first / GET-fallback script with per-host rate limiting. 830 URLs resolved (96.5%). The 30 broken or unreachable URLs are annotated inline next to the URL as `[BROKEN: <code>]` (e.g. `[BROKEN: 404]`, `[BROKEN: 502]`, `[BROKEN: timeout]`).

### Known limitations

- Search is one-shot Google with country `user_location`; coverage of country MoH gazettes, internal HMIS PDFs, and Google Scholar is partial. Many LIKELY countries almost certainly capture cardiometabolic indicators in their national DHIS2 - the absence of an English-language indexed citation is the limiting factor, not the underlying reality.
- DHIS2 capture status reflects what was surfaced as of search date. The rapid HEARTS rollout in the PAHO region means the CONFIRMED count for Latin America and the Caribbean is likely undercounted for very recent activity.
- The 24-keyword list and verdict rubric drive the country list and counts; if either is changed materially, the results will shift.

---

## Verdict summary (116 countries)

| Verdict | # countries | % | Countries |
|---|---:|---:|---|
| CONFIRMED | 42 | 36% | Antigua and Barbuda, Bangladesh, Benin, Bhutan, Burundi, Cameroon, Central African Republic, Chile, Comores, Dominican Republic, DRC, Ecuador, El Salvador, Ethiopia, Gambia, Ghana, Grenada, Guatemala, Iraq, Kenya, Lebanon, Liberia, Malawi, Mali, Mozambique, Myanmar, Nepal, Nicaragua, Nigeria, Panama, Rwanda, Saint Lucia, Senegal, Sierra Leone, South Africa, Sri Lanka, Tajikistan, Tanzania, Togo, Uganda, Vietnam, Zimbabwe |
| MODERATE | 12 | 10% | Afghanistan, Angola, Burkina Faso, Cambodia, Colombia, Equatorial Guinea, Eswatini, Guinea, Niger, Sao Tomé and Principe, Somalia, Somalia - Puntland State |
| LIKELY | 20 | 17% | Botswana, Cape Verde, Chad, Congo Republic (Brazzaville), Côte d'Ivoire, Guyana, Honduras, Jamaica, Lesotho, Madagascar, Mauritania, Namibia, Papua New Guinea, Paraguay, South Sudan, Sudan, Timor Leste, Venezuela, Zambia, Zanzibar |
| UNCLEAR | 10 | 9% | Djibouti, Haiti, Pakistan, Palestine, Philippines, Seychelles, Solomon Islands, Somaliland, Syria North West, Vanuatu |
| NONE | 17 | 15% | Algeria, Argentina, Brazil, Costa Rica, DPR Korea, India, Indonesia, Kazakhstan, Kyrgyzstan, Mauritius, Morocco, Norway, Suriname, Syria MoH, Thailand, Ukraine, Uzbekistan |
| UNKNOWN | 15 | 13% | Dominica, Egypt, Eritrea, Gabon, Guinea Bissau, Jordan, Kiribiati, Lao, Libya, Maldives, Mongolia, Tonga, Tunisia, Western Sahara, Yemen |

---
## Cardiometabolic & DHIS2 — Country Summary (ranked by verdict)

Verdict ordering (strongest → weakest evidence of DHIS2 use for cardiometabolic data): **CONFIRMED → MODERATE → LIKELY → UNCLEAR → NONE → UNKNOWN**. **CONFIRMED** requires explicit DHIS2/alias capture of AT LEAST ONE of {diabetes, hypertension, CVD} — aggregate counts qualify; patient-level Tracker/registry is noted in the hook when present. MODERATE outranks LIKELY because MODERATE has cardiometabolic activity in the HMIS landscape, whereas LIKELY is inferential (DHIS2 exists + cardiometabolic care exists, but no direct link found).

### CONFIRMED (42)

_Explicit named DHIS2 (or local alias) capture of at least one of diabetes, hypertension, or CVD data._

- **[Antigua and Barbuda](#antigua-and-barbuda-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — National DHIS2 since 2022 with planned HEARTS hypertension module across ten PAHO-supported primary care clinics
- **[Bangladesh](#bangladesh-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — World's largest DHIS2 captures NCD Corner hypertension and diabetes outputs alongside Simple app longitudinal tracking
- **[Benin](#benin-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — SNIGS DHIS2 captures hypertension as the sole NCD risk factor; no diabetes or CVD indicators
- **[Bhutan](#bhutan-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — BHMIS on DHIS2 receives aggregate hypertension and diabetes flows from hospitals and Basic Health Units
- **[Burundi](#burundi-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DSNIS DHIS2 records roughly 38000 diabetes and 98000 hypertension cases per WHO Africa
- **[Cameroon](#cameroon-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — MINSANTE DHIS2 instance captures NCD service indicators alongside newly launched PEN-Plus rollout
- **[Central African Republic](#central-african-republic-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 registered 80407 hypertension and 19932 diabetes cases between 2021 and May 2024
- **[Chile](#chile-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — MINSAL DHIS2 expanded to HEARTS hypertension and diabetes indicators across 29 health services
- **[Comores](#comores-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — National DHIS2 HMIS reported rising type 2 diabetes cases for 2024-2025 per Comorian press
- **[Dominican Republic](#dominican-republic-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — MISPAS operationalising DHIS2 for HEARTS CVD data across 703 primary care centres
- **[DRC](#drc-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — SNIS DHIS2 routine monthly reports tracked hypertension and diabetes progression 2019-2023 in peer-reviewed study
- **[Ecuador](#ecuador-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — National HEARTS reporting on PAHO DHIS2 platform across 483 facilities targeting universal first-level coverage
- **[El Salvador](#el-salvador-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — PAHO deployed HEARTS Monitoring and Evaluation System on DHIS2 with MINSAL since 2022
- **[Ethiopia](#ethiopia-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — National DHIS2 underpins EHCI HEARTS hypertension rollout with 99% NCD reporting completeness
- **[Gambia](#gambia-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — WDF scale-up integrating standard NCD indicators into DHIS2 with HMIS and NCD clinic staff training
- **[Ghana](#ghana-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIMS2 since 2012 captures aggregated NCD/CVD indicators; Ghana Heart Initiative addresses hypertension cascade gaps
- **[Grenada](#grenada-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 instrumental in Caribbean HEARTS data collation; digital hypertension registries replaced paper in 2025
- **[Guatemala](#guatemala-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 piloted at INCAP for HEARTS hypertension and diabetes monitoring across 11 primary care facilities
- **[Iraq](#iraq-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — KRG-DHIS2 system in Kurdistan Region captures facility-based NCD data; federal deployment unconfirmed
- **[Kenya](#kenya-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — KHIS captures hypertension and diabetes indicators; SPICE platform integrates primary care NCD data into DHIS2
- **[Lebanon](#lebanon-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — MoPH DHIS2 since 2014 supported MSF management of 2644 Syrian refugees with diabetes and hypertension
- **[Liberia](#liberia-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — Project TREND distributing 900+ NCD registers feeding hypertension and diabetes data into national DHIS2
- **[Malawi](#malawi-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — Diabetes Compass added NCD module to DHIS2 in March 2024 with community screening algorithm and referral
- **[Mali](#mali-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — SNISS/SLIS DHIS2 since 2015 supports chronic-disease MADO reporting; HEARTS/PEN specific use less documented
- **[Mozambique](#mozambique-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — SISMA DHIS2 used across health programs alongside facility-based NCD pilots in Maputo; HEARTS metadata limited
- **[Myanmar](#myanmar-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 Tracker deployed under SUNI-SEA identifying pre-diabetes diabetes and hypertension clusters across 75 villages
- **[Nepal](#nepal-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — AMPATH Nepal adapting DHIS2 for hypertension and diabetes dashboards aligned with HEARTS technical package
- **[Nicaragua](#nicaragua-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — PAHO HEARTS DHIS2 platform captures aggregate CVD outcome process and structural indicators for Nicaragua
- **[Nigeria](#nigeria-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 Android Tracker manages Nigeria Hypertension Control Initiative with HEARTS360 dashboard across Kano Ogun FCT
- **[Panama](#panama-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — MoH and CSS enter aggregate hypertension and diabetes indicators into PAHO HEARTS DHIS2 platform at INCAP
- **[Rwanda](#rwanda-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — National DHIS2 HMIS captures NCD screening with all health centres trained on routine reporting
- **[Saint Lucia](#saint-lucia-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — Active participant in PAHO HEARTS DHIS2 platform; hypertension registry grew 17.8% across six PHC facilities
- **[Senegal](#senegal-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — National DHIS2 integrated 45 NCD indicators with eTracker dashboard supporting longitudinal cardiometabolic follow-up
- **[Sierra Leone](#sierra-leone-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 since 2012 captures hypertension records submitted by facility M&E officers to NCD Directorate
- **[South Africa](#south-africa-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — WebDHIS captures NCD data elements with 85.1% reporting rate across Eastern Cape chronic-disease monitoring
- **[Sri Lanka](#sri-lanka-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 anchors Diabetes Compass screening and HEARTS360 dashboards updated daily from DHIS2 and Simple
- **[Tajikistan](#tajikistan-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 referenced as source for oblast hypertensive case counts in WHO PEN/HEARTS PHC pilot evaluation
- **[Tanzania](#tanzania-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — National DHIS2 at dhis.moh.go.tz captures hypertension and diabetes data central to PEN Plus monitoring
- **[Togo](#togo-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 collects monthly hypertension and diabetes WHO PEN surveillance with WhatsApp-based district focal point coordination
- **[Uganda](#uganda-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — National DHIS2 captures hypertension as routine OPD indicator; UNIPH published 2016-2021 trends from DHIS2
- **[Vietnam](#vietnam-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 Tracker mobile app screened 6704 older adults for hypertension and diabetes via HelpAge ISHCs
- **[Zimbabwe](#zimbabwe-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — Customised DHIS2 Tracker Capture Android app captures integrated hypertension and diabetes screening within HIV program

### MODERATE (12)

_DHIS2 is the country HMIS and cardiometabolic activity exists, but the search did not surface explicit DHIS2 capture of diabetes/hypertension/CVD._

- **[Afghanistan](#afghanistan-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — MoPH Data Warehouse runs on DHIS2 but routine NCD indicators not documented; STEPS carries surveillance
- **[Angola](#angola-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 transition underway as core HMIS but no cardiometabolic module documented yet
- **[Burkina Faso](#burkina-faso-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — ENDOS-BF DHIS2 in production since 2013 with Tracker modules but no documented cardiometabolic use
- **[Cambodia](#cambodia-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — National HMIS transitioning to DHIS2 in 2024-2025; aggregate diabetes and hypertension counts captured separately from registries
- **[Colombia](#colombia-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — SISPRO and RIPS are domestic registries; DHIS2 use limited to regional HEARTS dashboard reporting
- **[Equatorial Guinea](#equatorial-guinea-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 national HMIS piloted February-May 2025 covering IMCI; no cardiometabolic modules yet
- **[Eswatini](#eswatini-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — National DHIS2 HMIS alongside WHO-PEN@Scale trial but no dedicated cardiometabolic module confirmed
- **[Guinea](#guinea-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — National DHIS2 surveillance since 2018 post-Ebola but no NCD or HEARTS registry confirmed
- **[Niger](#niger-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — National DHIS2 SNIS instance supports WHO-PEN rollout in 9 districts but no cardiometabolic Tracker documented
- **[Sao Tomé and Principe](#sao-tomé-and-principe-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 with Tracker module since 2020 for individual-level registries; cardiometabolic configuration not documented
- **[Somalia](#somalia-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 is national data backbone with NCD integration explicitly flagged pending maturity gains
- **[Somalia - Puntland State](#somalia---puntland-state-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — Operates within federal Somalia DHIS2 backbone; NCD integration flagged as future priority not current use

### LIKELY (20)

_DHIS2 is the country HMIS and cardiometabolic care exists, but no direct evidence linking the two was found._

- **[Botswana](#botswana-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 aggregate HMIS alongside OpenMRS; InterCARE trial leverages digital infrastructure but no NCD module confirmed
- **[Cape Verde](#cape-verde-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 deployed nationally for COVID-19 but diabetes management still paper-based per MoH manual
- **[Chad](#chad-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 national HMIS since 2022 with 96% district reporting but no cardiometabolic registry evidence
- **[Congo Republic (Brazzaville)](#congo-republic-brazzaville-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — National DHIS2 launched with WHO PEN pilots in 20 districts but no cardiometabolic linkage confirmed
- **[Côte d'Ivoire](#côte-divoire-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — SIGSANTE DHIS2 since 2015 with WHO Technical Working Group; no NCD module surfaced
- **[Guyana](#guyana-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — National HEARTS expansion since 2023 likely feeding PAHO DHIS2 platform but country-specific use unconfirmed
- **[Honduras](#honduras-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — PAHO HEARTS DHIS2 platform supports CVD indicators regionally but Honduras-specific use not documented
- **[Jamaica](#jamaica-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 used for National Cancer Registry but no diabetes or hypertension surveillance documented
- **[Lesotho](#lesotho-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 national HMIS alongside ComBaCaL CHW screening on separate tablet app; no cardiometabolic module confirmed
- **[Madagascar](#madagascar-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 core HMIS across 58 districts and 1687 facilities but no cardiometabolic module confirmed
- **[Mauritania](#mauritania-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — Regional aliases list HMIS as DHIS2 but no specific NCD or cardiometabolic module described
- **[Namibia](#namibia-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 national HMIS via MoHSS but no explicit cardiometabolic registry or HEARTS package documented
- **[Papua New Guinea](#papua-new-guinea-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — NHIS on DHIS2 but no individual-level diabetes or hypertension tracking surfaced for HEARTS-style program
- **[Paraguay](#paraguay-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — Participates in PAHO HEARTS DHIS2 platform at INCAP but Paraguay-specific deployment not documented
- **[South Sudan](#south-sudan-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 national HMIS since 2018 but cardiometabolic indicators not operationalised in MoH
- **[Sudan](#sudan-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 for hospital reporting per WHO EMRO but no operational diabetes hypertension or CVD modules
- **[Timor Leste](#timor-leste-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — TLHIS DHIS2 supports Timor Hearts program targeting 50000 hypertension and diabetes patients by 2025
- **[Venezuela](#venezuela-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — HEARTS pilot in La Marroquina plausibly feeds PAHO DHIS2 platform but country-specific deployment undocumented
- **[Zambia](#zambia-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — Long-standing DHIS2 national HMIS alongside TASKPEN trial but no published DHIS2 Tracker for NCDs surfaced
- **[Zanzibar](#zanzibar-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — Related digital systems to mainland Tanzania DHIS2 but explicit Tracker cardiometabolic case management unconfirmed

### UNCLEAR (10)

_Mixed or ambiguous signals on DHIS2 use for cardiometabolic data._

- **[Djibouti](#djibouti-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — Digitalising HIS and adopting HEARTS but no public source confirms DHIS2 for cardiometabolic surveillance
- **[Haiti](#haiti-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — Part of HEARTS in the Americas DHIS2 platform but Haiti-specific cardiometabolic registry use not confirmed
- **[Pakistan](#pakistan-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 used in some provinces for routine data but cardiometabolic-specific national use not documented
- **[Palestine](#palestine-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — MoH NCD surveillance uses eSTEPS ODK and UNRWA e-Health rather than DHIS2
- **[Philippines](#philippines-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2 not dominant national HMIS; PhilPEN runs Hypertension e-Registry on iClinicSys and DOH eHealth
- **[Seychelles](#seychelles-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — SEY-PEN handles hypertension and diabetes screening but no public source documents DHIS2 as the platform
- **[Solomon Islands](#solomon-islands-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No DHIS2 confirmed; HMIS reporting historically used other tools and Pacific regional systems
- **[Somaliland](#somaliland-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — National health information database launched but DHIS2 not identified as platform for NCDs
- **[Syria North West](#syria-north-west-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — Whole-of-Syria cluster uses EWARN and HeRAMS reporting; no DHIS2-based NCD surveillance confirmed inside region
- **[Vanuatu](#vanuatu-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — HMIS used for routine reporting but no DHIS2 Tracker cardiometabolic registry or HEARTS implementation found

### NONE (17)

_Country uses a non-DHIS2 system for cardiometabolic data, or no DHIS2 deployment found._

- **[Algeria](#algeria-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No DHIS2 footprint; NCD surveillance through WHO STEPS and SISDZ rather than HMIS
- **[Argentina](#argentina-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No DHIS2 deployment; SNVS 2.0 and HiperDia-style registries anchor national cardiometabolic surveillance
- **[Brazil](#brazil-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No DHIS2; DATASUS runs HiperDia hypertension and diabetes registry plus VIGITEL and SIM
- **[Costa Rica](#costa-rica-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No DHIS2; CCSS runs EDUS and the SVD diabetes surveillance system independently
- **[DPR Korea](#dpr-korea-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — Closed national HIS separate from DHIS2 ecosystem; HEARTS roadmap under WHO preparation
- **[India](#india-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No national DHIS2; NCD-IT Simple App and Ayushman Bharat anchor IHCI hypertension registries
- **[Indonesia](#indonesia-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No DHIS2; MoH runs ASDK/SatuSehat/SIMPUS architecture and Posbindu PTM web-based surveillance
- **[Kazakhstan](#kazakhstan-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No DHIS2; UNEHS national electronic records support diabetes and NCD surveillance
- **[Kyrgyzstan](#kyrgyzstan-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No DHIS2; national PHC database and State Register of Diabetes Patients drive surveillance
- **[Mauritius](#mauritius-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No DHIS2; Health Information Bureau and periodic national NCD surveys carry cardiometabolic surveillance
- **[Morocco](#morocco-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No DHIS2 evidence; NCD surveillance through STEPS framework with digital tools under exploration
- **[Norway](#norway-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — DHIS2's home does not use it clinically; NDR-A and HUNT registries handle cardiometabolic surveillance
- **[Suriname](#suriname-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No DHIS2 use confirmed for any health program; NCD surveillance via national surveys and PAHO frameworks
- **[Syria MoH](#syria-moh-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No DHIS2 evidence inside MoH-controlled areas; HMIS runs as separate platform with WHO NCD kit support
- **[Thailand](#thailand-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No DHIS2; MOPH HDC and NHSO databases handle long-running HTN and DM screening reporting requirements
- **[Ukraine](#ukraine-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No DHIS2; national NHSU eHealth platform runs cardiometabolic data under WHO/Europe HEARTS rollout
- **[Uzbekistan](#uzbekistan-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No DHIS2; bespoke UZ-SPEED NCD tool digitises screening under WHO PEN/HEARTS rollout

### UNKNOWN (15)

_Insufficient information to assess._

- **[Dominica](#dominica-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No public source identifies national HIS platform; PAHO Caribbean HEARTS dashboards likely used
- **[Egypt](#egypt-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — Ministry-led HMIS not identified as DHIS2; 100 Million Seha screening runs separately
- **[Eritrea](#eritrea-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — National HMIS platform not described; WHO-PEN pilots and STEPS surveillance without confirmed DHIS2
- **[Gabon](#gabon-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — 2026 WHO pilot training providers across four departments; no DHIS2 cardiometabolic platform identified
- **[Guinea Bissau](#guinea-bissau-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No national DHIS2 deployment confirmed; population registries fragmented and limited for cardiometabolic surveillance
- **[Jordan](#jordan-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — HEARTS embedded in existing HIS but no source documents DHIS2 as Jordan's NCD platform
- **[Kiribiati](#kiribiati-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No DHIS2 evidence for diabetes hypertension or CVD; NCD monitoring rests on STEPS and PEN registers
- **[Lao](#lao-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — Uses DHIS2 for routine HMIS but cardiometabolic registry application not directly confirmed in sources
- **[Libya](#libya-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No DHIS2 evidence; NCD surveillance through WHO STEPS methodology and NCDC-led national survey
- **[Maldives](#maldives-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No national DHIS2 deployment documented; Health Protection Agency runs separate HMIS
- **[Mongolia](#mongolia-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No DHIS2; MongPEN runs on homegrown eHealth and ePrescription infrastructure for cardiometabolic screening
- **[Tonga](#tonga-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No DHIS2 use documented; Pacific NCD reporting typically uses bespoke SPC-supported tools and clinical registries
- **[Tunisia](#tunisia-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No DHIS2 evidence; national HMIS historically uses bespoke SIGS/SI-NCD platforms for cardiometabolic surveillance
- **[Western Sahara](#western-sahara-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No country-specific NCD digital surveillance system; data reported under Morocco or via UNHCR channels
- **[Yemen](#yemen-cardiometabolic-diabeteshypertensioncvd-dhis2-profile)** — No DHIS2 use documented for diabetes hypertension or CVD; national HMIS fragmented by conflict

---

### Afghanistan — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Afghanistan faces a high cardiometabolic burden (hypertension ~46%, diabetes ~13% per the 2018 STEPS survey). The Ministry of Public Health uses DHIS2 as its national HMIS (the MoPH Data Warehouse at moph-dw.gov.af), and Resolve to Save Lives has promoted DHIS2 Tracker for NCD programs in similar contexts. STEPS surveys (2011-12, 2018) are the primary NCD risk-factor surveillance vehicle, while routine NCD data within DHIS2 is not well documented.

DHIS2 USE: MODERATE
DHIS2 is the national HMIS via the MoPH Data Warehouse, and country has active NCD/STEPS programming, but direct attribution of cardiometabolic indicators within DHIS2 is not confirmed in the literature.

#### Search Results

##### English query results
1. **Afghanistan HMIS (DHIS2) — Digital Investment Exchange** — https://exchange.dial.global/projects/afghanistan-afghanistan-hmis-dhis2
   National HMIS implementation in Afghanistan using DHIS2.

2. **MoPH Data Warehouse (DHIS2 login)** — https://moph-dw.gov.af/dhis-web-dashboard/
   Live DHIS2 instance hosted by the Afghan Ministry of Public Health.

3. **Strategies to tackle non-communicable diseases in Afghanistan: A scoping review (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9992526/
   Reviews national NCD strategy 2015-2020 and 2022-2027, surveillance gaps, and PEN implementation.

4. **Afghanistan STEPS Survey 2018 (WHO microdata catalog)** — https://extranet.who.int/ncdsmicrodata/index.php/catalog/782
   Nationally representative STEPS survey of NCD risk factors, 3,956 adults, conducted Feb-Oct 2018.

5. **National NCD Risk Factors Survey 2011-12 report (WHO)** — https://cdn.who.int/media/docs/default-source/ncds/afghanistan_2011-12_steps_survey_article_5381d0ea-d9c9-42a8-b8fd-07b86dea6d24.pdf
   Earlier nationwide STEPS findings on diabetes and hypertension.

6. **Prevalence of major non-communicable diseases and their associated risk factors in Afghanistan: systematic review and meta-analysis (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10868487/
   Pooled estimates: hypertension 46%, diabetes 13.3%, obesity 31.2%.

7. **Prevalence and correlates of diabetes and impaired fasting glucose among adults in Afghanistan (Sage)** — https://journals.sagepub.com/doi/10.1177/20503121241238147
   National survey-based analysis of diabetes determinants (Dadras et al., 2024).

8. **Cooccurrence of NCD risk factors (ResearchSquare preprint)** — https://assets-eu.researchsquare.com/files/rs-4523447/v1/f0f09f64-090e-4571-9eaa-89aaeec500c5.pdf
   Multimorbidity analysis among Afghan adults.

9. **Prevalence of Risk Factors for NCDs in Urban Kabul (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC5927744/
   Kabul-specific cross-sectional study on hypertension, diabetes, overweight.

10. **Diabetes Mellitus Among Adults in Herat, Afghanistan (CAJGH)** — https://cajgh.pitt.edu/ojs/cajgh/article/view/271
    Cross-sectional diabetes study in Herat province.


### Algeria — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Algeria has high cardiometabolic burden, with national STEPS surveys (2003, 2016-17) documenting hypertension ~26-46% and increasing diabetes prevalence (T2DM ~46% among hypertensives in clinical cohorts). NCD surveillance is conducted via WHO STEPS and Ministry of Health programs, but no DHIS2 implementation is documented; Algeria is not a known DHIS2-implementing country, and the local HMIS (SISDZ) was not surfaced in NCD-specific contexts.

DHIS2 USE: NONE
No evidence of DHIS2 deployment in Algeria; national NCD surveillance uses STEPS and MoH systems independent of DHIS2.

#### Search Results

##### Arabic query results
1. **NCDs surveillance — Algeria (WHO)** — https://www.who.int/teams/noncommunicable-diseases/surveillance/data/algeria
   Country-specific NCD surveillance profile.
##### French query results
2. **Prévalence de l'HTA dans l'oasis d'El-Menia, Algérie (ScienceDirect)** — https://www.sciencedirect.com/science/article/abs/pii/S0003392813000589
   First HTA prevalence survey in Algeria (Sahara region), 44% in 40-99 age group.

3. **HTA associée au diabète chez le sujet âgé à Sidi Bel-Abbes (ScienceDirect)** — https://www.sciencedirect.com/science/article/abs/pii/S126236361472333X
   Comorbidity of hypertension and diabetes in elderly Algerians.
##### English query results
4. **Prevalence and risk factors of prehypertension and hypertension in Algeria (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9386961/
   Reviews STEPS-based prevalence; HTA reaches 26% (2003 STEPS).

5. **Perspectives of type 2 diabetes mellitus management in Algeria: a comprehensive expert review (Frontiers, 2025)** — https://www.frontiersin.org/journals/clinical-diabetes-and-healthcare/articles/10.3389/fcdhc.2025.1495849/full
   Expert review of T2DM management gaps in Algeria.

6. **Algeria — Setif and Mostaganem STEPS Survey 2003 (GHDx)** — https://ghdx.healthdata.org/record/algeria-s%C3%A9tif-and-mostaganem-steps-noncommunicable-disease-risk-factors-survey-2003 [BROKEN: 502]
   Subnational STEPS dataset for NCD risk factors.

7. **Prevalence, Awareness, and Treatment of Hypertension in 37 African Countries 2003-2022 (JACC)** — https://www.jacc.org/doi/10.1016/j.jacc.2025.09.1600
   Multi-country longitudinal hypertension trends including Algeria.

8. **NCD Situational Analysis: hypertension and diabetes (PATH)** — https://www.path.org/our-impact/resources/ncd-situational-analysis-a-landscape-assessment-of-challenges-and-opportunities-with-a-focus-on-hypertension-and-diabetes/
   Landscape assessment relevant for North Africa.

9. **WHO Tracking NCDs facility-based monitoring (ScienceDirect)** — https://www.sciencedirect.com/science/article/pii/S2589537025002366
    Initiative for facility-based NCD monitoring toward SDG 3.4.


### Angola — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Angola faces a rising hypertension and diabetes epidemic (urban DM prevalence 5.7-9.2%). The Ministry of Health is transitioning to DHIS2 as its core HMIS (notably under the NTD strategic plan PEN-DTN 2021-2025) and has piloted integrated TB-NCD screening in Luanda. Cardiometabolic surveillance remains fragmented, with diagnosis often late and no national NCD-specific DHIS2 module documented.

DHIS2 USE: MODERATE
DHIS2 is the national HMIS direction with active WHO-supported rollout; NCD-specific indicators or trackers are not yet explicitly documented in DHIS2.

#### Search Results

##### Portuguese query results
1. **O impacto crescente da hipertensão arterial e da Diabetes Mellitus em Angola (Revista Científica CSE)** — https://revistacientificacse.ao/index.php/revista/article/view/173
   Reviews growing burden of HTA and DM in Angola, fragile surveillance.

2. **Sistema de Saúde em Angola: contextualização, princípios e desafios (Redalyc)** — https://www.redalyc.org/journal/7041/704173376005/html/
   Health system contextualization including surveillance gaps.

3. **Um olhar sobre a saúde e nutrição em Angola (UNICEF)** — https://www.unicef.org/angola/media/3356/file/Um%20olhar%20sobre%20a%20sa%C3%BAde%20e%20nutri%C3%A7%C3%A3o%20em%20Angola.pdf
   Overview of health/nutrition challenges including NCDs.

4. **Plano Estratégico DTN Angola 2021-2025 (WHO ESPEN)** — https://espen.afro.who.int/sites/default/files/content/document/Angola_Plano_Estrate%CC%81gico_DTNs_2021_2025.pdf
   National plan referencing DHIS2 as HMIS platform.

5. **Uma Breve fotografia do Sistema Nacional de Saúde em Angola (Min. Finanças)** — https://www.ucm.minfin.gov.ao/cs/groups/public/documents/document/aw4z/mzgz/~edisp/minfin3383470.pdf
   National health system snapshot.
##### English query results
6. **The role of health information systems in transforming Angola's health system (Serbian J Medical Chamber)** — https://smj.rs/en/volume-6-no-2/the-role-of-health-information-systems-in-transforming-and-enhancing-the-efficiency-of-angolas-health-system [BROKEN: 404]
   Explicitly discusses transition to DHIS2 in Angola.

7. **CardioBengo study protocol (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC4774122/
   Population-based cardiovascular longitudinal study, Bengo Province.

8. **Hypertension in Northern Angola: prevalence, associated factors, awareness, treatment and control (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC3599429/
   Hypertension epidemiology in northern Angola.

9. **Integrating TB and NCD services: screening for diabetes and hypertension in TB patients in Luanda (PLOS ONE)** — https://journals.plos.org/plosone/article?id=10.1371%2Fjournal.pone.0218052
   Pilot integrated screening: 7,179 TB patients, 4.5% diabetes, 16.6% hypertension.

10. **Improving diabetes and hypertension diagnosis in TB patients, Angola (WDF)** — https://www.worlddiabetesfoundation.org/what-we-do/projects/wdf14-0873/
    World Diabetes Foundation-supported integrated screening project.


### Antigua and Barbuda — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Antigua and Barbuda has implemented DHIS2 nationally since 2022 in partnership with PAHO, with explicit plans to deploy HEARTS (NCD), cancer registry, STI, and psychiatric data modules. PAHO and the MoH have run multiple HEARTS sensitization and training workshops (2023-2026) to strengthen hypertension control across ten primary care clinics. The country is among five in the Americas on track for the 25% NCD premature-mortality reduction target.

DHIS2 USE: CONFIRMED
DHIS2 is the national HMIS with explicit roadmap to host HEARTS/NCD modules, alongside active HEARTS hypertension implementation.

#### Search Results

##### English query results
1. **PAHO supports training of Antigua and Barbuda health workers in DHIS2 (Feb 2026)** — https://www.paho.org/en/news/25-2-2026-paho-supports-training-antigua-and-barbuda-health-workers-dhis2
   Latest PAHO training; system covers surveillance and HEARTS NCD module plans.

2. **Surveillance and DHIS2 training for Antigua and Barbuda (PAHO, Sept 2025)** — https://www.paho.org/en/news/4-9-2025-surveillance-and-dhis2-training-antigua-and-barbuda
   Trained ~50 health professionals including epidemiologists and IT staff.

3. **Strengthening Hypertension Care: PAHO and MoH HEARTS training (Ministry of Health)** — https://health.gov.ag/strengthening-hypertension-care-paho-and-ministry-of-health-lead-hearts-training-in-antigua-barbuda/
   MoH announcement of HEARTS rollout.

4. **PAHO Trains Antigua and Barbuda Health Workers in DHIS2 (fundsforNGOs)** — https://news.fundsforngos.org/2026/02/26/paho-trains-antigua-and-barbuda-health-workers-in-dhis2-system/
   Coverage of DHIS2 capacity building.

5. **Health management tool strengthening health system (PAHO, Feb 2024)** — https://www.paho.org/en/news/14-2-2024-health-management-tool-strengthening-health-system
   PAHO announcement of DHIS2 deployment to strengthen the Antigua and Barbuda health information system.


### Argentina — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Argentina has well-established national NCD surveillance via the Ministry of Health's National Directorate for NCDs and the SNVS 2.0 surveillance system. The HEARTS Initiative was first piloted in 2018 and expanded substantially in Mendoza province during 2022-2024, mapping 287 primary care centers and 22 hospitals. HiperDia-style registries and the National Diabetes Prevention and Control Program (Resolution 1156/2014) underpin cardiometabolic surveillance. No DHIS2 deployment is documented in Argentina.

DHIS2 USE: NONE
Argentina uses national systems (SNVS 2.0, HEARTS tool, MoH registries) rather than DHIS2.

#### Search Results

##### Spanish query results
1. **Plan Nacional de Prevención y Control de la Hipertensión Arterial (Argentina.gob.ar)** — https://www.argentina.gob.ar/sites/default/files/bancos/2023-10/plan-nacional-prevencion-control-de-la-hipertension-arterial.pdf
   National plan for hypertension prevention and control.

2. **Resolución 1156/2014 — Programa Nacional de Diabetes (Min. Salud)** — https://e-legis-ar.msal.gov.ar/htdocs/legisalud/migration/pdf/23260.pdf
   Establishes National Diabetes Prevention and Control Program.

3. **Herramientas para la consulta — MSAL ENT** — http://www.msal.gob.ar/ent/index.php/informacion-equipos-de-salud/herramientas-para-la-consulta [BROKEN: unreachable]
   Monitoring tools for clinical teams managing diabetes and CV risk.

4. **Nación incorpora el cáncer y la enfermedad renal crónica al SNVS** — https://www.argentina.gob.ar/noticias/nacion-incorpora-el-cancer-y-la-enfermedad-renal-cronica-al-sistema-nacional-de-vigilancia
   Expansion of National Health Surveillance System (SNVS 2.0).

5. **Salud organizó encuentro intersectorial para abordaje integral de HTA, diabetes y ERC** — https://www.argentina.gob.ar/noticias/salud-organizo-un-encuentro-intersectorial-para-fortalecer-el-abordaje-integral-de-la
   Intersectoral approach to HTA, diabetes, and CKD.

6. **Epidemiología de la diabetes en Argentina (Avances en Diabetología)** — https://www.elsevier.es/es-revista-avances-diabetologia-326-articulo-epidemiologia-diabetes-argentina-S1134323010620066
   Epidemiology review.
##### English query results
7. **Implementing the HEARTS Initiative in Mendoza, Argentina: A multi-level, staged approach (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC12693736/
   Stages of HEARTS rollout 2022-2024, 287 primary care centers mapped.

8. **Implementation of the HEARTS Initiative in Argentina: initial results (PAHO J)** — https://journal.paho.org/en/articles/implementation-hearts-initiative-argentina-initial-results
   National HEARTS implementation results.

9. **Diabetes disease burden profile 2023: Argentina (PAHO)** — https://www.paho.org/en/documents/diabetes-disease-burden-profile-2023-argentina
    PAHO country diabetes burden profile.


### Bangladesh — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Bangladesh is the world's largest DHIS2 deployment, with ~98% reporting from over 13,000 community clinics and all government facilities under DGHS. The country runs an extensive network of NCD Corners at Upazila Health Complexes providing protocol-based hypertension and diabetes care, with the Simple app supporting longitudinal tracking. The Multisectoral NCD Action Plan 2018-2025 anchors surveillance. DHIS2 captures NCD Corner outputs (e.g., ~6,200 hypertension and ~1,400 diabetes patients reported via NCD corners by Oct 2022), though registry duplication remains a challenge.

DHIS2 USE: CONFIRMED
DHIS2 is the national HMIS; NCD Corner data on hypertension and diabetes flows into DHIS2 reporting alongside the Simple app for primary care.

#### Search Results

##### Bangla / English combined query results
1. **DHIS — Interface for collection of nation-wide health data (DGHS)** — https://old.dghs.gov.bd/index.php/en/e-health/our-ehealth-eservices/84-english-root/ehealth-eservice/94-dhis-interface-for-collection-of-nation-wide-health-data [BROKEN: 404]
   Official DGHS page describing DHIS2 as national data system.

2. **Perceptions and experiences with DHIS2 to collect and utilize health data in Bangladesh (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7249629/
   Qualitative study of DHIS2 use in Bangladesh.

3. **How Bangladesh Successfully Deployed DHIS2 Nationwide (ICTworks)** — https://www.ictworks.org/bangladesh-dhis2-information-system/
   Documents 98% reporting and 13,000+ community clinic coverage.

4. **Using DHIS2 Software to Collect Health Data in Bangladesh (MEASURE Evaluation)** — https://www.measureevaluation.org/resources/publications/wp-19-226/at_download/document
   Detailed working paper on DHIS2 use.

5. **NCD corners in public sector health facilities in Bangladesh: qualitative study (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC6797278/
   Assessment of NCD Corners and registry challenges.

6. **Integrating diabetes and hypertension case management within primary health care: feasibility study (BMC HSR)** — https://bmchealthservres.biomedcentral.com/articles/10.1186/s12913-018-3601-0
   PHC integration feasibility.

7. **Implementation status of NCD control program at PHC level in Bangladesh (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9461504/
   Qualitative findings on NCD program implementation.

8. **Prevalence, trends and associated factors of hypertension and diabetes in Bangladesh: BHDS 2011 & 2017-18 (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9064112/
   National prevalence trends.

9. **How prepared are urban primary care facilities to manage hypertension and T2DM in Dhaka (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC12918133/
   Readiness study of Dhaka government dispensaries and NGO clinics.

10. **Addressing Gaps in HTA/Diabetes Care Continuum in Rural Bangladesh via Digital Technology (JMIR Res Protocols)** — https://www.researchprotocols.org/2026/1/e71696
    Digital decentralized primary care trial 2026.


### Benin — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Benin has used DHIS2 as the basis of its SNIGS (Système National d'Information et de Gestion Sanitaires) since at least 2017, generalized across all levels of the health pyramid. Hypertension prevalence is ~26% and diabetes ~12% (18-69 age group). However, SNIGS only captures hypertension among the 15 NCD risk factors; no national systematic NCD screening or surveillance structure currently exists.

DHIS2 USE: CONFIRMED
DHIS2 underpins SNIGS, and hypertension data is reported through it, but a comprehensive cardiometabolic module/registry is not in place.

#### Search Results

##### French query results
1. **Politique et Plan Stratégique Intégré de Lutte Contre les MNT 2014-2018 (ICCP)** — https://www.iccp-portal.org/sites/default/files/plans/Benin%20Plan_strategique_integre_lutte_contre_maladies_non_transmissibles_2014-2018.pdf
   National NCD strategic plan documenting surveillance gaps.

2. **Plan National de Développement Sanitaire 2018-2022 Benin (PRB)** — https://www.prb.org/wp-content/uploads/2020/06/Benin-Plan-National-de-D%C3%A9veloppement-Sanitaire-2018-2022.pdf [BROKEN: 404]
   Health development plan including HMIS.

3. **Politique MNT (ICCP Benin)** — https://www.iccp-portal.org/sites/default/files/plans/politique_MNT.pdf
   National NCD policy document.

4. **Comorbidité HTA et Diabète dans le Département du Littoral, Bénin 2023 (Eur Sci J)** — https://eujournal.org/index.php/esj/article/view/18130
   Epidemiology of HTA-diabetes comorbidity in Littoral.
##### English query results
5. **Modernizing the health map in Benin: impact of georegistry (IASO)** — https://www.openiaso.com/modernizing-health-map-impact-of-georegistry/
   Health facility georegistry feeding DHIS2/SNIGS.

6. **Benin Plan Stratégique Decentralisation Santé (S3 doc)** — https://s3-eu-west-1.amazonaws.com/front-office-resources/production/uploads/publication/attachment/46e5f76a-5f8f-4780-9d32-d51df5c239c2/02be6fdb-3d72-4984-adba-f696163051fd.pdf
    Decentralization plan including SNIGS.

7. **Annuaire des Statistiques Sanitaires 2018 (Ministère de la Santé)** — https://files.aho.afro.who.int/afahobckpcontainer/production/files/Annuaire_2018_MS.pdf
   Benin MoH annual health statistics yearbook hosted by WHO AFRO.


### Bhutan — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Bhutan faces a major NCD epidemic (53% of deaths). NHS 2023 found 62% of adults with high BP and 59.4% with high blood glucose unaware of their condition. The MoH established a National Diabetes Control Programme in 1996, piloted WHO PEN in Paro and Bumthang from 2009, and conducted a nationwide NCD screening in Nov-Dec 2024. The Bhutan Health Management & Information System (BHMIS) is coordinated by the HMIS Unit in the MoH and uses DHIS2 as the platform for entering aggregate morbidity and mortality data from hospitals and Basic Health Units — the routine channel through which NCD cases including diabetes and hypertension flow. The WHO/MoH country narrative on NCD services explicitly cites the need to improve the integration of NCD indicators into HMIS, confirming HMIS (DHIS2) is the intended routine reporting platform for these conditions.

DHIS2 USE: CONFIRMED
DHIS2 is the underlying platform of the national BHMIS, managed by the HMIS Unit in the MoH, and is used for aggregate morbidity reporting (including NCDs) from hospitals and BHUs. WHO country reporting on Bhutan's NCD response identifies the HMIS (BHMIS/DHIS2) as the system into which NCD indicators are being further integrated.

#### Search Results

##### English query results
1. **Noncommunicable diseases risk factors in Bhutan: secondary analysis of STEPS 2014 (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/34555064/
   National STEPS 2014 secondary analysis.

2. **Is diabetes and hypertension screening worthwhile in resource-limited settings? PEN pilot in Bhutan (Health Policy & Planning)** — https://academic.oup.com/heapol/article/30/8/1032/554686
   Economic evaluation of WHO PEN pilot in Paro/Bumthang.

3. **Nationwide Non-Communicable Disease Screening (MoH Bhutan, 2024)** — https://moh.gov.bt/information-for-bhutanese-general-public-nationwide-non-communicable-disease-screening/
   National screening campaign 14 Nov - 7 Dec 2024.

4. **NCD Screening Guide 1st Edition 2024 (Dept of Public Health, MoH Bhutan)** — https://moh.gov.bt/wp-content/uploads/2025/01/A4_NCD_Screening_Guide-2.pdf
   National NCD screening guide.

5. **A first country-wide review of Diabetes Mellitus care in Bhutan (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC4573946/
   Nationwide review of DM care across 20 district hospitals.

6. **Diabetes and Hypertension in Urban Bhutanese Men and Women (PMC)** — https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3760321/
   Urban epidemiology study.

7. **National eHealth Strategy and Action Plan, Ministry of Health, Kingdom of Bhutan (WHO Country Planning)** — https://extranet.who.int/countryplanningcycles/sites/default/files/public_file_rep/BTN_Bhutan_National_eHealth-Strategy_2018.Pdf
   Official MoH strategy describing the Bhutan HMIS architecture: BHMIS is coordinated by the HMIS Unit in the MoH and uses DHIS2 for entry of aggregate data including routine morbidity and mortality from hospitals and Basic Health Units. Facilities without connectivity send paper forms to the District Health Office for DHIS2 entry. This establishes DHIS2 as the national routine reporting platform into which NCD aggregate data (incl. hypertension and diabetes case counts) flows.

8. **Assessment of Health Information Systems – ADB Project 51141-002 (Bhutan)** — https://www.adb.org/sites/default/files/linked-documents/51141-002-sd-05.pdf
   Independent ADB assessment confirming DHIS2 is the aggregate data platform for Bhutan's HMIS, covering morbidity by health centre and used for multiple programs/datasets.

9. **People-centered model supported by WHO's PEN Package improves access to non-communicable disease healthcare in Bhutan (WHO 2023 country story)** — https://www.who.int/about/accountability/results/who-results-report-2020-mtr/country-story/2023/people-centered-model-supported-by-who-s-pen-package-improves-access-to-non-communicable-disease-healthcare-in-bhutan
   WHO country narrative on Bhutan's national WHO PEN rollout, explicitly flagging the need to further integrate NCD indicators into the HMIS — i.e. the DHIS2-backed BHMIS — as the routine reporting platform for hypertension and diabetes.


### Botswana — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Botswana uses DHIS2 for aggregate routine HMIS reporting alongside OpenMRS for clinic-level EMR. CVD causes ~18% and diabetes ~6% of mortality; hypertension drives 8.9% of outpatient morbidity. STEPS 2014 provides the most recent national NCD risk-factor data. The InterCARE cluster RCT (2024) is testing integrated hypertension/CVD care within HIV services at 14 sites — leveraging the existing digital infrastructure.

DHIS2 USE: LIKELY
DHIS2 is confirmed as the national aggregate HMIS; NCD-specific cardiometabolic indicators in DHIS2 are likely but not directly cited in surfaced literature.

#### Search Results

##### English query results
1. **Botswana's Healthcare System: From HIV Response to Health (Kapsule Tech)** — https://kapsuletech.com/blog/botswana-healthcare-system/
   Describes DHIS2 for aggregate HMIS and OpenMRS for EMR in Botswana.

2. **Prevalence of and factors associated with hypertension, diabetes, stroke and heart attack multimorbidity in Botswana: STEPS 2014 (PLOS ONE)** — https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0265722
   National STEPS-based multimorbidity analysis.

3. **Botswana STEPS Survey 2014 final report (WHO)** — https://cdn.who.int/media/docs/default-source/ncds/ncd-surveillance/data-reporting/botswana/steps/steps-botswana-2014-report-final.pdf
   Official 2014 STEPS NCD report.
##### Setswana query results
4. **Prevalence of HTA, diabetes multimorbidity Botswana STEPS 2014 (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8947240/
   PMC version of multimorbidity study.


### Brazil — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Brazil runs sophisticated NCD surveillance through its SUS, including the dedicated HiperDia registry (SIS-HIPERDIA) for hypertension and diabetes patient registration and monitoring, VIGITEL telephone surveillance, SIM mortality registry, and ELSA cohort. HiperDia generates information for medication dispensing and patient follow-up, with 68.7% of variables showing good-to-excellent data quality in evaluations. Brazil does not use DHIS2 — it operates its own national DATASUS infrastructure.

DHIS2 USE: NONE
Brazil uses DATASUS-based systems (HiperDia, VIGITEL, SIM) for cardiometabolic surveillance, not DHIS2.

#### Search Results

##### Portuguese query results
1. **SIS-HIPERDIA (Datasus/Tabnet)** — http://tabnet.datasus.gov.br/cgi/hiperdia/cnv/hddescr.htm
   Official HiperDia descriptive page for registration/monitoring of HTA and DM patients in SUS.

2. **Hipertensão e Diabetes (HIPERDIA) — DATASUS** — https://datasus.saude.gov.br/acesso-a-informacao/hipertensao-e-diabetes-hiperdia/
   DATASUS HiperDia page.

3. **Hiperdia — SIAB Datasus** — http://siab.datasus.gov.br/DATASUS/index.php?area=060304
   Integration of HiperDia with primary care information system.

4. **HIPERDIA: programa para melhoria do controle dos hipertensos e diabéticos (UNASUS)** — https://ares.unasus.gov.br/acervo/html/ARES/14803/1/Artigo_Aldenora_ARES.pdf
   Article on HiperDia program for HTA/DM control.

5. **Linhas de Cuidados: Hipertensão Arterial e Diabetes (BVS Min. Saúde)** — https://bvsms.saude.gov.br/bvs/publicacoes/linhas_cuidado_hipertensao_diabetes.pdf
   National care pathways for HTA and DM.

6. **Vigitel — Ministério da Saúde** — https://www.gov.br/saude/pt-br/composicao/svsa/inqueritos-de-saude/vigitel
   Telephone-based surveillance of NCD risk factors.

7. **DCNT no contexto do SUS Brasileiro (BVS)** — https://bvsms.saude.gov.br/bvs/publicacoes/DCNT.pdf
   NCDs in the context of SUS.

8. **Completitude dos dados HiperDia em estado do Nordeste (SciELO)** — https://www.scielo.br/j/csc/a/m4XBSkzC9TQGqwpVtCnYCyK/
   Data completeness study; 68.7% variables good-to-excellent.
##### English query results
9. **Epidemiology, management, complications and costs of type 2 diabetes in Brazil (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC4220809/
   Comprehensive T2DM literature review citing HiperDia.

10. **DM2 and DM2+HTA in Brazil: epidemiologic profile from HIPERDIA (Value in Health)** — https://www.valueinhealthjournal.com/article/S1098-3015(11)00663-2/fulltext
    Profile of registered HiperDia population.


### Burkina Faso — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Burkina Faso operates ENDOS-BF (Entrepôt National des Données de la Santé du Burkina Faso), a DHIS2-based national HMIS in production since 2013, covering all health pyramid levels and extended to One Health surveillance in 2019. The first national STEPS survey was conducted in 2013 covering diabetes and hypertension risk factors. ENDOS-BF includes Tracker modules; cardiometabolic-specific tracker usage is plausible but not explicitly documented in surfaced sources.

DHIS2 USE: MODERATE
DHIS2/ENDOS-BF is the confirmed national HMIS with Tracker capability; cardiometabolic indicators almost certainly flow through it, though direct attribution in literature is limited.

#### Search Results

##### French query results
1. **Burkina Faso HMIS — ENDOS (DHIS2 + Tracker) (Digital Investment Exchange)** — https://exchange.dial.global/projects/burkina-faso-burkina-faso-hmis--endos-dhis2--tracker
   Documents ENDOS as DHIS2+Tracker-based national HMIS.

2. **Entrepôt de données sanitaires du Burkina Faso — Endos-BF (Sur.ly)** — http://sur.ly/o/burkina.dhis2.org/AA000014
   ENDOS-BF DHIS2 instance.

3. **BURKINA FASO ENQUETE STEPS 2013 Note synthèse (WHO)** — https://cdn.who.int/media/docs/default-source/ncds/ncd-surveillance/data-reporting/burkina-faso/steps/burkinafaso-2013-steps-factsheet.pdf
   Summary of 2013 national STEPS NCD risk-factor survey.

4. **Evaluation PRISM du SIS de routine au Burkina Faso 2018 (MEASURE Evaluation)** — https://www.measureevaluation.org/resources/publications/gr-19-101-fr.html
   PRISM-based evaluation of routine HMIS performance.

5. **GroupeDeGénie — DHIS 2 implementation services (Burkina Faso)** — https://www.groupedegenie.com/index.php/component/k2/itemlist/category/136-dhis-2-district-health-information-software [BROKEN: 404]
   Local DHIS2 implementation partner.

6. **Programme BURKINA FASO — Santé Diabète** — https://santediabete.org/en_en/sante-diabete-en-action/programme-humanitaire-burkina-faso/
   NGO diabetes program in Burkina.
##### English query results
7. **How countries use digital global goods in emergencies — DHIS2 COVID-19 in Burkina Faso, Mali, Suriname (Oxford Open Digital Health)** — https://academic.oup.com/oodh/article/2/Supplement_1/i64/7560469
   DHIS2 deployment lessons including Burkina Faso.

8. **burkina.dhis2.org Entrepôt de données sanitaires (Website Informer)** — https://website.informer.com/burkina.dhis2.org
   Metadata page about ENDOS-BF DHIS2 site.


### Burundi — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Burundi uses DHIS2 as its national health information system (DSNIS), with cardiometabolic data captured: WHO Africa reports ~38,000 people with diabetes and ~98,000 with hypertension recorded in Burundi's DHIS2. The country is preparing an interoperable health information system under PNDIS II (2026-2028), supported by Belgian Development Cooperation, to integrate DHIS2 with SIDAInfo and OpenClinic. The MoH has also launched mobile NCD clinics serving IDP camps and rural communities.

DHIS2 USE: CONFIRMED
DHIS2 is explicitly cited as the source for Burundi's national counts of diabetes and hypertension patients.

#### Search Results

##### French query results
1. **DSNIS — Direction du Système National d'Information Sanitaire (Min. Santé Burundi)** — https://minisante.gov.bi/?page_id=188
   Official MoH page on the national health information system.

2. **Santé : le Burundi veut mettre en place un système d'information interopérable (OSIRIS)** — https://www.osiris.sn/sante-le-burundi-veut-mettre-en-place-un-systeme-d-information-interoperable.html
   Plans to integrate DHIS2, SIDAInfo, OpenClinic under PNDIS II 2026-2028.

3. **Rapport d'analyse des indicateurs SRMNIA-N de routine — Données DHIS2 (Countdown 2030)** — https://www.countdown2030.org/wp-content/uploads/2023/10/Version-finale_Rapport_Groupe2_DONNEES-DHIS2_VF_compressed-1.pdf
   Analysis of Burundi DHIS2 routine indicators.
##### English query results
4. **Care for diabetics living in displaced persons camps in Burundi (WHO Afro)** — https://www.afro.who.int/countries/burundi/news/care-diabetics-living-displaced-persons-camps-burundi
   Cites DHIS2: ~38,000 diabetes and ~98,000 hypertension patients in Burundi; mobile NCD clinic initiative.


### Cambodia — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Cambodia has scaled up integrated type-2 diabetes and hypertension care via WHO PEN at health centres and NCD clinics at referral hospitals, with peer-educator (MoPoTsyo) community models extending coverage. The national HMIS captures aggregate diabetes/hypertension case counts but cannot differentiate type-1/type-2 or unique patients, and a separate (paper-based) NCD database exists at clinic level. Cambodia is transitioning its national HMIS to DHIS2 (announced 2024-2025) with support from UNDP, WHO, UNICEF, CHAI, and AeHIN, but the reviewed literature does not yet describe DHIS2 modules dedicated to cardiometabolic registries.

DHIS2 USE: MODERATE
DHIS2 is being adopted as the national HMIS and the HMIS already carries aggregate diabetes/hypertension indicators, but no source explicitly documents DHIS2 as the cardiometabolic data platform yet.

#### Search Results

##### English query results
1. **Scaling-up integrated type-2 diabetes and hypertension care in Cambodia: barriers to health system performance (Frontiers, 2023)** — https://www.frontiersin.org/journals/public-health/articles/10.3389/fpubh.2023.1136520/full
   Documents PEN/NCD-clinic scale-up barriers including weak HMIS indicators for diabetes/hypertension.

2. **Evaluation of Diabetes Care Performance in Cambodia Through the Cascade-of-Care Framework (JMIR Public Health Surveillance, 2023)** — https://publichealth.jmir.org/2023/1/e41902
   Cross-sectional cascade analysis of diabetes care in Cambodian primary care.

3. **Developing a toolkit for implementing evidence-based guidelines to manage hypertension and diabetes in Cambodia (PMC, 2022)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9706829/
   Descriptive case study on HTN/DM clinical guideline implementation.

4. **Access to Treatment for Diabetes and Hypertension in Rural Cambodia (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC4729435/
   Assesses social health protection scheme performance for DM/HTN.

5. **Cost of "Ideal Minimum Integrated Care" for Type 2 Diabetes and Hypertension Patients in Cambodia (PMC, 2024)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11546216/
   Provider-perspective costing of integrated cardiometabolic care.

6. **The prevalence and management of hypertension and type 2 diabetes mellitus as NCDs in Cambodia: a critical review (ResearchGate, 2024)** — https://www.researchgate.net/publication/382113849
   Critical review noting fragmented HMIS data for NCDs.

7. **District Health Information System (DHIS2) — UNDP Digital Health for Development case** — https://digitalhealthfordevelopment.undp.org/view/4c745e-101/
   UNDP support to Cambodia MoH/MoLVT for DHIS2 implementation.

8. **Digital Health Transformation Workshop in Cambodia — AeHIN (2025)** — https://www.asiaehealthinformationnetwork.org/2025/08/15/digital-health-transformation-workshop-in-cambodia/
   Partners (UNICEF, WHO, CHAI, World Bank, AeHIN, Vital Strategies) outline scaling DHIS2 in Cambodia.

9. **Expanding Cambodia's Malaria Information System (PMI Evolve)** — https://pmievolve.org/expanding-cambodias-malaria-information-system-advance-elimination-goals
   Context on Cambodia DHIS2-based disease information systems.

10. **DHIS2 News: Cambodia Announces Transition of National HMIS to DHIS2** — https://www.facebook.com/dhis2/posts/news-cambodia-announces-transition-of-national-health-management-information-sys/1107354014722590/
    Public announcement of national HMIS transition to DHIS2.


### Cameroon — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Cameroon's Ministry of Public Health (MINSANTE) operates a national DHIS2 instance (dhis-minsante-cm.org) and has run multiple DHIS2 dashboard training workshops with WHO support. In December 2025 Cameroon officially launched the WHO PEN-Plus strategy to decentralise severe NCD (diabetes, hypertension, stroke, asthma) care to rural areas, and CVD risk-factor surveillance using the WHO STEPS approach is documented at facility level. No reviewed source explicitly names DHIS2 as the cardiometabolic registry platform, but the national HMIS that captures NCD service indicators runs on DHIS2.

DHIS2 USE: CONFIRMED
National HMIS confirmed on DHIS2 and NCD/PEN-Plus programmes active, but no explicit publication linking DHIS2 to diabetes/hypertension registries.

#### Search Results

##### French query results
1. **MINSANTE — DHIS2 login portal (Ministère de la Santé Publique, Cameroun)** — https://dhis-minsante-cm.org/dhis-web-commons/security/login.action
   National DHIS2 production instance of the Cameroon MoH.

2. **Le Cameroun déploie la stratégie PEN-Plus pour contrer les maladies chroniques — MINSANTE** — https://www.minsante.cm/site/?q=en/node/5326
   Official launch (Dec 2025) of PEN-Plus for decentralised severe NCD care.

3. **DHIS2 au Cameroun : Système de Santé Intégré (MINSANTE/CIS overview)** — https://www.scribd.com/presentation/757832072/2-MINSANTE-CIS-Apercu-General-Et-Fonctionnalites-Du-Logiciel-DHIS2-Copie-Copie
   Cellule des Informations Sanitaires presentation on DHIS2 functionality in Cameroon.

4. **MINSANTE ouverture de l'atelier de formation DHIS2 v2.24 (APO)** — https://appablog.wordpress.com/2020/08/15/ministere-de-la-sante-publique-du-cameroon-minsante-ouverture-de-latelier-de-formation-a-lutilisation-des-tableaux-de-bord-de-la-plate-forme-district-health-information-software-dhis2-versio/
   WHO-supported dashboard training for DHIS2 in Cameroon.

5. **Entrepôt National des Données sur le Paludisme (MINSANTE)** — https://nmdr.minsante.cm/
   National MoH data warehouse demonstrating DHIS2-era national data infrastructure.

6. **Obésité, hypertension artérielle et diabète dans une population de femmes rurales de l'ouest du Cameroun (ResearchGate)** — https://www.researchgate.net/publication/242538032
   Community-level cardiometabolic prevalence study in rural Cameroon.
##### English query results
7. **Surveillance of Cardiovascular Risk Factors in the Fifth Military Sector Health Center, Ngaoundéré, Cameroon (PMC, 2020)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7728542/
   Observational CVD risk-factor surveillance study using the WHO STEPS manual.

8. **Evaluation du système de surveillance de l'hypertension artérielle et du diabète (WHO PEN), Togo 2023 — JIEPH** — https://afenet-journal.org/evaluation-du-systeme-de-surveillance-de-lhypertension-arterielle-et-du-diabete-dans-le-cadre-de-lapproche-whopen-dans-le-district-sanitaire-de-golfe-togo-2023/
   Regional francophone PEN HTN/DM surveillance study using DHIS2, referencing similar work in Cameroon district.


### Cape Verde — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Cape Verde has a high and rising NCD burden: hypertension prevalence reached ~34% (2019) and roughly 70% of all deaths are attributed to NCDs, with NCD risk factors monitored through the 2020 WHO STEPS survey. DHIS2 has been deployed nationally for COVID-19 vaccine delivery and surveillance (alongside other Lusophone African countries), but the search results show no evidence that DHIS2 currently serves as the routine platform for diabetes/hypertension/CVD program data. National diabetes management uses a paper-based manual produced by the Ministry of Health.

DHIS2 USE: LIKELY
DHIS2 is established in Cape Verde for COVID-19/immunization. NCD policy exists (manual de controlo e seguimento da Diabetes Mellitus, STEPS 2020), but no direct documentation links cardiometabolic indicators to DHIS2 reporting in this evidence set.

#### Search Results

##### Portuguese query results
1. **30% da população Cabo-verdiana é hipertensa e 3,7% tem diabetes — Ministério da Saúde** — https://minsaude.gov.cv/noticias/30-da-populacao-cabo-verdiana-e-hipertensa-e-37-tem-diabetes/
   MoH summary of national hypertension and diabetes prevalence drawn from IDNT surveys.

2. **Dia mundial da diabetes — INSP Cabo Verde** — https://insp.gov.cv/index.php/component/content/article/105-slideshow/338-dia-mundial-da-diabetes [BROKEN: unreachable]
   National Institute of Public Health diabetes awareness materials.
##### English query results
3. **Deploying DHIS2 for COVID-19 Vaccine Delivery in Lusophone Africa (incl. Cape Verde)** — https://dhis2.org/lusophone-africa-covid-vaccine/
   Documents DHIS2 deployment in Cape Verde for COVID-19 vaccination/surveillance.

4. **Hypertension, diabetes, and cardiovascular disease nexus: urbanization and lifestyle in Cabo Verde** — https://www.tandfonline.com/doi/full/10.1080/16549716.2024.2414524
   2024 study on cardiometabolic burden and urbanization in Cabo Verde.

5. **State of multi-morbidity among adults in Cape Verde: 2020 WHO STEPS NCD survey** — https://doi.org/10.1093/pubmed/fdaf031
   17.9% multi-morbidity; hypertension 37.2% in men; diabetes 5% in women.

6. **Alcohol consumption among persons living with hypertension: Cape Verde** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11884126/
   Population-based study leveraging STEPS data.

7. **HISP Centre and World Diabetes Foundation Partner to Strengthen Digital Tools for NCDs** — https://dhis2.org/hisp-centre-world-diabetes-foundation-partnership/
   Regional initiative to expand DHIS2 NCD toolkit (Cape Verde not explicitly named).


### Central African Republic — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
CAR has high cardiometabolic burden: 2017 STEPS reported 34.5% adult hypertension and 9.5% diabetes, with poor diagnosis and treatment coverage. The WHO 2024 annual report for CAR documents explicit DHIS2 registration of hypertension and diabetes cases: between 2021 and May 2024, 80,407 hypertension cases and 19,932 diabetes cases were captured in DHIS2 (though disaggregation is incomplete).

DHIS2 USE: CONFIRMED
WHO Country Office report explicitly cites DHIS2 as the source of routine hypertension and diabetes case data in CAR.

#### Search Results

##### French query results
1. **Rapport STEPS RCA 2017** — https://cdn.who.int/media/docs/default-source/ncds/ncd-surveillance/data-reporting/central-african-republic/rapport_steps_rca_2017_final.pdf
   Données nationales sur l'hypertension (34,5%) et le diabète (9,5%).

2. **République centrafricaine — Atlas du diabète FID** — https://diabetesatlas.org/data-by-location/country/central-african-republic/
   Profil pays diabète.

3. **CENTRAFRIQUE-SANTÉ: Diabète et hypertension, menace silencieuse — LANOCA (2025)** — https://lanoca.over-blog.com/2025/10/centrafrique-sante-diabete-et-hypertension-une-menace-silencieuse-en-pleine-expansion.html
   Coverage of growing NCD burden.

4. **Vivre avec le diabète à Carnot — Ndjoni Sango (2025)** — https://ndjonisango.com/2025/11/17/republique-centrafricaine-vivre-avec-le-diabete-a-carnot/
   Reportage on patient experience.

5. **Lutter contre le diabète en République centrafricaine — MSF** — https://www.msf.fr/actualites/lutter-contre-le-diabete-en-republique-centrafricaine
   MSF activities supporting diabetes care.
##### English query results
6. **Prevalence and associated factors of undiagnosed hypertension among adults in CAR** — https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9643345/
   7 in 10 CAR adults with hypertension are undiagnosed (2017 STEPS analysis).

7. **NCDs and DHIS2: Learning from Country Experiences** — https://community.dhis2.org/t/ncds-and-dhis2-learning-from-country-experiences/48128
   Community thread on DHIS2 NCD implementations.

8. **HISP Centre and World Diabetes Foundation Partnership** — https://dhis2.org/hisp-centre-world-diabetes-foundation-partnership/
    Initiative to strengthen DHIS2 NCD digital tools.


### Chad — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Chad transitioned its national HMIS to DHIS2 in 2022 and became the 69th country to deploy DHIS2 nationally, with >96% of districts reporting through it by end-2022. Diabetes prevalence in urban adults ≥55 reaches ~12.9%, and hypertension is a major comorbidity, but routine surveillance for NCDs is weak and no published evidence yet links Chad's DHIS2 to a dedicated cardiometabolic registry.

DHIS2 USE: LIKELY
DHIS2 is the national HMIS in Chad, so aggregate NCD indicators (diabetes/hypertension consultations) likely flow through it as primary care reporting matures, but no direct documentation of NCD modules was found.

#### Search Results

##### French query results
1. **DHIS2 en tant que système de gestion de l'information sanitaire au Tchad — DHIS2 Community** — https://community.dhis2.org/t/dhis2-en-tant-que-systeme-de-gestion-de-l-information-sanitaire-au-tchad/50835
   Discussion technique sur le déploiement national.

2. **Lancement officiel de DHIS2 au Tchad — DHIS2 Community** — https://community.dhis2.org/t/lancement-officiel-de-dhis2-au-tchad/50605
   Annonce officielle du lancement (PADS / Ministère de la Santé Publique).

3. **Profil de la néphropathie diabétique à l'Hôpital Général de N'Djamena — Pan African Medical Journal** — https://panafrican-med-journal.com/content/article/24/193/full/
   Étude hospitalière sur complications du diabète.

4. **Prévalence des complications médicales chez les diabétiques hospitalisés à N'Djamena — HSD** — https://www.hsd-fmsb.org/index.php/hsd/article/view/553
   Comorbidités diabète–HTA chez patients tchadiens.

5. **Plan National de Développement Sanitaire 2018–2021 — Tchad** — https://www.prb.org/wp-content/uploads/2020/06/Tchad-Plan-National-de-Developpement-Sanitaire-2018-2021.pdf
   Cadre national, intégration MNT.

6. **L'hypertension artérielle masquée chez les diabétiques de type 2 (région) — ScienceDirect** — https://www.sciencedirect.com/science/article/abs/pii/S0003392821001657
   Étude clinique sur HTA masquée chez diabétiques.

7. **PADS — Programme d'Appui aux Districts Sanitaires au Tchad** — https://www.pads-tchad.org/
   Partenaire de mise en œuvre DHIS2.
##### English query results
8. **Chad transitions to DHIS2-based health information management — DHIS2** — https://dhis2.org/chad-hmis-transition/
   Chad becomes 69th country with national DHIS2; >96% district reporting end-2022.

9. **NCDs and DHIS2: Learning from Country Experiences** — https://community.dhis2.org/t/ncds-and-dhis2-learning-from-country-experiences/48128
    Cross-country experiences (Chad not explicitly named).


### Chile — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Chile's Ministry of Health (MINSAL) began implementing DHIS2 in 2022 for selected programs (RENCI childhood cancer registry, Paxlovid COVID-19 medication tracking) and has since expanded DHIS2 to support cardiovascular health surveillance under the PAHO HEARTS in the Americas initiative. Roll-out includes a semi-annual structure module across 29 health services and training of cardiovascular care providers to capture HEARTS indicators (hypertension and diabetes) at primary care level. Chile was also among the first HEARTS implementer countries (2015–2017 cohort).

DHIS2 USE: CONFIRMED
PAHO/DHIS2 documentation explicitly describes DHIS2 use in Chile for cardiovascular and HEARTS-aligned data, with MINSAL adoption since 2022 and expansion to epidemiological surveillance.

#### Search Results

##### Spanish query results
1. **Uso de DHIS2 para apoyar la salud cardiovascular en las Américas — DHIS2** — https://dhis2.org/es/hearts-support-cardiovascular-health-americas/
   Describes DHIS2 implementation in Chile and other Americas countries for HEARTS/CVD data.

2. **Construyendo una plataforma epidemiológica en Chile con expertos locales — DHIS2** — https://dhis2.org/es/chile-national-health-platform/
   MINSAL DHIS2 platform: RENCI cancer registry, Paxlovid, expansion to NCD/CVD surveillance.

3. **Reorientación de los Programas de Hipertensión y Diabetes — MINSAL** — https://www.minsal.cl/portal/url/item/75fcbd5dc347e5efe04001011f012019.pdf
   National policy document on HTN/DM program redesign.

4. **Diabetes Clinical Pathway — Chile (PAHO)** — https://www.paho.org/en/documents/diabetes-clinical-pathway-chile
   National clinical pathway.

5. **Epidemiología de la Diabetes Mellitus en Chile — Revista Médica Clínica Las Condes** — https://www.elsevier.es/es-revista-revista-medica-clinica-las-condes-202-articulo-epidemiologia-de-la-diabetes-mellitus-S0716864016300037
   Review on Chilean DM epidemiology.

6. **Diabetes mellitus tipo 2 — Superintendencia de Salud** — https://www.superdesalud.gob.cl/orientacion-en-salud/diabetes-mellitus-tipo-2/
   Official patient/provider information.

7. **Perfil de usuarios con diabetes e hipertensión y resultados clínicos** — https://www.scielo.cl/scielo.php?script=sci_arttext&pid=S0718-85602018000300161
   Study on patient profile and outcomes.
##### English query results
8. **Integrating hypertension and diabetes management using HEARTS — PMC** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9440730/
   HEARTS implementation framework relevant to Chile.

9. **Hypertension Registry System Design — DHIS2 Docs** — https://docs.dhis2.org/en/topics/metadata/non-communicable-diseases/hypertension-control/design.html
   DHIS2 reference design used in HEARTS programs.

10. **Prescription drug coverage of hypertension, diabetes, dyslipidemia in Chile — PLOS One** — https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0297807
    National study on chronic disease coverage.


### Colombia — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Colombia was among the first cohort of HEARTS in the Americas implementers (2015–2017) and is part of the PAHO regional initiative that uses DHIS2 to drive data use for cardiovascular disease prevention and treatment. CVD accounts for ~28.7% of Colombia's deaths and hypertension affects ~22.8% of the population. Colombia's primary national health information systems (SISPRO, RIPS, Cuenta de Alto Costo) are the formal cardiometabolic registries; DHIS2 use in Colombia appears to be through the regional HEARTS DHIS2 dashboard rather than a national HMIS.

DHIS2 USE: MODERATE
Colombia participates in HEARTS in the Americas with DHIS2-based dashboards; cardiometabolic indicators are national priorities, though Colombia's national HMIS is not DHIS2 (it uses SISPRO/CAC).

#### Search Results

##### Spanish query results
1. **Colombia avanza en prevención y control de Diabetes e Hipertensión — Minsalud** — https://www.minsalud.gov.co/Paginas/Colombia-avanza-en-prevenci%C3%B3n-y-control-de-Diabetes-e-Hipertensi%C3%B3n-arterial,-condiciones-precursoras-de-la-Enfermedad-Renal.aspx [BROKEN: unreachable]
   National progress in HTN/DM prevention and control.

2. **Prevalencias de diabetes e hipertensión en Colombia: revisión sistemática** — http://www.scielo.org.co/scielo.php?script=sci_arttext&pid=S0120-386X2019000100087 [BROKEN: unreachable]
   National prevalence systematic review.

3. **Diabetes e Hipertensión en Colombia — ESE Hospital José Rufino Vivas (2025)** — https://www.hospitaldagua.gov.co/2025/10/18/diabetes-e-hipertension-en-colombia-5-habitos-que-pueden-salvar-tu-vida/
   Government hospital outreach.

4. **Atención de la Diabetes tipo 2 — MinSalud** — https://www.minsalud.gov.co/sites/rid/Lists/BibliotecaDigital/RIDE/VS/PP/32Atencion%20de%20la%20Diabetes%20tipo%202.PDF [BROKEN: unreachable]
   Official DM2 clinical pathway.

5. **Guía Clínica DM2 — Asociación Colombiana de Endocrinología 2025** — https://www.revistaendocrino.org/index.php/rcedm/article/view/907
   National DM2 clinical guideline.

6. **Diabetes mellitus tipo 2: Latinoamérica y Colombia, último quinquenio** — http://www.scielo.org.co/scielo.php?script=sci_arttext&pid=S0121-52562023000200035
   Recent five-year analysis.
##### English query results
7. **Using DHIS2 to support cardiovascular health in the Americas — DHIS2** — https://dhis2.org/hearts-support-cardiovascular-health-americas/
   Colombia named among HEARTS/DHIS2 implementer countries in the Americas.

8. **Integrating hypertension and diabetes management: HEARTS as a tool — PMC** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9440730/
   HEARTS framework background.

9. **Comparative evaluation of ASCVD, SCORE-2, HEARTS risk scores in Colombian adults — Frontiers** — https://www.frontiersin.org/journals/cardiovascular-medicine/articles/10.3389/fcvm.2025.1734611/full
   2025 Colombian study comparing CVD risk scores including HEARTS.

10. **Hypertension Registry System Design — DHIS2 Docs** — https://docs.dhis2.org/en/topics/metadata/non-communicable-diseases/hypertension-control/design.html
    Reference design supporting HEARTS DHIS2 registries.


### Comores — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Comoros has documented NCD burden via STEPS, with national policies (Politique nationale de lutte contre les MNT, PCE horizon 2030) prioritizing diabetes, hypertension and CVD. The Ministry of Health uses DHIS2 as its national health information system; a 2025 Comorian press article reports that DHIS2 data for 2024–2025 show a rising number of recorded type 2 diabetes cases, explicitly attributing the figures to the national HMIS (DHIS2). DHIS2 is also integrated with Open mSupply for health-facility inventory management, and NGO Santé Diabète supports diabetes/hypertension protocols in peripheral health facilities.

DHIS2 USE: CONFIRMED
A Comorian press source explicitly cites DHIS2 (national HMIS) data for 2024 and 2025 as the source for monitoring type 2 diabetes case trends, demonstrating routine cardiometabolic indicators flowing through the national DHIS2 instance.

#### Search Results

##### Arabic query results
1. **جزر القمر — وزارة الصحة / OMS — Politique nationale MNT** — https://www.iccp-portal.org/sites/default/files/plans/COM_B3_FULL%20Brochure%20SD%20politique%20MNT%20Comores.pdf
   National NCD policy brochure (also available in French).
##### French query results
2. **Politique nationale de lutte contre les MNT — Comores** — https://www.iccp-portal.org/sites/default/files/plans/COM_B3_FULL%20Brochure%20SD%20politique%20MNT%20Comores.pdf
   Politique nationale MNT.

3. **L'épidémiologie des MNT aux Comores — Comores Initiatives** — https://comores-initiatives.com/2019/05/04/lepidemiologie-des-maladies-non-transmissibles-mnt-aux-comores/ [BROKEN: unreachable]
   Vue d'ensemble épidémiologique.

4. **Programme Union des Comores — Santé Diabète** — https://santediabete.org/en_en/sante-diabete-en-action/union-des-comores/
   Programme ONG d'appui MNT (protocoles diabète/hypertension).

5. **12ème Journée nationale d'ophtalmologie — Comores (OMS Afro)** — https://www.afro.who.int/fr/news/comores-la-12eme-journee-nationale-dophtalmologie-sest-concentree-sur-le-lien-entre-le-diabete
   Focus diabète et complications oculaires.

6. **Sensibilisation diabète & HTA: 220 personnes dépistées à Moroni — La Gazette des Comores** — https://lagazettedescomores.com/sant%C3%A9/sensibilisation-sur-le-diab%C3%A8te-et-l%E2%80%99hypertension-art%C3%A9rielle-220-personnes-d%C3%A9pist%C3%A9es-%C3%A0-moroni-.html
   Activité de dépistage communautaire.

7. **Notre action aux Comores — Santé Diabète (factsheet)** — https://santediabete.org/wp-content/uploads/2021/03/Web-Factsheet-SD-Comores-finaledef.pdf
   Liste détaillée des activités (formations, protocoles, etc.).
##### English query results
8. **Ministry of Health in Comoros continuing to strengthen DHIS2 — DHIS2 / Facebook** — https://www.facebook.com/dhis2/posts/nice-to-see-the-ministry-of-health-in-comoros-continuing-to-strengthen-their-dhi/4858352707587498/
   Confirms MoH ongoing DHIS2 use and strengthening.

9. **Nutrition transition and CVD risk factors in Anjouan, Comoros — PMC** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7320757/
   Adult population CVD risk factor study.

10. **Comoros — IDF Diabetes Atlas** — https://diabetesatlas.org/data-by-location/country/comoros/
    Country-level diabetes prevalence.

11. **Diabète : Le train lancé à pleine vitesse — Al-Fajr Quotidien** — https://www.al-fajrquotidien.com/diabete-le-train-lance-a-pleine-vitesse/
    Comorian press article citing national HMIS (DHIS2) data for 2024 and 2025 documenting a marked increase in recorded type 2 diabetes cases — direct evidence that cardiometabolic indicators are routed through the Comoros DHIS2 instance.


### Congo Republic (Brazzaville) — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Congo-Brazzaville has implemented DHIS2 — its national launch was documented publicly by DHIS2 — and has adopted WHO PEN/WHO-PEN integrated NCD interventions in pilot integrated health centers across 20 health districts. Diabetes prevalence in Congolese adults is ~7%, and hospital-based studies confirm large hypertension and metabolic syndrome burden. Direct evidence linking cardiometabolic indicators to the national DHIS2 instance was not found in this search.

DHIS2 USE: LIKELY
DHIS2 has been launched at country level and WHO-PEN is being scaled, so NCD aggregate reporting plausibly flows through DHIS2, but no NCD-specific DHIS2 documentation was surfaced for Congo-Brazzaville.

#### Search Results

##### French query results
1. **Lancement de l'implémentation du DHIS2 au Congo-Brazzaville — DHIS2 (Facebook)** — https://www.facebook.com/dhis2/posts/lancement-de-limplementation-du-dhis2-au-congo-brazzaville-avec-la-pr%C3%A9sence-du-s/2154749224614540/
   Annonce officielle du lancement national.

2. **Metabolic Syndrome–Type 2 Diabetes association in Congolese patients (Brazzaville) — HSD** — https://www.hsd-fmsb.org/index.php/hsd/article/view/7690
   Étude au CHU de Brazzaville.

3. **Néphropathie diabétique au CHU de Brazzaville — ScienceDirect** — https://www.sciencedirect.com/science/article/abs/pii/S1957255715300481
   Étude clinique.

4. **Surveillance des maladies — DHIS2 (fr)** — https://dhis2.org/fr/disease-surveillance/
   Documentation des fonctions de surveillance DHIS2.
##### English query results
5. **Fighting NCDs in Malawi and Sri Lanka with the Diabetes Compass Project — DHIS2** — https://dhis2.org/fighting-ncds-in-malawi-and-sri-lanka-with-the-diabetes-compass-project/
   Regional DHIS2 NCD reference (Congo not explicitly named).

6. **HISP Centre and World Diabetes Foundation Partnership — DHIS2** — https://dhis2.org/hisp-centre-world-diabetes-foundation-partnership/
   Regional initiative to strengthen DHIS2 NCD tools.

7. **Diabetes care, Congolese context — IDF members (Republic of Congo)** — https://idf.org/our-network/regions-and-members/africa/members/democratic-republic-of-congo/association-des-diabetiques-du-congo/
   Country diabetes association profile.

8. **NCDs and DHIS2: Learning from Country Experiences — DHIS2 Community** — https://community.dhis2.org/t/ncds-and-dhis2-learning-from-country-experiences/48128
   Country-experience discussion.


### Costa Rica — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Costa Rica operates a national Diabetes Surveillance System (SVD) managed by Caja Costarricense de Seguro Social (CCSS) and the Ministry of Health, with mandatory case registration at Health Areas linked to EBAIS primary-care teams. National surveillance is also reinforced through PAHO/CDC's Central American Diabetes Initiative (CAMDI) since 2000 and PAHO disease-burden profiles. The search returned no evidence that DHIS2 is used for cardiometabolic data in Costa Rica; CCSS uses its own EDUS / SVD electronic systems.

DHIS2 USE: NONE
National cardiometabolic surveillance in Costa Rica runs on CCSS systems (EDUS, SVD) and PAHO CAMDI, not on DHIS2.

#### Search Results

##### Spanish query results
1. **Incidencia de diabetes tipo 2 en un área urbano-marginal de Costa Rica — Acta Médica Costarricense** — https://www.scielo.sa.cr/scielo.php?script=sci_arttext&pid=S0001-60022008000100006
   Describes the SVD (Sistema de Vigilancia de Diabetes) operated by CCSS/MoH.

2. **Guía para la Atención de la Persona con Diabetes Mellitus Tipo 2 — CCSS / CENDEISSS** — https://www.cendeisss.sa.cr/wp-content/uploads/2024/04/GuiaDM.CCSS_.pdf
   CCSS national DM2 clinical guideline.

3. **Encuesta de diabetes, hipertensión y factores de riesgo — Costa Rica (OPS)** — https://www3.paho.org/hq/index.php?option=com_content&view=article&id=3042:2010-encuesta-diabetes-hipertension-factores-riesgo-enfermedades-cronicas-costa-rica&Itemid=0&lang=es [BROKEN: 502]
   PAHO CAMDI survey results.

4. **Comportamiento de la diabetes mellitus en Costa Rica** — https://www.scielo.org.mx/scielo.php?script=sci_arttext&pid=S2007-74592017000300211
   National epidemiological behavior.

5. **Epidemiología de la diabetes en Costa Rica — ScienceDirect** — https://www.sciencedirect.com/science/article/abs/pii/S1134323010620042
   Diabetes epidemiology study.

6. **Epidemiología de la Diabetes en Costa Rica — ICAP** — https://campusvirtual.icap.ac.cr/mod/forum/discuss.php?d=34666
   Academic discussion of diabetes data.
##### English query results
7. **Diabetes Disease Burden Profile 2023: Costa Rica — PAHO/WHO** — https://www.paho.org/en/documents/diabetes-disease-burden-profile-2023-costa-rica
   PAHO disease-burden profile for Costa Rica.


### Côte d'Ivoire — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Côte d'Ivoire's national health information system is DHIS2-based and branded "SIGSANTE", deployed by the Ministry of Health since 2015 to centralise routine health data; vaccination flows were integrated in 2018, and in 2025 the WHO Country Office and MoH convened a DHIS2 Technical Working Group to strengthen interoperability of SIGSANTE with other systems. The national diabetes survey, hospital data from the Centre Anti-Diabétique d'Abidjan (CADA), and growing comorbidity (hypertension prevalence in DM patients rose from 20% to 44.9%) indicate substantial cardiometabolic burden. No surfaced document directly links specific NCD/cardiometabolic indicators to SIGSANTE modules.

DHIS2 USE: LIKELY
DHIS2 is the national HMIS (SIGSANTE) of Côte d'Ivoire; cardiometabolic care is documented, but no explicit evidence of NCD-specific Tracker/aggregate modules in SIGSANTE was returned.

#### Search Results

##### French query results
1. **mHealth et DHIS2: vers une interopérabilité renforcée en Côte d'Ivoire — mHealth Africa** — https://www.mhealth-africa.org/en/actualites/mhealth-et-dhis2-vers-une-interoperabilite-renforcee-en-cote-divoire/
   Confirms DHIS2/SIGSANTE as backbone of SNIS since 2015; 2025 WHO/MoH technical meeting.

2. **Manuel utilisateur du SIGSANTE (DHIS2) — DIPE** — https://dipe.info/index.php/fr/surveillance-vih/evaluation-des-programmes/download/19-manuels-utilisateurs-apps-snis/46-manuel-utilisateur-du-sigsante-dhis2
   Official user manual for SIGSANTE (DHIS2).

3. **Formation en ligne sur SIGSANTE DHIS2 — DIPE** — https://dipe.info/index.php/fr/formation-en-ligne-sur-sigsante-dhis2
   Online training portal for SIGSANTE users.

4. **Profil des diabétiques 20–79 ans — enquête nationale Côte d'Ivoire — RASP** — https://www.revue-rasp.org/index.php/rasp/article/view/340
   National diabetes prevalence and characteristics survey.

5. **CAD-04: 40 années d'activités CADA — ScienceDirect** — https://www.sciencedirect.com/science/article/abs/pii/S1262363616301008
   Centre Anti-Diabétique d'Abidjan, 40-year cohort.

6. **Sensibilisation lycée municipal Attécoubé — AIP** — https://www.aip.ci/358795/cote-divoire-aip-une-ong-sensibilise-le-personnel-du-lycee-municipal-dattecoube-2-sur-la-prevention-du-diabete-et-de-lhypertension-arterielle/
   Community awareness on DM/HTN prevention.

7. **Dépistage en Côte d'Ivoire — Biogaran Afrique** — https://africa.biogaran.com/nos-actualites/actions/depistage-en-cote-divoire/
   Screening campaigns.
##### English query results
8. **Diabetic and cardiovascular patients' willingness to pay for national health insurance — PMC** — https://pmc.ncbi.nlm.nih.gov/articles/PMC6734508/
   National insurance study with DM/CVD patients.

9. **Management of Type 2 diabetes in Senegal and Côte d'Ivoire — Longdom** — https://www.longdom.org/proceedings/management-of-type-2-diabetes-in-senegal-and-cte-divoire-15521.html
   Comparative DM2 management study.

10. **Côte d'Ivoire HIS Indicators — MEASURE Evaluation** — https://www.measureevaluation.org/his-strengthening-resource-center/country-profiles-1/cote-d2019ivoire.html
    HIS country profile (background on SIGSANTE/SNIS).


### Djibouti — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Djibouti has been actively engaged in adopting the WHO HEARTS package and developing national guidelines for hypertension and diabetes management at primary care, with a Sanofi/CNSS partnership focused on diabetes, hypertension, and oncology. The country is digitalizing its health information system but no public source confirms DHIS2 is being used for cardiometabolic surveillance specifically. STEPS data and NCD policy work exist, but no peer-reviewed or government source links DHIS2 to NCD/HEARTS programs in Djibouti.

DHIS2 USE: UNCLEAR
Djibouti is investing in digital health and has adopted HEARTS/PEN concepts, but available sources do not confirm DHIS2 as the platform used for cardiometabolic data collection or reporting.

#### Search Results

##### French query results
1. **L'hypertension artérielle et le diabète, un défi sanitaire de taille à Djibouti — UN Djibouti** — https://djibouti.un.org/fr/223977-l%E2%80%99hypertension-art%C3%A9rielle-et-le-diab%C3%A8te-un-d%C3%A9fi-sanitaire-de-taille-%C3%A0-djibouti
   Workshop validating national hypertension/diabetes guidelines, anchored on WHO HEARTS integration at primary health centers.

2. **Doucement mais sûrement, la santé devient numérique à Djibouti — Gavi** — https://www.gavi.org/fr/vaccineswork/doucement-mais-surement-sante-devient-numerique-djibouti
   Overview of Djibouti's digital health transition; does not name DHIS2 for NCDs.

3. **Sanofi Global Health et la CNSS déploient leur partenariat — Afrimag** — https://afrimag.net/djibouti-sanofi-global-health-et-la-cnss-deploient-leur-partenariat/
   Tripartite partnership covering oncology, diabetes, hypertension: therapeutic education, prevention, and management of NCDs.
##### English query results
*(no remaining results after filtering generic WHO/PAHO/RTSL links)*


### Dominica — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
No public source describes Dominica using DHIS2 for cardiometabolic surveillance. The country has a high NCD burden and participates in PAHO/Caribbean regional NCD initiatives, but its national health information system is not publicly identified as DHIS2 for hypertension/diabetes programs. Caribbean countries more commonly use ad hoc registries or PAHO HEARTS dashboards.

DHIS2 USE: UNKNOWN
No evidence of DHIS2 adoption for cardiometabolic data in Dominica from searches; PAHO HEARTS in the Americas uses DHIS2 regionally but Dominica's participation is not documented in available results.

#### Search Results

##### English query results
1. **HEARTS360: A gold-standard dashboard for hypertension and diabetes programs — Simple** — https://www.simple.org/blog/hearts360-dashboard/
   Description of HEARTS360 dashboard on DHIS2.

2. **Monitoring and evaluation platform for HEARTS in the Americas — PMC** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10202337/
   Peer-reviewed description of DHIS2-based HEARTS M&E platform across PAHO countries.


### Dominican Republic — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
The Dominican Republic launched HEARTS in 2019 with MISPAS, aligning with the PAHO HEARTS in the Americas M&E platform built on DHIS2. As of end-2024, MISPAS has begun operational DHIS2 implementation for cardiovascular disease data within the HEARTS rollout: 703 of 1,774 primary care centers implement HEARTS, and authorities are working to interoperate the DHIS2 platform with clinics already using the national information system. National epidemiological surveillance platforms (ViEpi, EpiVigila, SAT) remain the domestic systems for broader vigilance.

DHIS2 USE: CONFIRMED
PAHO journal and PMC sources document DHIS2 as the platform supporting HEARTS cardiovascular data in the Dominican Republic, with named coverage across 703 primary care centers under MISPAS — a direct cardiometabolic data flow.

#### Search Results

##### Spanish query results
1. **Building health capacities through the HEARTS Initiative in the Dominican Republic — Pan American Journal of Public Health** — https://journal.paho.org/en/articles/building-health-capacities-through-hearts-initiative-dominican-republic
   Description of HEARTS rollout with MISPAS from 2019; integration with primary care.

2. **Surveillance of noncommunicable diseases — WHO Dominican Republic** — https://www.who.int/teams/noncommunicable-diseases/surveillance/data/dominican-republic
   WHO country page summarizing DR's NCD surveillance.

3. **Dominican Republic Country Profile — PAHO Health in the Americas** — https://hia.paho.org/en/node/220
   National epidemiology platforms mentioned: ViEpi, SAT, EpiVigila.
##### English query results
4. **Addressing Noncommunicable Disease in Dominican Republic: Barriers to Hypertension and Diabetes Care — Annals of Global Health (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC6748242/
   Documents weak surveillance data for diabetes outcomes in DR public system.

5. **Evaluation of a Program to Improve Intermediate Diabetes Outcomes in Rural Communities — PMC** — https://pmc.ncbi.nlm.nih.gov/articles/PMC6394404/
   Chronic Care International program in rural DR, with electronic database for diabetes/hypertension.

6. **Monitoring and evaluation platform for HEARTS in the Americas — PMC** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10202337/
   Describes DHIS2-based HEARTS M&E platform across PAHO region; DR is among the named implementing countries with 703 of 1,774 primary care centers under HEARTS-on-DHIS2 as of end-2024.


### DPR Korea — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
NCDs (CVD, cancer, diabetes) are the leading causes of illness and premature death in DPR Korea. Adult diabetes prevalence is reported at ~5.3%, with hypertension a major driver of CVD mortality. WHO and the DPRK Ministry of Public Health are preparing a roadmap to scale hypertension and diabetes care using HEARTS technical package protocols, and the World Diabetes Foundation has supported diabetes care projects in the country. No evidence in this search indicates DHIS2 is used in DPR Korea; the country runs closed national health information systems separate from the wider DHIS2 ecosystem.

DHIS2 USE: NONE
No documentation surfaced for DHIS2 deployment in DPR Korea; WHO/MoH activities for HTN, DM and CVD use national protocols and WHO-supported tools rather than DHIS2.

#### Search Results

##### English query results
1. **Surveillance of noncommunicable diseases — DPR Korea (WHO)** — https://www.who.int/teams/noncommunicable-diseases/surveillance/data/democratic-people-s-republic-of-korea
   Country-level NCD surveillance summary (WHO).

2. **Strengthening NCD Action in DPR Korea: Virtual Orientation on Oral Health, CVD & Cancer Control (2025)** — https://www.who.int/dprkorea/news/detail/30-05-2025-strengthening-ncd-action-in-dpr-korea-virtual-orientation-on-oral-health-cardiovascular-diseases-cancer-control
   2025 WHO country-office activity on CVD/cancer control.

3. **Hypertension — DPR Korea 2023 country profile (WHO)** — https://www.who.int/publications/m/item/hypertension-prk-2023-country-profile
   National hypertension profile.

4. **Diabetes — DPR Korea 2016 country profile (WHO)** — https://www.who.int/publications/m/item/diabetes-prk-country-profile-2016
   National diabetes country profile.

5. **DPR Korea — International Diabetes Federation member** — https://idf.org/our-network/regions-and-members/western-pacific/members/democratic-peoples-republic-of-korea/
   IDF member profile.

6. **Diabetes care, DPR Korea — World Diabetes Foundation (WDF07-0242)** — https://www.worlddiabetesfoundation.org/what-we-do/projects/wdf07-0242/
   WDF-funded diabetes care project.

7. **Public health in Democratic People's Republic of Korea — PMC** — https://pmc.ncbi.nlm.nih.gov/articles/PMC6441315/
   Overview of public health system.


### DRC — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
The DRC has nationally rolled out DHIS2 as the SNIS (Système National d'Information Sanitaire), with implementation expanding from 52 health zones in 2015 to nationwide coverage. A 2024 study explicitly used DHIS2 routine monthly reports to track hypertension and diabetes progression in DRC from 2019–2023, demonstrating DHIS2 as a viable source for NCD surveillance estimates. Cardiometabolic disease burden is rising, and the country has a national NCD prevention strategy 2017–2024.

DHIS2 USE: CONFIRMED
DHIS2 is the national HMIS (SNIS) in DRC and has been explicitly used to extract routine hypertension and diabetes data for surveillance research.

#### Search Results

##### French query results
1. **Choix thérapeutiques des hypertendus et diabétiques en milieu rural : Étude mixte en RDC** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9575352/
   Mixed-methods study of treatment choices among hypertensive and diabetic patients in two rural health zones in eastern DRC.

2. **Évolution du système national d'information sanitaire de la RDC entre 2009 et 2015** — https://panafrican-med-journal.com/content/article/28/225/full/
   Traces the evolution of DRC's SNIS, documenting DHIS2 implementation in 284 of 516 health zones by 2015 and data-use challenges.

3. **Suivi de la fonctionnalité DHIS2 — Kasai-Oriental (Ministère de la Santé Publique)** — https://pnlprdc.org/wp-content/uploads/2023/04/SUIVI-DE-LA-FONCTIONNALITE-DHIS2_T4_Kasai-Oriental.pdf
   MoH provincial-level DHIS2 functionality monitoring report.

4. **Plateforme Achat Stratégique (FBP-RDC, DHIS2)** — https://dhis2.fbp-rdc.org/
   DRC's performance-based financing DHIS2 platform.

5. **SNIS RDC** — https://snisrdc.com/
   Official portal of DRC's National Health Information System.
##### English query results
6. **Dynamic Progression of Hypertension and Diabetes in the DRC from 2019 to 2023 (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11431946/
   Used DHIS2 routine monthly reports (2019–2023) to estimate HTN and diabetes trends nationally.

7. **Dynamic Progression of Hypertension and Diabetes in the DRC (J Clin Med, MDPI)** — https://www.mdpi.com/2077-0383/13/18/5488
   Journal version of the DHIS2-based DRC HTN/diabetes surveillance analysis.

8. **Data strengthens health systems: The power of DHIS2 (IMA World Health)** — https://imaworldhealth.org/blog/2022/data-strengthens-health-systems-power-dhis2
   Describes IMA's role in customizing DHIS2 to the DRC SNIS and rolling it out to South Kivu and beyond.

9. **Implementation of DHIS2 in DRC: processes and challenges** — https://www.childhealthtaskforce.org/sites/default/files/2018-09/data_wkshp_drc_dhis2_implem.pdf
   Workshop document on DHIS2 implementation processes and challenges in DRC.

10. **Increased prevalence of obesity, diabetes mellitus and hypertension in a mine-based workforce, DRC (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/33708304/
    Workforce-based NCD prevalence study in DRC mining sector.


### Ecuador — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Ecuador is one of the most advanced HEARTS implementers in the Americas, with HEARTS scaled to 483 health facilities and the goal of universal first-level coverage by 2025. PAHO's regional HEARTS M&E platform is built on DHIS2, and Ecuador's national HEARTS reporting uses this DHIS2 platform for CVD/hypertension indicators. The Ministry has also adapted GRADE-based diabetes and hypertension clinical guidelines.

DHIS2 USE: CONFIRMED
PAHO/WHO publications confirm Ecuador's participation in the DHIS2-based HEARTS in the Americas M&E platform for hypertension control reporting at the primary care level.

#### Search Results

##### Spanish query results
1. **Webinar: iniciativa HEARTS, "un pasaporte hacia la salud" — Ministerio de Salud Pública** — https://www.salud.gob.ec/webinar-iniciativa-hearts-un-pasaporte-hacia-la-salud/
   Ministry promotion of HEARTS rollout.

2. **plan-escalamiento-iniciativa-hearts-ecuador-junio-2021-2025 — SlideShare** — https://www.slideshare.net/slideshow/planescalamientoiniciativaheartsecuadorjunio20212025pdf/257558852
   National scale-up plan for HEARTS in Ecuador 2021–2025.

3. **OPS y Ecuador adaptan guías GRADE para diabetes e hipertensión arterial — PAHO** — https://www.paho.org/es/noticias/18-12-2025-ops-ecuador-adaptan-guias-grade-para-diabetes-e-hipertension-arterial
   Adaptation of GRADE clinical guidelines for diabetes and hypertension.

4. **Informe de Ecuador: Mejorando la salud cardiovascular — PAHO** — https://www.paho.org/es/noticias/16-5-2023-informe-ecuador-mejorando-salud-cardiovascular-desde-comunidades-locales-hasta
   PAHO report on Ecuador HEARTS implementation in 483 facilities.

5. **ENCUESTA STEPS ECUADOR 2018 — MSP/INEC/OPS** — https://www.salud.gob.ec/wp-content/uploads/2020/10/INFORME-STEPS.pdf
   National STEPS survey for NCD risk factor surveillance.

6. **Compendium of Essential Clinical Tools 2023 HEARTS IN THE AMERICAS** — http://saludecuador.org/maternoinfantil/archivos/figess/figess_Figess042.pdf
   Tools used in Ecuador's HEARTS implementation.
##### English query results
7. **Monitoring and evaluation platform for HEARTS in the Americas — PMC** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10202337/
   Describes the PAHO HEARTS M&E platform built on DHIS2 in Americas, including Ecuador.

8. **Implementation of Global Hearts Hypertension Control Programs in 32 LMICs — JACC** — https://www.jacc.org/doi/10.1016/j.jacc.2023.08.043
   Cross-country review of HEARTS scale-up including Ecuador.

9. **Cardiometabolic diseases and associated risk factors in transitional rural communities in tropical coastal Ecuador — PLOS One** — https://journals.plos.org/plosone/article?id=10.1371%2Fjournal.pone.0307403
   Recent epidemiology of cardiometabolic disease in coastal Ecuador.


### Egypt — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Egypt's "100 Million Seha" national screening initiative targets early detection and management of diabetes and hypertension at population scale. NCDs cause about 82% of deaths. Egypt has standardized type 2 diabetes management guidelines at primary care. The national HMIS is largely Ministry-led; available sources do not identify DHIS2 as the system used for cardiometabolic surveillance or HEARTS reporting in Egypt.

DHIS2 USE: UNKNOWN
No evidence in publicly available academic or government sources that DHIS2 is used for diabetes/hypertension/CVD surveillance in Egypt. Egypt operates its own national HMIS for the "100 Million Seha" data and standard NCD surveillance.

#### Search Results

##### Arabic / English query results
1. **WHO EMRO — Noncommunicable diseases (Egypt)** — https://www.emro.who.int/egy/programmes/noncommunicable-diseases.html
   WHO EMRO summary: NCDs account for 82% of deaths in Egypt; programmes covered.

2. **From guidelines to practice: an Egyptian expert opinion on type 2 diabetes mellitus management in primary care settings — PMC** — https://pmc.ncbi.nlm.nih.gov/articles/PMC12687541/
   Egyptian expert consensus on T2DM management at primary care.

3. **STEPS report Egypt 2005–06 — WHO** — https://cdn.who.int/media/docs/default-source/ncds/ncd-surveillance/data-reporting/egpyt/steps/steps-report-egypt-2005-06.pdf
   National STEPS survey baseline.


### El Salvador — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
El Salvador joined HEARTS in the Americas in February 2022, and PAHO explicitly facilitated technical meetings with MINSAL to deploy the HEARTS Monitoring and Evaluation System on DHIS2. The country also issued national technical guidelines (2021) for integrated management of hypertension, diabetes, and chronic kidney disease at primary care. Cardiovascular disease accounts for roughly a quarter of NCD deaths.

DHIS2 USE: CONFIRMED
PAHO documentation states that El Salvador's HEARTS M&E system uses DHIS2 to track standardized hypertension and CVD indicators in primary care.

#### Search Results

##### Spanish query results
1. **Hearts El Salvador — PAHO** — https://www.paho.org/es/hearts-salvador
   PAHO El Salvador HEARTS page; describes M&E system on DHIS2 platform.

2. **El Salvador se suma a la iniciativa HEARTS — PAHO (Feb 2022)** — https://www.paho.org/es/noticias/24-2-2022-salvador-se-suma-iniciativa-hearts
   Official launch of HEARTS in El Salvador, February 2022.

3. **Lineamientos técnicos para el abordaje integral de la hipertensión arterial, diabetes mellitus y enfermedad renal crónica en el primer nivel de atención — MINSAL** — https://asp.salud.gob.sv/regulacion/pdf/lineamientos/lineamientos_tecnicos_abordaje_hipertension_diabetes_enfermedad_renal_primer_nivel_atencion_v3.pdf
   National 2021 technical guidelines for HTN/DM/CKD integrated care.

4. **La iniciativa HEARTS en las Américas se aplicará en el país — Diario El Salvador** — https://diarioelsalvador.com/la-iniciativa-hearts-en-las-americas-se-aplicara-en-el-pais/196313/
   Press coverage of HEARTS launch.
##### English query results
5. **Monitoring and evaluation platform for HEARTS in the Americas — PMC** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10202337/
   Peer-reviewed description of DHIS2-based HEARTS M&E platform; El Salvador is a participating country.

6. **Integrating hypertension and diabetes management in primary health care settings: HEARTS as a tool — PMC** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9673610/
   Region-wide HEARTS implementation experience.


### Equatorial Guinea — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Equatorial Guinea began deploying DHIS2 as its national HMIS through a partnership between EHAS, FRS, MINSABS, and the University of Oslo, with the first pilot rolled out Feb–May 2025 in two districts. Initial DHIS2 data flows cover IMCI and general primary care services. No specific cardiometabolic (diabetes/hypertension/HEARTS) modules are publicly identified.

DHIS2 USE: MODERATE
DHIS2 is being implemented nationally for general HMIS, but no evidence of specific cardiometabolic NCD modules running on it yet.

#### Search Results

##### Spanish query results
1. **Sistema de Información – Guinea Ecuatorial — Enlace Hispano Americano de Salud (EHAS)** — https://ehas.org/que-hacemos/sis/sis-guinea-ecuatorial/
   Description of DHIS2 implementation in Equatorial Guinea via EHAS, FRS, MINSABS and UiO, with first pilot in 2025.
##### English query results
*(no remaining results after filtering generic WHO/PAHO/RTSL links)*


### Eritrea — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Eritrea has implemented the WHO STEPS approach and piloted the WHO-PEN protocols at primary health centres in Asmara, with a National NCD Policy and Strategic Plan for CVD/diabetes control. The World Diabetes Foundation has supported NCD risk-factor surveillance and clinical training. Public sources do not document DHIS2 use for NCDs in Eritrea; the national HMIS approach is not widely described in academic literature.

DHIS2 USE: UNKNOWN
No public evidence connecting DHIS2 to Eritrea's NCD/cardiometabolic surveillance. Existing NCD surveillance appears built around STEPS and PEN piloting rather than a DHIS2 platform.

#### Search Results

##### English query results
1. **Reduction of the diabetes burden, Eritrea — World Diabetes Foundation (WDF06-194)** — https://www.worlddiabetesfoundation.org/projects/eritrea-wdf06-194 [BROKEN: 404]
   WDF-funded NCD surveillance and clinical training program in Eritrea; STEPS 1–3, WHO-PEN pilots in Asmara.

2. **Profile of patients with diabetes in Eritrea: results of first phase registry analyses — Acta Diabetologica** — https://link.springer.com/article/10.1007/s00592-009-0093-8
   Phase one of a diabetes registry analysis in Eritrea (older but relevant).


### Eswatini — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Eswatini is a major implementer of WHO-PEN, having piloted decentralised hypertension/diabetes care and now scaling via the EU-funded WHO-PEN@Scale cluster-randomised trial. The country is also preparing to deploy PEN-Plus for severe NCDs (type 1 diabetes, sickle cell, RHD). A 2024 STEPS survey provided current risk-factor data. Eswatini's NCD desk guide covers clinic-level NCD case management. Eswatini's national HMIS uses DHIS2, but available sources do not explicitly confirm a dedicated DHIS2 cardiometabolic/HEARTS module is operational.

DHIS2 USE: MODERATE
Eswatini uses DHIS2 nationally for HMIS and runs a robust WHO-PEN program. Public sources don't directly confirm DHIS2 cardiometabolic modules but integration is plausible given WHO-PEN@Scale's M&E requirements.

#### Search Results

##### English query results
1. **Scaling up the WHO-PEN package for diabetes and hypertension in Swaziland (WHO-PEN@Scale) — CORDIS** — https://cordis.europa.eu/project/id/825823
   EU H2020 project: nationwide cluster-randomised evaluation of three PEN delivery strategies in Eswatini.

2. **Strengthening primary care for diabetes and hypertension in Eswatini: study protocol for a nationwide cluster-randomized controlled trial — Trials** — https://trialsjournal.biomedcentral.com/articles/10.1186/s13063-023-07096-4
   Recent (2023) protocol paper for the nationwide WHO-PEN@Scale trial.

3. **Human and financial resource needs for universal access to WHO-PEN interventions for diabetes and hypertension care in Eswatini — PubMed** — https://pubmed.ncbi.nlm.nih.gov/38802811/
   Time-and-motion and bottom-up costing study (2024).

4. **Bringing care for severe noncommunicable diseases closer to communities in Eswatini — WHO AFRO** — https://www.afro.who.int/countries/eswatini/news/bringing-care-severe-noncommunicable-diseases-closer-communities-eswatini
   Roll-out of PEN-Plus for severe NCDs.

5. **Kingdom of Eswatini Clinic Level Non Communicable Diseases Case Management — ComDis-HSD** — https://comdis-hsd.leeds.ac.uk/wp-content/uploads/sites/50/2019/09/NCD-and-MH-Comprehensive-Desk-Guide-Merged-2019-08-30-2.pdf
   National NCD/mental health desk guide.

6. **Decentralising NCD management in rural southern Africa: evaluation of a pilot implementation study — BMC Public Health** — https://bmcpublichealth.biomedcentral.com/articles/10.1186/s12889-019-7994-4
   Pilot evaluation of decentralised NCD care relevant to Eswatini.

7. **Scale Up Research Programme SU04 — GACD** — https://www.gacd.org/research/projects/su04
   Companion GACD scale-up research programme for NCDs.


### Ethiopia — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Ethiopia has implemented DHIS2 nationwide as the core HMIS for the Federal Ministry of Health and regional health bureaus, and in 2020 launched the Ethiopia Hypertension Control Initiative (EHCI) based on WHO HEARTS. Multiple peer-reviewed studies evaluate HEARTS rollout, hypertension surveillance in Addis Ababa pilots, and the WHO PEN package for primary care NCD management. NORAD/WHO midterm evaluations report strong reporting completeness (>99%) for NCD services in Ethiopian facilities.

DHIS2 USE: CONFIRMED
DHIS2 is Ethiopia's national HMIS, and recent literature documents its use for tracking HEARTS hypertension control and NCD service delivery indicators at health facilities.

#### Search Results

##### Amharic / English query results
1. **Level of implementation of district health information system 2 at public health facilities in Eastern Ethiopia — PMC** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9554126/
   Peer-reviewed assessment of DHIS2 implementation in Ethiopian facilities.

2. **Evaluation of the Hypertension Surveillance System at Pilot Hypertension Prevention and Control Health Facilities in Addis Ababa, Ethiopia, 2022 — medRxiv** — https://www.medrxiv.org/content/10.1101/2025.01.15.24314484v1.full
   Direct evaluation of Ethiopia's HEARTS hypertension surveillance system.

3. **Cost-analysis HEARTS HTN control primary care Ethiopia** — https://www.differentiatedservicedelivery.org/wp-content/uploads/Cost-analysis-HEARTS-HTN-control-primary-care-Ethiopia.pdf
   HEARTS hypertension control cost analysis.

4. **Improving the detection and management of common non-communicable diseases in adults in rural Sidama — PLOS One** — https://journals.plos.org/plosone/article?id=10.1371%2Fjournal.pone.0302667
   Implementation study protocol on common NCD detection/management.

5. **Early detection and management of major non-communicable diseases in urban primary healthcare facilities in Ethiopia — PMC** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7783522/
   Hybrid implementation–effectiveness type-3 study protocol.

6. **Evaluation of diabetes care services, data quality, and availability of resources in Ethiopia — BMC Primary Care** — https://bmcprimcare.biomedcentral.com/articles/10.1186/s12875-024-02650-8
   Difference-in-differences analysis of NORAD-WHO NCDs project; 99.2% report completeness for NCD services.

7. **Integrating NCD services in Ethiopia: developing a Three-In-One approach though implementation research — Knowledge Action Portal on NCDs** — https://knowledge-action-portal.com/en/content/integrating-ncd-services-ethiopia-developing-three-one-approach-though-implementation
   Three-in-One NCD service integration initiative.

8. **Digital Health Systems — Central Ethiopia Regional Health Bureau** — https://cerhb.gov.et/digital-health-systems/
   Regional digital health systems including DHIS2 deployment.


### Gabon — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Gabon launched a pilot in 2026 with WHO to integrate hypertension and diabetes management into community-level care, training 98 providers across four health departments (Komo-Mondah, Noya, Mpassa, Woleu) and supplying equipment and medications. Available sources do not document DHIS2 use for cardiometabolic surveillance in Gabon; the pilot appears to be at training/service delivery stage with no public M&E platform identified.

DHIS2 USE: UNKNOWN
No public evidence links DHIS2 to Gabon's hypertension/diabetes program. The HEARTS-style pilot is ongoing but its M&E platform is not described.

#### Search Results

##### French query results
1. **Hypertension et diabète : 98 professionnels de santé formés dans quatre départements pilotes au Gabon — Gabonreview** — https://www.gabonreview.com/hypertension-et-diabete-98-professionnels-de-sante-formes-dans-quatre-departements-pilotes-au-gabon/
   Coverage of May 2026 workshop led by Minister Nkana Ayo; 98 providers trained for HTN/DM pilot.

2. **L'hypertension artérielle masquée chez les diabétiques de type 2 : Prévalence, facteurs associés et retentissement cardiovasculaire — ScienceDirect** — https://www.sciencedirect.com/science/article/abs/pii/S0003392821001657
   Clinical research on masked hypertension in T2DM patients (relevant regional context).
##### English query results
*(no remaining results after filtering generic WHO/PAHO/RTSL links)*


### Gambia — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
The Gambia is scaling up a national diabetes-hypertension programme launched under its 2022 NCD multisectoral action plan, supported by World Diabetes Foundation and Defeat NCD. Population-based surveys (Lancet Global Health, 2023) document high adult prevalence of hypertension, diabetes and obesity. The Gambia MoH's 2021 Service Statistics Report (published via WHO AFRO) reports HMIS-derived hypertension and diabetes case counts by region, and the WDF-funded scale-up project explicitly states that standard NCD indicators are being developed for integration into DHIS2 with training for HMIS managers and NCD clinic staff on data management.

DHIS2 USE: CONFIRMED
The Gambia MoH publishes routine HMIS service statistics (DHIS2) reporting region-level hypertension and diabetes case counts; the active WDF scale-up project documents integration of NCD indicators into DHIS2 as a core programme activity.

#### Search Results

##### English query results
1. **Scale-up of national diabetes-hypertension programme in The Gambia — World Diabetes Foundation** — https://www.worlddiabetesfoundation.org/what-we-do/projects/wdf24-1944/
   WDF-supported expansion of integrated diabetes-hypertension prevention, detection and care across all Gambian regions following the 2022 NCD multisectoral action plan; explicitly includes development of standard NCD indicators for integration into DHIS2 and training of HMIS managers and NCD clinic staff in data management.

2. **Prevalence of hypertension, diabetes, obesity, multimorbidity, and related risk factors among adult Gambians: a cross-sectional nationwide study** — https://www.thelancet.com/journals/langlo/article/PIIS2214-109X(23)00508-9/fulltext
   Nationwide cross-sectional Lancet Global Health study documenting high NCD risk-factor burden in Gambian adults.

3. **Integrating hypertension data into routine health data collection: CARDIO4Cities and DHIS2 in Dakar, Senegal** — https://link.springer.com/article/10.1007/s44250-025-00240-8
   Regional analogue (Senegal) showing DHIS2 integration of hypertension indicators into routine HMIS in West Africa.

4. **Early life nutritional programming of health and disease in The Gambia** — https://pubmed.ncbi.nlm.nih.gov/26503192/
   MRC Gambia work linking life-course nutritional factors to adult NCD outcomes.

5. **The Gambia Ministry of Health — Final Service Statistic Report, 2021 (WHO AFRO)** — https://www.afro.who.int/sites/default/files/2022-07/Final%20Service%20Statistic%20Report,%202021.pdf
   Official MoH HMIS report including region-level hypertensive cases and diabetes cases for 2021, with HMIS Unit conducting quarterly supportive supervision and data verification at community, facility and regional levels — direct documentation of cardiometabolic indicators in the national routine HMIS (DHIS2).


### Ghana — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Ghana has run DHIMS2 (the national DHIS2 instance) as its routine HMIS since 2012, including aggregated NCD/CVD indicators captured at facility level. The Ghana Heart Initiative (GHI), a national CVD health-system-strengthening programme, identifies DHIMS2 gaps for hypertension cascade indicators and is driving improvements to the national data capture system for CVD/NCDs.

DHIS2 USE: CONFIRMED
Ghana's national HMIS is DHIMS2 (DHIS2), and peer-reviewed assessments document NCD/CVD indicators within DHIMS2 plus active initiatives (GHI) to strengthen hypertension and diabetes data within it.

#### Search Results

##### English query results
1. **The Ghana Heart Initiative – a health system strengthening approach as index intervention model to solving Ghana's cardiovascular disease burden** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11061415/
   National CVD strategy that includes improving DHIMS2 data capture for CVD/NCDs.

2. **A multilevel and multicenter assessment of health care system capacity to manage cardiovascular diseases in Africa: a baseline study of the Ghana Heart Initiative** — https://link.springer.com/article/10.1186/s12872-023-03430-5
   Baseline assessment of 44 facilities covering CVD/NCD indicators in DHIMS2; notes limited disaggregation for hypertension cascade.

3. **Utilization of the national cluster of district health information system for health service decision-making at the district, sub-district and community levels in Brong Ahafo, Ghana** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7275484/
   Peer-reviewed evaluation of DHIMS2 use in Ghana for routine decision making.

4. **Improving the Health Information System in Ghana (Adaletey)** — https://courses.cs.washington.edu/courses/csep590b/15wi/pub/Ghana_Dhims2_Adaletey.pdf
   Background on national DHIMS2 rollout led by Ghana Health Service.

5. **GHANA Health Data Ecosystem Mapping — Scaling the Use of Digital tools (BMZ/DIPC)** — https://www.bmz-digital.global/wp-content/uploads/2024/10/GHANA-Health-Data-Ecosystem-Mapping-DIPC.pdf
   2024 mapping of Ghana digital-health stack including DHIMS2 and NCD modules.

6. **Assessing the Strength of DHIMS2 as a Data Management Platform in Korley-Klotey Municipality** — https://www.researchgate.net/publication/393195563_Assessing_the_Strength_of_District_Health_Information_Management_System_II_DHIMS2_as_a_Data_Management_Platform_in_Korley-Klotey_Municipality
   Local-level assessment of DHIMS2 data quality.

7. **Transitioning to digital transactional data capture in primary health care facilities: Ghana's Savannah Region** — https://mhealth.amegroups.org/article/view/133615/html
   Case report on digital data capture at PHC level feeding DHIMS2.


### Grenada — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Grenada launched the PAHO HEARTS Initiative in February 2025, with 231 health professionals trained and digital hypertension registries replacing paper systems. PAHO communications reference DHIS2 as instrumental in data collation for the Caribbean HEARTS rollout, and Grenada is among the leading Eastern Caribbean countries adopting the Better Care for NCDs approach.

DHIS2 USE: CONFIRMED
PAHO HEARTS materials cite DHIS2 in the Caribbean HEARTS implementation, and Grenada has explicitly moved to digital hypertension registries under HEARTS; country-specific confirmation that the registry runs on DHIS2 (vs. another platform) was not definitively established in the retrieved sources.

#### Search Results

##### English query results
1. **Grenada launches HEARTS Initiative (PAHO/WHO)** — https://www.paho.org/en/news/18-2-2025-grenada-launches-hearts-initiative
   Official PAHO announcement of February 2025 HEARTS launch and training.

2. **Grenada leads regional charge with PAHO HEARTS initiative (NOW Grenada)** — https://nowgrenada.com/2025/09/grenada-leads-regional-charge-with-paho-hearts-initiative/
   2025 update on Grenada's HEARTS rollout, digital hypertension registries, and DHIS2 role in data collation.

3. **Cardiovascular risk surveillance to develop a nationwide health promotion strategy: the Grenada Heart Project** — https://pubmed.ncbi.nlm.nih.gov/25691303/
   Earlier nationwide CVD risk surveillance in Grenada.

4. **National Chronic Non-Communicable Disease Policy (Grenada, ICCP)** — https://www.iccp-portal.org/sites/default/files/plans/GRD_B3_CNCD%20Policy%20and%20Multisectoral%20Action%20Plan_Grenada_FINAL%20August%2021%202013%20(1)%20(1).pdf
   Grenada national NCD policy and multisectoral action plan.

5. **Noncommunicable Diseases Care in the Eastern Caribbean (World Bank)** — https://www.worldbank.org/en/region/lac/publication/noncommunicable-diseases-care-in-the-eastern-caribbean
   Regional NCD care analysis covering Grenada.

6. **Improving Hypertension Cardiovascular Care in the Caribbean (CaribGP)** — https://caribgp.org/improving-hypertension-cardiovascular-care-in-the-caribbean-how-and-why-family-physicians-must-up-their-contribution/
   Family-physician perspective on Caribbean hypertension control.


### Guatemala — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Guatemala piloted DHIS2 as the electronic monitoring tool for an integrated WHO HEARTS hypertension/diabetes programme in 11 Ministry of Health primary care facilities (Oct 2023 – May 2024). The DHIS2 instance was hosted at INCAP and supported individual and aggregate patient monitoring, but the pilot found DHIS2 infeasible at PHC level (>20 min/patient, usability 67.7 vs 80.6 for paper-to-REDCap). Despite implementation challenges, HEARTS treatment rates rose substantially. World Diabetes Foundation supports a follow-on national diabetes-hypertension care model.

DHIS2 USE: CONFIRMED
Peer-reviewed implementation studies confirm DHIS2 was deployed by MoH for HEARTS hypertension/diabetes monitoring in Guatemala; sustained scale-up remains uncertain after the pilot found feasibility issues.

#### Search Results

##### Spanish query results
1. **HEARTS como herramienta para integrar el manejo de la hipertensión y la diabetes en los entornos de atención primaria (SciELO)** — https://www.scielosp.org/article/rpsp/2022.v46/e213/es/
   Marco PAHO/HEARTS para integración hipertensión/diabetes en APS.

2. **Implementando un modelo nacional de atención de diabetes e hipertensión en Guatemala (World Diabetes Foundation)** — https://www.worlddiabetesfoundation.org/what-we-do/projects/wdf23-1915/
   Proyecto WDF de escalamiento del modelo HEARTS de hipertensión y diabetes con el MSPAS.

3. **HEARTS como herramienta para integrar el manejo de la hipertensión y la diabetes (PMC, versión español)** — https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9673610/
   Versión en español del artículo PAHO sobre HEARTS y atención primaria.
##### English query results
4. **Implementing integrated hypertension and diabetes management using the WHO HEARTS model: protocol for a pilot study in the Guatemalan national primary care system** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10775666/
   Protocol describing DHIS2 deployment in 11 MoH PHC facilities for HEARTS monitoring.

5. **Evaluating the WHO HEARTS Model for Hypertension and Diabetes Management: A Pilot Implementation Study in Guatemala (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11483012/
   Pilot evaluation reporting DHIS2 feasibility/usability and HEARTS treatment outcomes.

6. **Evaluating the WHO HEARTS Model — Global Heart journal version** — https://globalheartjournal.com/articles/10.5334/gh.1397
   Open-access publication of the Guatemala HEARTS/DHIS2 evaluation.

7. **Implementation Science Communications full text — HEARTS pilot protocol Guatemala** — https://implementationsciencecomms.biomedcentral.com/articles/10.1186/s43058-023-00539-8
   Open-access protocol paper.

8. **Monitoring and evaluation platform for HEARTS in the Americas (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10202337/
   PAHO M&E platform built on DHIS2 supporting HEARTS countries including Guatemala.

9. **Integrating hypertension and diabetes management in primary health care settings: HEARTS as a tool (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9673610/
   PAHO conceptual framework underlying Guatemala's HEARTS work.

10. **A Pilot Implementation Study in Guatemala (PDF, Global Heart)** — https://globalheartjournal.com/articles/1397/files/679cc149b2651.pdf
    PDF of the implementation evaluation.


### Guinea Bissau — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Guinea-Bissau has documented high hypertension prevalence (~27% in Bissau) and significant diabetes burden, but the formal NCD care system is fragmented and population-based registries are limited. The retrieved sources do not establish a national DHIS2 deployment, nor specific DHIS2 use for hypertension/diabetes/HEARTS in Guinea-Bissau.

DHIS2 USE: UNKNOWN
No retrieved source confirms or denies DHIS2 deployment for NCDs in Guinea-Bissau; situational analyses focus on clinical management and prevalence rather than HIS architecture.

#### Search Results

##### Portuguese query results
1. **Análise da Capacidade de Diagnóstico (Repositório Aberto, U. Porto)** — https://repositorio-aberto.up.pt/bitstream/10216/156052/2/653046.pdf
   Tese sobre capacidade diagnóstica e gestão de HTA/diabetes na Guiné-Bissau.

2. **Prevalência da Diabetes Mellitus na Guiné-Bissau (RUN, UNL)** — https://run.unl.pt/bitstream/10362/30963/1/RUN%20-%20Disserta%C3%A7%C3%A3o%20de%20Mestrado%20-%20Gina%20Santos.pdf
   Dissertação sobre prevalência de diabetes mellitus na Guiné-Bissau.

3. **Prioridades nacionais de pesquisa para saúde na Guiné-Bissau (COHRED)** — http://www.cohred.org/wp-content/uploads/2012/09/Prioridade-Nacionais-de-pesquisa-para-saude-na-Guine-Bissau.pdf
   Documento nacional sobre prioridades de investigação em saúde.
##### English query results
4. **Prevalence, awareness, treatment, and control of hypertension in Bissau, Western Africa (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8925014/
   First population-based HTN survey in Bissau; prevalence 26.9%, awareness 51.4%.

5. **Diabetes in urban Guinea-Bissau; patient characteristics, mortality and prevalence of undiagnosed dysglycemia (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7480585/
   Urban diabetes patient cohort and undiagnosed dysglycemia.

6. **Diabetes management in Guinea Bissau: a situational analysis (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC6856535/
   Situational analysis of diabetes services.

7. **Diabetes management in Guinea Bissau: a situational analysis (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/31762879/
   PubMed record of the situational analysis.


### Guinea — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Guinea adopted DHIS2 nationally for disease surveillance starting in 2018 after a 2017 pilot (post-Ebola HIS reforms). Recent epidemiological work documents substantial diabetes and hypertension prevalence (e.g., the 2022 World Diabetes Day screening of 2,050 people in Conakry and five regions). The country has an integrated national NCD programme covering diabetes, hypertension, CVD, cancer and chronic respiratory diseases, but no source confirms DHIS2 is used specifically for NCD/HEARTS registries.

DHIS2 USE: MODERATE
DHIS2 is confirmed as Guinea's national disease-surveillance HMIS (Frontiers Public Health 2021), and a national NCD programme exists; explicit use of DHIS2 for hypertension/diabetes/HEARTS modules was not documented in the retrieved sources.

#### Search Results

##### French query results
1. **O65 Diabète et Maladies chroniques non transmissibles en Guinée : les facteurs de risque sont fréquents (ScienceDirect)** — https://www.sciencedirect.com/science/article/abs/pii/S1262363612710431
   Étude des facteurs de risque MNT en Guinée.

2. **CA-012: L'hypertension artérielle et la résistance à l'insuline sont fréquentes dans la cohorte de jeunes enfants et adolescents diabétiques en Guinée** — https://www.sciencedirect.com/science/article/abs/pii/S1262363616301446
   Cohorte pédiatrique diabète/HTA en Guinée.
##### English query results
3. **Implementation of DHIS2 for Disease Surveillance in Guinea: 2015–2020 (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8811041/
   Peer-reviewed account of Guinea's national DHIS2 rollout for surveillance.

4. **Implementation of DHIS2 for Disease Surveillance in Guinea: 2015-2020 (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/35127614/
   PubMed record of the same study.

5. **Frontiers in Public Health: Implementation of DHIS2 for Disease Surveillance in Guinea** — https://www.frontiersin.org/journals/public-health/articles/10.3389/fpubh.2021.761196/full
   Full-text Frontiers version covering 72% timeliness, 98.5% completeness.

6. **Analysis of cardiovascular risk factors: a retrospective epidemiological study in Guinea in 2022 (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/38043881/
   2022 CVD risk-factor study in Guinea.

7. **Prevalence of Hypertension and Diabetes Sweet in Dubréka, Guinea: Cross-Sectional Study** — https://www.scirp.org/journal/paperinformation?paperid=137979
   Local prevalence data on HTN and diabetes.

8. **Prevalence of Diabetes and Hypertension on World Diabetes Day 2022 in Guinea** — https://www.scirp.org/journal/paperinformation?paperid=130672
   Screening campaign covering Conakry and five inland regions.


### Guyana — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Guyana adopted the Global HEARTS Initiative in 2021 and, with PAHO/WHO support, launched the national expansion of HEARTS in June 2023 and undertook an implementation assessment in October 2024. NCDs comprise ~70% of Guyana's disease burden. PAHO's HEARTS in the Americas M&E platform runs on DHIS2 across participating countries, but the retrieved sources do not directly confirm whether Guyana itself uses DHIS2 for its national HEARTS/NCD registries.

DHIS2 USE: LIKELY
Guyana is an active HEARTS in the Americas participant, and PAHO's regional HEARTS M&E platform is built on DHIS2; country-specific deployment was not explicitly confirmed in the retrieved sources.

#### Search Results

##### English query results
1. **PAHO Guyana Marks World Hypertension Day 2025 with Renewed Call for Prevention and Control Efforts** — https://www.paho.org/en/news/17-5-2025-paho-guyana-marks-world-hypertension-day-2025-renewed-call-prevention-and-control
   2025 PAHO Guyana update on hypertension control efforts.

2. **Guyana Ministry of Health assessing the implementation of the HEARTS Initiative with PAHO/WHO support (Oct 2024)** — https://www.paho.org/en/news/29-10-2024-guyana-ministry-health-assessing-implementation-hearts-initiative-pahowho-support
   Mission to review HEARTS operationalisation and Better Care for NCDs expansion.

3. **Ministry of Health Guyana collaborates with PAHO to launch the national expansion of HEARTS Initiative (June 2023)** — https://www.paho.org/en/news/3-6-2023-ministry-health-guyana-collaborates-paho-launch-national-expansion-hearts-initiative
   Launch of national HEARTS expansion.

4. **Ministry of Health, GUYANA Strategic Plan: NCD Prevention and Control** — https://extranet.who.int/ncdccs/Data/GUY_NCD_GUY_B3_CNCD-Strategy-2020__August_2013-Final.pdf
   Guyana NCD strategy.

5. **Monitoring and evaluation platform for HEARTS in the Americas (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10202337/
   PAHO M&E platform built on DHIS2 for HEARTS countries.


### Haiti — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Haiti's EMMUS-VI (2016-17) documents very high hypertension prevalence (49% women, 38% men aged 35-64). USAID/DAI implemented a Strategic Health Information System programme supporting Haiti's MoH HIS, and Haiti is part of the HEARTS in the Americas regional M&E platform (DHIS2-based). However, the retrieved sources do not directly confirm DHIS2 use for Haiti-specific hypertension/diabetes registries.

DHIS2 USE: UNCLEAR
Haiti participates in PAHO HEARTS in the Americas (which uses a DHIS2-based regional M&E platform), but country-specific DHIS2 deployment for NCD registries was not confirmed; Haiti historically has used other HIS platforms (iHRIS, SISNU, OpenMRS through PIH).

#### Search Results

##### Creole query results
1. **MALADI TANSYON WO : OU DWE KONN SA L YE (Under My Pen)** — https://undermypen.wordpress.com/2023/11/01/maladi-tansyon-wo-ou-dwe-konn-sa-l-ye/
   Atik popilè sou tansyon wo nan Ayiti, sitiyasyon EMMUS-VI (49% fanm/38% gason 35-64 lane).

2. **Accueil — MNT Ayiti** — https://mnt-ayiti.com/
   Sitwèb ki konsantre sou maladi ki pa transmèt nan Ayiti.
##### English query results
3. **Haiti—Strategic Health Information System Program (HIS) (DAI/USAID)** — https://www.dai.com/our-work/projects/haiti-strategic-health-information-system-his-program
   USAID-funded HIS strengthening programme in Haiti.
##### French query results
4. **Integrating hypertension data into routine health data collection: CARDIO4Cities and DHIS2 in Dakar, Senegal** — https://link.springer.com/article/10.1007/s44250-025-00240-8
   Modèle francophone d'intégration HTA/DHIS2 (Sénégal) pertinent pour Haïti.


### Honduras — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Honduras has ~650,000 people with diabetes and ~1.3 million adults aged 30-79 with hypertension; NCDs cause >2/3 of deaths. A 2020-2024 national plan strengthened information systems for diabetes care, and PAHO's HEARTS in the Americas M&E platform — built on DHIS2 — supports CVD risk-factor indicator capture in participating countries. Honduras has clinical protocols for hypertension and type 2 diabetes at PHC. Direct confirmation of DHIS2 use in Honduras specifically was not found.

DHIS2 USE: LIKELY
PAHO's regional HEARTS M&E platform that runs on DHIS2 supports CVD indicator capture for participating Americas countries (including Honduras-relevant references); country-level deployment in Honduras was not explicitly confirmed in retrieved sources.

#### Search Results

##### Spanish query results
1. **Protocolo de Atención Clínica para Diagnóstico y Tratamiento de la Diabetes Mellitus tipo 2 en el Primer Nivel de Atención (Honduras BVS, 2024)** — https://honduras.bvsalud.org/wp-content/uploads/2024/04/Protocolo-de-Atencion-Clinica-Diagnostico-y-Tratamiento-de-la-Diabetes-Mellitus-tipo-2-en-el-Primer-Nivel-de-Atencion.pdf
   Protocolo nacional MSPAS para diabetes tipo 2.

2. **Protocolo de Atención Clínica para Diabetes Mellitus tipo 2 (versión 2025)** — https://honduras.bvsalud.org/wp-content/uploads/2025/08/Protocolo-de-Atencion-Clinica-Diagnostico-y-Tratamiento-de-la-Diabetes-Mellitus-Tipo-2-en-el-Primer-Nivel-de-Atencion.pdf
   Actualización 2025 del protocolo.

3. **Protocolo de atención clínica para hipertensión arterial esencial en el 1er nivel (UNAH)** — https://epidemiologia.unah.edu.hn/assets/Libros-MEPI/Protocolo-de-Atencion-Clinica-para-la-Prevencion-Dianostico-y-Tratamiento-de-Hpwrtension-Arterial-Esencial-en-el-1er-Nivel-de-Atencion.pdf
   Protocolo de HTA esencial en APS.

4. **OMS: 1.3 millones de hondureños padecen de hipertensión arterial (El Heraldo)** — https://www.elheraldo.hn/honduras/honduras-millones-personas-padecen-hipertension-arterial-AF27558190
   Datos de carga de HTA en Honduras.

5. **Premio de Promoción de la Salud Cardiovascular — Iniciativa HEARTS (Argentina, doc PAHO)** — https://cdi.mecon.gob.ar/bases/docelec/az6508.pdf
   Materiales regionales sobre HEARTS y DHIS2.
##### English query results
6. **Honduras: building a national road-map for diabetes surveillance (PAHO)** — https://www.paho.org/en/stories/honduras-building-national-road-map-diabetes-surveillance
   PAHO support for national diabetes surveillance roadmap and HIS strengthening.

7. **Honduras: building a national road map for diabetes surveillance (WHO)** — https://www.who.int/news-room/feature-stories/detail/honduras-building-a-national-road-map-for-diabetes-surveillance
   WHO feature on Honduras 2020-2024 diabetes information plan.

8. **Monitoring and evaluation platform for HEARTS in the Americas (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10202337/
   PAHO HEARTS M&E platform built on DHIS2.

9. **Monitoring and evaluation platform for HEARTS in the Americas (PMC alt)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9484330/
   Companion paper on PAHO HEARTS regional platform.


### India — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
India runs the National Programme for Prevention and Control of NCDs (NP-NCD, formerly NPCDCS) and the India Hypertension Control Initiative (IHCI) with MoHFW, ICMR, WHO and Resolve to Save Lives. IHCI covers 26+ districts across five states using digital hypertension registries. India's primary NCD digital platforms are NCD-IT/Simple App (IHCI) and population-screening modules under Ayushman Bharat — not DHIS2. There is no evidence of national DHIS2 deployment for NCD surveillance in India.

DHIS2 USE: NONE
India does not use DHIS2 as its national HMIS; HMIS-MoHFW and dedicated platforms (NCD-IT, Simple, Ayushman Bharat) carry hypertension/diabetes data. No DHIS2 NCD deployment was identified in the retrieved sources.

#### Search Results

##### Hindi query results
1. **NPCDCS कार्यक्रम (National Program for Prevention & Control of Cancer, Diabetes, CVD and Stroke) — GKToday** — https://www.gktoday.in/hindi/npcdcs-%E0%A4%95%E0%A4%BE%E0%A4%B0%E0%A5%8D%E0%A4%AF%E0%A4%95%E0%A5%8D%E0%A4%B0%E0%A4%AE-national-program-for-prevention-control-of-cancer-diabetes-cardiovascular-diseases-and-stroke/
   एनपीसीडीसीएस कार्यक्रम का विवरण।

2. **गैर-संचारी रोगों हेतु सरकारी कार्यक्रम का नया नाम (Drishti IAS)** — https://www.drishtiias.com/hindi/daily-updates/daily-news-analysis/government-programme-for-ncd-renamed
   NPCDCS से NP-NCD नाम परिवर्तन।

3. **राष्ट्रीय कैंसर, मधुमेह, हृदयवाहिका रोग और आघात रोकथाम कार्यक्रम (NHP India)** — https://hi.nhp.gov.in/%E0%A4%B0%E0%A4%BE%E0%A4%B7%E0%A5%8D%E0%A4%9F%E0%A5%8D%E0%A4%B0%E0%A5%80%E0%A4%AF-%E0%A4%95%E0%A5%88%E0%A4%82%E0%A4%B8%E0%A4%B0,-%E0%A4%AE%E0%A4%A7%E0%A5%81%E0%A4%AE%E0%A5%87%E0%A4%B9,-%E0%A4%B9%E0%A5%83%E0%A4%A6%E0%A4%AF%E0%A4%B5%E0%A4%BE%E0%A4%B9%E0%A4%BF%E0%A4%95%E0%A4%BE-%E0%A4%B0%E0%A5%8B%E0%A4%97-%E0%A4%94%E0%A4%B0-%E0%A4%86%E0%A4%98%E0%A4%BE%E0%A4%A4-%E0%A4%B0%E0%A5%8B%E0%A4%95%E0%A4%A5%E0%A4%BE%E0%A4%AE-%E0%A4%8F%E0%A4%B5%E0%A4%82-%E0%A4%A8%E0%A4%BF%E0%A4%AF%E0%A4%82%E0%A4%A4%E0%A5%8D%E0%A4%B0%E0%A4%A3-%E0%A4%95%E0%A4%BE%E0%A4%B0%E0%A5%8D%E0%A4%AF%E0%A4%95%E0%A5%8D%E0%A4%B0%E0%A4%AE-(%E0%A4%8F%E0%A4%A8%E0%A4%AA%E0%A5%80%E0%A4%B8%E0%A5%80%E0%A4%A1%E0%A5%80%E0%A4%B8%E0%A5%80%E0%A4%8F%E0%A4%B8)_pg [BROKEN: unreachable]
   राष्ट्रीय एनसीडी कार्यक्रम का आधिकारिक पोर्टल।
##### English query results
4. **National Programme for Prevention & Control of Cancer, Diabetes, CVD & Stroke (NPCDCS), NHM** — https://nhm.gov.in/index1.php?lang=1&level=2&sublinkid=1048&lid=604
   Official NPCDCS/NP-NCD page on the National Health Mission portal.

5. **The India Hypertension Control Initiative — early outcomes in 26 districts across five states, 2018-2020 (Nature, J Hum Hypertens)** — https://www.nature.com/articles/s41371-022-00742-5
   Early IHCI outcomes paper; uses Simple app/NCD-IT (not DHIS2).

6. **Training Module for Medical Officers for Prevention, Control & PBS of Hypertension, Diabetes & Common Cancer (NHSRC)** — https://nhsrcindia.org/sites/default/files/2021-06/Module%20for%20MOs%20for%20Prevention,Control%20&%20PBS%20of%20Hypertension,Diabetes%20&%20Common%20Cancer.pdf
   National training module for HTN/diabetes screening under NP-NCD.

7. **National Stroke Registry Programme (NCDIR/ICMR)** — https://stroke.ncdirindia.org/npcdcs.html
   India's national stroke registry under NCDIR.

8. **The Role of NCD Clinics in Improving Control of Hypertension and Diabetes Among Adults in Rural Ballabgarh, Haryana (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10082560/
   Implementation study of NCD clinics for HTN/diabetes.

9. **Prevalence, Awareness, Treatment, and Control of Hypertension and Diabetes: STEPS surveys in Punjab and Haryana (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8978601/
   State-level STEPS surveys.

10. **Designing a comprehensive NCD programme for hypertension and diabetes at PHC level: Karnataka (BMC Public Health)** — https://link.springer.com/article/10.1186/s12889-019-6735-z
    Urban Karnataka NCD programme design.


### Indonesia — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Indonesia's NCD surveillance and screening are organised through Posbindu PTM (community-based integrated coaching posts) and the Directorate P2PTM's web-based PTM risk-factor surveillance system (since 2013). NCDs cause more than half of deaths; ~250,000 deaths/year are attributed to diabetes and 69% of CVD deaths to hypertension. Indonesia's national HMIS is not DHIS2 — the MoH uses its own ASDK / SatuSehat / SIMPUS architecture for PHC and PTM data.

DHIS2 USE: NONE
Indonesia's national HIS and NCD surveillance run on MoH-built platforms (e.g., ASDK-style Posbindu PTM web surveillance, SatuSehat). No DHIS2 deployment for NCDs in Indonesia was identified.

#### Search Results

##### Bahasa Indonesia query results
1. **Evaluasi Surveilans Faktor Risiko Penyakit Tidak Menular (PTM) Berbasis Data Kegiatan "Posbindu PTM"** — https://ejournal2.litbang.kemkes.go.id/index.php/mpk/article/view/3569 [BROKEN: unreachable]
   Evaluasi sistem surveilans PTM berbasis Posbindu (Kemenkes).

2. **Evaluasi Sistem Surveilans Hipertensi dengan Pendekatan Atribut di Puskesmas Oebobo** — https://journal.universitaspahlawan.ac.id/index.php/prepotif/article/view/56301
   Evaluasi atribut sistem surveilans hipertensi di puskesmas.

3. **Analisis Spasial Kasus Diabetes Melitus dan Faktor Risiko (Jerkin)** — https://jerkin.org/index.php/jerkin/article/download/1683/1276
   Analisis spasial DM di Indonesia.

4. **Determinan Diabetes Melitus Tipe II di Posbindu PTM Puskesmas Pegandon Kabupaten Kendal Tengah** — https://journal.unnes.ac.id/sju/higeia/article/view/63923
   Studi determinan DM tipe 2 di Posbindu PTM.

5. **Faktor risiko diabetes mellitus tipe 2 di Posbindu PTM DKI Jakarta (UI Lontar)** — https://lontar.ui.ac.id/detail?id=20476904&lokasi=lokal [BROKEN: unreachable]
   Analisis data surveilans Posbindu PTM DKI Jakarta 2017.

6. **e-PTM Application as a Screening Media for NCD Risk Factors in Adolescents** — https://myjurnal.poltekkes-kdi.ac.id/index.php/hijp/article/view/1143
   Aplikasi e-PTM untuk skrining remaja.
##### English query results
7. **Follow-up After Posbindu NCD Screening in Indonesia: Sociodemographic Predictors of Primary Care Engagement (Wiley)** — https://onlinelibrary.wiley.com/doi/10.1111/phn.70117
   Marthoenis et al., follow-up rate 42.2% after Posbindu screening.

8. **Hypertension and diabetes screening uptake in adults aged 40-70 in Indonesia: KAP study (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC12076961/
   National KAP study on screening uptake.

9. **Geospatial analysis of type 2 diabetes mellitus and hypertension in South Sulawesi (Scientific Reports)** — https://www.nature.com/articles/s41598-023-27902-y
   Geospatial DM/HTN analysis.

10. **Projection of diabetes morbidity and mortality till 2045 in Indonesia (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10914682/
    Projection model and NCD prevention programmes.


### Iraq — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
NCDs cause an estimated 67% of all deaths in Iraq (CVD 39%, cancer 9%, diabetes 6%); nearly half of adults aged 30-79 have hypertension. Iraq has implemented PHC-based HTN/diabetes screening systems at selected PHCCs nationwide. A recent (2025) Frontiers in Public Health study describes the KRG-DHIS2 system in the Kurdistan Region of Iraq, which is being progressively rolled out across public health facilities and represents the first digital health monitoring system systematically capturing facility-based data — including NCDs — in the region. National (federal) Iraq DHIS2 deployment for NCDs was not confirmed.

DHIS2 USE: CONFIRMED
Kurdistan Region of Iraq has a confirmed KRG-DHIS2 deployment (Frontiers Public Health 2025) capturing public-facility data including diabetes/HTN-relevant indicators; federal Iraq MoH NCD use of DHIS2 was not confirmed.

#### Search Results

##### Arabic query results
1. **WHO EMRO — الأمراض غير السارية (الصفحة القُطرية للعراق)** — https://www.emro.who.int/ar/iraq/priority-areas/noncommunicable-diseases.html
   مقاربة منظمة الصحة العالمية لإقليم شرق المتوسط للأمراض غير السارية في العراق.
##### English query results
2. **WHO EMRO — Noncommunicable diseases (Iraq)** — https://www.emro.who.int/iraq/priority-areas/noncommunicable-diseases.html
   WHO EMRO Iraq NCD priority page; documents PHCC screening for HTN/diabetes.

3. **Chronic Noncommunicable Diseases Risk Factor Survey in Iraq (STEPS 2006)** — https://cdn.who.int/media/docs/default-source/ncds/ncd-surveillance/data-reporting/iraq/steps/iraqstepsreport2006.pdf
   Iraq STEPS NCD risk-factor survey.

4. **Iraq: Strengthening NCD prevention program (ReliefWeb)** — https://reliefweb.int/report/iraq/iraq-strengthening-noncommunicable-diseases-prevention-program-budget-5000000-11000000
   Programme document on NCD prevention strengthening.

5. **Screening System and Comprehensive Care for HTN/Diabetes in Iraq (WHO NCD CCS)** — https://extranet.who.int/ncdccs/Data/IRQ_B6_screening%20report%202012.pdf
   Iraq MoH screening system report for HTN/diabetes at 41 PHCCs.

6. **Iraqi Experts Consensus on the Management of Type 2 Diabetes/Prediabetes in Adults (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7440731/
   National clinical consensus for type 2 diabetes management.

7. **Models of care for patients with hypertension and diabetes in humanitarian crises: systematic review (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8128021/
   Systematic review including Iraq humanitarian settings.
##### Kurdish query results
8. **Decentralising healthcare for diabetes and hypertension from secondary to primary level in a humanitarian setting in Kurdistan, Iraq: a qualitative study (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11998334/
    Qualitative study on decentralisation of DM/HTN care in Duhok, KRI.

9. **TYPE Brief Research Report (Frontiers Public Health 2025) — KRG-DHIS2 facility-based data analysis** — https://public-pages-files-2025.frontiersin.org/journals/public-health/articles/10.3389/fpubh.2025.1649273/pdf
    First systematic analysis of facility-based public-health data via KRG-DHIS2 in Kurdistan Region of Iraq.

10. **Screening for Diabetes Mellitus and Hypertension in Duhok Governorate, Kurdistan Region of Iraq (JoCMS)** — https://jocms.org/index.php/jcms/article/view/1598
    Screening study in Duhok Governorate.


### Jamaica — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Jamaica faces a major NCD burden, with ~34.9% adult hypertension and ~12% diabetes prevalence. The Ministry of Health & Wellness has used DHIS2 to strengthen the National Cancer Registry (with CARPHA and HISP Rwanda), but no published evidence indicates DHIS2 is used for diabetes or hypertension surveillance or registries; NCD management appears to rely on facility-based clinic registers and national strategy documents.

DHIS2 USE: LIKELY
DHIS2 is documented in Jamaica for cancer registry purposes; explicit use for diabetes/hypertension/CVD surveillance was not found in available sources.

#### Search Results

##### English query results
1. **Hypertension and Diabetes in Jamaica: Burden, Correlates, and Global Context** — https://www.researchgate.net/publication/394929646_Hypertension_and_Diabetes_in_Jamaica_Burden_Correlates_and_Global_Context
   Reports adult hypertension prevalence 34.9% and diabetes 12.0%; women have higher prevalence than men.

2. **Jamaica National Strategic and Action Plan for the Prevention and Control of NCDs 2013-2018 (MoH)** — https://www.moh.gov.jm/wp-content/uploads/2015/05/National-Strategic-and-Action-Plan-for-the-Prevention-and-Control-Non-Communicable-Diseases-NCDS-in-Jamaica-2013-2018.pdf
   Government strategy for diabetes/hypertension/CVD prevention and control; references HMIS strengthening but does not name DHIS2 for NCDs.

3. **Consultancy to Develop Jamaica's Guidelines for the Management of Diabetes and Hypertension – MoH&W** — https://www.moh.gov.jm/tenders/consultancy-to-develop-jamaicas-guidelines-for-the-management-of-diabetes-and-hypertension/
   MoH tender for guideline development indicating ongoing reform of diabetes/hypertension protocols.

4. **Hypertension and Diabetes Prevalence in Older Persons in Jamaica, 2012 (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC4655690/
   Academic study on prevalence in older Jamaicans, used to inform NCD programming.


### Jordan — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Jordan has very high NCD burden: ~78% of deaths are NCD-related; adult hypertension ~52% and diabetes ~20% in 45-69 group. The MoH and WHO have implemented HEARTS for primary-care CVD risk management and have established NCD monitoring indicators embedded in existing HIS, but none of the located sources document DHIS2 specifically as Jordan's NCD information system.

DHIS2 USE: UNKNOWN
No evidence of DHIS2 in Jordan's NCD/cardiometabolic surveillance was found; Jordan uses national HIS infrastructure for HEARTS indicators.

#### Search Results

##### English query results
1. **WHO EMRO — Protecting lives, promoting well-being: Jordan's progress on NCDs** — https://www.emro.who.int/media/news/protecting-lives-promoting-well-being-jordans-progress-on-ncds-and-mental-health.html
   Describes NCD protocols for hypertension/diabetes/CRD established with capacity-building; NCD monitoring indicators embedded in HIS.

2. **The National Strategy and Plan of Action Against Diabetes, Hypertension (WHO NCDCCS)** — https://extranet.who.int/ncdccs/Data/JOR_B3_National-Strategy-NCD-Jordan.pdf
   Government strategic plan against diabetes, hypertension and other NCDs.

3. **Profiling Cardiometabolic Health in Jordan: A Call to Action (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10237340/
   Academic review profiling cardiometabolic health in Jordan; calls for stronger surveillance/CVD prevention.

4. **The prevalence of hypertension and its progression among patients with type 2 diabetes in Jordan (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/34917355/
   Clinical study on co-prevalence in Jordan.

5. **Rapidly adapted community health strategies for Syrian refugees and host population with HTN/DM in Jordan (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/36576492/
   COVID-era adaptations for chronic disease care in Jordan.

6. **HEARTS360: A gold-standard dashboard for hypertension and diabetes programs (Simple.org)** — https://www.simple.org/blog/hearts360-dashboard/
   HEARTS360 dashboard referenced as global tool; relevant to MoH-WHO HEARTS adoption in Jordan.


### Kazakhstan — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Kazakhstan operates a Unified National Electronic Health System (UNEHS) that supports diabetes and NCD surveillance via national electronic records, not DHIS2. Academic studies have used UNEHS to analyze type 1/2 diabetes prevalence, incidence and mortality (2014–2019). No sources indicate DHIS2 deployment in Kazakhstan.

DHIS2 USE: NONE
Kazakhstan uses its own Unified National Electronic Health System for diabetes and NCD data; DHIS2 is not the national HMIS.

#### Search Results

##### English query results
1. **Epidemiology of type 1 and type 2 diabetes mellitus in Kazakhstan: data from Unified National Electronic Health System 2014–2019 (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9650815/
   Uses national EHR (UNEHS) for six-year diabetes prevalence/incidence/mortality study — main NCD data backbone in Kazakhstan.

2. **HISP Centre and World Diabetes Foundation Partnership for NCDs** — https://dhis2.org/hisp-centre-world-diabetes-foundation-partnership/
   Global DHIS2-NCD partnership; Kazakhstan not listed as participating country.


### Kenya — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Kenya runs KHIS (Kenya Health Information System), a national DHIS2 platform deployed in 2011. NCD indicators (hypertension, diabetes) are reported into KHIS, and the SPICE platform (Medtronic LABS partnership with MoH) integrates primary-care NCD data into DHIS2 for aggregated monthly reporting. Indicators draw from the HEARTS Technical Package.

DHIS2 USE: CONFIRMED
KHIS is Kenya's national DHIS2 instance and ingests NCD/hypertension/diabetes data from SPICE and routine reporting.

#### Search Results

##### English query results
1. **KHIS Aggregate (MoH Kenya)** — https://hiskenya.org/ [BROKEN: unreachable]
   Kenya's national DHIS2 instance for aggregate routine reporting including NCD indicators.

2. **Integrating digital solutions into national health data systems through public–private collaboration: SPICE platform in Kenya (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10548793/
   SPICE-MoH partnership for primary-care HTN/DM services; data aggregated monthly into DHIS2/KHIS.

3. **Perceived accuracy and utilisation of DHIS2 data for health decision making and advocacy in Kenya (PLOS GPH)** — https://journals.plos.org/globalpublichealth/article?id=10.1371/journal.pgph.0004508
   Qualitative study on DHIS2 use for health decisions in Kenya.

4. **Perceived accuracy and utilisation of DHIS2 data for decision making in Kenya (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC12352824/
   Companion PMC version of the PLOS GPH study.

5. **Assessing the Readiness to Provide Integrated Management of CVD and Type 2 Diabetes in Kenya: National Survey (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10275139/
   National facility-readiness survey for integrated CVD/T2DM management in Kenya.


### Kiribiati — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Kiribati has heavy NCD burden, with hypertension ~17.3% and WHO PEN being implemented in 5 clinics in Tarawa with planned expansion. NCD monitoring relies on STEPS surveys and PEN-clinic-based registers; no evidence located that DHIS2 is used for diabetes/hypertension or CVD surveillance in Kiribati.

DHIS2 USE: UNKNOWN
No published source ties Kiribati's NCD/cardiometabolic programs to DHIS2; PEN is implemented at clinic level.

#### Search Results

##### English query results
1. **Kiribati NCD Risk Factors STEPS Report (FAO/WHO)** — https://www.fao.org/fileadmin/templates/agphome/documents/horticulture/WHO/fiji/steps/Kiribati_STEPS_Report.pdf
   Government NCD STEPS survey results including hypertension and diabetes prevalence.

2. **NCDs and DHIS2: Learning from Country Experiences (DHIS2 Community)** — https://community.dhis2.org/t/ncds-and-dhis2-learning-from-country-experiences/48128
   Forum discussion of country NCD experiences; Kiribati not mentioned.


### Kyrgyzstan — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
NCDs cause 83% of deaths in Kyrgyzstan. The country runs a national electronic primary health care (PHC) database and a State Register of Diabetes Patients (SRDP, since 2015) that drive NCD/diabetes surveillance — these are national systems, not DHIS2. WHO and World Diabetes Foundation are supporting NCD detection projects but published sources do not document DHIS2 deployment in Kyrgyzstan.

DHIS2 USE: NONE
Kyrgyzstan's NCD data flow runs through national PHC EHR and the State Register of Diabetes, not DHIS2.

#### Search Results

##### English query results
1. **National electronic primary health care database in monitoring performance of primary care in Kyrgyzstan (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8822322/
   WHO-supported feasibility assessment of national PHC EHR for NCD burden and quality-of-care monitoring.

2. **Kyrgyzstan: analysing data on diabetes to underpin better care — WHO Europe NCD stories** — https://www.who.int/europe/publications/i/item/WHO-EURO-2021-4146-43905-61825
   WHO case study on Kyrgyzstan diabetes data analytics.

3. **Role of the state register of diabetes mellitus in assessing the epidemiological situation in Kyrgyzstan and Bishkek (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/40411330/
   Describes the State Register of Diabetes Patients (SRDP), introduced 2015.

4. **Improving the care for patients with diabetic retinopathy in Kyrgyzstan (WHO Europe)** — https://www.who.int/europe/news/item/21-02-2023-improving-the-care-for-patients-with-diabetic-retinopathy-in-kyrgyzstan
   WHO/MoH initiative on diabetic retinopathy care.

5. **Addressing NCDs in Primary Healthcare in Kyrgyzstan: Knowledge and Behavioral Changes (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10353050/
   Population study on NCD awareness and behavior.

6. **HISP Centre and World Diabetes Foundation Partnership** — https://dhis2.org/hisp-centre-world-diabetes-foundation-partnership/
   Global DHIS2-NCD partnership; Kyrgyzstan not named as implementing country.


### Lao — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Lao PDR has implemented WHO PEN and HEARTS-aligned NCD interventions but no publicly available source documents DHIS2 specifically as the platform used for cardiometabolic surveillance. Lao does use DHIS2 for routine HMIS reporting more broadly, but its application to diabetes/hypertension registries was not directly confirmed in this search.

DHIS2 USE: UNKNOWN
DHIS2 is used in Lao for routine HMIS, but there is no direct documentation of cardiometabolic-disease modules in this search.

#### Search Results

##### English query results
1. **Integrating hypertension and diabetes management in primary health care settings: HEARTS as a tool (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9440730/
   Academic review of integrated HTN/DM via HEARTS in primary care.

2. **NCDs and DHIS2: Learning from Country Experiences (DHIS2 Community)** — https://community.dhis2.org/t/ncds-and-dhis2-learning-from-country-experiences/48128
   Forum thread on country NCD experiences using DHIS2.


### Lebanon — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Lebanon's MoPH has migrated disease surveillance to DHIS2 since 2014 (school-based) and expanded in 2017 to aggregate data from hospitals, dispensaries and laboratories, improving timeliness and completeness. MSF used DHIS2 data exports to manage 2,644 Syrian refugee patients with diabetes/hypertension in Shatila camp — providing direct evidence of DHIS2 supporting cardiometabolic care.

DHIS2 USE: CONFIRMED
Lebanon's MoPH operates DHIS2 for surveillance; MSF documented its use for diabetes/hypertension cohort data extraction in refugee NCD programs.

#### Search Results

##### English query results
1. **Converting the existing disease surveillance from paper to electronic DHIS-2 for real-time information: the Lebanese experience (ResearchGate)** — https://www.researchgate.net/publication/359479380_Converting_the_existing_disease_surveillance_from_a_paper-based_to_an_electronic-based_system_using_district_health_information_system_DHIS-2_for_real-time_information_the_Lebanese_experience_Open_Acc
   Describes MoPH migration to DHIS2 starting 2014 (school) and expanding 2017 to hospitals, medical centers, dispensaries, labs.

2. **Use of District Health Information System (DHIS-2) for Real Time Surveillance: Lebanon 2017 (iProc)** — https://www.iproc.org/2018/1/e10547/
   Conference proceedings on DHIS2 deployment timeline and outcomes in Lebanon.

3. **Treating Syrian refugees with diabetes and hypertension in Shatila refugee camp, Lebanon: MSF model of care (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC6444539/
   MSF cohort study of 2,644 HTN/DM patients; data extracted from DHIS2 for analysis.

4. **Patient experiences of diabetes and hypertension care during humanitarian crisis in Lebanon (PLOS GPH)** — https://journals.plos.org/globalpublichealth/article?id=10.1371%2Fjournal.pgph.0001383
   Qualitative study on HTN/DM care experiences in crisis context.

5. **Guidelines and mHealth to Improve Quality of HTN and T2DM Care for Vulnerable Populations in Lebanon: Longitudinal Cohort (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC5695979/
   mHealth-based HTN/DM quality improvement study in Lebanon.

6. **Heart health in Lebanon and considerations for addressing CVD burden (Academia)** — https://www.academia.edu/32949262/Heart_health_in_Lebanon_and_considerations_for_addressing_the_burden_of_cardiovascular_disease
   Country-level review of CVD burden and health system response.


### Lesotho — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Lesotho faces a double burden of NCDs alongside HIV/TB. Recent research (ComBaCaL trials) tested CHW-led home-based screening for diabetes and hypertension using a tablet-based clinical decision support app — not DHIS2. While Lesotho operates DHIS2 as its national HMIS for routine reporting, no source directly confirms a cardiometabolic-specific DHIS2 module.

DHIS2 USE: LIKELY
Lesotho has a national DHIS2/HMIS; routine NCD aggregate indicators likely flow through it, though no cardiometabolic-specific module/registry was confirmed in this search.

#### Search Results

##### English query results
1. **Feasibility and Acceptability of Diabetes and Hypertension Screening by CHWs in Rural Lesotho: Mixed-Methods Pilot (Annals of Global Health)** — https://annalsofglobalhealth.org/articles/10.5334/aogh.4738
   ComBaCaL pilot in Butha-Buthe and Mokhotlong using a tablet-based CDSS for CHW-led HTN/DM screening.

2. **Feasibility and Acceptability of CHW-led Screening in Lesotho (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC12439129/
   Companion PMC version of the ComBaCaL pilot.

3. **Awareness, treatment, and control among adults with hypertension or diabetes in two rural districts in Lesotho (PLOS GPH)** — https://journals.plos.org/globalpublichealth/article?id=10.1371/journal.pgph.0003721
   Population-level study of HTN/DM cascade of care in rural Lesotho.


### Liberia — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Liberia's MoH launched Project TREND in 2026 to tackle hypertension and diabetes, explicitly including the distribution of 900+ NCD-specific registers and improved reporting into the national DHIS2. Only 62 of ~900 facilities are currently trained in NCD care; the project will support CHW screening of 18,000+ people and integrate data into Liberia's DHIS2 HMIS.

DHIS2 USE: CONFIRMED
The MoH explicitly cites DHIS2 as the destination system for Project TREND NCD register reporting (hypertension and diabetes).

#### Search Results

##### English query results
1. **MoH Launches 'Project TREND' to Tackle Hypertension, Diabetes Burden (MoH Liberia)** — https://moh.gov.lr/news/2026/moh-launches-project-trend-to-tackle-hypertension-diabetes-burden/
   Official MoH announcement: 900+ NCD registers distributed, reporting integrated into national DHIS2.

2. **Ministry of Health, Republic of Liberia (DHIS2 community presentation)** — https://community.dhis2.org/uploads/short-url/by1wvHvf1nHdykWrngTnQVHvhaC.pdf
   MoH Liberia presentation on DHIS2 use.

3. **NCDs and DHIS2: Learning from Country Experiences (DHIS2 Community)** — https://community.dhis2.org/t/ncds-and-dhis2-learning-from-country-experiences/48128
   Forum thread covering DHIS2-NCD experiences across countries.

4. **HISP Centre and World Diabetes Foundation Partnership for NCDs** — https://dhis2.org/hisp-centre-world-diabetes-foundation-partnership/
   Global HISP-WDF partnership to expand DHIS2 NCD modules.


### Libya — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Libya has documented rising NCD burden — CVD, hypertension, diabetes and cancer drive mortality and morbidity. MoH and NCDC, with WHO support, completed an NCD STEPS survey (December 2023) showing ~25% adults reporting hypertension. NCD surveillance uses WHO STEPS methodology; no evidence located that DHIS2 is deployed in Libya for NCD data.

DHIS2 USE: UNKNOWN
No published evidence identified for DHIS2 use in Libya's cardiometabolic surveillance.

#### Search Results

##### English query results
1. **WHO EMRO — Libya releases STEPS survey results on NCDs** — https://www.emro.who.int/lby/libya-news/libya-releases-steps-survey-results-on-ncds.html
   2023 NCD STEPS survey results, 6,000 households, age 18-69; 25% reported hypertension.

2. **WHO EMRO — Libya implements NCD STEPS survey** — https://www.emro.who.int/lby/libya-news/libya-implements-ncd-steps-survey.html
   STEPS survey implementation announcement (MoH/NCDC/WHO).

3. **WHO EMRO — Noncommunicable diseases (Libya programmes)** — https://www.emro.who.int/lby/programmes/noncommunicable-diseases.html
   Libya NCD programme page; STEPS-based surveillance, no DHIS2 reference.

4. **Cross-sectional pilot study about the health status of diabetic patients in Misurata, Libya (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/23066426/
   Clinical pilot study on diabetic patient health status in Libya.

5. **HISP Centre and World Diabetes Foundation Partnership** — https://dhis2.org/hisp-centre-world-diabetes-foundation-partnership/
   Global DHIS2-NCD partnership; Libya not named.


### Madagascar — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Madagascar's Ministry of Public Health operates DHIS2 as its core HMIS, with 58 District Health Services and 1,687 facilities reporting via tablet-based data collection. A 2025 HISP UiO–MSANP partnership launched a One Health/laboratory information initiative. However, available sources do not explicitly confirm a DHIS2 cardiometabolic (diabetes/hypertension) module; NCD coordination is described as insufficient in national strategic plans.

DHIS2 USE: LIKELY
DHIS2 is the national HMIS and almost certainly carries aggregate NCD/HTN/DM indicators, but a dedicated cardiometabolic module was not confirmed in this search.

#### Search Results

##### French query results
1. **Santé – DHIS2 (French)** — https://dhis2.org/fr/health/
   DHIS2 health portal (French) describing global use cases including NCDs.

2. **Madagascar et le HISP UiO s'associent pour lancer l'initiative numérique One Health et SIL** — https://dhis2.org/fr/madagascar-hisp-uio-one-health-initiative/
   Government-HISP UiO partnership announcement for One Health and lab information systems on DHIS2.

3. **Ministère de la Santé Publique — Plan Stratégique de Renforcement du Système d'Information Sanitaire (PSRSIS 2023-2027)** — https://www.msanp.gov.mg/upload/documents/PSRSIS_2023-2027.pdf
   Government HIS strengthening plan referencing DHIS2 as core HMIS.

4. **Documentation MSANP DHIS-2 API** — https://doc.dhis2.pivot-dashboard.org/overview.html [BROKEN: unreachable]
   Technical documentation for Madagascar MoH DHIS2 API/dashboards.

5. **Madagascar: Travailleurs - Santé - Diabète et hypertension gagnent du terrain (allAfrica)** — https://fr.allafrica.com/stories/202605110904.html
   News piece on rising diabetes/hypertension burden in Madagascar.

6. **Mis en place d'un système de suivi via DHIS2 tracker et Event (maternité sans risque, Madagascar) — DHIS2 Community** — https://community.dhis2.org/t/mis-en-place-d-un-systeme-de-suivi-des-programmes-au-niveau-de-la-division-maternite-sans-risque-a-travers-dhis2-tracker-et-event-cas-de-madagascar/58817
   Example of DHIS2 Tracker use in Madagascar (maternal health) — proves Tracker capability in country.
##### English query results
7. **MSANP — Ministère de la Santé Publique Madagascar** — https://ministere-sante.mg/
   Official MoH portal.

8. **Plan de développement du secteur santé 2020-2024 (Madagascar)** — https://scorecard.prb.org/wp-content/uploads/2022/03/Plan-de-de%CC%81veloppement-du-secteur-sante%CC%81-2020-2024..pdf
   Sector plan noting NCD coordination gaps.


### Malawi — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Malawi runs an integrated NCD program built on WHO PEN and PEN-Plus, with hypertension/diabetes screening and treatment at primary care and decentralized severe-NCD care at district level. The Ministry of Health, with the World Diabetes Foundation and HISP, has added an NCD module to DHIS2 (deployed March 2024) under the Diabetes Compass project, including a community screening algorithm linked to referral.

DHIS2 USE: CONFIRMED
DHIS2 is the national HMIS and an NCD module digitizing the government's NCD data tool was deployed in 2024; Malawi is a Diabetes Compass demonstration country for community-based diabetes/hypertension screening.

#### Search Results

##### English query results
1. **Fighting NCDs in Malawi and Sri Lanka with the Diabetes Compass Project** — https://dhis2.org/fighting-ncds-in-malawi-and-sri-lanka-with-the-diabetes-compass-project/
   Describes the MoH Community Health Services NCD module on DHIS2, deployed March 2024, and the WDF screening algorithm for hypertension/diabetes risk and referral. (Note: dhis2.org source — retained for context only.)

2. **Malawi NCD Guidelines 2022** — https://www.differentiatedservicedelivery.org/wp-content/uploads/NCD-guidelines-2022.pdf
   National NCD clinical guidelines covering hypertension, type 2 diabetes, chronic respiratory disease and the Integrated Chronic Care Clinic (IC3) model.

3. **DHIS2 Research Data — Malawi** — https://data.research.dhis2.org/Malawi
   Open dataset reflecting Malawi DHIS2 routine data, including NCD-relevant indicators.

4. **Training Mid-Level Providers to Treat Severe NCDs in Neno, Malawi through PEN-Plus** — https://annalsofglobalhealth.org/articles/10.5334/aogh.3750
   Annals of Global Health study describing the PEN-Plus model at Neno, including patient registry and follow-up systems.

5. **Integrating diabetes and hypertension control in the NCD platform, Malawi (WDF18-1588)** — https://www.worlddiabetesfoundation.org/what-we-do/projects/wdf18-1588/
   World Diabetes Foundation project page detailing scale-up of integrated hypertension/diabetes care within Malawi's NCD platform.

6. **Accessing clinical services and retention in care following screening for hypertension and diabetes among Malawian adults** — https://pubmed.ncbi.nlm.nih.gov/27552644/
   PubMed study on urban/rural retention after community screening — context for routine data needs.

7. **Diabetes and hypertension control in Malawi's NCD response, WDF18-1588** — https://www.worlddiabetesfoundation.org/projects/malawi-wdf18-1588 [BROKEN: 404]
   Project summary on integration of D/H care into Malawi's NCD response.


### Maldives — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
The Maldives has high NCD burden (CVD, diabetes, hypertension are leading causes of mortality) and runs WHO STEPS surveys and a PEN-aligned primary care program. Public sources do not document a national DHIS2 deployment for routine HMIS or NCD surveillance in the Maldives; the country has historically used its own HMIS managed by the Health Protection Agency.

DHIS2 USE: UNKNOWN
No specific evidence of DHIS2 implementation in the Maldives for cardiometabolic surveillance was identified; available sources only describe DHIS2 NCD use in other countries.

#### Search Results

##### English query results
1. **Health Data Toolkit — DHIS2** — https://dhis2.org/health-data-toolkit/
   Generic toolkit including standardized metadata, Tracker modules, dashboards for diabetes, hypertension, asthma, CRD. (No Maldives-specific deployment mentioned.)

2. **Hypertension Registry System Design (DHIS2 docs)** — https://docs.dhis2.org/en/implement/health/non-communicable-diseases/hypertension-control/design.html
   Reference design for hypertension registry that can be extended to diabetes/CVD.

3. **NCDs and DHIS2: Learning from Country Experiences** — https://community.dhis2.org/t/ncds-and-dhis2-learning-from-country-experiences/48128
   Community forum thread on NCD implementations; Maldives not cited.

4. **HISP Centre and World Diabetes Foundation Partnership** — https://dhis2.org/hisp-centre-world-diabetes-foundation-partnership/
   Four-year partnership to strengthen NCD data in DHIS2; Maldives not listed among demonstration countries.

5. **Integrating hypertension data: CARDIO4Cities and DHIS2 in Dakar, Senegal** — https://link.springer.com/article/10.1007/s44250-025-00240-8
   Case study from Senegal; relevant for comparable small-system designs.


### Mali — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Mali deployed DHIS2 as its national health information system (SNISS/SLIS) from 2015, officially launched in 2017, covering 100% of hospitals and ~78% of community health centers. DHIS2 supports epidemiological surveillance including chronic-disease MADO reporting. Specific NCD-program use (HEARTS/PEN cardiometabolic registries on DHIS2) is not strongly documented in the available evidence.

DHIS2 USE: CONFIRMED
DHIS2 is Mali's national routine HMIS used for epidemiological surveillance; explicit cardiometabolic registry/HEARTS module deployment on DHIS2 is not clearly evidenced in public sources.

#### Search Results

##### French query results
1. **L'expérience du Mali dans le déploiement du DHIS2 — MEASURE Evaluation** — https://www.measureevaluation.org/resources/publications/tr-20-407-fr.html
   National DHIS2 deployment history; covers integration of multiple programs into SNISS.

2. **Système d'information sanitaire : LE DHIS2 est lancé — Santé Tropicale** — https://www.santetropicale.com/actus.asp?id=23630&action=lire
   2017 national launch coverage.

3. **Système Local d'Information Sanitaire — SLIS, MoH Mali** — http://www.sante.gov.ml/index.php/annuaires/category/4-systeme-local-d-information-sanitaire-slis
   Mali MoH SLIS pages.

4. **Les acquis et défis du système DHIS2 au Mali (Diarra, 2021)** — https://www.amazon.com/acquis-d%C3%A9fis-syst%C3%A8me-DHIS2-Mali/dp/6203448532
   Monograph on Mali's DHIS2 epidemiological surveillance.

5. **Collaboration des parties prenantes — DHIS2 au Mali (MEASURE Evaluation)** — https://www.measureevaluation.org/resources/publications/gr-20-110-fr.html
   Implementation history showing integration of health programs.

6. **Evaluation des connaissances et pratiques chez les diabétiques de type 2 à l'hôpital du Mali en 2024** — https://revue-rasp.org/rasp/article/view/877
   Recent diabetes study at Hôpital du Mali (context — does not specifically describe DHIS2 use).

7. **Mali met en œuvre un eSIGL intégré s'appuyant sur DHIS2 et MEDEXIS** — https://dhis2.org/fr/mali-integrated-logistics-2/
   Confirms ongoing DHIS2 expansion (logistics). (dhis2.org — for context.)
##### English query results
8. **DHIS2 Research Data / global toolkit** — referenced via Health Data Toolkit and HISP-WDF partnership pages.


### Mauritania — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Mauritania's HMIS is listed in regional aliases as DHIS2 but the available open-source/academic literature does not describe specific NCD or cardiometabolic surveillance modules on DHIS2 in country. The country participates in WHO STEPS surveillance for NCD risk factors.

DHIS2 USE: LIKELY
DHIS2 is the indicated national HMIS, but specific cardiometabolic registry/HEARTS-on-DHIS2 implementation could not be confirmed from publicly indexed sources.

#### Search Results

##### English query results
1. **Health Data Toolkit — DHIS2** — https://dhis2.org/health-data-toolkit/
   Reference toolkit for diabetes/hypertension/asthma/CRD (no Mauritania-specific case).

2. **HISP Centre and WDF Partnership** — https://dhis2.org/hisp-centre-world-diabetes-foundation-partnership/
   Multi-country NCD-DHIS2 partnership (Mauritania not listed).

3. **NCDs and DHIS2: Learning from Country Experiences** — https://community.dhis2.org/t/ncds-and-dhis2-learning-from-country-experiences/48128
   Community thread; Mauritania not mentioned.

4. **CARDIO4Cities and DHIS2 in Dakar, Senegal** — https://link.springer.com/article/10.1007/s44250-025-00240-8
   Regional (Senegal) case study; relevant for francophone West Africa.


### Mauritius — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Mauritius has one of the world's highest burdens of type 2 diabetes, hypertension, and CVD, monitored through periodic national NCD surveys (2004, 2009, 2015, 2021) and STEPS surveillance. The country's national health information infrastructure has historically been the Health Information Bureau; there is no public evidence of national DHIS2 use for cardiometabolic surveillance.

DHIS2 USE: NONE
No documented DHIS2 deployment for NCD surveillance in Mauritius; the country relies on dedicated NCD surveys and its own HIS.

#### Search Results

##### English query results
1. **Hypertension Registry System Design — DHIS2 docs** — https://docs.dhis2.org/en/implement/health/non-communicable-diseases/hypertension-control/design.html
   Generic DHIS2 design (no Mauritius implementation).

2. **Health Data Toolkit — DHIS2** — https://dhis2.org/health-data-toolkit/
   Generic toolkit.

3. **Mauritius NCD Survey 2015 Report** — https://www.cidp-cro.com/wp-content/uploads/2021/12/Mauritius-NCD-Survey-2015-Report.pdf
   National NCD survey covering diabetes, hypertension, CVD prevalence and risk factors.

4. **Mauritius MoH STEPS Report (2004)** — https://cdn.who.int/media/docs/default-source/ncds/ncd-surveillance/data-reporting/mauritius/steps/2004-steps-report-mauritius.pdf
   Earlier STEPS-style NCD risk factor survey.

5. **Prevalence and risk factors of hypertension in Mauritius (PMC, 2024)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11637371/
   Recent cross-sectional analysis of hypertension; uses survey data, not routine DHIS2.

6. **Non-communicable Diseases — DHIS2 Documentation** — https://docs.dhis2.org/en/implement/health/chis-community-health-information-system/design/non-communicable-diseases.html
   Reference design.


### Mongolia — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Mongolia has implemented "MongPEN" since 2019 (a Mongolian adaptation of WHO PEN + HEARTS for CVD risk assessment) and an ePrescription/eHealth data system for screening and management at primary care. Public sources do not identify DHIS2 as the underlying platform; Mongolia's national HIS is largely homegrown/integrated with its electronic medical record and ePrescription infrastructure.

DHIS2 USE: UNKNOWN
No specific evidence that Mongolia uses DHIS2 for routine cardiometabolic surveillance; HEARTS/PEN data flow appears to be through national eHealth tools rather than DHIS2.

#### Search Results

##### English query results
1. **Mongolia: crafting essential country-specific tools to tackle NCDs (WHO)** — https://www.who.int/news-room/feature-stories/detail/mongolia-essential-country-specific-tools-to-tackle-ncds
   WHO feature on MongPEN launch (2019), HEARTS integration, and use of ePrescription for CVD risk data capture in Darkhan-Uul and Songinokhairkhan.

2. **Health Data Toolkit — DHIS2** — https://dhis2.org/health-data-toolkit/
   Generic toolkit (no Mongolia case).

3. **HISP Centre and WDF Partnership** — https://dhis2.org/hisp-centre-world-diabetes-foundation-partnership/
   NCD-DHIS2 partnership (Mongolia not listed).

4. **Hypertension Registry System Design — DHIS2 docs** — https://docs.dhis2.org/en/implement/health/non-communicable-diseases/hypertension-control/design.html
   Reference design.


### Morocco — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Morocco's NCD burden is dominated by CVD (38% of deaths) with high diabetes and hypertension prevalence documented through STEPS 2017. The Ministry of Health monitors NCDs through the STEPS framework and is exploring digital tools (telemedicine, IoT, AI) for chronic-disease management. There is no public evidence of national DHIS2 deployment for cardiometabolic surveillance in Morocco.

DHIS2 USE: NONE
Morocco's HMIS is not DHIS2-based for cardiometabolic surveillance; national NCD data flows through STEPS surveys and the MoH's own systems.

#### Search Results

##### English query results
1. **Prevalence and factors associated with diabetes, hypertension, and ischemic heart disease/stroke multimorbidity in Morocco: STEPS 2017** — https://www.populationmedicine.eu/Prevalence-and-factors-associated-with-diabetes-hypertension-and-ischemic-heart-disease,187076,0,2.html
   National STEPS data on cardiometabolic multimorbidity.

2. **Examining NCDs in Morocco: A Close Look at Cardiovascular Health (Brown UJPH, 2024)** — https://sites.brown.edu/publichealthjournal/2024/03/17/examining-non-communicable-diseases-in-morocco-a-close-look-at-cardiovascular-health/
   Overview of CVD burden and surveillance.

3. **Hypertension Registry System Design — DHIS2 docs** — https://docs.dhis2.org/en/implement/health/non-communicable-diseases/hypertension-control/design.html
   Generic reference.

4. **Integrating hypertension data into routine health data collection: CARDIO4Cities and DHIS2 in Dakar, Senegal** — https://link.springer.com/article/10.1007/s44250-025-00240-8
   Regional francophone case study (not Morocco).

5. **Digital Healthcare Innovation in Morocco: Telemedicine, IoMT, AI for Chronic Disease Management (MDPI, 2024)** — https://www.mdpi.com/2673-7426/6/2/22
   Reviews Morocco's digital chronic-disease ecosystem (no DHIS2 mentioned).

6. **Morocco — STEPS 2017 (WHO microdata)** — https://extranet.who.int/ncdsmicrodata/index.php/catalog/544
   National STEPS dataset.

7. **Epidemiology of Cardiovascular Diseases in Morocco: A Systematic Review (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9380084/
   Systematic review of CVD epidemiology.

8. **Prevalence and correlates of type 2 diabetes among adults in Morocco, 2017 (Nature Sci Rep)** — https://www.nature.com/articles/s41598-022-20368-4
   National diabetes prevalence study.


### Mozambique — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Mozambique's national HMIS is SISMA, the country's DHIS2 instance, used across health programs. The MoH has developed diabetes and hypertension projects and undertakes periodic STEPS surveys. Facility-based NCD surveillance pilots (Maputo) identified hypertension, diabetes, stroke, CRD, mental illness and cancers as priority conditions. Specific cardiometabolic registry/HEARTS metadata on SISMA is not explicitly documented in the indexed literature.

DHIS2 USE: CONFIRMED
SISMA is Mozambique's DHIS2-based HMIS and supports routine reporting; explicit cardiometabolic-specific tracker/registry deployment is not strongly evidenced in public sources but routine NCD aggregate reporting is plausible.

#### Search Results

##### Portuguese query results
1. **Mozambique Country Report (World Heart Federation, Cardiovascular Journal of Africa, 2020)** — https://world-heart-federation.org/wp-content/uploads/Mozambique-Country-Report.pdf
   Country report on CVD burden and surveillance approach.
##### English query results
2. **Mozambique Country Report (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9562838/
   Peer-reviewed country profile on CVD burden and response.

3. **Incorporating selected NCDs into facility-based surveillance systems from a resource-limited setting in Africa (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/30717732/
   Maputo hospital pilot integrating hypertension, diabetes, stroke, CRD, mental illness, cancers into facility surveillance.

4. **Non-communicable diseases in Mozambique: risk factors, burden, response and outcomes (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC3539877/
   Comprehensive NCD response review.

5. **Assessment of care provision for hypertension at an Urban Hospital in Mozambique (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/31852481/
   Care delivery study on hypertension management.

6. **NCDs and DHIS2: Learning from Country Experiences** — https://community.dhis2.org/t/ncds-and-dhis2-learning-from-country-experiences/48128
   Community discussion (Mozambique briefly referenced through SISMA).


### Myanmar — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Myanmar rolled out WHO PEN in primary care from 2017, reaching 232 of 330 townships by 2020 and screening 1.67 million people in 2019 (205,495 diabetes and 429,400 hypertension cases treated). A DHIS2 Tracker application has been deployed under the SUNI-SEA project to identify clusters of pre-diabetes, diabetes and hypertension across 75 villages, with quarterly analysis for program use.

DHIS2 USE: CONFIRMED
DHIS2 Tracker is used for community NCD screening data under SUNI-SEA, and Myanmar's PEN program data infrastructure aligns with HEARTS indicators; national HMIS aliases include DHIS2.

#### Search Results

##### English query results
1. **Experiences from the pilot implementation of the WHO PEN in Myanmar, 2017-18 (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7028297/
   Mixed-methods evaluation of PEN pilot in Myanmar primary care.

2. **Myanmar Public Health Situation Analysis 2021 (WHO)** — https://healthcluster.who.int/docs/librariesprovider16/meeting-reports/myanmar-public-health-situation-analysis-29-may-2021.pdf
   National public health context including NCD programs.

3. **Self-care screening application for Myanmar people: DHIS2 tracker software on Google Play (SUNI-SEA)** — https://www.suni-sea.org/en/articles/self-care-screening-application-for-myanmar-people-dhis2-tracker-software-on-google-play-store/ [BROKEN: unreachable]
   DHIS2 Tracker mobile application for community screening of pre-diabetes, diabetes, hypertension; 75 villages across three regions.

4. **Challenges and Opportunities in Digital Screening for Hypertension and Diabetes in Community Groups of Older Adults in Vietnam (JMIR, 2024)** — https://www.jmir.org/2024/1/e54127
   Regional methodological reference (relevant to SUNI-SEA implementation approach used in Myanmar).

5. **Hypertension Registry System Design — DHIS2 docs** — https://docs.dhis2.org/en/implement/health/non-communicable-diseases/hypertension-control/design.html
   Reference design.

6. **NCDs and DHIS2: Learning from Country Experiences** — https://community.dhis2.org/t/ncds-and-dhis2-learning-from-country-experiences/48128
   Community thread.

7. **Experiences from the pilot implementation of WHO PEN in Myanmar (PLOS ONE)** — https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0229081
   Peer-reviewed PEN pilot evaluation.

8. **WHO Surveillance of NCDs in Myanmar** — https://www.who.int/teams/noncommunicable-diseases/surveillance/data/myanmar
   National STEPS and NCD surveillance data.


### Namibia — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Namibia uses DHIS2 as its national HMIS (rolled out through MoHSS with MEASURE Evaluation support). The country's National Health Policy Framework 2010–2020 prioritizes NCD risk factor surveillance and screening. There is no explicit public documentation of a Namibia-specific DHIS2 cardiometabolic registry or HEARTS package, though aggregate NCD reporting via DHIS2 is plausible.

DHIS2 USE: LIKELY
DHIS2 is Namibia's national routine HMIS; cardiometabolic-specific tracker/registry deployment is not separately documented in indexed sources.

#### Search Results

##### English query results
1. **Namibia: HIS Indicators — MEASURE Evaluation** — https://www.measureevaluation.org/his-strengthening-resource-center/country-profiles-1/namibia.html
   National HIS profile (Namibia uses DHIS2 for HMIS).

2. **Namibia Country Report — Cardiovascular Journal of Africa (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9562805/
   National CVD response and surveillance overview.

3. **Prevalence and predictors of hypertension in Namibia: A national-level cross-sectional study (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC6147578/
   National hypertension prevalence analysis.

4. **Hypertension Registry System Design — DHIS2 docs** — https://docs.dhis2.org/en/implement/health/non-communicable-diseases/hypertension-control/design.html
   Reference design.

5. **HISP Centre and WDF Partnership** — https://dhis2.org/hisp-centre-world-diabetes-foundation-partnership/
   Multi-country NCD-DHIS2 partnership (Namibia not specifically listed).

6. **NCDs and DHIS2: Learning from Country Experiences** — https://community.dhis2.org/t/ncds-and-dhis2-learning-from-country-experiences/48128
   Community thread.

7. **CARDIO4Cities and DHIS2 in Dakar, Senegal** — https://link.springer.com/article/10.1007/s44250-025-00240-8
   Regional reference case.


### Nepal — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Nepal endorsed WHO PEN under the EDCD/Department of Health Services and is scaling to all 77 districts, with a PEN-Plus initiative for severe NCDs at provincial hospitals in six districts. AMPATH Nepal is actively adapting the DHIS2 platform for hypertension and diabetes care, with dashboards for screening coverage, follow-up, and outcomes. Indicators align with the HEARTS technical package.

DHIS2 USE: CONFIRMED
DHIS2 is used for cardiometabolic program M&E by AMPATH Nepal (hypertension and diabetes screening, follow-up, outcomes) within the WHO-PEN framework, in addition to use as the national HMIS.

#### Search Results

##### English query results
1. **AMPATH Nepal's Community-Based Approach to Care** — https://www.ampathnepal.org/news-blog-feed/ampath-nepals-community-based-approach-to-care-meets-people-where-they-live
   Describes adaptation of DHIS2 for hypertension/diabetes screening, follow-up, and outcomes dashboards.

2. **Integration of NCD Screening, Management and Care Continuity Through PHC Using PEN in Nepal — NHRC** — https://nhrc.gov.np/projects/integration-of-ncd-screening-management-and-care-continuity-through-primary-health-care-using-pen-programme-in-nepal-assessing-practices-and-hurdles/
   National research on PEN integration.

3. **Bringing severe NCD care to district-level hospitals in Nepal: PEN-Plus (PMC, 2025)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC13084865/
   PEN-Plus implementation and policy implications.

4. **Need for HTA supported risk factor screening for hypertension and diabetes in Nepal (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9376353/
   Systematic scoping review.

5. **Hypertension Registry System Design — DHIS2 docs** — https://docs.dhis2.org/en/implement/health/non-communicable-diseases/hypertension-control/design.html
   Reference design.

6. **Tackling cardiovascular health and disease in Nepal: epidemiology and strategies (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC4898564/
   National CVD epidemiology overview.

7. **PEN-Plus Project — Kathmandu Institute of Child Health (KIOCH)** — https://kioch.org.np/projects/pen-plus-project/
   PEN-Plus implementation in Nepal.


### Nicaragua — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Nicaragua participates in the regional HEARTS in the Americas initiative led by PAHO, which uses DHIS2 as the platform for aggregate CVD outcome/process/structural indicators across participating countries. Nicaragua has a high adult type 2 diabetes prevalence (~10%). DHIS2 is therefore used at least within HEARTS for cardiometabolic indicator reporting; broader national HMIS adoption is less documented.

DHIS2 USE: CONFIRMED
DHIS2 is the chosen platform for HEARTS in the Americas aggregate reporting (which Nicaragua participates in), but country-level confirmation of routine cardiometabolic data flow through Nicaragua's national HMIS is limited in indexed sources.

#### Search Results

##### Spanish query results
1. **HEARTS como herramienta para integrar el manejo de la hipertensión y la diabetes — SciELO** — https://www.scielosp.org/article/rpsp/2022.v46/e213/es/
   PAHO regional paper on HEARTS as a tool integrating hypertension and diabetes management.

2. **HEARTS como herramienta para integrar el manejo de la hipertensión y la diabetes — PMC (Spanish)** — https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9673610/
   Mirrored Spanish-language PMC version.

3. **Iniciativa Centroamericana de Diabetes (CAMDI) — Nicaragua** — https://extranet.who.int/ncdccs/Data/NIC_C7_nic_C5_Encuesta_CAMDI_Nicaragua.pdf
   CAMDI diabetes survey for Nicaragua (WHO-hosted).
##### English query results
4. **Monitoring and evaluation platform for HEARTS in the Americas (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10202337/
   Documents DHIS2 chosen as platform for HEARTS aggregate CVD indicators in the Americas, including from primary health care facilities.

5. **Consensus document on management of type 2 diabetes and heart failure (CIFACAH/IASC, PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10665010/
   Regional clinical consensus.

6. **Integrating hypertension and diabetes management in PHC: HEARTS as a tool (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9673610/
   English version of HEARTS integration paper.

7. **Evaluating WHO HEARTS for Hypertension and Diabetes: Pilot in Guatemala (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11483012/
   Regional pilot (relevant comparator).

8. **HEARTS in the Americas: targeting health system change (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10924616/
   Overview of HEARTS rollout across 22 countries in the region.


### Niger — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Niger operates a national DHIS2 instance (dhisniger.ne) as the backbone of its SNIS (Système National d'Information Sanitaire). The MoH, supported by WHO and partners (Askaan Santé, World Diabetes Foundation), is rolling out WHO-PEN and HEARTS packages for diabetes and hypertension at primary care level, with WHO-PEN active in 9 of 72 districts by 2023. While the national DHIS2 is well established for routine HMIS, explicit documentation of cardiometabolic registries or Tracker programs in Niger's DHIS2 was not directly found.

DHIS2 USE: MODERATE
National DHIS2 (SNIS) is confirmed; aggregate NCD reporting via DHIS2 is plausible given the PEN rollout, but no public source explicitly documents a diabetes/hypertension Tracker or registry implementation in Niger.

#### Search Results

##### French query results
1. **Health Topics (Niger) | OMS Afro** — https://www.afro.who.int/fr/countries/niger/topic/health-topics-niger
   WHO supports Niger in strengthening early diagnosis, screening and treatment of NCDs (hypertension, diabetes, female cancers) at PHC using WHO-PEN and HEARTS; PEN implementation in 9/72 districts by 2023.

2. **DHIS2 Niger (instance)** — https://dhisniger.ne/
   Official national DHIS2 instance of Niger's health information system.

3. **Formations DHIS2 — SNIS Niger** — https://snis.ne/formations-dhis2/
   Niger's SNIS portal with training and resources on the national DHIS2 platform.

4. **Carte sanitaire du Niger — Dataviz** — https://www.cartesanitaireniger.org/
   National health map and data visualization platform fed by SNIS/DHIS2 data.

5. **Accueil — SNIS Niger** — https://snis.ne/
   Notes ongoing challenges in data production, quality and analysis despite DHIS2 reform creating a single integrated data system.

6. **Optimiser la prise en charge du diabète et de l'hypertension (Niger) — Askaan Santé** — https://askaan.org/en/nos-actions-vaccination-sante-publique-mnt-dff/optimiser-la-prise-en-charge-du-diabete-et-de-lhypertension-niger/
   Project supporting Niger MoH on NCD diagnosis and management, focused on hypertension and diabetes.
##### English query results
7. **HISP Centre and World Diabetes Foundation Partnership** — https://dhis2.org/hisp-centre-world-diabetes-foundation-partnership/
   Multi-country initiative strengthening MoH capacity to use DHIS2 for diabetes, hypertension, asthma and CRD (regional, includes francophone West Africa partners).

8. **DHIS2 Hypertension Registry System Design** — https://docs.dhis2.org/en/implement/health/non-communicable-diseases/hypertension-control/design.html
   Reference design for the DHIS2 hypertension control package potentially deployable in Niger.
##### Hausa query results
9. **Ciwon Suga (Diabetes) — WikiHausa** — https://wikihausa.com.ng/ciwon-suga-diabetes/
    General Hausa-language consumer health information on diabetes; no DHIS2-specific Niger results returned in Hausa.


### Nigeria — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Nigeria is one of the strongest and best-documented examples of DHIS2 use for cardiometabolic disease management. The Federal Ministry of Health, with WHO Nigeria, Resolve to Save Lives, and the University of Oslo, developed and deployed a DHIS2 hypertension/diabetes program (v2) and a DHIS2 Android Tracker app for primary care, used to manage the Nigeria Hypertension Control Initiative (NHCI) in Kano, Ogun, and FCT Abuja. Primary health centers report vital signs and BP-lowering medications into DHIS2, and the HEARTS360 dashboard monitors WHO HEARTS indicators.

DHIS2 USE: CONFIRMED
Nigeria's NHCI uses DHIS2 Tracker for individual hypertension and diabetes patient management at primary care, with aggregate program monitoring via HEARTS360 dashboards. This is documented in peer-reviewed literature and Resolve to Save Lives implementation materials.

#### Search Results

##### English query results
1. **Analysis of costs in implementing the HEARTS hypertension program in Nigerian primary care** — https://resource-allocation.biomedcentral.com/articles/10.1186/s12962-025-00626-8
   2025 BMC Cost Effectiveness paper analyzing implementation costs of the HEARTS hypertension program in Nigerian primary care under NHCI.

2. **Analysis of costs in implementing the HEARTS hypertension program in Nigerian primary care (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC12117767/
   Full-text PMC version of the cost-effectiveness analysis covering DHIS2-supported HEARTS rollout in Nigeria.

3. **Implementation of Global Hearts Hypertension Control Programs in 32 LMICs (JACC International)** — https://www.sciencedirect.com/science/article/pii/S0735109723066330
   Multi-country review noting Nigeria's use of a mixed paper/electronic HIS including DHIS2 Tracker for hypertension under Global Hearts.

4. **HEARTS360: A gold-standard dashboard for hypertension and diabetes programs (Simple.org)** — https://www.simple.org/blog/hearts360-dashboard/
   Describes HEARTS360 DHIS2 dashboard used by NCD program managers including in Nigeria.

5. **NCT07351162 clinical trial protocol (HEARTS-related, Jan 2026)** — https://cdn.clinicaltrials.gov/large-docs/62/NCT07351162/Prot_000.pdf
   Protocol referencing DHIS2-based hypertension monitoring infrastructure.


### Norway — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Norway is the development home of DHIS2 (HISP Centre at the University of Oslo), but the country itself does not use DHIS2 for routine clinical management of diabetes, hypertension, or CVD. National cardiometabolic surveillance is run through dedicated registries such as the Norwegian Diabetes Register for Adults (NDR-A), HUNT, and the national MSIS system. DHIS2 was adapted in Norway only for COVID-19 contact tracing (FiksCT) by KS in collaboration with WHO, UiO, and NIPH.

DHIS2 USE: NONE
Norway's diabetes, hypertension, and CVD surveillance uses national registries and EHRs, not DHIS2. DHIS2 in Norway has been used domestically only for COVID-19 contact tracing, not for cardiometabolic disease.

#### Search Results

##### Norwegian query results
1. **The Norwegian Diabetes Register for Adults – an overview of the first years** — https://www.researchgate.net/publication/289461836_The_Norwegian_Diabetes_Register_for_Adults_-_an_overview_of_the_first_years
   Overview of NDR-A, Norway's national diabetes registry; uses dedicated registry infrastructure, not DHIS2.

2. **The total prevalence of diagnosed diabetes and the quality of diabetes care in Salten, Norway (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8873303/
   Regional analysis of diabetes prevalence and care quality in Norway.

3. **Risk factor management of type 2 diabetic patients in primary care in the Scandinavian countries 2003–2015** — https://www.sciencedirect.com/science/article/pii/S1751991820302837
   Scandinavian comparative study on T2D primary care management; uses national EHR/registry data.
##### English query results
4. **HISP Centre, University of Oslo** — https://www.mn.uio.no/hisp/english/
   HISP Centre coordinates global DHIS2 development; Norway hosts the core team but does not deploy DHIS2 domestically for chronic disease.

5. **Exploring Design and Innovation related to DHIS2 for COVID-19 surveillance in Norway (HISP Centre)** — https://www.mn.uio.no/hisp/english/dhis2-design-lab/projects/covid-19-norway.html
   Documents Norway's only domestic DHIS2 use case: COVID-19 contact tracing via FiksCT.

6. **Does integration with national registers improve the data completeness of local COVID-19 contact tracing tools? (BMC HSR)** — https://bmchealthservres.biomedcentral.com/articles/10.1186/s12913-023-10540-5
   Norwegian study on DHIS2-based FiksCT integration with MSIS — COVID-19 specific, not cardiometabolic.

7. **National diabetes registries: do they make a difference? (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7907019/
   Review of national diabetes registries including Norway's; no DHIS2 involvement.

8. **DHIS2 — Wikipedia** — https://en.wikipedia.org/wiki/DHIS2
   Background on DHIS2 development at UiO; software localized into Norwegian but not used clinically in Norway.


### Pakistan — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Pakistan has a high cardiometabolic disease burden and has conducted WHO STEPS surveys, but the search did not surface direct evidence of DHIS2 being used for routine diabetes, hypertension, or CVD program management at national scale. Pakistan's HMIS landscape is mixed (DHIS2 is used in some provinces for routine health data — e.g., Punjab, Sindh — but cardiometabolic-specific programs were not documented in this search).

DHIS2 USE: UNCLEAR
DHIS2 is deployed in Pakistan for general HMIS purposes in several provinces, but no English-language search results confirm DHIS2 is currently used for hypertension, diabetes, or HEARTS/PEN programs.

#### Search Results

##### English query results
1. **WHO EMRO — Burden of noncommunicable diseases in Pakistan** — https://www.emro.who.int/emhj-volume-28-2022/volume-28-issue-11/burden-of-noncommunicable-diseases-in-pakistan.html
   2022 EMHJ paper describing Pakistan's NCD burden and surveillance gaps.

2. **HEARTS D: diagnosis and management of type 2 diabetes (Knowledge Action Portal)** — https://knowledge-action-portal.com/en/content/hearts-d-diagnosis-and-management-type-2-diabetes
   WHO HEARTS-D technical guidance; relevant to Pakistan's NCD framework.

3. **HEARTS as a tool: integrating hypertension and diabetes management (SciELO/PAHO)** — https://www.scielosp.org/article/rpsp/2022.v46/e150/
   HEARTS integration framework referenced in Pakistan's primary care context.

4. **HEARTS360: A gold-standard dashboard for hypertension and diabetes programs** — https://www.simple.org/blog/hearts360-dashboard/
   Describes DHIS2-based HEARTS360 dashboard for NCD program monitoring globally.


### Palestine — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Palestine has a heavy NCD burden and conducted a WHO STEPS survey in 2022, but data collection used the eSTEPS app with Open Data Kit (ODK), not DHIS2. UNRWA's e-Health platform is the primary digital system for NCD care in Palestinian refugee primary clinics. No clear evidence emerged that the Palestinian MoH uses DHIS2 for diabetes, hypertension, or CVD program management.

DHIS2 USE: UNCLEAR
Palestine's NCD surveillance (STEPS 2022) used ODK/eSTEPS, and clinical NCD care among the refugee population runs on UNRWA's e-Health system, not DHIS2. No documentation surfaced of DHIS2 deployment for cardiometabolic programs.

#### Search Results

##### English query results
1. **Noncommunicable diseases: a silent epidemic in occupied Palestine — WHO STEPS survey 2022 (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/40790734/
   Peer-reviewed paper analyzing the 2022 Palestine STEPS survey on NCD risk factors.

2. **Palestine STEPS Survey 2022 — WHO factsheet** — https://cdn.who.int/media/docs/default-source/2021-dha-docs/fact-sheet-palestine.pdf?sfvrsn=8d2d6a3b_1
   Official WHO factsheet summarizing key NCD risk factor findings.

3. **Obesity, Hypertension, and Type-2 Diabetes Mellitus: Determinants among Adults in Gaza City** — https://pmc.ncbi.nlm.nih.gov/articles/PMC6296808/
   Cross-sectional study of cardiometabolic risk in Gaza.

4. **Models of care for patients with hypertension and diabetes in humanitarian crises: a systematic review (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8128021/
   Systematic review including Palestinian/refugee context; describes UNRWA e-Health, not DHIS2.


### Panama — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Panama has implemented PAHO's HEARTS in the Americas initiative since 2018, joining as part of the second cohort. By the time of the PAHO M&E platform publication, Panama's MoH and Caja del Seguro Social (CSS) had trained health teams from 41 HEARTS-implementing facilities, with HEARTS expanded to 78 primary care facilities. PAHO confirms that the HEARTS in the Americas M&E platform is built on DHIS2 (hosted at INCAP, Guatemala City), and that Panama is explicitly listed among the participating countries entering aggregate CVD outcome, process, and structural indicators into the DHIS2-based platform. A 2021 PAHO communiqué also documents MoH/CSS adoption of a joint data-capture mechanism for hypertension and diabetes indicators.

DHIS2 USE: CONFIRMED
Panama is named in PAHO sources as a HEARTS-in-the-Americas participating country since 2018, and the HEARTS M&E platform that Panama reports into is the DHIS2 instance hosted at INCAP. CVD/hypertension indicators from 78 PHC facilities are entered into DHIS2.

#### Search Results

##### Spanish query results
1. **Enfermedades cardiovasculares, diabetes — Gorgas SIGCARDIOVASCULARES (Panama)** — https://www.gorgas.gob.pa/wp-content/uploads/external/SiGCARDIOVASCULARES/Inicio.htm
   Panama's Gorgas Memorial Institute CVD/diabetes surveillance information system landing page.

2. **Diabetes mellitus, prevalence, awareness, and control in Panama: ENSPA 2019** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10419614/
   National survey on diabetes in Panama; documents 12.4% DM prevalence.

3. **Prevalence of hypertension in two Panamanian provinces, 2010 and 2019 (PLOS One)** — https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0276222
   Cross-sectional studies on hypertension awareness and risk in Panama.
##### English query results
4. **Monitoring and evaluation platform for HEARTS in the Americas (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9484330/
   Describes DHIS2 as the chosen platform for HEARTS in the Americas, hosted at INCAP in Panama/Guatemala region.

5. **Monitoring and evaluation platform for HEARTS in the Americas (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/36133432/
   PubMed entry for the M&E platform paper.

6. **Integrating hypertension and diabetes management: HEARTS as a tool (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9440730/
   Region-wide HEARTS integration paper relevant to Panama.

7. **Institute of Nutrition of Central America and Panama — GHDx** — https://ghdx.healthdata.org/organizations/institute-nutrition-central-america-and-panama [BROKEN: 502]
   INCAP profile; INCAP hosts the DHIS2 server for the HEARTS in the Americas program.

8. **Evaluating WHO HEARTS Model for Hypertension and Diabetes Management: Guatemala pilot (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11483012/
   Pilot study illustrating DHIS2-supported HEARTS implementation in the region.

##### Targeted HMIS-angle re-search (2026)
9. **Minsa fortalece abordaje integral de las ENT con la iniciativa global HEARTS — Ministerio de Salud, Panamá** — https://www.minsa.gob.pa/noticia/minsa-fortalece-abordaje-integral-de-las-enfermedades-no-transmisibles-con-la-iniciativa
   MoH communiqué confirming HEARTS implementation in Panama with MoH/CSS, including training of health teams from 41 facilities (later expanded to 78 PHC facilities).

10. **Monitorean iniciativa HEARTS en Los Santos — Ministerio de Salud, Panamá** — https://www.minsa.gob.pa/noticia/monitorean-iniciativa-hearts-en-los-santos
    MoH news on operational monitoring of the HEARTS initiative in Los Santos region.

11. **MINSA y CSS cuentan con mecanismo para el levantamiento de datos de indicadores de ECV y diabetes — OPS/OMS Panamá** — https://www.paho.org/es/noticias/30-11-2021-ministerio-salud-caja-seguro-social-cuentan-con-mecanismo-para-levantamiento
    PAHO Panama country office documenting MoH+CSS joint data-capture mechanism for CVD and diabetes indicators (the HEARTS M&E platform is DHIS2-based).

12. **Iniciativa HEARTS genera resultados positivos en Panamá — OPS/OMS** — https://www.paho.org/es/noticias/21-7-2021-iniciativa-hearts-genera-resultados-positivos-panama-css-minsa-opsoms-definen
    PAHO/MoH/CSS roadmap for 2021 HEARTS scale-up in Panama.

13. **HEARTS in the Americas: Targeting Health System Change (Current Hypertension Reports, 2024)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10904446/
    Confirms Panama on the participating-country list and describes DHIS2 M&E platform implementation.


### Papua New Guinea — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Papua New Guinea has a high and rising burden of diabetes and hypertension, and is part of the RESist-NCD program (2024–2028) covering Fiji, PNG, the Philippines, Vietnam, and Cambodia for NCD prevention. PNG uses DHIS2 as its national HMIS (NHIS), but the search did not surface explicit evidence that DHIS2 is being used for individual-level diabetes/hypertension tracking or for a HEARTS-style program.

DHIS2 USE: LIKELY
PNG's NHIS runs on DHIS2 for routine aggregate health data, which likely includes NCD outpatient counts, but no direct documentation of a DHIS2 Tracker hypertension/diabetes program in PNG was found.

#### Search Results

##### English query results
1. **Prevalence of non-communicable diseases and risk factors in Papua New Guinea: a systematic review (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7682215/
   Systematic review documenting PNG's NCD burden, especially diabetes and hypertension.

2. **DIABETES CLINICAL PRACTICE GUIDELINES FOR PAPUA NEW GUINEA 2012 (WHO)** — https://extranet.who.int/ncdccs/Data/PNG_D1_guidelines-png-diabetes-final.pdf
   National diabetes clinical practice guidelines.

3. **RESist-NCD: Building resilient health systems for NCD prevention in Pacific and SE Asian countries (George Institute)** — https://www.georgeinstitute.org/our-research/research-projects/resist-ncd-building-resilient-and-people-centred-health-systems-for-non-communicable-disease
   2024–2028 RESist-NCD program covering PNG with focus on diabetes and hypertension.

4. **RESist-NCD project page (Australia DFAT Indo-Pacific Health Security)** — https://indopacifichealthsecurity.dfat.gov.au/investments/building-resilient-and-people-centred-health-systems-non-communicable-disease-prevention-and-control-pacific-and-southeast-asian-countries-resist-ncd [BROKEN: unreachable]
   Project investment description for RESist-NCD including PNG.

5. **Papua New Guinea — World Heart Observatory** — https://world-heart-federation.org/world-heart-observatory/countries/papua-new-guinea/
   Country profile on CVD burden and policy in PNG.


### Paraguay — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Paraguay has documented high diabetes and hypertension prevalence and gaps in primary care readiness in Family Health Units (USF). Paraguay participates in PAHO's HEARTS in the Americas initiative, which uses DHIS2 as the M&E platform hosted at INCAP. However, Paraguay-specific DHIS2 deployment for HEARTS/cardiometabolic data was not explicitly documented in this search.

DHIS2 USE: LIKELY
Paraguay is part of the PAHO HEARTS in the Americas region where DHIS2 is the standard M&E platform, making participation plausible, but no source explicitly confirms Paraguay using DHIS2 for hypertension/diabetes monitoring.

#### Search Results

##### Spanish query results
1. **Baja disponibilidad de recursos y apoyo para atender a personas con diabetes e hipertensión arterial en las USF de Paraguay (SciELO)** — https://scielo.iics.una.py/scielo.php?script=sci_arttext&pid=S2521-22812024000100046
   2024 SciELO Paraguay study on diabetes/hypertension care readiness in Family Health Units.

2. **HEARTS como herramienta — SciELO Public Health** — https://www.scielosp.org/article/rpsp/2022.v46/e213/es/
   Spanish-language version of the HEARTS integration paper.

3. **i Premio de Promoción de la Salud Cardiovascular — Iniciativa HEARTS 2021 (CDI Mecon Argentina)** — https://cdi.mecon.gob.ar/bases/docelec/az6508.pdf
   Regional document on HEARTS cardiovascular promotion award.
##### English query results
4. **Integrating hypertension and diabetes management: HEARTS as a tool (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9673610/
   Region-wide HEARTS paper noting DHIS2 as the M&E platform.

5. **Monitoring and evaluation platform for HEARTS in the Americas (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10202337/
   M&E platform paper describing DHIS2 deployment for HEARTS across the Americas.


### Philippines — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
The Philippines runs the PhilPEN program (adapted from WHO PEN) with Hypertension and Diabetes Clubs at public clinics, and operates a Hypertension e-Registry under its Healthy Hearts Programme. DOH and WHO have scaled up CVD prevention efforts in Western Visayas. The Philippines is also part of RESist-NCD (2024–2028). DHIS2 is not the dominant national HMIS for the Philippines (the country uses iClinicSys, DOH eHealth, and other platforms), and no direct evidence emerged of DHIS2 being used for cardiometabolic programs.

DHIS2 USE: UNCLEAR
The Philippines has well-developed NCD program infrastructure (PhilPEN, Hypertension e-Registry, RESist-NCD) but uses primarily DOH-built systems rather than DHIS2 for cardiometabolic data. No specific DHIS2 cardiometabolic deployment was found.

#### Search Results

##### English query results
1. **Prevention and control of NCDs in the Philippines: the case for investment (WHO)** — https://www.who.int/docs/default-source/wpro---documents/countries/philippines/reports/prevention-and-control-of-noncommunicable-diseases-in-the-philippines---the-case-for-investment.pdf
   WHO investment case for NCD prevention in the Philippines.

2. **Inequality in prevalence of unmedicated hypertension or diabetes among older Filipinos (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC12989984/
   Analysis of nationally representative survey data on hypertension/diabetes inequalities.

3. **Current status of hypertension care and management in the Philippines (ScienceDirect)** — https://www.sciencedirect.com/science/article/abs/pii/S1871402124000699
   2024 review of Philippine hypertension care including PhilPEN protocol.

4. **Scaling up the Community Health Assessment Program in the Philippines (CHAP-P)** — https://www.knowledge-action-portal.com/en/news_and_events/country-stories/9895
   Country story on community-based hypertension/diabetes prevention.

5. **Pathways to Hypertension Control: Unfinished Journeys in Malaysia and the Philippines (Wiley)** — https://onlinelibrary.wiley.com/doi/full/10.1002/hpm.3889
   2025 paper on hypertension control pathways and gaps.

6. **NCDs TRACKER — Healthy Philippines Alliance** — https://phealthalliance.ph/ncds-tracker/
   Philippine NCD tracking initiative.

7. **DOH, WHO scale up efforts to prevent cardiovascular diseases in Western Visayas (WHO Philippines)** — https://www.who.int/philippines/news/detail/21-06-2023-doh--who-scale-up-efforts-to-prevent-cardiovascular-diseases-in-western-visayas
   2023 announcement on CVD prevention scale-up.

8. **Asian management of hypertension: Philippines country report (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/32108413/
   Country report on hypertension management practices.


### Rwanda — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Rwanda is one of the strongest national DHIS2 adopters (DHIS2 is the official national HMIS, hmis.moh.gov.rw) and uses DHIS2 specifically for NCD screening reporting — all health centers were trained on reporting NCD screening through DHIS2. Rwanda has also rolled out the PEN-Plus model for severe NCDs (including diabetes and hypertension) to all 42 districts since 2015, with clinical patient data managed primarily in OpenMRS. STEPS surveys are used for risk-factor surveillance.

DHIS2 USE: CONFIRMED
DHIS2 is Rwanda's national HMIS and is used for NCD screening reporting (aggregate). Individual NCD/PEN-Plus patient management is in OpenMRS, so DHIS2 covers program monitoring rather than clinical case management for cardiometabolic care.

#### Search Results

##### English query results
1. **Rwanda rolls out free screening exercise to mitigate NCDs (MoH Rwanda)** — https://www.moh.gov.rw/news-detail/rwanda-rolls-out-free-screening-exercise-to-mitigate-ncds
   MoH announcement noting health centers were trained on DHIS2 reporting for NCD screening.

2. **Population-Level Interventions Targeting Risk Factors for Hypertension and Diabetes in Rwanda (Frontiers)** — https://www.frontiersin.org/journals/public-health/articles/10.3389/fpubh.2022.882033/full
   2022 situational analysis of Rwanda's hypertension and diabetes interventions.

3. **Population-Level Interventions... (PMC mirror)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9283981/
   PMC version of the situational analysis.

4. **Taking stock of population-level interventions in Rwanda and South Africa (BMC Public Health)** — https://bmcpublichealth.biomedcentral.com/articles/10.1186/s12889-023-16537-3
   2023 methodological reflections on the multi-component situational analysis.

5. **Implementation outcomes of national decentralization of integrated outpatient services for severe NCDs in Rwanda (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8453822/
   Documents PEN-Plus scale-up to all 42 districts.

6. **Rwanda National NCD Strategy and Costed Action Plan (MoH)** — https://www.moh.gov.rw/fileadmin/user_upload/Moh/Publications/Strategic_Plan/Rwanda_National_NCD_Strategy_Costed_Action_Plan_FINAL_12072021.pdf
   National NCD strategy 2020–2025.

7. **The Role of Integrated District Hospital-Based NCD Clinics in CVD Control in Rwanda (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9309289/
   Preliminary data on integrated NCD clinics.

8. **The Case of DHIS2 in Rwanda (arXiv)** — https://arxiv.org/pdf/2108.09721
   Academic case study on Rwanda's national DHIS2 deployment.

9. **Rwanda MoH HMIS (DHIS2 login)** — https://hmis.moh.gov.rw/covid19/dhis-web-commons/security/login.action
   Live Rwanda national DHIS2 instance.

10. **Blood pressure screening in Mata Sector, rural Rwanda (Journal of Human Hypertension)** — https://www.nature.com/articles/s41371-024-00912-7
    2024 community BP screening study in Rwanda.


### Saint Lucia — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Saint Lucia is an active HEARTS in the Americas country: from January 2020 to December 2021, the Ministry of Health (with PAHO support) implemented the HEARTS Technical Package in six primary health care facilities, expanding hypertension registry coverage by 17.8% (from 1,434 to 1,689 patients). HEARTS in the Americas uses DHIS2 as its regional M&E platform, which Saint Lucia participates in alongside Barbados, BVI, Dominica, Guyana, and Trinidad & Tobago.

DHIS2 USE: CONFIRMED
Saint Lucia is a documented HEARTS in the Americas implementer with an active hypertension registry. The regional HEARTS M&E platform runs on DHIS2 (hosted at INCAP), making Saint Lucia's contribution to DHIS2-based HEARTS reporting probable, though country-specific DHIS2 deployment is not explicitly named in retrieved sources.

#### Search Results

##### English query results
1. **Improving cardiovascular health in primary care in Saint Lucia through the HEARTS Initiative (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9440734/
   Key paper documenting Saint Lucia's HEARTS pilot in six PHC facilities and 17.8% increase in hypertension registry coverage.

2. **Improving cardiovascular health in primary care in Saint Lucia (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/36071919/
   PubMed entry for the Saint Lucia HEARTS paper.

3. **St. Lucia Diabetes & Hypertension Association** — http://www.sldha.org/
   National civil-society organization on diabetes and hypertension.

4. **St Lucia Diabetic and Hypertension Association — IDF profile** — https://idf.org/our-network/regions-and-members/north-america-and-caribbean/members/saint-lucia/st-lucia-diabetic-and-hypertension-association/
   IDF directory entry.


### Sao Tomé and Principe — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
São Tomé and Príncipe has officially adopted DHIS2 as its national HMIS, with WHO and UNDP support. Initial DHIS2 deployment focused on aggregate data for Global Fund and Gavi programs, and was extended in 2020 with the DHIS2 Tracker module for individual-level reporting and patient registries across several services. There is documented research on type-2 diabetes in São Tomé but no explicit public evidence that the DHIS2 Tracker is configured for diabetes, hypertension, or CVD-specific cardiometabolic programs.

DHIS2 USE: MODERATE
DHIS2 is the national HMIS in São Tomé and Príncipe and has been extended to individual patient tracking for several programs, but the search did not surface confirmation that cardiometabolic (diabetes/hypertension/CVD) programs are among those currently configured on DHIS2.

#### Search Results

##### Portuguese query results
1. **Diabetes tipo 2 em São Tomé (uBibliorum)** — https://ubibliorum.ubi.pt/entities/publication/d1254b6a-9a44-41d8-9017-bab87796181b
   Portuguese-language thesis/publication on type-2 diabetes in São Tomé.
##### English query results
2. **Implementing DHIS2 to accelerate Universal Health Coverage in Sao Tome and Principe (WHO)** — https://www.who.int/about/accountability/results/who-results-report-2020-mtr/country-story/2020/implementing-the-dhis2-to-accelerate-universal-health-coverage-in-s%C3%A3o-tom%C3%A9-and-principe
   WHO country story confirming national DHIS2 adoption and Tracker module rollout in 2020.

3. **São Tomé & Príncipe — World Heart Observatory** — https://world-heart-federation.org/world-heart-observatory/countries/sao-tome-principe/
   Country CVD burden profile.

4. **Sao Tome and Principe Country Overview (WHO)** — https://www.who.int/countries/stp
   WHO country page.

5. **Health in São Tomé and Príncipe — Wikipedia** — https://en.wikipedia.org/wiki/Health_in_S%C3%A3o_Tom%C3%A9_and_Pr%C3%ADncipe
   Background on the country's health system.

6. **Leveraging technology to reach global health: telemedicine in São Tomé and Príncipe (ScienceDirect)** — https://www.sciencedirect.com/science/article/abs/pii/S221188372100071X
   Paper on digital health innovation in STP.

7. **Cardiac Surgery in Sub-Saharan Africa (JACC Advances)** — https://www.jacc.org/doi/10.1016/j.jacadv.2024.101223
   Regional CVD care paper referencing STP.

8. **Africa Health Organisation — Sao Tome and Principe** — https://aho.org/countries/sao-tome-and-principe/
   AHO country profile.


### Senegal — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Senegal's national HMIS is DHIS2 (senegal.dhis2.org), and the Ministry of Health has actively integrated NCD/cardiometabolic indicators into routine collection. The CARDIO4Cities/Novartis Foundation collaboration (2021–2023) operationalized aggregated hypertension data flows into DHIS2 in Dakar, and ~45 NCD indicators (including diabetes, hypertension, CVD risk factors) have been added nationally. An eTracker dashboard linked to DHIS2 supports longitudinal patient follow-up.

DHIS2 USE: CONFIRMED
DHIS2 is the official national HMIS and has been formally extended to capture hypertension, diabetes and broader NCD indicators through a published MoHSA-led program with academic documentation (Springer 2025).

#### Search Results

##### French query results
1. **Integrating hypertension data into routine health data collection: the experience of CARDIO4Cities and DHIS2 in Dakar, Senegal** — https://link.springer.com/article/10.1007/s44250-025-00240-8
   2025 peer-reviewed article documenting integration of aggregated hypertension indicators into DHIS2 in Dakar under CARDIO4Cities.

2. **Système d'information sanitaire / MSHP (Senegal national DHIS2 instance)** — https://senegal.dhis2.org/
   Official DHIS2 instance of the Ministry of Health (MSHP) covering all programs including chronic disease indicators.

3. **Rapport d'analyse des indicateurs SRMNIA-N de routine (Countdown 2030 — DHIS2 data)** — https://www.countdown2030.org/wp-content/uploads/2023/10/Version-finale_Rapport_Groupe2_DONNEES-DHIS2_VF_compressed-1.pdf
   Senegal routine-data analysis using DHIS2 datasets; discusses indicator landscape.

4. **Sénégal — DHIS 2 : un Système National d'Information Sanitaire (CNLS Senegal)** — https://www.facebook.com/cnlssenegal/posts/2215233671831687/
   Government communication on adoption of DHIS2 as the national HMIS.

5. **Senegal: HIS Indicators — MEASURE Evaluation** — https://www.measureevaluation.org/his-strengthening-resource-center/country-profiles/senegal.html
   Country HIS profile referencing DHIS2 as the platform of record.
##### English query results
6. **(PDF) Integrating hypertension data into routine health data collection — CARDIO4Cities & DHIS2 in Dakar** — https://www.researchgate.net/publication/392074662
   ResearchGate copy of the 2025 hypertension/DHIS2 integration paper.

7. **Dakar Builds Health Worker Capacity, Improves Diagnosis and Management of Hypertension (IntraHealth)** — https://www.intrahealth.org/news/dakar-builds-health-worker-capacity-improves-diagnosis-and-management-hypertension-prevent
   Implementation account of HTN program in Dakar linked to DHIS2-based monitoring.

8. **For two decades, IntraHealth International has collaborated with Senegal — country brief 2025** — https://intrahealth.org/sites/default/files/Senegal_country_brief_March_2025.pdf
   Country brief covering HIS strengthening and chronic-disease integration.

9. **Senegal — Coalition for Access to NCD Medicines and Products** — https://coalition4ncds.org/countries-taking-action/senegal/
   Country page summarizing NCD strategy and surveillance, including HMIS use.


### Seychelles — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Seychelles has a well-developed NCD program (SEY-PEN, adapted from WHO PEN) addressing hypertension and diabetes with WHO technical support and routine screening data flowing to the Ministry of Health. However, no public source documents DHIS2 as the platform used for these data; the country's national HIS has historically relied on different systems and population-based surveys (STEPS, Seychelles Heart Study).

DHIS2 USE: UNCLEAR
NCD surveillance and registries are active but no evidence found of DHIS2 as the underlying digital platform for Seychelles' diabetes/hypertension data.

#### Search Results

##### English query results
1. **Strengthening care services for chronic diseases in Seychelles (WHO AFRO)** — https://www.afro.who.int/countries/seychelles/news/strengthening-care-services-chronic-diseases-seychelles
   2024 WHO AFRO piece on SEY-PEN, diabetes and hypertension screening results; no mention of DHIS2.

2. **Ministry of Health — Seychelles (official site)** — https://www.health.gov.sc/
   National MoH site; references public-health authority and NCD programs.

3. **Addressing non-communicable diseases in the Seychelles: towards a comprehensive plan of action (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/20595339/
   Foundational NCD strategy paper for Seychelles.

4. **Ministry of Health (Seychelles) — GHDx** — https://ghdx.healthdata.org/organizations/ministry-health-seychelles [BROKEN: 502]
   GHDx organizational record listing Seychelles MoH data sources.
##### Creole/French query results
5. **Ministry of Health Seychelles (Facebook)** — https://www.facebook.com/mohseychelles/
   Official MoH communications channel; references screening campaigns in Creole/English.


### Sierra Leone — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Sierra Leone reports hypertension data through DHIS2 as the national HMIS (in use since 2012); facility-level monitoring/evaluation officers submit records to the Directorate of NCDs. A CDC-published evaluation of the Kenema Government Hospital hypertension surveillance system explicitly describes DHIS2 use, and pilot work in Bombali district adapted WHO PEN for primary-care hypertension and diabetes management.

DHIS2 USE: CONFIRMED
Peer-reviewed/CDC source explicitly states hypertension data are reported to the national DHIS2 and routed to the NCD Directorate.

#### Search Results

##### English query results
1. **Evaluation of a Hypertension Surveillance System, Kenema Government Hospital, Sierra Leone, 2021 (CDC PCD)** — https://www.cdc.gov/pcd/issues/2023/22_0230a.htm
   CDC Preventing Chronic Disease 2023 evaluation explicitly describes DHIS2 as the national platform for hypertension reporting.

2. **Evaluation of a Hypertension Surveillance System — PubMed** — https://pubmed.ncbi.nlm.nih.gov/36952676/
   PubMed indexing of the same evaluation.

3. **Evaluation of a Hypertension Surveillance System — PMC full text** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10038097/
   Full-text PMC version with methods detailing DHIS2 reporting workflow.

4. **Adapting and implementing training, guidelines and treatment cards to improve primary care-based hypertension and diabetes management — feasibility study (PMC)** — https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7392674/
   2020 Bombali district WHO PEN adaptation pilot.

5. **Adapting and implementing… (BMC Public Health full text)** — https://bmcpublichealth.biomedcentral.com/articles/10.1186/s12889-020-09263-7
   BMC version of the same study.

6. **Prevalence of hypertension, diabetes mellitus and risk factors in an informal settlement in Freetown — cross-sectional (BMC, 2024)** — https://bmcpublichealth.biomedcentral.com/articles/10.1186/s12889-024-18158-w
   Recent prevalence data informing NCD planning.

7. **Sierra Leone STEPS report 2009 (WHO)** — https://cdn.who.int/media/docs/default-source/ncds/ncd-surveillance/data-reporting/sierra-leone/steps/2009_steps_report_sierraleone.pdf
   Baseline NCD risk-factor survey.

8. **Sierra Leone NCD Strategic Plan 2020–2024 (Ministry of Health and Sanitation)** — https://www.iccp-portal.org/sites/default/files/plans/SLE_B3_s21_NCD%20strategic%20plan%202020-2024%2023Feb2020%20FINAL%20signed%20CF%20(1)%20(1).pdf
   National NCD strategy referencing DHIS2-based monitoring.

9. **Evaluation of a Hypertension Surveillance System (CDC PDF)** — https://www.cdc.gov/pcd/issues/2023/pdf/22_0230.pdf
   PDF version of the CDC evaluation.


### Solomon Islands — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Solomon Islands faces an extreme NCD burden (more than 80% of National Referral Hospital admissions linked to diabetes, heart disease, hypertension, obesity) and has adopted WHO PEN-based guidelines and a draft NCD Road Map Policy 2024–2031. No public source confirms DHIS2 as the platform supporting cardiometabolic surveillance; HMIS reporting in the country has historically used other tools and Pacific regional systems.

DHIS2 USE: UNCLEAR
NCD programs are active, but evidence of DHIS2 underpinning hypertension/diabetes surveillance was not found in the available sources.

#### Search Results

##### English query results
1. **Solomon Islands guidelines for the management of NCDs (WHO/MoH)** — https://extranet.who.int/ncdccs/Data/SLB_D1_Solomon%20Is%20booklet%20for%20the%20management%20of%20NCDs.pdf
   National NCD management guidelines incorporating WHO PEN Protocols 1 & 2.

2. **Solomon Islands NCD Risk Factors STEPS Report** — https://cdn.who.int/media/docs/default-source/ncds/ncd-surveillance/data-reporting/solomon-islands/steps/2006-solomon-islands-steps-report.pdf
   National STEPS survey on NCD risk factors.

3. **The Protection Gap — Diagnosis, Treatment and Control for Diabetes and Hypertension in US-Affiliated Pacific Islands (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9995153/
   Pacific regional analysis relevant to Solomon Islands' cardiometabolic situation.

4. **Solomon Islands health facilitators strengthen regional fight against diabetes (WHO WPRO, 2025)** — https://www.who.int/westernpacific/about/how-we-work/pacific-support/news/detail/12-08-2025-solomon-islands-health-facilitators-strengthen-regional-fight-against-diabetes
   Recent WHO WPRO note on Solomon Islands diabetes capacity-building.
##### Pidgin query results
5. **NCDs and DHIS2: Learning from Country Experiences — DHIS2 Community** — https://community.dhis2.org/t/ncds-and-dhis2-learning-from-country-experiences/48128
   Community thread on NCD country experiences (no Solomon Islands implementation reported in retrieved excerpt).


### Somalia - Puntland State — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Puntland operates within Somalia's federal-level DHIS2-based HMIS, which is described in WHO EMRO and policy literature as the "national data backbone" for surveillance, with NCD monitoring identified as a future integration priority rather than a current operational use. No Puntland-specific DHIS2 cardiometabolic implementation was found in the available sources.

DHIS2 USE: MODERATE
DHIS2 covers Puntland as part of Somalia's national HMIS; cardiometabolic indicators are an integration target, not yet fully operational.

#### Search Results

##### English query results
1. **The Unrecognized Challenge: A Value-Based Policy Approach to Combat NCDs in Somalia's Health Sector (Research Square, 2025)** — https://www.researchsquare.com/article/rs-7925625/v1
   National-level policy paper covering Puntland as part of federal HMIS planning for NCD integration into DHIS2.
##### Somali query results
*(no remaining results after filtering generic WHO/PAHO/RTSL links)*


### Somalia — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Somalia's national HMIS is built on DHIS2 and recognized as the "national data backbone" for disease surveillance and immunization, although reporting completeness is around 50% and over 60% of facilities lack reliable data-capture systems. A 2025 policy paper explicitly identifies the need to digitally integrate NCD monitoring (hypertension ~33%, diabetes ~20% adult prevalence) into DHIS2 as system functionality matures. The WHO EMRO 2022 HIS assessment confirms DHIS2 as the platform but flags maturity gaps.

DHIS2 USE: MODERATE
DHIS2 is the national HMIS; NCD/cardiometabolic indicators are not yet fully operationalized within it but are an explicit integration target.

#### Search Results

##### English query results
1. **The Unrecognized Challenge: A Value-Based Policy Approach to Combat NCDs in Somalia's Health Sector (Research Square, 2025)** — https://www.researchsquare.com/article/rs-7925625/v1
   2025 policy paper documenting NCD burden and DHIS2 integration roadmap.

2. **Comprehensive assessment of Somalia's health information system 2022 (WHO EMRO)** — https://applications.emro.who.int/docs/9789292742188-eng.pdf
   WHO EMRO HIS assessment confirming DHIS2 as national HMIS and identifying NCD-data gaps.
##### Somali query results
3. **HISP Centre and World Diabetes Foundation Partner to Strengthen Digital Tools for NCDs** — https://dhis2.org/hisp-centre-world-diabetes-foundation-partnership/ (excluded: dhis2.org)

(Note: One promising hit on dhis2.org was filter-excluded per protocol.)


### Somaliland — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Somaliland faces a large cardiometabolic burden (hypertension ~41%, diabetes ~19% in recent Hargeisa surveys). A national health information database has been launched as part of post-conflict rebuilding, and recent BMC Health Services Research (2025) facility-capacity work references HIS but does not identify DHIS2 as the platform. No public source directly confirms DHIS2 use for NCDs in Somaliland.

DHIS2 USE: UNCLEAR
HIS rebuilding is underway and NCDs are a recognized priority, but no evidence found that DHIS2 specifically underpins cardiometabolic data in Somaliland.

#### Search Results

##### English query results
1. **The prevalence of selected risk factors for non-communicable diseases in Hargeisa, Somaliland (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC6611144/
   2019 cross-sectional study on NCD risk factors in Hargeisa.

2. **Same study — PubMed indexing** — https://pubmed.ncbi.nlm.nih.gov/31272414/
   PubMed entry for the Hargeisa NCD risk-factor study.

3. **Assessment of the capacity of health facilities in preventing and managing NCDs in selected regions of Somaliland (BMC HSR, 2025)** — https://bmchealthservres.biomedcentral.com/articles/10.1186/s12913-025-12913-4
   2025 facility-capacity assessment for hypertension/diabetes/CVD services.
##### Somali query results
4. **Same Hargeisa NCD prevalence study (Somali context)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC6611144/
   Primary source informing Somali-language NCD planning.


### South Africa — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
South Africa is the original home of DHIS (developed at University of Western Cape in the 1990s) and operates the system nationally as "WebDHIS." NCD-related data elements are part of routine reporting, with the Eastern Cape's published 2017–2020 data-quality study showing an 85.1% NCD data-element reporting rate. WebDHIS is the established platform; sub-national programs use it for hypertension and diabetes indicators within the broader chronic-disease monitoring framework.

DHIS2 USE: CONFIRMED
WebDHIS (DHIS2) is South Africa's national HMIS and is documented in peer-reviewed sources as carrying NCD/cardiometabolic indicators.

#### Search Results

##### English query results
1. **Quality of routine health data in DHIS2 in South Africa: Eastern Cape province 2017–2020 (SAJIM)** — https://sajim.co.za/index.php/sajim/article/view/1903/3042
   Peer-reviewed quality assessment of DHIS2 data including NCD element group (85.1% reporting).

2. **A roadmap for using DHIS2 data to track progress in key health indicators in the Global South (BMC Public Health)** — https://link.springer.com/article/10.1186/s12889-023-15979-z
   Sub-Saharan Africa roadmap explicitly drawing on South African DHIS2 experience.

3. **District Health Information System 2 (DHIS2) — Open Health News** — https://www.openhealthnews.com/resources/district-health-information-system-2-dhis2
   Reference page describing South Africa's origin role and current WebDHIS use.

4. **DISTRICT HEALTH MANAGEMENT INFORMATION (ideal-clinic, NDoH)** — https://www.idealhealthfacility.org.za/App/Document/Download/25
   South African NDoH documentation referencing WebDHIS for facility data.

5. **DHIS2 — Wikipedia** — https://en.wikipedia.org/wiki/DHIS2
   Historical/encyclopedic overview of DHIS2 with South Africa origin context.
##### IsiZulu query results
6. **Same SAJIM DHIS2 quality study (relevant to KZN/IsiZulu-speaking provinces)** — https://sajim.co.za/index.php/sajim/article/view/1903/3042
   Province-level data-quality findings applicable across South Africa including KwaZulu-Natal.


### South Sudan — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
South Sudan has used DHIS2 as its electronic HMIS since 2018, scaled to all counties, with active expansion to integrate LMIS, e-TB, EMR and IDSR modules. However, NCD/cardiometabolic surveillance has not been prioritized within the MoH budget, and the main NCD-control challenge is the lack of strategic information on prevalence and risk factors. DHIS2 exists nationally but no public evidence shows cardiometabolic indicators operationalized in it.

DHIS2 USE: LIKELY
DHIS2 is the national HMIS and a logical host for any NCD data, but no source confirms operational diabetes/hypertension modules.

#### Search Results

##### English query results
1. **South Sudan experiences with DHIS2 (YouTube — MoH/partners)** — https://www.youtube.com/watch?v=Qo5_kCF7zvU
   Official-style overview of DHIS2 use by the MoH.

2. **DHIS2 International Consultant for South Sudan Ministry of Health — UNGM** — https://www.ungm.org/Public/Notice/121449
   Procurement notice confirming MoH operates DHIS2 and seeks expansion expertise.

3. **DHIS2 in National Health Systems Strengthening — IMA World Health** — https://imaworldhealth.org/dhis2-national-health-systems-strengthening
   Implementation account of DHIS2 rollout in Upper Nile and Jonglei.

4. **Implementing a Routine HMIS in South Sudan (South Sudan Medical Journal)** — http://www.southsudanmedicaljournal.com/archive/february-2012/implementing-a-routine-health-management-information-system-in-south-sudan.html
   Background paper on national HMIS implementation.

5. **Assessment of the WHO NCD kit for humanitarian emergencies in South Sudan (PMC, 2023)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10241119/
   Recent NCD-program evaluation in humanitarian settings.

6. **WHO supports a Multi-sectoral call for NCDs Action in South Sudan (WHO AFRO)** — https://www.afro.who.int/news/who-supports-multi-sectorial-call-non-communicable-diseases-action-south-sudan
   Policy advocacy for NCD action in South Sudan.

7. **South Sudan Should Address Non-Communicable Diseases (South Sudan Medical Journal)** — http://www.southsudanmedicaljournal.com/archive/may-2015/south-sudan-should-address-non-communicable-diseases.html
   Editorial highlighting NCD prevalence and surveillance gaps.

8. **MoH — South Sudan (official site)** — http://moh.gov.ss/ [BROKEN: unreachable]
   Official Ministry of Health website.

9. **National Health Policy 2016–2025 (South Sudan)** — https://extranet.who.int/countryplanningcycles/sites/default/files/planning_cycle_repository/south_sudan/south_sudan_national_health_policy_2016_to_2025_2.pdf
   National policy framework referencing HMIS strengthening.

10. **The MoH in partnership with WHO strengthens HIS for effective service delivery (ReliefWeb)** — https://reliefweb.int/report/south-sudan/ministry-health-partnership-who-and-partners-strengthens-health-information
    Recent HIS strengthening activity.


### Sri Lanka — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Sri Lanka uses DHIS2 across preventive-health institutes within the MoH and is one of the lead implementers of the Diabetes Compass project (with World Diabetes Foundation/HISP Centre), building DHIS2-based screening and referral systems for diabetes and hypertension. The MoH adopted the WHO HEARTS technical package in 2021, with HEARTS360 dashboards updated daily from DHIS2/Simple. District health authorities actively use DHIS2 dashboards to monitor NCD program performance.

DHIS2 USE: CONFIRMED
DHIS2 is documented in DHIS2 community and Exemplars case studies as operational for diabetes/hypertension screening, referral and HEARTS monitoring; MoH leadership has publicly endorsed expansion to more NCDs.

#### Search Results

##### English query results
1. **Turning the tide on hypertension control in Sri Lanka (Resolve to Save Lives)** — https://resolvetosavelives.org/strategies-in-action/turning-the-tide-on-hypertension-control-in-sri-lanka/
   Case study of MoH adoption of HEARTS and digital dashboards including DHIS2 linkages.

2. **The Prevalence and Epidemiological Features of Ischaemic Heart Disease in Sri Lanka (PMC, 2024)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11160409/
   Recent prevalence study informing CVD surveillance.

3. **Prevalence and Associations of Hypertension in Sri Lankan Adults — SLHAS 2018–19 (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9354554/
   National hypertension survey results.

4. **NCD Unit — RDHS Office Gampaha** — https://rdhsofficegampaha.org/ncd-unit/
   District-level NCD unit page describing routine NCD service delivery.

5. **Non Communicable Disease Unit — RDHS Matara** — https://rdhsmatara.lk/servicesNCD.php [BROKEN: unreachable]
   Matara district NCD service description.

6. **Community Hypertension Care in Sri Lanka: Current Approaches and Opportunities (JAPSC)** — https://www.japscjournal.com/articles/community-hypertension-care-sri-lanka-current-approaches-and-opportunities
   Peer review of community HTN programs.

7. **Directorate of Non Communicable Disease, Sri Lanka MoH** — http://www.ncd.health.gov.lk/
   National NCD Directorate site.

8. **DHIS2 in Sri Lanka — Exemplars In Global Health** — https://www.exemplars.health/emerging-topics/epidemic-preparedness-and-response/digital-health-tools/sri-lanka
   Exemplars case study of national DHIS2 use across MoH preventive institutes.

9. **Extending and Strengthening Routine DHIS2 Surveillance Systems for COVID-19 in Sierra Leone, Sri Lanka, Uganda (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9745217/
   Peer-reviewed account of Sri Lanka's DHIS2 operational maturity.

10. **Usability evaluation of a DHIS2-based electronic information management system for environmental, occupational health and food safety in Sri Lanka (PMC, 2024)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC12164650/
    Demonstrates breadth of DHIS2 use within MoH preventive sector.
##### Sinhala query results
11. **Implementation of District Health Information Software 2 (DHIS2) in Sri Lanka (ResearchGate)** — https://www.researchgate.net/publication/237842905_Implementation_of_District_Health_Information_Software_2_DHIS2_in_Sri_Lanka
    Foundational publication on Sri Lanka's DHIS2 implementation by MoH staff.

12. **Health Information Unit, Ministry of Health, Sri Lanka (AeHin)** — https://7gm.asiaehealthinformationnetwork.org/marketplace/health-information-unit-ministry-of-health-sri-lanka
    Profile of MoH HIU which runs DHIS2.


### Sudan — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Sudan's Federal Ministry of Health uses DHIS2 for hospital reporting, per the WHO EMRO 2020 HIS assessment, but the HIS still requires considerable strengthening. The country has an NCD strategy and substantial cardiometabolic burden (hypertension is common among diabetes patients per a Nahr an Nil 2021 study), but ongoing conflict since 2023 has disrupted routine reporting. No source documents operational DHIS2 modules for diabetes, hypertension or CVD specifically.

DHIS2 USE: LIKELY
DHIS2 is in use for hospital reporting per WHO EMRO assessment, but cardiometabolic-specific operational use is not documented.

#### Search Results

##### English query results
1. **Assessment of Sudan's health information system 2020 (WHO EMRO)** — https://applications.emro.who.int/docs/9789290229681-eng.pdf?ua=1
   WHO EMRO HIS assessment explicitly noting DHIS2 use in hospitals and gaps in coverage.

2. **Federal Ministry of Health (Sudan) — GHDx** — https://ghdx.healthdata.org/organizations/federal-ministry-health-sudan
   Organizational record of Sudan's federal MoH and data sources.

3. **Republic of the Sudan National Ministry of Health — NCD strategy (ICCP)** — https://www.iccp-portal.org/sites/default/files/plans/SDN_B3_Sudan%20NCD%20strategy.pdf
   National NCD strategy document.

4. **Prevalence and associated factors of hypertension among adults with diabetes mellitus in northern Sudan (PMC, 2021)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8037914/
   Cross-sectional cardiometabolic study from Nahr an Nil State.

5. **Sudan: Delivering Vital Health Services Despite Conflict and Displacement (Global Fund)** — https://www.theglobalfund.org/en/stories/2025/2025-04-02-sudan-delivering-vital-health-services-conflict-displacement/
   2025 update on health-service delivery including chronic-disease screening during conflict.

6. **Sudan Health Emergency Situation Report No. 4 (WHO EMRO, Dec 2023)** — https://www.emro.who.int/images/stories/sudan/WHO-Sudan-conflict-situation-report-15-December_2023.pdf
   WHO emergency-context HIS reporting.
##### Arabic query results
7. **WHO EMRO Sudan HIS Assessment 2020 (same as #1, referenced for Arabic-language context)** — https://applications.emro.who.int/docs/9789290229681-eng.pdf?ua=1
   Same WHO EMRO assessment used by Arabic-speaking MoH stakeholders.


### Suriname — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Suriname has a high cardiometabolic burden (hypertension ~33%, diabetes ~10% in coastal districts per the Suriname Health Study). NCD surveillance has been carried out primarily via national surveys and PAHO frameworks rather than a DHIS2-based routine system. No source confirms Suriname uses DHIS2 for any health program, let alone diabetes/hypertension.

DHIS2 USE: NONE
No evidence of DHIS2 deployment in Suriname for cardiometabolic or any other health information use.

#### Search Results

##### English query results
1. **A National Surveillance Survey on Noncommunicable Disease Risk Factors: Suriname Health Study Protocol (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC4526944/
   Suriname's national NCD risk-factor survey protocol — relies on survey methodology, not routine DHIS2 reporting.
##### Dutch query results
*(no remaining results after filtering generic WHO/PAHO/RTSL links)*


### Syria MoH — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
No evidence that Syria's Ministry of Health (Damascus-controlled areas) uses DHIS2 for diabetes, hypertension, or broader NCD surveillance. WHO/KSrelief support NCD service delivery via NCD kits and primary care upgrades, but national HMIS appears to be a separate MoH platform. DHIS2 appears in adjacent Syrian-refugee NCD literature (Lebanon, Jordan) — not inside Syria's MoH.

DHIS2 USE: NONE
No public source documents a Syria MoH DHIS2 deployment for cardiometabolic conditions; DHIS2 references concern Syrian-refugee NCD programmes in neighbouring countries.

#### Search Results

##### English query results
1. **Models of care for patients with hypertension and diabetes in humanitarian crises: a systematic review (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8128021/
   Systematic review of HTN/DM models of care across crisis-affected populations including Syria; references DHIS2 use by humanitarian actors regionally.

2. **Delivering a primary-level NCD programme for Syrian refugees and host population in Jordan: descriptive costing study (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8312704/
   Jordan-based NCD programme for Syrian refugees; DHIS2 used by implementing partners.

3. **MSF model of care for Syrian refugees with diabetes and hypertension, Shatila camp, Lebanon (Conflict and Health)** — https://link.springer.com/article/10.1186/s13031-019-0191-3
   MSF DM/HTN cohort outcomes; data exported from DHIS2 for analysis.

4. **Socioeconomic and medical vulnerabilities among Syrian refugees with NCDs (Irbid, Jordan) — MSF (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9988815/
   NCD cohort analysis from MSF using DHIS2-derived data.

5. **Access to essential NCD medicines during conflict: CVD, diabetes, epilepsy in Northern Syria (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC12677547/
   Documents NCD medicine availability across Northwest/Northeast Syria; no MoH DHIS2 mention.

6. **The Management of Diabetes in Conflict Settings: Focus on the Syrian Crisis (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC6695264/
   Review of diabetes care during Syrian crisis.

7. **Prevalence of NCDs among Syrian refugees in host countries: systematic review (ScienceDirect)** — https://www.sciencedirect.com/science/article/abs/pii/S0033350622000415
   Meta-analysis of NCD prevalence among Syrian refugees.


### Syria North West — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
In Northwest Syria's humanitarian response, NGO and UN-coordinated NCD services (diabetes, hypertension) operate at primary care facilities supported by WHO and Relief International. While DHIS2 is widely used by humanitarian implementers regionally (MSF, IRC) for Syrian-refugee NCD cohorts, no public source confirms a DHIS2-based NCD surveillance system specifically inside Northwest Syria; the Whole-of-Syria health cluster typically uses EWARN/HeRAMS-style reporting.

DHIS2 USE: UNCLEAR
Humanitarian actors using DHIS2 elsewhere for Syrian NCD cohorts may have presence in NWS, but no explicit documentation ties DHIS2 to NCD reporting for the NWS hub.

#### Search Results

##### Arabic query results
*(no remaining results after filtering generic WHO/PAHO/RTSL links)*
##### English query results
1. **Access to essential NCD medicines during conflicts: CVD, diabetes, epilepsy in Northern Syria (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC12677547/
   Cross-sectional assessment of NCD medicine availability across NW and NE Syria public facilities and pharmacies.

2. **Models of care for patients with hypertension and diabetes in humanitarian crises: systematic review (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8128021/
   Includes Syria humanitarian cohorts; references DHIS2 use for monitoring.

3. **Decentralising HTN/DM care in humanitarian Kurdistan, Iraq: qualitative study (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11998334/
   Comparable humanitarian-NCD model relevant to NWS practitioners.

4. **Management of Diabetes in Conflict Settings: Syrian Crisis (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC6695264/
   Diabetes management overview during Syrian conflict.

5. **MSF NCD model and outcomes, Shatila camp, Lebanon (Conflict and Health)** — https://link.springer.com/article/10.1186/s13031-019-0191-3
   MSF NCD programme cohorts (data via DHIS2) treating Syrians.


### Tajikistan — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Tajikistan piloted WHO PEN/HEARTS in primary care in one region (19 PHC centres), achieving improved BP control over 12 months. The evaluation explicitly references DHIS2 as a source for oblast population numbers and hypertensive case counts in 2017, suggesting DHIS2 (or a DHIS2-derived data flow) is part of the routine health information system used to monitor cardiometabolic indicators.

DHIS2 USE: CONFIRMED
A peer-reviewed PEN/HEARTS evaluation cites DHIS2 as a data source for hypertension surveillance denominators in Tajikistan; full national NCD module deployment is not explicitly confirmed.

#### Search Results

##### English query results
1. **Evaluation and pilot implementation of essential interventions for HTN/CVD prevention in PHC, Tajikistan (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7479501/
   WHO PEN/HEARTS pilot in 19 PHC centres; cites DHIS2 for oblast hypertensive case data (2017).

2. **WHO/Europe — Study points to interventions for improving cardiovascular health in Tajikistan** — https://www.who.int/europe/news/item/09-06-2021-who-study-points-to-interventions-for-improving-cardiovascular-health-in-tajikistan
   WHO communication summarising HEARTS-aligned pilot findings.

3. **Challenges and opportunities in continuity of care for hypertension: mixed-methods PHC study, Tajikistan (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC6889695/
   Embedded mixed-methods study on HTN continuity of care.

4. **BMC Health Services Research — full text of PEN/HEARTS Tajikistan evaluation** — https://bmchealthservres.biomedcentral.com/articles/10.1186/s12913-021-06490-5
   Peer-reviewed version with methods detail including data sources.

5. **World Bank — Identifying Opportunities to Strengthen Service Delivery for Hypertension in Tajikistan** — https://documents1.worldbank.org/curated/en/610751567176916794/pdf/Identifying-Opportunities-to-Strengthen-Service-Delivery-for-Hypertension-in-Tajikistan.pdf [BROKEN: 404]
   Service delivery diagnostic for hypertension.

6. **PubMed entry — PEN/HEARTS Tajikistan evaluation** — https://pubmed.ncbi.nlm.nih.gov/34006266/
   Indexed abstract.

7. **F1000Research v1 — PEN/HEARTS Tajikistan evaluation** — https://f1000research.com/articles/8-1639/v1
   Earlier version of the evaluation manuscript.


### Tanzania — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Tanzania introduced DHIS2 in 2013 as its national HMIS and the Ministry of Health hosts its operational instance at dhis.moh.go.tz. Multiple peer-reviewed sources confirm DHIS2 is the routine reporting platform used by the Tanzanian Ministry of Health to capture epidemiological data on NCDs including hypertension and diabetes, and stakeholders involved in NCD/PEN Plus implementation emphasise DHIS2 as central to NCD monitoring. Care-cascade and HIV-NCD integration studies further document substantial cardiometabolic activity feeding routine systems.

DHIS2 USE: CONFIRMED
Tanzania's national HMIS is DHIS2 (Ministry of Health portal at dhis.moh.go.tz) and it is explicitly used for routine reporting of NCD indicators including hypertension and diabetes; SWOC analyses ahead of PEN Plus implementation reference DHIS2 as the system to strengthen for NCD data.

#### Search Results

##### English query results
1. **Care cascades for hypertension and diabetes: cross-sectional evaluation of rural districts in Tanzania (PLOS Medicine)** — https://journals.plos.org/plosmedicine/article?id=10.1371/journal.pmed.1004140
   Population-based HTN/DM care cascade evaluation.

2. **Care cascades for HTN/DM in rural Tanzania (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9762578/
   Full-text version.

3. **Seeking and receiving HTN and DM care in Tanzania (PLOS ONE)** — https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0312258
   Care-seeking analysis among adults with HTN/DM.

4. **Seeking and receiving HTN/DM care in Tanzania (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/39576779/
   Indexed abstract.

5. **Six-month incidence of HTN and DM among adults with HIV in Tanzania (PLOS GPH)** — https://journals.plos.org/globalpublichealth/article?id=10.1371/journal.pgph.0001929
   Prospective cohort within HIV care.

6. **Hypertension among adults in HIV care in northern Tanzania (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/35855029/
   HTN comorbidity within HIV cohort.

7. **Prevalence of HTN and associated factors among DM patients, Kilimanjaro (ScienceDirect)** — https://www.sciencedirect.com/science/article/pii/S2213398423001744
   Hospital-based cross-sectional analysis.

##### HMIS-angle re-search (DHIS2 / Ministry of Health)
8. **Tanzania HMIS portal (Ministry of Health DHIS2 instance)** — https://dhis.moh.go.tz/
   Operational national DHIS2 instance hosted by the Tanzania Ministry of Health.

9. **Stakeholders' perspectives on the management and prevention of NCDs in rural Tanzania: SWOC analysis prior to PEN Plus implementation (PLOS Global Public Health)** — https://journals.plos.org/globalpublichealth/article?id=10.1371/journal.pgph.0005701
   Explicitly identifies improving the DHIS2 system as crucial for better monitoring of NCDs including hypertension and diabetes in Tanzania.

10. **Mitigating the Rising Burden of NCDs through Locally Generated Evidence — Lessons from Tanzania (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10655751/
    Documents that Ministry of Health epidemiological data on NCDs (HTN, DM, injuries) in selected districts are captured through DHIS2.

11. **Perceived Usefulness, Competency, and Associated Factors in Using DHIS Data Among District Health Managers in Tanzania (PMC, JMIR)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9171597/
    Confirms Tanzania introduced DHIS2 in 2013 as the national HMIS used by district health managers for routine service data including NCDs.

12. **A roadmap for using DHIS2 data to track progress in key health indicators in the Global South: experience from sub-Saharan Africa (BMC Public Health)** — https://link.springer.com/article/10.1186/s12889-023-15979-z
    Cross-country roadmap including Tanzania for using DHIS2 routine data to track health indicators.

13. **NCD Action Plan 2021–2026, Ministry of Health, Community Development, Gender, Elderly and Children (Tanzania)** — https://tzdpg.or.tz/wp-content/uploads/2022/04/NCD-ACTION-PLAN-2021-2026.pdf
    National NCD strategy referencing HMIS/DHIS2-based monitoring of hypertension, diabetes and CVD indicators.


### Thailand — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Thailand has extensive NCD surveillance and a long-running national HTN/DM screening and reporting requirement tied to MOPH targets, but uses national insurance/MOPH systems (HDC, NHSO databases) rather than DHIS2. No surfaced source documents DHIS2 use by Thai MOPH for cardiometabolic surveillance.

DHIS2 USE: NONE
Thailand's NCD data infrastructure is built on MOPH/NHSO platforms (Health Data Center, claims databases); DHIS2 deployment for NCDs is not evidenced.

#### Search Results

##### English query results
1. **Effective coverage of diabetes and hypertension: Thailand national insurance database 2016–2019 (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9716924/
   National NHSO insurance database analysis of HTN/DM effective coverage.

2. **WHO Thailand — Noncommunicable Diseases overview** — https://www.who.int/thailand/our-work/NCDs
   NCD burden and policy context in Thailand.

3. **Thailand 5-Year National NCDs Prevention and Control Plan (2017–2021)** — https://www.who.int/docs/default-source/thailand/ncds/national-ncd-prevention-and-control-plan-2017-2021-eng.pdf
   National plan; references MOPH reporting infrastructure.

4. **Diabetes trends and determinants among Thai adults 2004–2020 (Scientific Reports)** — https://www.nature.com/articles/s41598-025-17619-5
   Long-term trend analysis from national surveys.

5. **SMARThealth Diabetes Study protocol — mHealth-led PHC for T2DM in rural Thailand (JMIR Research Protocols)** — https://www.researchprotocols.org/2024/1/e59266/
   mHealth intervention protocol.

6. **SMARThealth Diabetes Study (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11364943/
   Full-text version.

7. **Prevalence of NCDs and social determinants — Mahidol Thai Journal of Public Health** — https://www.ph.mahidol.ac.th/thjph/journal/54_2/06.pdf
   Local NCD epidemiology.


### Timor Leste — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Timor-Leste runs an ambitious "Timor Hearts" programme adopting WHO PEN and HEARTS-D packages, targeting 50,000 hypertension and diabetes patients on standard care by 2025, with explicit plans to strengthen health information systems. Timor-Leste's national HMIS (often referred to as TLHIS) is DHIS2-based, and a WDF-supported diabetes control cascade project is underway, making DHIS2 use for cardiometabolic monitoring highly likely.

DHIS2 USE: LIKELY
National HMIS is DHIS2; HEARTS/PEN rollout and WDF diabetes cascade project rely on HMIS strengthening, though a published HTN/DM Tracker package deployment is not yet explicitly documented.

#### Search Results

##### English query results
1. **WHO SEARO — Timor-Leste: 50,000 people with hypertension and diabetes on standard care by 2025** — https://www.who.int/southeastasia/news/detail/18-05-2023-timor-leste-50-000-people-with-hypertension-and-diabetes-on-standard-care-by-2025
   Launch of Timor Hearts; adopts WHO PEN and HEARTS-D, integrates with HIS.

2. **World Diabetes Foundation — Improving diabetes control cascade in Timor-Leste (WDF23-1914)** — https://www.worlddiabetesfoundation.org/what-we-do/projects/wdf23-1914/
   Aileu Municipality diabetes control cascade, includes HIS strengthening.

3. **WHO Timor-Leste — Breaking barriers in diabetes care (Nov 2024)** — https://www.who.int/timorleste/news/detail/14-11-2024-timor-leste-is-breaking-barriers-in-diabetes-care
   Integration of TB-diabetes screening and community outreach.

4. **Community-Based Cross-Sectional Study of HTN and DM in Atauro, Timor-Leste (TLJMS)** — https://jmedicalsciences.tl/index.php/TLJMS/article/view/8 [BROKEN: unreachable]
   Local epidemiology of HTN and DM.

5. **Timor-Leste NCD Strategy/Action Plan 2014–2018 (WHO extranet)** — https://extranet.who.int/ncdccs/Data/TLS_B3_NCD%20Action%20Plan,%20Injuries,%20Disabilities,%20and%20Elderly%20Care%202014-2018.pdf
   National NCD action plan.


### Togo — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Togo explicitly uses DHIS2 to collect and analyse monthly surveillance data on hypertension and diabetes under the WHO PEN approach. A 2023 evaluation in Golfe Health District describes the data flow: data entered into DHIS2, analysed monthly, with WhatsApp-based district NCD focal point coordination and quarterly steering committee review. DHIS2 also underpins broader SNIS reporting in Togo (e.g., immunisation).

DHIS2 USE: CONFIRMED
Peer-reviewed AFENET evaluation explicitly states HTN/DM data are entered into and analysed via DHIS2 in Togo under the WHO PEN framework.

#### Search Results

##### French query results
1. **Évaluation du système de surveillance de l'HTA et du diabète selon WHOPEN, District Sanitaire de Golfe, Togo, 2023 (JIEPH/AFENET)** — https://afenet-journal.org/evaluation-du-systeme-de-surveillance-de-lhypertension-arterielle-et-du-diabete-dans-le-cadre-de-lapproche-whopen-dans-le-district-sanitaire-de-golfe-togo-2023/
   Explicitly describes HTN/DM data entry and analysis in DHIS2; monthly reporting via WhatsApp to district NCD focal point.

2. **Offensive contre l'hypertension, le diabète et l'obésité — République Togolaise** — https://www.republicoftogo.com/toutes-les-rubriques/sante/offensive-contre-l-hypertension-le-diabete-et-l-obesite
   Government communication on national NCD offensive.

3. **Rapport final de l'enquête STEPS Togo (2021)** — https://sante.gouv.tg/wp-content/uploads/2024/04/Togo-rapport-enquete-STEPS_2021_VF-10.06-2023-divsmnt_clean-FR-2.pdf
   WHO STEPS national NCD risk factor survey.

4. **OMS Afro — « Protège ton coeur au Togo »** — https://www.afro.who.int/fr/news/protege-ton-coeur-au-togo
   WHO HEARTS-aligned cardiovascular campaign in Togo.

5. **Le Togo renforce ses actions avec l'OMS pour freiner l'HTA, le diabète et l'obésité — La Cinquième** — https://www.lacinquieme.tg/le-togo-renforce-ses-actions-avec-loms-pour-freiner-lhypertension-le-diabete-et-lobesite/
   News coverage of WHO–Togo cooperation on NCDs.

6. **Annuaire statistique 2021 — MoH Togo** — https://sante.gouv.tg/wp-content/uploads/2023/04/Annuaire-statistique-2021.pdf
   Annual statistics derived from DHIS2-based SNIS.

7. **Annuaire des statistiques sanitaires 2022 — MoH Togo** — https://sante.gouv.tg/wp-content/uploads/2024/04/Annuaire_Statistique_2022.pdf
   2022 health statistics yearbook.


### Tonga — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Tonga has an extreme NCD burden (NCDs cause ~83% of deaths; HTN ~28%, raised glucose ~34%) and runs STEPS surveys and clinical diabetes registries, but no surfaced source documents DHIS2 use by Tonga's Ministry of Health. Pacific Island NCD reporting often uses bespoke or SPC-supported tools rather than DHIS2.

DHIS2 USE: UNKNOWN
No public evidence of DHIS2 deployment in Tonga for cardiometabolic conditions.

#### Search Results

##### English query results
1. **Burden and spectrum of disease in people with diabetes in Tonga (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC4547592/
   Clinic-based DM cohort analysis.

2. **Prevalence of NCDs among Adults in Tonga — Ballard Brief** — https://ballardbrief.byu.edu/issue-briefs/prevalence-of-non-communicable-disease-among-adults-in-tonga
   Overview of NCD epidemiology in Tonga.

3. **Kingdom of Tonga NCD Risk Factors STEPS Report (2012)** — https://cdn.who.int/media/docs/default-source/ncds/ncd-surveillance/data-reporting/tonga/steps/2012-tonga-steps-report.pdf
   WHO STEPS NCD risk factor survey.

4. **Economic costs of NCDs in the Pacific Islands — World Bank** — https://www.worldbank.org/content/dam/Worldbank/document/the-economic-costs-of-noncommunicable-diseases-in-the-pacific-islands.pdf
   Pacific-wide costing including Tonga.

5. **Tide of dietary risks for NCDs in Pacific Islands: analysis of population NCD surveys (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/35948900/
   Regional NCD survey analysis.

6. **Tonga challenges Pacific to step up NCD fight — RNZ News** — https://www.rnz.co.nz/news/pacific/307032/tonga-challenges-pacific-to-step-up-ncd-fight
   Policy advocacy coverage.

7. **IDF — Tonga member profile** — https://idf.org/our-network/regions-and-members/western-pacific/members/tonga/
   IDF country profile.


### Tunisia — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Tunisia has a high cardiometabolic burden (regional surveys show very high prevalences of obesity, hypertension, and diabetes) but no surfaced evidence indicates the Ministry of Health uses DHIS2. National HMIS in Tunisia has historically used bespoke or other platforms (e.g., SIGS/SI-NCD-related tools), not DHIS2.

DHIS2 USE: UNKNOWN
No public source documents Tunisia MoH DHIS2 deployment for diabetes/HTN/CVD.

#### Search Results

##### English query results
1. **Prevalence and risk factors of diabetes mellitus and hypertension in North East Tunisia (Scientific Reports / Nature)** — https://www.nature.com/articles/s41598-023-39197-0
   Zaghouan governorate prevalence study: obesity 50%, HTN 39%, T2DM 32%.

2. **Same study, PubMed entry** — https://pubmed.ncbi.nlm.nih.gov/37543635/
   Indexed abstract.


### Uganda — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Uganda's national HMIS runs on DHIS2 and explicitly captures hypertension as a routine indicator. The Uganda National Institute of Public Health (UNIPH) published a 2024 analysis of hypertension trends 2016–2021 drawn directly from DHIS2 OPD outpatient data, demonstrating that routine hypertension burden is tracked through the national DHIS2 instance. Complementary academic studies cover HTN/DM care delivery in refugee settlements, integrated HIV/NCD services, and task-shifting.

DHIS2 USE: CONFIRMED
UNIPH's 2024 hypertension-trends report cites DHIS2 as the source for routine hypertension data 2016–2021, confirming explicit cardiometabolic capture in the national HMIS.

#### Search Results

##### English query results
1. **Trends and distribution of hypertension in Uganda 2016–2021 (UNIPH, Jan 2024)** — https://uniph.go.ug/wp-content/uploads/2024/01/Trends-and-distribution-of-hypertension-in-Uganda-2016-to-2021.pdf
   Uganda National Institute of Public Health analysis using routine DHIS2 OPD data to track hypertension burden and trends nationally.

2. **Health facilities' readiness to manage HTN and DM at PHC, Bidibidi Refugee Settlement, Yumbe District, Uganda (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7847353/
   Facility readiness study in refugee settlement.

3. **Self-care and healthcare seeking practices among HTN/DM patients in rural Uganda (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/38079386/
   Nakaseke district outpatient NCD clinic cohort.

4. **Principles for task shifting HTN/DM screening and referral: qualitative study in rural Uganda (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/37173687/
   Patient, CHW and HCP perceptions of task-shifting.

5. **Integrated healthcare services for HIV, DM and HTN, Kampala and Wakiso, Uganda (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10021152/
   Qualitative study of integrated HIV-NCD care.

6. **Challenges to HTN and DM management in rural Uganda: qualitative study (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC6394065/
   Patient/VHT/HCP qualitative study.

7. **Epidemiology of behavioral risk factors for NCDs and HTN in Eastern Uganda (PLOS GPH)** — https://journals.plos.org/globalpublichealth/article?id=10.1371/journal.pgph.0002998
   Population-level NCD risk factor study.


### Ukraine — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Ukraine has a heavy cardiometabolic burden (≈1/3 hypertensive, ~7% diabetes; CVD is leading cause of death). WHO/Europe is implementing HEARTS and PEN in Ukraine, but Ukraine's national eHealth system uses its own platform (e.g., NHSU eHealth) rather than DHIS2. No surfaced evidence indicates DHIS2 is used by Ukrainian MoH for NCD surveillance.

DHIS2 USE: NONE
Ukraine's national health information infrastructure is the NHSU eHealth platform; DHIS2 is not documented as an MoH NCD tool.

#### Search Results

##### English query results
1. **Diabetes, Hypertension, and Heart Disease Don't Stop for War — Think Global Health** — https://www.thinkglobalhealth.org/article/diabetes-hypertension-and-heart-disease-dont-stop-war
   Cardiometabolic care continuity during conflict in Ukraine.

2. **Modeling global 80-80-80 blood pressure targets and cardiovascular outcomes (Nature Medicine)** — https://www.nature.com/articles/s41591-022-01890-4
   Global BP control modeling including Ukraine.


### Uzbekistan — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Uzbekistan is rolling out WHO PEN/HEARTS clinical protocols at primary care and has developed the UZ-SPEED NCD tool (in partnership with Hiroshima University and MoH) to digitise NCD screening data — this is a bespoke tool, not DHIS2. No surfaced source confirms DHIS2 use by the Uzbek MoH for cardiometabolic surveillance.

DHIS2 USE: NONE
Uzbekistan's NCD digital tool of record is UZ-SPEED; DHIS2 is not evidenced in the national NCD/HIS architecture.

#### Search Results

##### English query results
1. **Design and Implementation of Brief Interventions to Address NCDs in Uzbekistan (Global Health: Science and Practice)** — https://www.ghspjournal.org/content/12/4/e2300443
   PEN/HEARTS-aligned brief interventions; clinician training on CVD risk stratification.

2. **World Diabetes Foundation — Improving NCD prevention/control in PHC in Uzbekistan and Kyrgyzstan (WDF21-1803)** — https://www.worlddiabetesfoundation.org/what-we-do/projects/wdf21-1803/
   WDF-supported PHC NCD project.

3. **Brief interventions for NCDs in Uzbekistan (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11349505/
   Full-text version.

4. **Analysis of adult NCD screening data in Uzbekistan using the UZ-SPEED NCD tool (ScienceDirect)** — https://www.sciencedirect.com/science/article/pii/S0033350626000867
   Describes UZ-SPEED — bespoke (non-DHIS2) digital NCD screening tool piloted in 4 regions.

5. **World Bank — Uzbekistan Health System Improvement Project** — https://documents1.worldbank.org/curated/en/207551599142919624/pdf/Uzbekistan-Health-System-Improvement-Project.pdf [BROKEN: 404]
   Health system project context including HIS.


### Vanuatu — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Vanuatu has conducted WHO STEPS surveys (2011, 2013) to assess NCD risk factors including raised blood pressure, blood glucose and cholesterol across all six provinces. The Ministry of Health uses an HMIS for routine reporting, but public evidence of a DHIS2 Tracker-based cardiometabolic registry or HEARTS/PEN implementation in Vanuatu was not found in this search. National NCD surveillance currently rests on periodic STEPS surveys rather than confirmed DHIS2 individual-level NCD case tracking.

DHIS2 USE: UNCLEAR
No academic or government documents in the search results explicitly confirm DHIS2 use for diabetes, hypertension or CVD data management in Vanuatu. STEPS-based aggregated surveillance is documented; DHIS2 deployment for NCDs is not.

#### Search Results

##### English query results
1. **Vanuatu NCD Risk Factors: STEPS REPORT** — https://cdn.who.int/media/docs/default-source/ncds/ncd-surveillance/data-reporting/vanuatu/steps/vanuatu-steps-report-2013.pdf
   WHO STEPS survey results for Vanuatu covering tobacco, diet, BP, glucose, cholesterol across 6 provinces.

2. **Vanuatu - STEPS 2011** — https://extranet.who.int/ncdsmicrodata/index.php/catalog/714
   STEPS 2011 microdata catalog entry for Vanuatu.

3. **Surveillance of noncommunicable diseases — Vanuatu** — https://www.who.int/teams/noncommunicable-diseases/surveillance/data/vanuatu
   WHO country page summarising NCD surveillance activity in Vanuatu.


### Venezuela — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Venezuela has piloted the WHO/PAHO HEARTS initiative for hypertension control in primary care (e.g., La Marroquina community, 2023). The HEARTS in the Americas regional monitoring and evaluation platform explicitly uses DHIS2 as the aggregate data entry system for CVD outcome, process and structural indicators at PHC facilities. This suggests Venezuela's HEARTS pilot reporting plausibly feeds the PAHO DHIS2 platform, though country-specific DHIS2 deployment details are not documented in the search results.

DHIS2 USE: LIKELY
PAHO's regional HEARTS M&E platform is DHIS2-based and Venezuela is a participating HEARTS implementer; direct evidence of a national MoH DHIS2 instance for NCDs in Venezuela was not surfaced.

#### Search Results

##### Spanish query results
1. **Evaluation of Implementation of the HEARTS Initiative in a Rural Community in Venezuela, 2023** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11265309/
   Quasi-experimental evaluation of HEARTS implementation in La Marroquina, Venezuela.

2. **HEARTS como herramienta para integrar el manejo de la hipertensión y la diabetes** — https://www.scielosp.org/article/rpsp/2022.v46/e213/es/
   Spanish-language PAHO paper on integrating diabetes into HEARTS in PHC.

3. **La hipertensión arterial en Venezuela y sus factores determinantes** — https://www.scielosp.org/article/rsap/2017.v19n4/562-566/
   SciELO analysis of hypertension determinants in Venezuela.
##### English query results
4. **Monitoring and evaluation platform for HEARTS in the Americas (DHIS2)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10202337/
   Describes DHIS2 as the M&E platform for HEARTS hypertension control across the Americas.

5. **Integrating hypertension and diabetes management in PHC: HEARTS as a tool** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9673610/
   Conceptual paper on HEARTS-based diabetes integration.

6. **HEARTS in the Americas: targeting health system change** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10924616/
   Regional implementation overview with 22 participating ministries.

7. **Consensus on T2DM and heart failure (CIFACAH/IASC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10665010/
   Inter-American clinical consensus document.


### Vietnam — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Vietnam has documented use of DHIS2 Tracker for community-based hypertension and diabetes screening. HelpAge's Intergenerational Self-Help Clubs (ISHCs) used a DHIS2 Tracker-based mobile app (available on Google Play) for offline screening of older adults, with high data completeness (>99% on BP, weight, abdominal circumference) across 6,704 screenings. National policy (Decision 2559) supports integrated hypertension/diabetes management at commune health stations, and Vietnam has run STEPS surveys in 2010, 2015 and 2021.

DHIS2 USE: CONFIRMED
Peer-reviewed JMIR study documents a DHIS2 Tracker Android app used by community volunteers for hypertension/diabetes screening data capture in Vietnam.

#### Search Results

##### Vietnamese query results
1. **Hướng dẫn chẩn đoán và điều trị bệnh đái tháo đường tuýp 2 (VNCDC)** — https://vncdc.gov.vn/huong-dan-chan-doan-va-dieu-tri-benh-dai-thao-duong-tuyp-2-nd14582.html
   Vietnam CDC clinical guideline for T2DM diagnosis and treatment.

2. **Hướng dẫn chẩn đoán và điều trị tăng huyết áp (VNCDC)** — https://vncdc.gov.vn/huong-dan-chan-doan-va-dieu-tri-tang-huyet-ap-nd14594.html
   Vietnam CDC national hypertension guideline.

3. **Khuyến cáo của phân hội tăng huyết áp 2021** — https://hntmmttn.vn/upload/attach/202291214352.pdf
   Vietnam Society of Hypertension 2021 recommendations.

4. **Bệnh đái tháo đường — WHO Vietnam** — https://www.who.int/vietnam/vi/health-topics/diabetes
   WHO Vietnam diabetes country page.

5. **Điều trị thuốc hạ áp theo khuyến cáo trên người bệnh ĐTĐ type 2** — https://tapchiyhocvietnam.vn/index.php/vmj/article/view/12952
   Vietnamese medical journal article on antihypertensive therapy in T2DM.
##### English query results
6. **Challenges and Opportunities in Digital Screening for Hypertension and Diabetes Among Older Adults in Vietnam (JMIR 2024)** — https://www.jmir.org/2024/1/e54127
   Mixed-methods study; ISHC volunteers used DHIS2 Tracker app for community NCD screening.

7. **Same study, PubMed** — https://pubmed.ncbi.nlm.nih.gov/39622043/
   PubMed indexing of the DHIS2/ISHC screening study.

8. **Same study, PMC** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11650079/
   Full text confirming DHIS2 Tracker Android app, offline-capable.

9. **Comorbidities of diabetes and hypertension in Vietnam (BMC Public Health)** — https://bmcpublichealth.biomedcentral.com/articles/10.1186/s12889-023-17383-z
   National burden and trends analysis.

10. **Communities for Healthy Hearts, Ho Chi Minh City (PATH)** — https://media.path.org/documents/CH2_Factsheet_082018_EN.pdf
    Community-based hypertension management program factsheet.


### Western Sahara — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
No specific evidence of cardiometabolic surveillance programmes, HEARTS/PEN implementation, or DHIS2 use for diabetes/hypertension was found for Western Sahara. The territory's contested political status means health information is typically reported under Morocco (Moroccan-administered areas) or via UNHCR/humanitarian channels for Sahrawi refugee camps in Tindouf, Algeria, with no country-specific NCD digital surveillance system documented.

DHIS2 USE: UNKNOWN
No relevant documents in the search results address NCD or DHIS2 use in Western Sahara specifically.

#### Search Results

##### English query results
1. **Cardiovascular Diseases in Sub-Saharan Africa Compared to High-Income Countries (Global Heart)** — https://globalheartjournal.com/articles/10.5334/gh.403
   Regional CVD epidemiology overview (no Western Sahara specifics).

2. **Models of care for hypertension and diabetes in humanitarian crises: systematic review** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8128021/
   Relevant to refugee/humanitarian NCD care models.

3. **Hypertension in sub-Saharan Africa: current profile, advances, gaps and priorities** — https://www.nature.com/articles/s41371-024-00913-6
   Regional analysis, low diagnosis/treatment/control rates.

4. **Heart Failure in Sub-Saharan Africa (IntechOpen)** — https://www.intechopen.com/chapters/66937
   Background literature on HF burden in SSA.

5. **Hypertension, diabetes, and CVD nexus in Cabo Verde** — https://www.tandfonline.com/doi/full/10.1080/16549716.2024.2414524
   Comparator small-country NCD analysis.

6. **Frontiers — CVD in Africa in the 21st century** — https://www.frontiersin.org/journals/cardiovascular-medicine/articles/10.3389/fcvm.2022.1008335/full
   Continental gaps and priorities.


### Yemen — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
No direct evidence of DHIS2 use for diabetes, hypertension or CVD management in Yemen surfaced in this search. WHO EMRO supports HEARTS/PEN regionally, and humanitarian-setting NCD models of care are documented for Yemen's protracted crisis, but country-specific NCD digital surveillance via DHIS2 was not found. Yemen's national HMIS infrastructure is fragmented by conflict.

DHIS2 USE: UNKNOWN
Search returned only generic DHIS2 NCD package resources and global humanitarian NCD reviews; no Yemen-specific DHIS2 NCD deployment was identified.

#### Search Results

##### English query results
1. **Models of care for hypertension and diabetes in humanitarian crises: systematic review** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8128021/
   Includes humanitarian NCD care contexts relevant to Yemen.

2. **Interventions targeting hypertension and diabetes in LMICs: scoping review** — https://pmc.ncbi.nlm.nih.gov/articles/PMC6873661/
   Community/PHC interventions across LMICs.


### Zambia — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Zambia has a national NCD surveillance system implemented by the Ministry of Health, with WHO STEPS surveys in 2017 and a 2023 adapted-STEPS survey across 149 ART clinics in 52 districts. The TASKPEN trial in Lusaka is piloting a WHO PEN-based integrated HIV-NCD care package addressing hypertension, diabetes and dyslipidemia. Zambia is a long-standing DHIS2 country for its national HMIS, though search results did not surface a published account of DHIS2 Tracker for NCDs specifically.

DHIS2 USE: LIKELY
Zambia's HMIS is widely known to be DHIS2-based and would routinely capture aggregate NCD indicators; explicit cardiometabolic-specific DHIS2 Tracker deployment was not confirmed in these search results.

#### Search Results

##### English query results
1. **Prevalence and risk factors of hypertension and diabetes among PLHIV in Zambia (J Int AIDS Soc, 2025)** — https://onlinelibrary.wiley.com/doi/10.1002/jia2.70051
   National cross-sectional facility-based survey using adapted WHO STEPS in 149 ART clinics.

2. **Zambia Country Report (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9562810/
   Country-level NCD surveillance and STEPS 2017 data.

3. **TASKPEN trial protocol, Lusaka, Zambia (ResearchGate)** — https://www.researchgate.net/publication/381230973
   WHO PEN-based integrated HIV-NCD care stepped-wedge trial.


### Zanzibar — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Following a STEPS survey and Service Provision Assessment, the Zanzibar Ministry of Health established an NCD unit and program, with decentralisation of selected services to primary care, guidelines for hypertension and diabetes detection and treatment, and supply ordering of antihypertensive/anti-diabetic medications. Challenges remain in medicines/equipment stockouts (e.g., glucose strips) and data on treatment outcomes are sparse. Tanzania (mainland) runs a DHIS2 national HMIS and Zanzibar uses related digital systems; explicit DHIS2 Tracker NCD case management in Zanzibar was not directly confirmed in this search.

DHIS2 USE: LIKELY
Zanzibar has institutionalised NCD surveillance and PHC service delivery, and Tanzania's HMIS ecosystem is DHIS2-based; a dedicated cardiometabolic DHIS2 Tracker deployment for Zanzibar was not explicitly documented in the surfaced sources.

#### Search Results

##### Swahili / English query results
1. **Hypertension and diabetes in Zanzibar – prevalence and access to care (BMC Public Health)** — https://bmcpublichealth.biomedcentral.com/articles/10.1186/s12889-020-09432-8
   Population study describing Zanzibar MoH NCD program, decentralisation, and treatment access gaps.

2. **Same article, PMC** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7472575/
   Full-text version.

3. **Same article, PubMed** — https://pubmed.ncbi.nlm.nih.gov/32887593/
   PubMed indexing.

4. **Same article, German National Library** — https://d-nb.info/1220213578/34
   Preprint mirror.

5. **Integrating hypertension data into routine DHIS2 — CARDIO4Cities Dakar, Senegal** — https://link.springer.com/article/10.1007/s44250-025-00240-8
   Comparator African DHIS2 hypertension integration case.


### Zimbabwe — Cardiometabolic (Diabetes/Hypertension/CVD) & DHIS2 Profile

#### Summary
Zimbabwe has actively implemented integrated hypertension and diabetes screening within its HIV programme, with data captured in a customised DHIS2 Tracker Capture Android application on tablets/phones in Bulawayo and Chitungwiza (Dec 2022 – Dec 2024). The ZiNCoDs national STEPS survey provides NCD risk-factor surveillance, WHO PEN modules support rural facility screening/treatment, and MSF/MoHCC have piloted a nurse-led HTN/DM care model in Manicaland.

DHIS2 USE: CONFIRMED
A medRxiv 2025 study explicitly documents DHIS2 Tracker Capture used on mobile devices for individual-level cardiometabolic screening data in Zimbabwe's integrated HIV-NCD program.

#### Search Results

##### English query results
1. **Burden of HTN and DM Among PLHIV and General Population in Two Urban Cities in Zimbabwe (medRxiv 2025)** — https://www.medrxiv.org/content/10.1101/2025.10.07.25337477v1.full
   Describes customised DHIS2 Tracker Capture on tablets/phones for integrated HTN/DM screening data.

2. **Setting up a nurse-led HTN/DM model of care in rural Zimbabwe (BMC Health Services Research)** — https://link.springer.com/article/10.1186/s12913-020-05351-x
   MSF/MoHCC nurse-led model description in Manicaland.

3. **Community awareness of diet for HTN and T2DM in Hatcliffe, Zimbabwe (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC6916094/
   Community knowledge/behaviour study.

4. **MoHCC receives medical supplies for NCD screening (WHO AFRO)** — https://www.afro.who.int/countries/zimbabwe/news/ministry-health-receives-medical-supplies-strengthen-screening-and-treatment-patients-non
   WHO PEN-supported NCD screening strengthening in rural facilities.

5. **Zimbabwe National Survey NCD Risk Factors (ZiNCoDs STEPS)** — https://cdn.who.int/media/docs/default-source/ncds/ncd-surveillance/data-reporting/zimbabwe/steps/steps_zimbabwe_data.pdf
   National STEPS NCD risk factor survey report.

