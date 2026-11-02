# DHIS2 for NCD Data — Africa CDC SPARK-NCD Member States

_Consolidated country report · Global July 2026 status re-checked by web search on 2 November 2026_

This report pulls together the DHIS2 NCD country profiles for the 11 SPARK-NCD member states: Republic of Botswana, Kingdom of Eswatini, Federal Democratic Republic of Ethiopia, Republic of Kenya, Kingdom of Lesotho, Republic of Namibia, Republic of Rwanda, Federal Republic of Somalia, Republic of South Sudan, United Republic of Tanzania, Republic of Zambia. It covers three NCD domains (cardiometabolic, cancer, chronic respiratory disease) plus an overall NCD summary. Each country × domain pair has two separate ratings: **system maturity** (how deeply DHIS2 captures the data) and **evidence level** (how well a source supports that rating).

Baseline ratings come from structured, multi-language NCD web searches in July 2026 (cardiometabolic, cancer and CRD country profiles); each original profile is included below under its country and domain. For this report every country × domain was searched again on 2 November 2026. A rating was changed only when a specific new source justified it. Changed cells are marked **†** and the earlier rating is listed under [Changes since the baseline](#changes-since-the-baseline). These changes apply **only to this report**; the source profiles are unchanged.

### How to read the ratings

**System maturity** describes how deeply DHIS2 captures data for the condition. **Evidence level** describes how directly the sources support that maturity rating. For example, Tanzania cancer is rated Tracker (pilot) · Indicative: a DHIS2 oncology tracker is being piloted, but the documentation is still indirect.

| System maturity | Meaning |
|---|---|
| **Tracker** | Patient- or case-level data captured in DHIS2 Tracker (e.g. a cancer registry, NCD screening tracker). |
| **Aggregate** | Routine counts for the condition (screening, cases, treatment) reported in DHIS2 data sets. |
| **HMIS-only** | DHIS2 is the national HMIS, but no source shows this condition's data being captured in it. |
| **None** | No DHIS2 use for this condition. |
| **Unknown** | Cannot be assessed from available sources. |

A qualifier in parentheses shows deployment status: **pilot** (limited sites or scope), **planned** (in an official plan, not yet live), **partial** (only part of the condition or only some facilities, e.g. cervical screening but no registry), or **abandoned**. No qualifier means operational.

| Evidence level | Meaning |
|---|---|
| **Confirmed** | A named source (government report, peer-reviewed paper, programme document) states that DHIS2 captures this condition's data. |
| **Indicative** | Indirect support only: DHIS2 is the HMIS and the condition is likely reported, or the capture is planned or piloted. |
| **Absent** | Searched, but no evidence of DHIS2 capture for this condition was found. |
| **Unknown** | Not assessed. |

The **All NCDs** summary takes the deepest maturity across the three domains, together with that domain's evidence level. Sources exclude dhis2.org pages. Government reports, peer-reviewed papers and WHO, Africa CDC and UNICEF documents from the last five years were preferred.

## Contents

- [Summary table](#summary-table)
- [Counts](#counts)
- [Changes since the baseline](#changes-since-the-baseline)
- [Country profiles](#country-profiles)
  - [Botswana](#botswana)
  - [Eswatini](#eswatini)
  - [Ethiopia](#ethiopia)
  - [Kenya](#kenya)
  - [Lesotho](#lesotho)
  - [Namibia](#namibia)
  - [Rwanda](#rwanda)
  - [Somalia](#somalia)
  - [South Sudan](#south-sudan)
  - [Tanzania](#tanzania)
  - [Zambia](#zambia)

## Summary table

Format: Maturity · Evidence. † = changed by the November 2026 re-check.

| Country | All NCDs (summary) | Cardiometabolic | Cancer | CRD |
|---|---|---|---|---|
| [Botswana](#botswana) | **HMIS-only** · Indicative | HMIS-only · Indicative | HMIS-only · Indicative | HMIS-only · Indicative † |
| [Eswatini](#eswatini) | **Aggregate (planned)** · Indicative | HMIS-only · Indicative | Aggregate (planned) · Indicative | HMIS-only · Indicative |
| [Ethiopia](#ethiopia) | **Aggregate** · Confirmed | Aggregate · Confirmed | Aggregate · Confirmed † | Aggregate · Confirmed |
| [Kenya](#kenya) | **Tracker** · Confirmed | Aggregate · Confirmed | Tracker · Confirmed | Aggregate (partial) · Confirmed † |
| [Lesotho](#lesotho) | **HMIS-only** · Indicative | HMIS-only · Indicative | HMIS-only · Indicative † | HMIS-only · Indicative |
| [Namibia](#namibia) | **HMIS-only** · Indicative | HMIS-only · Indicative | HMIS-only · Absent | HMIS-only · Indicative |
| [Rwanda](#rwanda) | **Tracker** · Confirmed | Tracker · Confirmed † | Tracker · Confirmed | Aggregate · Confirmed |
| [Somalia](#somalia) | **HMIS-only (planned)** · Indicative | HMIS-only (planned) · Indicative | HMIS-only · Absent | HMIS-only · Indicative |
| [South Sudan](#south-sudan) | **Aggregate** · Confirmed † | Aggregate · Confirmed † | Aggregate (partial) · Confirmed † | Aggregate · Confirmed † |
| [Tanzania](#tanzania) | **Tracker (pilot)** · Indicative † | Aggregate · Confirmed | Tracker (pilot) · Indicative † | Aggregate (partial) · Confirmed † |
| [Zambia](#zambia) | **Aggregate (partial)** · Confirmed | Aggregate · Indicative † | Aggregate (partial) · Confirmed | HMIS-only · Indicative |

_Summary = the deepest maturity across the three domains, with that domain's evidence level._

## Counts

### System maturity

| Maturity | All NCDs | Cardiometabolic | Cancer | CRD |
|---|---:|---:|---:|---:|
| Tracker | 3 | 1 | 3 | 0 |
| Aggregate | 4 | 5 | 4 | 5 |
| HMIS-only | 4 | 5 | 4 | 6 |
| None | 0 | 0 | 0 | 0 |
| Unknown | 0 | 0 | 0 | 0 |
| **Total** | 11 | 11 | 11 | 11 |

### Evidence level

| Level | All NCDs | Cardiometabolic | Cancer | CRD |
|---|---:|---:|---:|---:|
| Confirmed | 5 | 5 | 5 | 5 |
| Indicative | 6 | 6 | 4 | 6 |
| Absent | 0 | 0 | 2 | 0 |
| Unknown | 0 | 0 | 0 | 0 |
| **Total** | 11 | 11 | 11 | 11 |

Non-operational qualifiers (counted under their maturity level above): Eswatini Cancer (planned); Kenya CRD (partial); Somalia Cardiometabolic (planned); South Sudan Cancer (partial); Tanzania Cancer (pilot); Tanzania CRD (partial); Zambia Cancer (partial).

## Changes since the baseline

| Country | Domain | Baseline | Re-check | Reason |
|---|---|---|---|---|
| Botswana | CRD | Unknown · Unknown | HMIS-only · Indicative | Upgraded from Unknown. MEASURE Evaluation confirms all public facilities report through DHIS2, the national aggregate HMIS. No asthma or COPD capture found, so HMIS-only, matching the cardio and cancer rows. |
| Ethiopia | Cancer | Aggregate (pilot) · Indicative | Aggregate · Confirmed | Upgraded from Aggregate (pilot) · Indicative. The HISP Ethiopia webinar deck (May 2024, slides 7-8, 13) lists cervical cancer screening (by method and result), treatment and one-year outcomes in the routine HMIS datasets entered into DHIS2. Breast cancer is captured through ESV-ICD11 morbidity reporting (slide 10). The national cancer registry launched in 2025 does not name a platform. |
| Kenya | CRD | Aggregate · Confirmed | Aggregate (partial) · Confirmed | Qualifier changed from operational to partial. The MoH lung-health assessment of 199 facilities (reported April 2026) found CRDs poorly represented in health information systems: asthma/COPD registers in only 6%/5% of facilities, post-TB lung disease not recorded. KHIS asthma counts via MOH 705 remain. |
| Lesotho | Cancer | HMIS-only · Absent | HMIS-only · Indicative | Evidence raised from Absent to Indicative. A 2024 MoH/CDC paper says the eRegister EMR covers cervical cancer screening and its data are aggregated into DHIS2 (automated reporting confirmed only for HIV/TB/PMTCT). Indirect; no cancer dataset documented. |
| Rwanda | Cardiometabolic | Aggregate · Confirmed | Tracker · Confirmed | Upgraded from Aggregate. Since July 2022, hypertension and diabetes screening has run through a DHIS2 NCD tracker (HISP Rwanda/RBC), with positive cases referred. The MoH 2024/25 report cites 'NCDs Tracker' data and full DHIS2 integration. The tracker covers screening; clinical follow-up stays in OpenMRS. The HISP Rwanda webinar deck (2024, slides 10-11) names the “NCD Screening Tracker (Hypertension and Diabetes)” for individual-level data. |
| South Sudan | Cardiometabolic | HMIS-only · Indicative | Aggregate · Confirmed | Upgraded from HMIS-only · Indicative. The MoH 2024/25 performance report tabulates CVD, diabetes and hypertension service counts by state for 2021–2025, sourced from DHIS2, and states hypertension detection is 'reported through DHIS2' (public facilities only, incomplete). |
| South Sudan | Cancer | HMIS-only · Absent | Aggregate (partial) · Confirmed | Upgraded from HMIS-only · Absent. The 2025 Health Statistics Abstract (Table 9) gives cervical cancer OPD cases by state for 2021–24, sourced from DHIS2; cancer also appears in DHIS2 inpatient diagnoses. There is no national cancer database, hence partial. |
| South Sudan | CRD | HMIS-only · Indicative | Aggregate · Confirmed | Upgraded from HMIS-only · Indicative. Performance report Table 24 gives asthma OPD cases by state for 2021–25 (23,810 rising to 39,558), sourced from DHIS2. The 2025 Abstract lists COPD and asthma in DHIS2 inpatient case-fatality tables. |
| South Sudan | All NCDs | HMIS-only · Indicative | Aggregate · Confirmed | Recomputed from domain changes. |
| Tanzania | Cancer | HMIS-only · Indicative | Tracker (pilot) · Indicative | Upgraded from HMIS-only. A KCMC hospital pilot of the DHIS2 Oncology Tracker is under way. In Feb 2026 the University of Dar es Salaam DHIS2 Lab ran a national cancer registry workshop that demonstrated the DHIS2 Oncology module integrated with CanReg5. Not yet national. |
| Tanzania | CRD | HMIS-only · Indicative | Aggregate (partial) · Confirmed | Upgraded from HMIS-only · Indicative. Tanzania’s national HMIS runs on DHIS2, and MoH HMIS outpatient diagnosis tables by council (2013–2014) report Bronchial Asthma counts. Partial: COPD is not a separate diagnosis, and recent studies describe CRD variables in DHIS2 as incomplete. |
| Tanzania | All NCDs | Aggregate · Confirmed | Tracker (pilot) · Indicative | Recomputed from domain changes. |
| Zambia | Cardiometabolic | HMIS-only · Indicative | Aggregate · Indicative | Maturity upgraded from HMIS-only. The MoH NHSP 2022–2026 M&E Framework lists indicator 2.3.8 'Incidence of NCDs' (hypertension, diabetes) with a 2021 HMIS baseline and HMIS (DHIS2-based) as source. It does not name DHIS2 directly, so evidence stays Indicative. |

## Country profiles

### Botswana

_Republic of Botswana_

**All NCDs — Maturity:** HMIS-only · **Evidence:** Indicative (driven by cardiometabolic)

#### Botswana — Cardiometabolic (diabetes / hypertension / CVD)

**Maturity:** HMIS-only · **Evidence:** Indicative

DHIS2 is confirmed as the national aggregate HMIS; NCD-specific cardiometabolic indicators in DHIS2 are likely but not directly cited in surfaced literature.

> **Re-check (2 November 2026):** Kept. DHIS2 is the aggregate HMIS for all public facilities. The 2024 STEPS survey (WHO/MoH, 2025) gives survey data on hypertension and diabetes, but no source shows those indicators captured in DHIS2.
>
> - [Botswana: HIS Indicators (MEASURE Evaluation country profile)](https://measureevaluation.cpc.unc.edu/his-strengthening-resource-center/country-profiles-1/botswana.html) (2019)
> - [What the STEPS survey reveals about health in Botswana (WHO AFRO)](https://www.afro.who.int/fr/node/22762) (2025)

<details>
<summary>Original cardiometabolic search profile: summary and results</summary>

**Summary**
Botswana uses DHIS2 for aggregate routine HMIS reporting alongside OpenMRS for clinic-level EMR. CVD causes ~18% and diabetes ~6% of mortality; hypertension drives 8.9% of outpatient morbidity. STEPS 2014 provides the most recent national NCD risk-factor data. The InterCARE cluster RCT (2024) is testing integrated hypertension/CVD care within HIV services at 14 sites — leveraging the existing digital infrastructure.

Original single-token verdict (superseded by the two-axis rating above): LIKELY
DHIS2 is confirmed as the national aggregate HMIS; NCD-specific cardiometabolic indicators in DHIS2 are likely but not directly cited in surfaced literature.

**Search Results**

*English query results*
1. **Botswana's Healthcare System: From HIV Response to Health (Kapsule Tech)** — https://kapsuletech.com/blog/botswana-healthcare-system/
   Describes DHIS2 for aggregate HMIS and OpenMRS for EMR in Botswana.

2. **Prevalence of and factors associated with hypertension, diabetes, stroke and heart attack multimorbidity in Botswana: STEPS 2014 (PLOS ONE)** — https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0265722
   National STEPS-based multimorbidity analysis.

3. **Botswana STEPS Survey 2014 final report (WHO)** — https://cdn.who.int/media/docs/default-source/ncds/ncd-surveillance/data-reporting/botswana/steps/steps-botswana-2014-report-final.pdf
   Official 2014 STEPS NCD report.
*Setswana query results*
4. **Prevalence of HTA, diabetes multimorbidity Botswana STEPS 2014 (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8947240/
   PMC version of multimorbidity study.

</details>

#### Botswana — Cancer

**Maturity:** HMIS-only · **Evidence:** Indicative

Botswana uses DHIS2 for general health information management, malaria surveillance, and supply chain data. No evidence was found of cancer-specific data collection, screening tracking, or cancer registry functions within DHIS2.

> **Re-check (2 November 2026):** Kept. No source links the Botswana National Cancer Registry or cervical screening data to DHIS2. The 2022–2024 screening studies used medical records.

<details>
<summary>Original cancer search profile: summary and results</summary>

**Summary**
Botswana uses DHIS2 as its national health management information system, including for malaria case-based surveillance and supply chain management. However, no evidence was found that DHIS2 is specifically used for cancer screening or cancer registry functions. The Botswana National Cancer Registry (BNCR) was established in 1999 as a population-based registry endorsed by IARC, collecting data from referral and district hospitals through traditional methods. Cervical cancer burden is high due to HIV co-infection, and a See-and-Treat programme is expanding, but these programmes do not appear to be integrated into DHIS2.

Original single-token verdict (superseded by the two-axis rating above): LOW
Botswana uses DHIS2 for general health information management, malaria surveillance, and supply chain data. No evidence was found of cancer-specific data collection, screening tracking, or cancer registry functions within DHIS2.

**Search Results**

*English query results*
(Query: Botswana cancer DHIS2 screening registry)

1. **Botswana National Cancer Registry (AFCRN)** -- https://afcrn.org/membership/members/118-bncr
   Profile of the BNCR, a population-based cancer registry established in 1999 under the Ministry of Health with IACR/IARC support, collecting data from hospitals across the country.

2. **Cancer Care and Prevention -- Rutgers Global Health** -- https://globalhealth.rutgers.edu/where-we-work/botswana/cancer-care-and-prevention/
   Overview of Rutgers' cancer care and prevention partnerships in Botswana, including cervical cancer screening initiatives.

3. **Cervical Cancer in Botswana: Current State and Future Steps for Screening and Treatment Programs** -- https://pmc.ncbi.nlm.nih.gov/articles/PMC4630577/
   Review of cervical cancer burden in Botswana, noting high HIV co-infection rates and limited screening infrastructure.

4. **Trends in Cancer Incidence and Associated Risk Factors in People Living with and Without HIV in Botswana: Population-Based Cancer Registry Data Analysis 1990-2021** -- https://www.mdpi.com/2072-6694/17/14/2374
   Analysis of 30 years of BNCR data examining cancer incidence trends by HIV status.

*Setswana query results*
(Query: Botswana cancer DHIS2 kankere)

5. **Factors Related to Advanced Stage of Cancer Presentation in Botswana** -- https://ascopubs.org/doi/10.1200/JGO.18.00129
   Study examining why cancers are frequently diagnosed at advanced stages in Botswana, identifying barriers to early detection.

</details>

#### Botswana — Chronic respiratory disease (COPD / asthma)

**Maturity:** HMIS-only · **Evidence:** Indicative †

CRD burden and research documented, but sources confirm neither DHIS2 as national HMIS nor any DHIS2 CRD capture, leaving both axes unassessable.

> **Re-check (2 November 2026):** Upgraded from Unknown. MEASURE Evaluation confirms all public facilities report through DHIS2, the national aggregate HMIS. No asthma or COPD capture found, so HMIS-only, matching the cardio and cancer rows.
>
> - [Botswana: HIS Indicators (MEASURE Evaluation country profile)](https://measureevaluation.cpc.unc.edu/his-strengthening-resource-center/country-profiles-1/botswana.html) (2019)

<details>
<summary>Original CRD search profile: summary and results</summary>

**Summary**
Botswana has a documented CRD burden — high asthma prevalence among Gaborone schoolchildren and long-standing epidemiological work on adult lung disease — and is a participating site in the multi-country CHEST-Africa observatory on asthma and COPD. However, no source in these searches confirmed DHIS2 as Botswana's national HMIS or linked any Botswana CRD data to DHIS2. The evidence is ambiguous: CRD care and research clearly exist, but the country's HMIS platform and any DHIS2 CRD capture remain unestablished.

Original single-token verdict (superseded by the two-axis rating above): UNCLEAR
CRD burden and research are documented, but neither DHIS2 as the national HMIS nor any DHIS2 CRD capture could be confirmed from the sources found.

**Search Results**

*English query results*
1. **Prevalence of asthma among school children in Gaborone, Botswana | PMC** — https://pmc.ncbi.nlm.nih.gov/articles/PMC5111997/
   Reports high asthma prevalence and symptom severity among schoolchildren in Gaborone.
2. **The epidemiology of adult lung disease in Botswana | PubMed** — https://pubmed.ncbi.nlm.nih.gov/11495270/?dopt=Abstract
   Epidemiological overview of adult lung disease burden in Botswana.
3. **Chronic respiratory disease observatory for Africa (CHEST-Africa): study protocol | PMC** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11256067/
   Multi-country asthma/COPD prevalence and economic-impact study protocol covering African sites.
4. **Availability of diagnostic services and essential medicines for non-communicable respiratory diseases in African countries | PMC** — https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7987206/
   Regional assessment of spirometry and COPD/asthma essential-medicine availability relevant to Botswana.
5. **Chronic respiratory diseases | WHO Regional Office for Africa** — https://www.afro.who.int/health-topics/chronic-respiratory-diseases
   WHO AFRO overview of CRD burden and response across the African region.

</details>

[↑ Summary table](#summary-table)

---

### Eswatini

_Kingdom of Eswatini_

**All NCDs — Maturity:** Aggregate (planned) · **Evidence:** Indicative (driven by cancer)

#### Eswatini — Cardiometabolic (diabetes / hypertension / CVD)

**Maturity:** HMIS-only · **Evidence:** Indicative

WHO-PEN hypertension/diabetes care plus DHIS2 national HMIS, but no source confirms an operational cardiometabolic DHIS2 module.

> **Re-check (2 November 2026):** Kept. NCDIMH Plan 2024–28 says NCD data are 'largely generated from' CMIS (EMR), with indicators drawn from CMIS and HMIS-inpatient. CMIS syncs to DHIS2 dashboards, but NHSSP says DHIS2 is not yet fully operational. Indirect only.
>
> - [Eswatini National Health Sector Response Plan for the Prevention and Control of NCDs, Injuries and Mental Health 2024–2028](https://www.afro.who.int/sites/default/files/2026-02/NCD%20Report_0.pdf) (2024)
> - [Eswatini Client Management Information System (ECMIS) - Khulisa Management Services](https://www.khulisa.com/khulisa-projects/khulisa-project-list-item/eswatini-client-management-information-system/)
> - [Kingdom of Eswatini National Health Sector Strategic Plan 2024/25–2027/28](https://www.afro.who.int/sites/default/files/2025-02/NHSSP%20ESWATINI%20.pdf) (2024)

<details>
<summary>Original cardiometabolic search profile: summary and results</summary>

**Summary**
Eswatini is a major implementer of WHO-PEN, having piloted decentralised hypertension/diabetes care and now scaling via the EU-funded WHO-PEN@Scale cluster-randomised trial. The country is also preparing to deploy PEN-Plus for severe NCDs (type 1 diabetes, sickle cell, RHD). A 2024 STEPS survey provided current risk-factor data. Eswatini's NCD desk guide covers clinic-level NCD case management. Eswatini's national HMIS uses DHIS2, but available sources do not explicitly confirm a dedicated DHIS2 cardiometabolic/HEARTS module is operational.

Original single-token verdict (superseded by the two-axis rating above): MODERATE
Eswatini uses DHIS2 nationally for HMIS and runs a robust WHO-PEN program. Public sources don't directly confirm DHIS2 cardiometabolic modules but integration is plausible given WHO-PEN@Scale's M&E requirements.

**Search Results**

*English query results*
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

</details>

#### Eswatini — Cancer

**Maturity:** Aggregate (planned) · **Evidence:** Indicative

DHIS2 is national HMIS; registry uses CanReg5, but elimination plan documents cervical cancer/LEEP data integration into DHIS2 for 2025-2026.

> **Re-check (2 November 2026):** Kept. NCCP 2025–2029 budgets integrating at least 5 priority cancer indicators into DHIS2/EMR. WHO's Q4-2025 newsletter lists integrating cervical/LEEP data into DHIS2 as a next step. Still planned, and the registry still uses CanReg5.
>
> - [The Kingdom of Eswatini National Cancer Control Plan 2025-2029](https://www.iccp-portal.org/sites/default/files/2026-09/Final%20Eswatini%20NCCP%202025-2029_compressed.pdf) (2025)
> - [WHO Eswatini Newsletter December 2025 Issue 4](https://www.afro.who.int/sites/default/files/2026-01/ESWATINI%20NEWSLETTER%20Q4%202025%20.pdf) (2025)

<details>
<summary>Original cancer search profile: summary and results</summary>

**Summary**
Eswatini (formerly Swaziland) uses DHIS2 as its national health management information system. The country has one of Africa's highest cervical cancer rates, compounded by high HIV prevalence. A national cancer registry was re-established in 2015 using CanReg5 software. While a Cervical Cancer Elimination Acceleration Plan (2024-2028) is underway with plans to integrate cervical cancer data into DHIS2, no cancer-specific DHIS2 modules are currently documented as operational.

Original single-token verdict (superseded by the two-axis rating above): NO EVIDENCE
Despite DHIS2 being the national HMIS and active cervical cancer elimination efforts, no evidence was found of operational DHIS2-based cancer data collection. Plans to integrate cervical cancer LEEP procedure data into DHIS2 were noted for 2025-2026, but the cancer registry currently uses CanReg5 separately.

**Search Results**

*English query results*
1. **Eswatini Cervical Cancer Elimination Acceleration Plan 2025-2030** — https://www.iccp-portal.org/sites/default/files/2025-07/Eswatini%20Cervical%20Cancer%20Elimination%20Acceleration%20Plan%202025-2030.pdf
   National plan targeting WHO 90-70-90 cervical cancer elimination goals, with mention of integrating data into DHIS2.

2. **Strengthening Capacity of Healthcare Workers to Fast-Track Cervical Cancer Elimination in Eswatini (WHO AFRO)** — https://www.afro.who.int/countries/eswatini/news/strengthening-capacity-healthcare-workers-fast-track-cervical-cancer-elimination-eswatini
   WHO report on LEEP training program (October 2025) with next steps including DHIS2 data integration.

3. **Eswatini National Cancer Registry (AFCRN)** — https://afcrn.org/index.php/membership/membership-list/146-swazilandncr
   African Cancer Registry Network profile of Eswatini's re-established national cancer registry using CanReg5 software.

4. **Cervical Cancer Elimination Acceleration Plan in Eswatini (ICCP Portal)** — https://www.iccp-portal.org/news/cervical-cancer-elimination-acceleration-plan-eswatini
   Portal entry for Eswatini's cervical cancer elimination strategy and acceleration plan.

5. **Eswatini, burdened doubly with HIV and cervical cancer, targets safety for the next generation (Gavi)** — https://www.gavi.org/vaccineswork/eswatini-burdened-doubly-hiv-and-cervical-cancer-targets-safety-next-generation
   Gavi article on Eswatini's dual HIV-cervical cancer burden and HPV vaccination efforts.

</details>

#### Eswatini — Chronic respiratory disease (COPD / asthma)

**Maturity:** HMIS-only · **Evidence:** Indicative

National DHIS2 use plus asthma-inclusive NCD pilot care, but no source links DHIS2 to CRD data capture.

> **Re-check (2 November 2026):** Kept. NCDIMH Plan 2024–28 reports asthma diagnoses (1,225 in 2021, 2,463 in 2022) from CMIS daily morbidity reports, and says recent COPD data are unavailable. CMIS syncs to DHIS2 dashboards, but no source says DHIS2 captures CRD data.
>
> - [Eswatini National Health Sector Response Plan for the Prevention and Control of NCDs, Injuries and Mental Health 2024–2028](https://www.afro.who.int/sites/default/files/2026-02/NCD%20Report_0.pdf) (2024)

<details>
<summary>Original CRD search profile: summary and results</summary>

**Summary**
Eswatini is an active DHIS2-using country (health-sector reporting, plus a DHIS2-based education EMIS) and has documented NCD activity relevant to CRD: repeated WHO STEPS risk-factor surveys (2014 and 2024), a pilot decentralising uncomplicated NCD care — including asthma, per the COMDIS-HSD NCD care package — to nurse-led clinics in Lubombo region, and PEN-Plus rollout for severe NCDs. However, no surfaced source explicitly names DHIS2 capture of COPD/asthma/CRD data.

Original single-token verdict (superseded by the two-axis rating above): MODERATE
DHIS2 is in national use and NCD/asthma care and surveillance activity is well documented, but no source explicitly links DHIS2 to CRD data capture.

**Search Results**

*English query results*
1. **Decentralising NCD management in rural southern Africa: evaluation of a pilot implementation study — BMC Public Health** — https://bmcpublichealth.biomedcentral.com/articles/10.1186/s12889-019-7994-4
   Evaluation of decentralised, nurse-led management of uncomplicated NCDs (including asthma under the COMDIS-HSD package) in a rural Eswatini district using clinic registers.
2. **Decentralising non-communicable disease care in the Kingdom of Eswatini: pilot study in Lubombo region — COMDIS-HSD** — https://comdis-hsd.leeds.ac.uk/resource/decentralising-non-communicable-disease-care-in-the-kingdom-of-eswatini-successes-challenges-and-recommendations-from-a-pilot-study-in-lubombo-region/
   Policy resource summarising successes, challenges, and recommendations from the Lubombo NCD decentralisation pilot.
3. **Person-centred, integrated NCD and HIV decentralized drug distribution in Eswatini and South Africa** — https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10323318/
   Documents integration of NCD care into decentralized drug distribution channels in Eswatini, indicating maturing chronic-disease service delivery.
4. **Bringing care for severe noncommunicable diseases closer to communities in Eswatini — WHO AFRO** — https://www.afro.who.int/countries/eswatini/news/bringing-care-severe-noncommunicable-diseases-closer-communities-eswatini
   Describes PEN-Plus decentralisation of severe NCD management in Eswatini.
5. **STEPS Survey 2024 — WHO AFRO** — https://www.afro.who.int/publications/steps-survey-2024
   Country report for Eswatini's latest nationwide WHO STEPwise NCD risk-factor survey.
6. **Eswatini — STEPS 2014 — WHO NCD Microdata Repository** — https://extranet.who.int/ncdsmicrodata/index.php/catalog/688
   Earlier national STEPS survey establishing NCD risk-factor surveillance in Eswatini.
7. **Surveillance of noncommunicable diseases — Eswatini — WHO** — https://www.who.int/teams/noncommunicable-diseases/surveillance/data/eswatini
   WHO country surveillance page summarising Eswatini's NCD surveillance instruments.

*siSwati query results*
No relevant siSwati-language results. The query returned only regional asthma/COPD clinical guidelines (South Africa) and a DHIS2 education-sector report; no siSwati-language CRD-DHIS2 content was found.

</details>

[↑ Summary table](#summary-table)

---

### Ethiopia

_Federal Democratic Republic of Ethiopia_

**All NCDs — Maturity:** Aggregate · **Evidence:** Confirmed (driven by cardiometabolic)

#### Ethiopia — Cardiometabolic (diabetes / hypertension / CVD)

**Maturity:** Aggregate · **Evidence:** Confirmed

Nationwide DHIS2 HMIS; NORAD/WHO evaluations report >99% reporting completeness for NCD services — operational aggregate capture.

> **Re-check (2 November 2026):** Kept. RTSL 2026 fact sheet: the Simple App has been interoperable with Ethiopia's DHIS2 since 2021 and covers hypertension and diabetes in 683 facilities (160,000+ patients). Patient-level data stays in Simple, not a DHIS2 tracker, so Aggregate holds. The HISP Ethiopia webinar deck (2024, slides 7-8) lists hypertension, CVD-risk and diabetes screening, treatment, cohort and outcome data elements in the routine HMIS datasets.
>
> - [NCD M&E in Ethiopia — HISP Ethiopia, HISP Community Webinar “NCD Data in National HMIS”](https://drive.google.com/drive/folders/1-6WVy2IsVxvzPpXnG8j_9BHpEDXpc13i) (2024)
> - [Resolve to Save Lives – Updated Ethiopia Country Profile Fact Sheet 2026](https://resolvetosavelives.org/wp-content/uploads/2026/03/Updated_Ethiopia-Country-Profile_Fact-Sheet_2026.pdf) (2026)

<details>
<summary>Original cardiometabolic search profile: summary and results</summary>

**Summary**
Ethiopia has implemented DHIS2 nationwide as the core HMIS for the Federal Ministry of Health and regional health bureaus, and in 2020 launched the Ethiopia Hypertension Control Initiative (EHCI) based on WHO HEARTS. Multiple peer-reviewed studies evaluate HEARTS rollout, hypertension surveillance in Addis Ababa pilots, and the WHO PEN package for primary care NCD management. NORAD/WHO midterm evaluations report strong reporting completeness (>99%) for NCD services in Ethiopian facilities.

Original single-token verdict (superseded by the two-axis rating above): CONFIRMED
DHIS2 is Ethiopia's national HMIS, and recent literature documents its use for tracking HEARTS hypertension control and NCD service delivery indicators at health facilities.

**Search Results**

*Amharic / English query results*
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

</details>

#### Ethiopia — Cancer

**Maturity:** Aggregate · **Evidence:** Confirmed †

Mature national DHIS2; IARC-HISP initiative onboarding routine cancer indicator reporting, but integration in progress rather than operational proof.

> **Re-check (2 November 2026):** Upgraded from Aggregate (pilot) · Indicative. The HISP Ethiopia webinar deck (May 2024, slides 7-8, 13) lists cervical cancer screening (by method and result), treatment and one-year outcomes in the routine HMIS datasets entered into DHIS2. Breast cancer is captured through ESV-ICD11 morbidity reporting (slide 10). The national cancer registry launched in 2025 does not name a platform.
>
> - [NCD M&E in Ethiopia — HISP Ethiopia, HISP Community Webinar “NCD Data in National HMIS”](https://drive.google.com/drive/folders/1-6WVy2IsVxvzPpXnG8j_9BHpEDXpc13i) (2024)
> - [Launch of the National Cancer Registry and National Cancer Control Plan (REACCT-CAN, Addis Ababa University)](https://reacct-can.aau.edu.et/news_detail/4/) (2025)

<details>
<summary>Original cancer search profile: summary and results</summary>

**Summary**
Ethiopia has DHIS2 deployed nationwide as its Health Management Information System (HMIS) since 2019, with the system demonstrating high maturity and positive impacts on maternal and child health service performance. Ethiopia has one population-based cancer registry (Addis Ababa City Cancer Registry, established 2011) covering 3 million people, and recently completed a landmark cervical cancer screening campaign (275,607 women screened, 103% of target). The WHO-IARC-HISP collaboration is actively supporting cancer data integration into DHIS2, with Ethiopia identified as part of the global initiative to enable routine facility reporting of cancer indicators through DHIS2 for cancer prevention, diagnosis, treatment, and care monitoring.

Original single-token verdict (superseded by the two-axis rating above): MODERATE
Ethiopia has a mature national DHIS2 implementation and is actively participating in the global IARC-HISP initiative to integrate cancer indicators into routine DHIS2 reporting. While the existing Addis Ababa cancer registry predates this integration, Ethiopia is onboarding cancer as a new domain for routine facility reporting in DHIS2, enabling tracking of medicine sources, treatment reach, and program outcomes. Enhanced registers and digital reporting platforms from the recent cervical cancer campaign will be integrated into routine health information systems.

**Search Results**

*English query results*
1. **First data from a population based cancer registry in Ethiopia (ScienceDirect)** — https://www.sciencedirect.com/science/article/abs/pii/S1877782118300183
   Describes the Addis Ababa City Cancer Registry established in September 2011, the only population-based cancer registry in Ethiopia, covering a population of just over three million.

2. **"A national milestone in women's health" - Ethiopia concludes Month-Long Cervical Cancer Campaign (WHO AFRO)** — https://www.afro.who.int/countries/ethiopia/news/national-milestone-womens-health-ethiopia-concludes-month-long-cervical-cancer-campaign-record
   Reports 275,607 women screened (103% of target), with enhanced registers and digital reporting platforms to be integrated into routine health information systems.

3. **National Cancer Control Plan of Ethiopia 2025-2029** — https://www.iccp-portal.org/sites/default/files/plans/NCCP%20Ethiopia%20Final%20%20Submitted%20%20PDF_1.pdf
   Ethiopia's strategic plan focusing on three high-burden cancers (breast, cervical, and childhood cancers) with proven interventions for maximum public health impact.

4. **Strengthening Ethiopia's Health Information System: A Journey to Unified DHIS2 (Ethiopian Journal of Health Development)** — https://ejhd.org/index.php/ejhd/article/view/6302
   Documents Ethiopia's DHIS2 implementation journey, harmonization strategy, and digital health blueprint following "one plan, one budget, one report" principle.

5. **Level of implementation of district health information system 2 at public health facilities in Eastern Ethiopia (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9554126/
   Peer-reviewed study on DHIS2 implementation levels across Ethiopian health facilities.

6. **Maturity Assessment of District Health Information System Version 2 Implementation in Ethiopia (JMIR)** — https://medinform.jmir.org/2024/1/e50375
   Assesses Ethiopia's DHIS2 maturity at the "defined stage" (score=2.81) with plans to reach "manageable stage" (score=4.09) by 2025.

7. **Assessing the influence of the health system on access to cervical cancer prevention, screening, and treatment services in Addis Ababa (PLOS One)** — https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0300152
   Study examining cervical cancer services at 51 health centers in Addis Ababa, finding 79% provide awareness messages, 80% treat precancer lesions, and 71% perform cervical screening.

8. **Contributions of DHIS2 to maternal and child health service performance in Ethiopia (Archives of Public Health)** — https://archpublichealth.biomedcentral.com/articles/10.1186/s13690-025-01641-0
   Interrupted time series mixed-methods study documenting positive impacts of DHIS2 on MCH indicators including institutional delivery rates and service coverage.

</details>

#### Ethiopia — Chronic respiratory disease (COPD / asthma)

**Maturity:** Aggregate · **Evidence:** Confirmed

Published study extracted DHIS2 quarterly-reported asthma/COPD cases (2013-2019, Amhara), confirming routine aggregate CRD reporting in national DHIS2.

> **Re-check (2 November 2026):** Kept. Re-confirmed the peer-reviewed time-series study that used DHIS2 quarterly-reported asthma/COPD cases (2013–2019, three Amhara zones). Found nothing newer that changes the rating. The HISP Ethiopia webinar deck (2024, slide 10) confirms asthma is captured in national ESV-ICD11 morbidity and mortality reporting.
>
> - [NCD M&E in Ethiopia — HISP Ethiopia, HISP Community Webinar “NCD Data in National HMIS”](https://drive.google.com/drive/folders/1-6WVy2IsVxvzPpXnG8j_9BHpEDXpc13i) (2024)
> - [Trends, seasonal variations and forecasting of chronic respiratory disease morbidity in charcoal producing areas, northwest Ethiopia: time series analysis](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC11774925/) (2025)

<details>
<summary>Original CRD search profile: summary and results</summary>

**Summary**
Ethiopia uses DHIS2 as its national HMIS (Federal Ministry of Health), and peer-reviewed research explicitly documents extraction of DHIS2 quarterly-reported chronic respiratory disease cases — asthma and COPD — from Amhara region zones (Awi, East Gojjam, West Gojjam) for 2013–2019 trend and forecasting analysis. This is direct, named evidence that routine CRD morbidity data are captured and reported through DHIS2 in Ethiopia, alongside a substantial body of Ethiopian CRD clinical research.

Original single-token verdict (superseded by the two-axis rating above): CONFIRMED
A published time-series study explicitly analysed DHIS2 quarterly-reported asthma, COPD, and CRD cases (2013–2019) from Ethiopia's routine reporting system.

**Search Results**

*Amharic query results*
1. **Disease Severity and Medication Use Among Patients with Persistent Asthma at Tikur Anbessa Specialized Hospital (ASAP)** — https://researchgate.net/publication/351309112_Disease_Severity_and_Medication_Use_Among_Patients_with_Persistent_Asthma_Seen_at_Tikur_Anbessa_Specialized_Hospital_TASH_in_Ethiopia_Selective_Findings_from_the_African_Severe_Asthma_Project_ASAP
   Hospital-based severe asthma cohort in Addis Ababa under the African Severe Asthma Project, part of an emerging Ethiopian respiratory patient registry effort.
2. **Early readmission and its predictors among patients treated for acute exacerbations of chronic obstructive respiratory disease in Ethiopia** — https://www.researchgate.net/publication/344499023_Early_readmission_and_its_predictors_among_patients_treated_for_acute_exacerbations_of_chronic_obstructive_respiratory_disease_in_Ethiopia_A_prospective_cohort_study
   Prospective cohort documenting chronic obstructive respiratory disease care and outcomes in Ethiopian hospitals.
3. **Mortality and its predictors among patients treated for acute exacerbations of chronic obstructive respiratory diseases in Jimma Medical Center — PLOS ONE** — https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0239055
   Observational study of chronic obstructive respiratory disease exacerbation outcomes at a major Ethiopian referral hospital.

*English query results*
4. **Trends, seasonal variations and forecasting of chronic respiratory disease morbidity in charcoal producing areas, northwest Ethiopia: time series analysis** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11774925/
   Key evidence: retrospective study extracting DHIS2 quarterly-reported asthma, COPD, and CRD cases (2013–2019) from three Amhara zones, confirming routine CRD reporting flows through DHIS2 to zonal, regional, and federal levels.
5. **DHIS2 — Ministry of Health, Ethiopia** — https://www.moh.gov.et/projects-3-col/dhis2
   Official FMOH page confirming DHIS2 as Ethiopia's national health information platform.
6. **Unveiling the role of DHIS2 in enhancing data quality and accessibility in primary healthcare facilities: Evidence from Ethiopia** — https://www.ncbi.nlm.nih.gov/pmc/articles/PMC11654967/
   Study of DHIS2 data quality and use at Ethiopian primary healthcare facilities.
7. **Routine data in a primary care performance dashboard, Ethiopia** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11197649/
   Describes use of DHIS2 routine data in Ethiopian primary care performance monitoring.
8. **Health system's availability and readiness of health facilities for chronic non-communicable diseases: Evidence from the Ethiopian national surveys** — https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10890768/
   National facility survey evidence on readiness to deliver chronic NCD (including CRD) services in Ethiopia.
9. **Improving the detection and management of non-communicable diseases in a rural primary healthcare unit in Sidama, Ethiopia: study protocol** — https://www.medrxiv.org/content/10.1101/2024.04.11.24305685.full.pdf
   Protocol for strengthening NCD detection and management at primary care level in southern Ethiopia.

</details>

[↑ Summary table](#summary-table)

---

### Kenya

_Republic of Kenya_

**All NCDs — Maturity:** Tracker · **Evidence:** Confirmed (driven by cancer)

#### Kenya — Cardiometabolic (diabetes / hypertension / CVD)

**Maturity:** Aggregate · **Evidence:** Confirmed

NCD indicators reported into KHIS and SPICE integrates aggregated monthly NCD data into DHIS2 — operational aggregate capture, explicitly stated.

> **Re-check (2 November 2026):** Kept. Found no new Kenya-specific KHIS NCD documentation. The 2025 Nairobi study notes MoH concern about weak use of DHIS2 data for diabetes/hypertension. The SHA/DHA hospital HMIS migration concerns EMRs, not KHIS replacement. The prior SPICE→KHIS source still stands.
>
> - [Routine health information systems in diabetes and hypertension management, Nairobi (Kenyatta University repository)](https://ir-library.ku.ac.ke/handle/123456789/31425) (2025)
> - [From SHA portal to HMIS: what you need to know of the Duale big switch (Nation)](https://www.nation.africa/kenya/health/from-sha-portal-to-hmis-what-you-need-to-know-of-the-duale-big-switch--5618322) (2025)

<details>
<summary>Original cardiometabolic search profile: summary and results</summary>

**Summary**
Kenya runs KHIS (Kenya Health Information System), a national DHIS2 platform deployed in 2011. NCD indicators (hypertension, diabetes) are reported into KHIS, and the SPICE platform (Medtronic LABS partnership with MoH) integrates primary-care NCD data into DHIS2 for aggregated monthly reporting. Indicators draw from the HEARTS Technical Package.

Original single-token verdict (superseded by the two-axis rating above): CONFIRMED
KHIS is Kenya's national DHIS2 instance and ingests NCD/hypertension/diabetes data from SPICE and routine reporting.

**Search Results**

*English query results*
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

</details>

#### Kenya — Cancer

**Maturity:** Tracker · **Evidence:** Confirmed

Kenya has a fully operational DHIS2-based National Cancer Registry (NaCaRE-KE) launched in 2021, with comprehensive cancer data collection including patient demographics, diagnosis, staging, treatment, and survivorship. The system has been evaluated for total cost of ownership and end-user perception, with published research in peer-reviewed journals.

> **Re-check (2 November 2026):** Kept. A peer-reviewed 2025 cost/user-perception study confirms NaCaRE-KE is DHIS2-based. A 2026 report says NCI-K's EMPOWER screening platform links to NaCaRe; the article does not name DHIS2.
>
> - [Determining the total cost of ownership and end user perception of the Kenya National Cancer Registry (NaCaRE-KE): a DHIS2-based digital health system](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC12011077/) (2025)
> - [Digital tracking and early screening key to tackling Kenya's cancer burden (EWN)](https://www.ewn.co.za/2026/03/16/digital-tracking-and-early-screening-key-to-tackling-kenyas-cancer-burden) (2026)

<details>
<summary>Original cancer search profile: summary and results</summary>

**Summary**
Kenya's National Cancer Registry (NaCaRE-KE) is built on DHIS2, launched in 2021 through an Act of Parliament establishing the National Cancer Institute of Kenya (NCI-K). The system collects patient demographics, diagnosis, staging, treatment, and survivorship data across health facilities. Published research documents the total cost of ownership of the DHIS2-based system. The live system is accessible at nacare.ncikenya.go.ke. Kenya is one of the most advanced implementations of DHIS2 for cancer registration globally.

Original single-token verdict (superseded by the two-axis rating above): HIGH
Kenya has a fully operational DHIS2-based National Cancer Registry (NaCaRE-KE) launched in 2021, with comprehensive cancer data collection including patient demographics, diagnosis, staging, treatment, and survivorship. The system has been evaluated for total cost of ownership and end-user perception, with published research in peer-reviewed journals.

**Search Results**

*English query results*
1. **Determining the total cost of ownership and end user perception of the Kenya National Cancer Registry (NaCaRE-KE): a DHIS2-based digital health System (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC12011077/
   Peer-reviewed research estimating the total cost of ownership of the NaCaRE-KE system across five facilities in Nairobi County, received June 2024, accepted March 2025.

2. **Determining the total cost of ownership and end user perception of the Kenya National Cancer Registry (NaCaRE-KE) (Oxford Academic)** — https://academic.oup.com/oodh/article/doi/10.1093/oodh/oqaf007/8099176
   Oxford Open Digital Health publication of the NaCaRE-KE total cost of ownership and user perception study.

3. **National Cancer Registry of Kenya System (NaCaRe-KE) (Digital Impact Exchange)** — https://exchange.dial.global/projects/national-cancer-registry-of-kenya-system-nacareke
   Digital Impact Exchange profile of the NaCaRE-KE system documenting its DHIS2-based architecture and implementation.

4. **National Cancer Institute Kenya** — https://www.ncikenya.go.ke/
   Official website of the National Cancer Institute of Kenya, the custodian of the NaCaRE-KE system.

5. **About Us — National Cancer Institute Kenya** — https://ncikenya.go.ke/about/
   Background on NCI Kenya's establishment through Act of Parliament and its mandate for cancer surveillance and control.

6. **The National Cancer Control Strategy 2023-2027 (Kenya Ministry of Health)** — http://guidelines.health.go.ke:8000/media/NATIONAL_CANCER_CONTROL_STRATEGY_2023-2027_7uTQQP4.pdf
   Kenya's national cancer control strategy document outlining the strategic framework including the NaCaRE-KE registry system.

7. **Cancer Registry — KEMRI** — https://www.kemri.go.ke/cancer-registry/
   Kenya Medical Research Institute cancer registry page documenting cancer surveillance research activities.

8. **Determining the Total cost of ownership of the Kenya National Cancer Registry (ResearchGate)** — https://www.researchgate.net/publication/390327545_Determining_the_Total_cost_of_ownership_and_end_user_perception_of_the_Kenya_National_Cancer_Registry_NaCaRE_-KE_a_DHIS2-based_digital_health_system
   ResearchGate listing of the NaCaRE-KE total cost of ownership study.

9. **National Cancer Institute of Kenya on X** — https://x.com/ncikenya?lang=en
   NCI Kenya's social media presence sharing updates on cancer surveillance and registry operations.

</details>

#### Kenya — Chronic respiratory disease (COPD / asthma)

**Maturity:** Aggregate (partial) · **Evidence:** Confirmed †

KHIS (DHIS2) captures asthma via MOH 705 outpatient morbidity; CHAI reviewed KHIS asthma/COPD data; KEMRI adding CRD indicators.

> **Re-check (2 November 2026):** Qualifier changed from operational to partial. The MoH lung-health assessment of 199 facilities (reported April 2026) found CRDs poorly represented in health information systems: asthma/COPD registers in only 6%/5% of facilities, post-TB lung disease not recorded. KHIS asthma counts via MOH 705 remain.
>
> - [Non-TB lung patients left behind in Kenya – MoH (The Star)](https://www.the-star.co.ke/news/2026-04-01-non-tb-lung-patients-left-behind-in-kenya-moh) (2026)

<details>
<summary>Original CRD search profile: summary and results</summary>

**Summary**
Kenya's national HMIS is the Kenya Health Information System (KHIS), a DHIS2 platform in use since 2011, fed by the MOH 705 daily outpatient morbidity summaries in which asthma is a reportable condition. CHAI's iPACT Lung Health Program explicitly reviewed national reporting systems including KHIS for available asthma and COPD data, and KEMRI documents ongoing work to incorporate additional asthma, COPD, and chronic lung disease indicators into KHIS.

Original single-token verdict (superseded by the two-axis rating above): CONFIRMED
Kenya's DHIS2-based KHIS captures asthma via MOH 705 outpatient morbidity reporting, and the CHAI iPACT programme explicitly names KHIS as a national reporting system reviewed for asthma/COPD data, with KEMRI documenting further CRD indicator integration into KHIS.

**Search Results**

*English query results*
1. **Chronic Respiratory Diseases — KEMRI** — https://www.kemri.go.ke/chronic-respiratory-diseases/ [BROKEN: 500]
   Kenya Medical Research Institute page stating efforts are underway to incorporate indicators for asthma, COPD, and other chronic lung diseases into the Kenya Health Information System (KHIS).
2. **Lung Health Program (iPACT) — CHAI Kenya Repository** — https://www.chaiportal.org/program/details?summary=44
   Programme description reporting a review of national reporting systems, including EMR and KHIS, to assess available data on TB, asthma, COPD, and post-TB lung disease.
3. **KHIS/DHIS2 Aggregate — MOH Virtual Academy (Ministry of Health Kenya)** — https://elearning.health.go.ke/course/index.php?categoryid=5
   Official MoH e-learning courses confirming KHIS Aggregate is powered by DHIS2 and used for national health data entry and analysis.
4. **MOH 705A Daily Outpatient Morbidity Summary — completed reporting form** — https://www.studocu.com/row/document/kenya-medical-training-college/community-health-nursing/moh-705a-september-summary-for-reporting/12640338
   Example of the MOH 705A outpatient morbidity summary — the facility reporting tool (including asthma as a morbidity line) whose data is entered into KHIS/DHIS2.
5. **Transforming the Kenya Health Information System (KHIS) to an Early Warning and Real-Time Electronic Disease Notification System** — https://www.researchgate.net/publication/351283367_Transforming_the_Kenya_Health_Information_System_KHIS_to_an_Early_Warning_and_Real-Time_Electronic_Disease_Notification_System_Optimization_for_Epidemiology_Disease_Surveillance_and_Response_in_Kenya
   Describes KHIS (DHIS2) as Kenya's routine reporting platform since 2011 and its optimization for disease surveillance.
6. **Chronic respiratory disease in adult outpatients in three African countries: a cross-sectional study** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8734192/
   Multi-country outpatient study (including Kenya) showing asthma, COPD, and chronic bronchitis dominate chronic respiratory diagnoses and noting suboptimal spirometry-based diagnosis.
7. **Perceived accuracy and utilisation of DHIS2 data for health decision making and advocacy in Kenya (PLOS Global Public Health)** — https://journals.plos.org/globalpublichealth/article?id=10.1371/journal.pgph.0004508
   Qualitative study across 15 counties on the quality and use of Kenya's DHIS2 data for decision-making.
8. **Centre for Respiratory Diseases Research (CRDR), Nairobi — KEMRI** — https://www.kemri.go.ke/centre-for-respiratory-diseases-research-crdr-nairobi/ [BROKEN: 500]
   KEMRI's dedicated research centre covering non-communicable respiratory diseases including COPD and asthma.

</details>

[↑ Summary table](#summary-table)

---

### Lesotho

_Kingdom of Lesotho_

**All NCDs — Maturity:** HMIS-only · **Evidence:** Indicative (driven by cardiometabolic)

#### Lesotho — Cardiometabolic (diabetes / hypertension / CVD)

**Maturity:** HMIS-only · **Evidence:** Indicative

Lesotho has a national DHIS2/HMIS; routine NCD aggregate indicators likely flow through it, though no cardiometabolic-specific module/registry was confirmed in this search.

> **Re-check (2 November 2026):** Kept. PEN-Plus adopted in 2025 (51 staff oriented in six districts), but no data-system detail. A 2024 MoH/CDC paper confirms DHIS2 as the national aggregate HMIS fed by eRegister; expansion to NCD areas is still planned.
>
> - [From disease specific to universal health coverage in Lesotho: successes and challenges encountered in Lesotho's digital health journey (Oxford Open Digital Health)](https://academic.oup.com/oodh/article/doi/10.1093/oodh/oqae021/7712269) (2024)
> - [Bringing Lifesaving care closer: Lesotho advances NCD response with PEN-Plus (WHO AFRO)](https://www.afro.who.int/countries/lesotho/news/bringing-lifesaving-care-closer-lesotho-advances-ncd-response-pen-plus) (2025)

<details>
<summary>Original cardiometabolic search profile: summary and results</summary>

**Summary**
Lesotho faces a double burden of NCDs alongside HIV/TB. Recent research (ComBaCaL trials) tested CHW-led home-based screening for diabetes and hypertension using a tablet-based clinical decision support app — not DHIS2. While Lesotho operates DHIS2 as its national HMIS for routine reporting, no source directly confirms a cardiometabolic-specific DHIS2 module.

Original single-token verdict (superseded by the two-axis rating above): LIKELY
Lesotho has a national DHIS2/HMIS; routine NCD aggregate indicators likely flow through it, though no cardiometabolic-specific module/registry was confirmed in this search.

**Search Results**

*English query results*
1. **Feasibility and Acceptability of Diabetes and Hypertension Screening by CHWs in Rural Lesotho: Mixed-Methods Pilot (Annals of Global Health)** — https://annalsofglobalhealth.org/articles/10.5334/aogh.4738
   ComBaCaL pilot in Butha-Buthe and Mokhotlong using a tablet-based CDSS for CHW-led HTN/DM screening.

2. **Feasibility and Acceptability of CHW-led Screening in Lesotho (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC12439129/
   Companion PMC version of the ComBaCaL pilot.

3. **Awareness, treatment, and control among adults with hypertension or diabetes in two rural districts in Lesotho (PLOS GPH)** — https://journals.plos.org/globalpublichealth/article?id=10.1371/journal.pgph.0003721
   Population-level study of HTN/DM cascade of care in rural Lesotho.

</details>

#### Lesotho — Cancer

**Maturity:** HMIS-only · **Evidence:** Indicative †

DHIS2 covers all 333 facilities for HIV/TB/maternal programs, but no cancer-specific module is documented.

> **Re-check (2 November 2026):** Evidence raised from Absent to Indicative. A 2024 MoH/CDC paper says the eRegister EMR covers cervical cancer screening and its data are aggregated into DHIS2 (automated reporting confirmed only for HIV/TB/PMTCT). Indirect; no cancer dataset documented.
>
> - [From disease specific to universal health coverage in Lesotho: successes and challenges encountered in Lesotho's digital health journey (Oxford Open Digital Health)](https://academic.oup.com/oodh/article/doi/10.1093/oodh/oqae021/7712269) (2024)

<details>
<summary>Original cancer search profile: summary and results</summary>

**Summary**
Lesotho uses DHIS2 as its national health management information system, rolled out across all ten districts and covering all 333 health facilities with over 40 million records. The country has a high burden of cervical cancer driven by high HIV rates. A Digital Health Strategy 2025-2030 was validated in December 2024. Lesotho falls within the GICR sub-Saharan Africa Hub covering 48 countries for cancer registry development. Despite comprehensive DHIS2 coverage for health programs including HIV, TB, and maternal health, no cancer-specific DHIS2 modules are documented.

Original single-token verdict (superseded by the two-axis rating above): NO EVIDENCE
Lesotho has comprehensive DHIS2 implementation across all health facilities for multiple health programs, but no evidence was found of DHIS2 being used specifically for cancer registration, screening data, or cancer surveillance.

**Search Results**

*English query results*
1. **In Lesotho, New Health Information System Provides Streamlined, Integrated Data Across Health Programs (ICAP at Columbia University)** — https://icap.columbia.edu/news-events/in-lesotho-new-health-information-system-provides-streamlined-integrated-data-across-health-programs/
   ICAP report on Lesotho's DHIS2 rollout across all 333 health facilities covering HIV, TB, maternal health, and other programs.

2. **Improving DHIS2 Integration for Lesotho's Health Information Ecosystem (DHIS2 Community)** — https://community.dhis2.org/t/improving-dhis2-integration-for-lesothos-health-information-ecosystem/47463
   DHIS2 Community discussion on improving DHIS2 integration across Lesotho's health information systems.

3. **Lesotho validates its Digital Health Strategy 2025-2030 (WHO AFRO)** — https://www.afro.who.int/countries/lesotho/news/lesotho-validates-its-digital-health-strategy-2025-2030-step-towards-transforming-healthcare-through
   WHO report on Lesotho's validation of its Digital Health Strategy 2025-2030 in December 2024.

4. **Determinants of cervical cancer screening uptake in Lesotho: evidence from 2024 Demographic and Health Survey (BMC Public Health)** — https://bmcpublichealth.biomedcentral.com/articles/10.1186/s12889-025-23593-4
   Research on cervical cancer screening determinants in Lesotho using 2024 DHS data, highlighting low screening coverage despite high cervical cancer burden.

5. **Lesotho — GLOBOCAN Factsheet (IARC)** — https://gco.iarc.fr/today/data/factsheets/populations/426-lesotho-fact-sheets.pdf
   IARC GLOBOCAN fact sheet providing cancer incidence and mortality estimates for Lesotho.

</details>

#### Lesotho — Chronic respiratory disease (COPD / asthma)

**Maturity:** HMIS-only · **Evidence:** Indicative

DHIS2 is Lesotho's national HMIS and NCD reporting expansion is underway, but no CRD-specific data capture or programme documentation surfaced — the link is inferential.

> **Re-check (2 November 2026):** Kept. No new evidence on CRD data systems. WHO's 2025 PEN-Plus release only notes the growing chronic respiratory illness burden. Digital expansion to all NCD areas is still planned (OUP 2024). Link to DHIS2 remains inferential.

<details>
<summary>Original CRD search profile: summary and results</summary>

**Summary**
Lesotho's Ministry of Health, with ICAP support, deployed DHIS2 as the national web-based HMIS covering all 10 districts and all 333 health facilities, and new Millennium Challenge Corporation investment is expanding digital reporting to all communicable and non-communicable disease areas. NCD service activity is documented (e.g., community diabetes/hypertension screening), but no source links Lesotho's DHIS2 to chronic respiratory disease (COPD/asthma) data capture, and the Sesotho-language search returned no relevant results.

Original single-token verdict (superseded by the two-axis rating above): LOW EVIDENCE
DHIS2 is Lesotho's national HMIS and NCD reporting expansion is underway, but no CRD-specific data capture or programme documentation surfaced — the link is inferential.

**Search Results**

*English query results*
1. **In Lesotho, New Health Information System Provides Streamlined, Integrated Data Across Health Programs — ICAP at Columbia University** — https://icap.columbia.edu/news-events/in-lesotho-new-health-information-system-provides-streamlined-integrated-data-across-health-programs/
   Describes the national DHIS2 rollout across all ten districts and 333 health facilities as Lesotho's unified reporting platform.
2. **From disease specific to universal health coverage in Lesotho: successes and challenges encountered in Lesotho's digital health journey** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11932418/
   Reviews Lesotho's digital health evolution, noting MCC investments expanding digital health services to all reported communicable and non-communicable disease areas.
3. **Lesotho — Noncommunicable diseases CCS Indicators (WHO via HDX)** — https://data.humdata.org/dataset/who-data-for-lesotho/resource/a5fb9e24-5aa3-41c3-aac8-8d11bd2d4fee [BROKEN: 404]
   WHO country cooperation strategy NCD indicator dataset for Lesotho.
4. **Feasibility and Acceptability of Diabetes and Hypertension Screening and Diagnosis by Community Health Workers in Rural Lesotho** — https://pmc.ncbi.nlm.nih.gov/articles/PMC12439129/
   Evidence of active NCD screening programmes in Lesotho's primary care and community platforms (cardiometabolic, not CRD-specific).
5. **Toolkit for the analysis and use of facility-based data on noncommunicable diseases — Module 3: DHIS2 user manual (WHO)** — https://www.who.int/publications/i/item/9789240113510
   Global WHO toolkit for facility-based NCD data in DHIS2 — the standard pathway through which DHIS2 countries like Lesotho add NCD/CRD indicators.

*Sesotho query results*
No relevant results. The Sesotho-language query returned only translated general-health pages from outside Lesotho, with no DHIS2 or CRD surveillance content.

</details>

[↑ Summary table](#summary-table)

---

### Namibia

_Republic of Namibia_

**All NCDs — Maturity:** HMIS-only · **Evidence:** Indicative (driven by cardiometabolic)

#### Namibia — Cardiometabolic (diabetes / hypertension / CVD)

**Maturity:** HMIS-only · **Evidence:** Indicative

DHIS2 is Namibia's national routine HMIS; cardiometabolic-specific tracker/registry deployment is not separately documented in indexed sources.

> **Re-check (2 November 2026):** Kept. The 2017 NCD plan names 'NCD register -> PHC reports, DHIS' as the source for hypertension/diabetes control indicators (planned). A 2022 QI study found HTN data reported by Excel workbook because the HIV EMR lacks HTN fields. Nothing newer found.
>
> - [National Multisectoral Strategic Plan for Prevention and Control of NCDs in Namibia (MoHSS)](https://extranet.who.int/ncdccs/Data/NAM_B3_NAMIBIA%20NATIONAL%20MULTISECTORAL%20STRATEGIC%20PLAN%20FOR%20PREVENTION%20AND%20CONTROL%20OF%20NCDs.pdf) (2017)
> - [Integrating hypertension and HIV care in Namibia: A quality improvement collaborative approach (PLOS ONE)](https://journals.plos.org/plosone/article?id=10.1371%2Fjournal.pone.0272727) (2022)

<details>
<summary>Original cardiometabolic search profile: summary and results</summary>

**Summary**
Namibia uses DHIS2 as its national HMIS (rolled out through MoHSS with MEASURE Evaluation support). The country's National Health Policy Framework 2010–2020 prioritizes NCD risk factor surveillance and screening. There is no explicit public documentation of a Namibia-specific DHIS2 cardiometabolic registry or HEARTS package, though aggregate NCD reporting via DHIS2 is plausible.

Original single-token verdict (superseded by the two-axis rating above): LIKELY
DHIS2 is Namibia's national routine HMIS; cardiometabolic-specific tracker/registry deployment is not separately documented in indexed sources.

**Search Results**

*English query results*
1. **Namibia: HIS Indicators — MEASURE Evaluation** — https://www.measureevaluation.org/his-strengthening-resource-center/country-profiles-1/namibia.html
   National HIS profile (Namibia uses DHIS2 for HMIS).

2. **Namibia Country Report — Cardiovascular Journal of Africa (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9562805/
   National CVD response and surveillance overview.

3. **Prevalence and predictors of hypertension in Namibia: A national-level cross-sectional study (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC6147578/
   National hypertension prevalence analysis.

4. **CARDIO4Cities and DHIS2 in Dakar, Senegal** — https://link.springer.com/article/10.1007/s44250-025-00240-8
   Regional reference case.

</details>

#### Namibia — Cancer

**Maturity:** HMIS-only · **Evidence:** Absent

DHIS2 is national HMIS, but NNCR uses CanReg software and cancer data management operates outside DHIS2.

> **Re-check (2 November 2026):** Kept. The national cancer registry (run by the Cancer Association of Namibia) still uses CanReg5, with a 1995–2024 report pending. The 2025 cervical screening campaign used existing registers. No DHIS2 cancer dataset or registry link found.
>
> - [Cancer Registry - Cancer Association of Namibia](https://www.can.org.na/cancerregistry)

<details>
<summary>Original cancer search profile: summary and results</summary>

**Summary**
Namibia uses DHIS2 for its national Health Management Information System. The Namibian National Cancer Registry (NNCR) was established in 1995 as a population-based national cancer registry covering the entire population. The registry is based at the Cancer Association of Namibia in Windhoek, with data collection primarily at the Dr A.B. May Cancer Care Centre of Windhoek Central Hospital. Data management historically used CanReg4 software. The Ministry of Health and Social Services has developed a National Cancer Prevention and Control Plan with WHO support. There are no cancer-specific DHIS2 modules documented for the country, and cancer data management operates outside of the DHIS2 platform.

Original single-token verdict (superseded by the two-axis rating above): NO EVIDENCE
No evidence of cancer-specific DHIS2 modules or cancer registry integration with DHIS2 in Namibia. The NNCR uses CanReg software for cancer data management.

**Search Results**

*English query results*
1. **Cancer Registry — Cancer Association of Namibia** — https://www.can.org.na/?page_id=493
   Official page of the Namibian National Cancer Registry operated by the Cancer Association of Namibia since 1995.

2. **Namibian National Cancer Registry (AFCRN)** — https://afcrn.org/index.php/membership/membership-list/125-ncr
   African Cancer Registry Network membership page for the Namibian National Cancer Registry.

3. **Cancer Prevention and Control in Namibia (WHO AFRO)** — https://www.afro.who.int/news/cancer-prevention-and-control-namibia
   WHO Regional Office for Africa article on cancer prevention and control efforts in Namibia.

4. **Cancer Association of Namibia (UICC)** — https://www.uicc.org/membership/cancer-association-namibia
   UICC membership page for the Cancer Association of Namibia describing cancer control activities.

5. **Namibia — GLOBOCAN Factsheet (IARC)** — https://gco.iarc.fr/today/data/factsheets/populations/516-namibia-fact-sheets.pdf
   IARC GLOBOCAN fact sheet providing cancer incidence and mortality estimates for Namibia.

</details>

#### Namibia — Chronic respiratory disease (COPD / asthma)

**Maturity:** HMIS-only · **Evidence:** Indicative

DHIS2 is Namibia's confirmed national HMIS and CRD care/research exists in-country, but the link between DHIS2 and CRD data is only inferential.

> **Re-check (2 November 2026):** Kept. The 2017 NCD plan uses HMIS mortality data and plans to add NCD indicators to routine HMIS, but nothing specific to CRD. CDC confirms DHIS2 as the aggregate national HIS. No asthma/COPD DHIS2 evidence in 2024–2026 sources.
>
> - [CDC in Namibia](https://www.cdc.gov/global-health/countries/namibia.html)

<details>
<summary>Original CRD search profile: summary and results</summary>

**Summary**
Namibia's Ministry of Health and Social Services (MOHSS) runs its national health information management system on DHIS2, with CDC support for facility-to-national data flows. NCD surveillance activity exists (WHO NCD surveillance country page) and Namibian research on asthma, chronic bronchitis and COPD linked to air pollution has been published, but no CRD-specific national programme or any explicit DHIS2 CRD/COPD/asthma data capture surfaced in searches.

Original single-token verdict (superseded by the two-axis rating above): LOW EVIDENCE
DHIS2 is Namibia's confirmed national HMIS and CRD care/research exists in-country, but the link between DHIS2 and CRD data is only inferential.

**Search Results**

*English query results*
1. **CDC in Namibia (US CDC Global Health)** — https://www.cdc.gov/global-health/countries/namibia.html
   Confirms CDC support to Namibia's MOHSS for the national health information management system (DHIS2), which collects aggregated data from health facilities up to national level.
2. **Surveillance of noncommunicable diseases — Namibia (WHO)** — https://www.who.int/teams/noncommunicable-diseases/surveillance/data/namibia
   WHO country page summarising all NCD surveillance activity (surveys and monitoring) for Namibia.
3. **Ambient air pollution and non-communicable respiratory illness in sub-Saharan Africa: a systematic review (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/35422005/
   Systematic review including a Namibian study on asthma, chronic bronchitis, allergic rhinitis and COPD in relation to air pollution exposure.
4. **The epidemiology of noncommunicable respiratory disease in sub-Saharan Africa, the Middle East, and North Africa (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC5610297/
   Regional review of asthma and COPD epidemiology covering southern Africa, contextualising Namibia's CRD burden.
5. **Toolkit for analysis and use of facility-based data on noncommunicable diseases: Module 3 — DHIS2 user manual (WHO)** — https://www.who.int/publications/i/item/9789240113510
   WHO manual for configuring DHIS2 for NCD (including asthma/COPD) facility data — a global resource relevant to DHIS2 countries such as Namibia, but with no Namibia-specific CRD deployment documented.

</details>

[↑ Summary table](#summary-table)

---

### Rwanda

_Republic of Rwanda_

**All NCDs — Maturity:** Tracker · **Evidence:** Confirmed (driven by cardiometabolic)

#### Rwanda — Cardiometabolic (diabetes / hypertension / CVD)

**Maturity:** Tracker · **Evidence:** Confirmed †

All health centers report NCD screening through national DHIS2; patient-level clinical data lives in OpenMRS, capping maturity at Aggregate.

> **Re-check (2 November 2026):** Upgraded from Aggregate. Since July 2022, hypertension and diabetes screening has run through a DHIS2 NCD tracker (HISP Rwanda/RBC), with positive cases referred. The MoH 2024/25 report cites 'NCDs Tracker' data and full DHIS2 integration. The tracker covers screening; clinical follow-up stays in OpenMRS. The HISP Rwanda webinar deck (2024, slides 10-11) names the “NCD Screening Tracker (Hypertension and Diabetes)” for individual-level data.
>
> - [Non-Communicable Disease Data Management in Rwanda — HISP Rwanda, HISP Community Webinar “NCD Data in National HMIS”](https://drive.google.com/drive/folders/1-6WVy2IsVxvzPpXnG8j_9BHpEDXpc13i) (2024)
> - [NCDs in Rwanda and Sub-Saharan Africa – briefing document (NCD Alliance)](https://forum.ncdalliance.org/wp-content/uploads/2025/02/NCDs-in-Rwanda-and-Sub-Saharan-Africa_briefing-doc-2025.pdf) (2025)
> - [Non-Communicable Diseases Annual Report 2024-2025 (Rwanda MoH/RBC)](https://www.moh.gov.rw/fileadmin/user_upload/Moh/Publications/Reports/new_annual_Report/Non-Communicable_Diseases_Annual_Report_2024-2025_signed.pdf) (2025)

<details>
<summary>Original cardiometabolic search profile: summary and results</summary>

**Summary**
Rwanda is one of the strongest national DHIS2 adopters (DHIS2 is the official national HMIS, hmis.moh.gov.rw) and uses DHIS2 specifically for NCD screening reporting — all health centers were trained on reporting NCD screening through DHIS2. Rwanda has also rolled out the PEN-Plus model for severe NCDs (including diabetes and hypertension) to all 42 districts since 2015, with clinical patient data managed primarily in OpenMRS. STEPS surveys are used for risk-factor surveillance.

Original single-token verdict (superseded by the two-axis rating above): CONFIRMED
DHIS2 is Rwanda's national HMIS and is used for NCD screening reporting (aggregate). Individual NCD/PEN-Plus patient management is in OpenMRS, so DHIS2 covers program monitoring rather than clinical case management for cardiometabolic care.

**Search Results**

*English query results*
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

</details>

#### Rwanda — Cancer

**Maturity:** Tracker · **Evidence:** Confirmed

Rwanda has a fully operational DHIS2 Oncology Tracker integrated with the National Cancer Registry since 2019, with nationwide cancer surveillance, direct facility-level data entry, and CanReg5 interoperability. Rwanda is a pioneer and reference implementation for DHIS2-based cancer registration globally.

> **Re-check (2 November 2026):** Kept and reinforced. The MoH 2024/25 report says the National Cancer Registry has been integrated into DHIS2 since 2019, covering 5 referral, 43 district and 23 private facilities. AFCRN describes the CanReg5→DHIS2 Oncology tracker move. An MoH data-platform doc lists the oncology tracker feed. The HISP Rwanda webinar deck (2024) also lists a Cancer Screening dataset in the HMIS.
>
> - [Non-Communicable Disease Data Management in Rwanda — HISP Rwanda, HISP Community Webinar “NCD Data in National HMIS”](https://drive.google.com/drive/folders/1-6WVy2IsVxvzPpXnG8j_9BHpEDXpc13i) (2024)
> - [Non-Communicable Diseases Annual Report 2024-2025 (Rwanda MoH/RBC)](https://www.moh.gov.rw/fileadmin/user_upload/Moh/Publications/Reports/new_annual_Report/Non-Communicable_Diseases_Annual_Report_2024-2025_signed.pdf) (2025)
> - [Rwanda National Cancer Registry – AFCRN membership profile](https://afcrn.org/index.php/membership/membership-list/167-rwanda-ncr) (2024)
> - [Data Sources – HIC Engineering Library (Rwanda MoH)](https://docs.nhic.moh.gov.rw/en/platform/data-sources/) (2026)

<details>
<summary>Original cancer search profile: summary and results</summary>

**Summary**
Rwanda is a global leader in DHIS2-based cancer registration. The Rwanda National Cancer Registry (RNCR) was re-established in 2018 by the Rwanda Biomedical Centre with NIH, Vital Strategies, IARC, and AFCRN support, and integrated into DHIS2. The DHIS2 Oncology Tracker, introduced in 2019, enables nationwide cancer surveillance with health facility focal persons entering data directly. From 2007 to 2023, 46,801 cancer cases were registered (5,548 in 2023). Data is exported from DHIS2 to CanReg5 for quality checks and analysis. Rwanda, along with Maldives and Jamaica, piloted the DHIS2 cancer registration approach.

Original single-token verdict (superseded by the two-axis rating above): HIGH
Rwanda has a fully operational DHIS2 Oncology Tracker integrated with the National Cancer Registry since 2019, with nationwide cancer surveillance, direct facility-level data entry, and CanReg5 interoperability. Rwanda is a pioneer and reference implementation for DHIS2-based cancer registration globally.

**Search Results**

*English query results*
1. **Rwanda National Cancer Registry (AFCRN)** — https://afcrn.org/index.php/membership/membership-list/167-rwanda-ncr
   African Cancer Registry Network profile of Rwanda's National Cancer Registry.

2. **IARC and HISP Centre collaborate to scale up standardized cancer registries with DHIS2** — Reference to Rwanda as pioneer in DHIS2 cancer registration alongside Maldives and Jamaica.

3. **HISP Rwanda Applications** — https://hisprwanda.org/dhis2-2/applications-2-2/
   HISP Rwanda's DHIS2 applications including cancer registry tools.

4. **About the GICR — E-NNOVATE project** — https://gicr.iarc.fr/news/2023/12/04/e-nnovate-iacr-write-up/
   IARC Global Initiative for Cancer Registry Development including Rwanda's experience.

5. **5th World Congress on Global Healthcare — Marc Hagenimana** — https://www.healthcare.scientexconference.com/speakers/Marc-Hagenimana-
   Presentation on Rwanda's DHIS2 cancer registry experience at international congress.

</details>

#### Rwanda — Chronic respiratory disease (COPD / asthma)

**Maturity:** Aggregate · **Evidence:** Confirmed

MoH NCD report presents asthma patient trends sourced from HMIS with NCD indicators fully integrated into national DHIS2.

> **Re-check (2 November 2026):** Kept. The MoH 2024/25 NCD report charts cumulative asthma patients for 2019–2025 with HMIS as the source, and says NCD indicators are fully integrated into the national DHIS2. Found no CRD tracker or other change.
>
> - [Non-Communicable Diseases Annual Report 2024-2025 (Rwanda MoH/RBC)](https://www.moh.gov.rw/fileadmin/user_upload/Moh/Publications/Reports/new_annual_Report/Non-Communicable_Diseases_Annual_Report_2024-2025_signed.pdf) (2025)

<details>
<summary>Original CRD search profile: summary and results</summary>

**Summary**
Rwanda runs its national HMIS on DHIS2 (rolled out 2012) and has fully integrated NCD indicators into it. The Ministry of Health's Non-Communicable Diseases Annual Report 2024-2025 presents asthma patient trends (2019-2025) with the data source explicitly stated as "Health Information Management System (HMIS)," and the report states NCD indicators were "fully integrated into the national DHIS2 system." This constitutes explicit capture of a chronic respiratory disease (asthma) in Rwanda's DHIS2-based HMIS.

Original single-token verdict (superseded by the two-axis rating above): CONFIRMED
Rwanda's MoH NCD report shows asthma patient-count trends sourced from HMIS, and the report confirms NCD indicators (including asthma) are integrated into the national DHIS2 system.

**Search Results**

*Kinyarwanda query results*
1. **Abakozi ba Leta mu Rwanda hose bagiye gupimwa indwara zitandura (MIFOTRA)** — https://www.mifotra.gov.rw/news-detail/abakozi-ba-leta-mu-rwanda-hose-bagiye-gupimwa-indwara-zitandura
   Government announcement of nationwide NCD screening for public servants, including respiratory conditions, indicating active NCD/CRD service delivery whose data feeds the HMIS.
2. **Indwara zitandura nizo zihitana Abanyarwanda benshi - MINISANTE (Kigali Today)** — https://kigalitoday.com/ubuzima/indwara/article/indwara-zitandura-nizo-zihitana-abanyarwanda-benshi-minisante
   Ministry of Health reporting on the NCD burden (including respiratory disease case counts by year), reflecting routine NCD data monitoring.

*French query results*
3. **The Rwandan Health Management Information System (MSH technical brief)** — https://msh.org/wp-content/uploads/2014/12/ihssp_techbr2_final_webv.pdf
   Describes Rwanda's HMIS upgrade to DHIS2 for routine facility and community health data collection nationwide.
4. **Health management information system (HMIS) data verification: a case study in four districts in Rwanda (PLOS One)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7367468/
   Confirms Rwanda's national routine reporting runs on DHIS2, with high data completeness across facility levels.

*English query results*
5. **Non-Communicable Diseases Annual Report 2024-2025 (Rwanda Ministry of Health)** — https://www.moh.gov.rw/fileadmin/user_upload/Moh/Publications/Reports/new_annual_Report/Non-Communicable_Diseases_Annual_Report_2024-2025_signed.pdf
   Presents asthma patient trends 2019-2025 sourced explicitly from "Health Information Management System (HMIS)" and states NCD indicators were fully integrated into the national DHIS2 system — direct evidence of CRD (asthma) capture in DHIS2.
6. **Chronic respiratory disease observatory for Africa (CHEST-Africa): study protocol** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11256067/
   Multi-country asthma/COPD prevalence and determinants protocol including Rwanda, documenting the CRD burden underpinning Rwanda's surveillance.
7. **Prevalence of atopy, asthma and COPD in an urban and a rural area of an African country** — https://www.sciencedirect.com/science/article/pii/S0954611111002137
   Population study (Kigali and Huye District, Rwanda) quantifying airflow obstruction, asthma and COPD prevalence.
8. **Treating persistent asthma in rural Rwanda: characteristics, management and outcomes** — https://www.ingentaconnect.com/content/iuatld/ijtld/2017/00000021/00000010/art00020
   Documents structured asthma care and outcome tracking in rural Rwanda, consistent with facility-level CRD data collection.

</details>

[↑ Summary table](#summary-table)

---

### Somalia

_Federal Republic of Somalia_

**All NCDs — Maturity:** HMIS-only (planned) · **Evidence:** Indicative (driven by cardiometabolic)

#### Somalia — Cardiometabolic (diabetes / hypertension / CVD)

**Maturity:** HMIS-only (planned) · **Evidence:** Indicative

DHIS2 is national HMIS backbone, but 2025 policy paper only identifies NCD integration as a future need, not current capture.

> **Re-check (2 November 2026):** Kept. No evidence NCD data is captured yet. The WHO Somalia/WDF project for the federal MoH (2025–2028) plans to integrate relevant indicators into DHIS2 and train staff on NCD data reporting, which matches the planned status.
>
> - [Strengthening Somalia's Response to Diabetes and other Non-Communicable Diseases (WDF24-1949, with WHO Somalia)](https://www.worlddiabetesfoundation.org/what-we-do/projects/wdf24-1949/) (2025)

<details>
<summary>Original cardiometabolic search profile: summary and results</summary>

**Summary**
Somalia's national HMIS is built on DHIS2 and recognized as the "national data backbone" for disease surveillance and immunization, although reporting completeness is around 50% and over 60% of facilities lack reliable data-capture systems. A 2025 policy paper explicitly identifies the need to digitally integrate NCD monitoring (hypertension ~33%, diabetes ~20% adult prevalence) into DHIS2 as system functionality matures. The WHO EMRO 2022 HIS assessment confirms DHIS2 as the platform but flags maturity gaps.

Original single-token verdict (superseded by the two-axis rating above): MODERATE
DHIS2 is the national HMIS; NCD/cardiometabolic indicators are not yet fully operationalized within it but are an explicit integration target.

**Search Results**

*English query results*
1. **The Unrecognized Challenge: A Value-Based Policy Approach to Combat NCDs in Somalia's Health Sector (Research Square, 2025)** — https://www.researchsquare.com/article/rs-7925625/v1
   2025 policy paper documenting NCD burden and DHIS2 integration roadmap.

2. **Comprehensive assessment of Somalia's health information system 2022 (WHO EMRO)** — https://applications.emro.who.int/docs/9789292742188-eng.pdf
   WHO EMRO HIS assessment confirming DHIS2 as national HMIS and identifying NCD-data gaps.
*Somali query results*
*(no remaining results after filtering generic dhis2.org links)*

</details>

#### Somalia — Cancer

**Maturity:** HMIS-only · **Evidence:** Absent

UNICEF/HISP-supported DHIS2 with Tracker for HIV/surveillance/immunization, but no cancer registries and no cancer-specific DHIS2 use documented.

> **Re-check (2 November 2026):** Kept. The UICC member profile (updated July 2026) says the MoH is drafting its first National Cancer Control Plan (2026–2030) and 'establishing the national cancer registry'. No platform is named, and no cancer data in DHIS2 was found.
>
> - [Ministry of Health – Somalia (UICC member profile)](https://www.uicc.org/membership/ministry-health-somalia) (2026)

<details>
<summary>Original cancer search profile: summary and results</summary>

**Summary**
Somalia has DHIS2 deployment supported by UNICEF and HISP UiO, with a three-year agreement (from 2023) to strengthen the DHIS2-based health management information system through enhanced technical support, local capacity building, and improved data quality. DHIS2 Tracker is used for disease surveillance, HIV monitoring, and immunization records. There are no formal cancer registries in Somalia, though a 2020 study from a tertiary care hospital in Mogadishu reported on 1,306 cancer cases from 2017-2020. Somalia is not an IACR member. Due to limited cancer infrastructure, there is no cancer-specific DHIS2 use.

Original single-token verdict (superseded by the two-axis rating above): NO EVIDENCE
No evidence of cancer-specific DHIS2 modules in Somalia. No formal cancer registries exist, and DHIS2 deployment focuses on disease surveillance, HIV, and immunization. Very limited cancer infrastructure due to protracted conflict.

**Search Results**

*English query results*
1. **Cancer Incidence and Distribution at a Tertiary Care Hospital in Somalia from 2017 to 2020: An Initial Report of 1306 Cases (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7534047/
   Hospital-based cancer incidence report from a tertiary care facility in Mogadishu, Somalia.

2. **Cancer Registration in the Middle East, North Africa, and Turkey: Scope and Challenges (JCO Global Oncology)** — https://ascopubs.org/doi/10.1200/GO.21.00065
   Review of cancer registration challenges across MENA countries noting Somalia lacks formal cancer registry systems.

3. **The distribution of cancer cases in Somalia (ResearchGate)** — https://www.researchgate.net/publication/320475788_The_distribution_of_cancer_cases_in_Somalia
   Research on the distribution and patterns of cancer cases in Somalia.

4. **Somalia — GLOBOCAN Factsheet (IARC)** — https://gco.iarc.fr/today/data/factsheets/populations/706-somalia-fact-sheets.pdf
   IARC GLOBOCAN fact sheet providing cancer incidence and mortality estimates for Somalia.

</details>

#### Somalia — Chronic respiratory disease (COPD / asthma)

**Maturity:** HMIS-only · **Evidence:** Indicative

DHIS2 underpins national HIMS; NCD/CRD indicator integration into DHIS2 is recommended, not implemented; no confirmed CRD capture.

> **Re-check (2 November 2026):** Kept. No source shows asthma or COPD in Somalia's DHIS2. The planned WHO/WDF NCD indicator integration focuses on diabetes and hypertension.

<details>
<summary>Original CRD search profile: summary and results</summary>

**Summary**
DHIS2 is the platform underpinning Somalia's National Health Information Management System (HIMS), used for routine health data recording and disease surveillance across the federal states. NCD activity including chronic respiratory disease (COPD, asthma) is increasingly visible and WHO reports recommend integrating NCD indicators into the existing DHIS2, but no source confirms DHIS2 currently capturing asthma/COPD data. The link between DHIS2 and CRD data is proposed, not yet realized.

Original single-token verdict (superseded by the two-axis rating above): MODERATE
DHIS2 is confirmed as the national HMIS and CRD care/NCD activity exists, but NCD/CRD integration into DHIS2 is described as a recommendation rather than implemented CRD data capture.

**Search Results**

*Somali query results*
1. **Somali Asthma Cultural Profile — EthnoMed** — https://ethnomed.org/resource/somali-asthma-cultural-profile/
   Documents asthma (nef/az'ma) as a recognized condition in Somali urban and rural settings, evidencing CRD relevance.

*English query results*
2. **Comprehensive assessment of Somalia's health information system 2022 (WHO EMRO)** — https://applications.emro.who.int/docs/9789292742188-eng.pdf [BROKEN: unreachable]
   Describes Somalia's standardized reporting forms and DHIS2 use across the national HIS.
3. **The Silent Epidemic: Strengthening Governance and Health System Resilience against NCDs in Somalia (Research Square)** — https://www.researchsquare.com/article/rs-7923919/v1
   Notes chronic respiratory disease among emerging NCDs and recommends integrating NCD indicators into Somalia's existing DHIS2.
4. **Strengthening Somalia's Health System: Merging NCD and Mental Health Strategies for UHC** — https://www.opastpublishers.com/open-access-articles/strengthening-somalias-health-system-merging-noncommunicable-disease-and-mental-health-strategies-for-uhc-amidst-fragility-9983.html
   States the national HIMS is being standardized on the DHIS2 platform and projects rising asthma drug budget burden.
5. **Somalia Healthcare System: Fragmented Delivery, Recovery (KapsuleTech)** — https://kapsuletech.com/blog/somalia-healthcare-system/
   Overview noting NCDs including chronic respiratory disease present but with thin long-term care models.
6. **Comprehensive assessment of Somalia's HIS 2022 (Heritage Institute baseline study)** — http://www.heritageinstitute.org/wp-content/uploads/2020/05/Somalia-Healthcare-System-A-Baseline-Study-and-Human-Capital-Development-Strategy.pdf
   Baseline HMIS assessment describing performance/capacity constraints of the DHIS2-based system.

</details>

#### Somalia — sub-national entities

These rows are not included in the summary table or the counts.

| Entity | All NCDs | Cardiometabolic | Cancer | CRD |
|---|---|---|---|---|
| Somalia - Puntland State | HMIS-only (planned) · Indicative | HMIS-only (planned) · Indicative | HMIS-only · Absent † | HMIS-only · Indicative |
| Somaliland | HMIS-only · Indicative † | HMIS-only · Indicative † | HMIS-only · Absent † | HMIS-only (partial) · Indicative |

- **Somalia - Puntland State — Cancer:** First assessed in this re-check. No Puntland cancer registry or cancer data in DHIS2 was found, and the federal registry is still being set up. Puntland reports into the DHIS2-based national HMIS.
- **Somaliland — Cardiometabolic:** The MoHD 2022 HMIS review confirms Somaliland's HMIS moved to DHIS2. The Hargeisa NCD clinic keeps paper records (PAMJ 2026). No NCD dataset in DHIS2 was found. [HMIS Review and Assessment Report (Somaliland MoHD)](https://somalilandmohd.com/wp-content/uploads/2023/03/HMIS-Report_Nov-2022_Final-Signed.pdf) [Implementing noncommunicable disease clinics to improve control of hypertension in Somaliland (Pan Afr Med J 54:82)](https://www.panafrican-med-journal.com/content/article/54/82/full/)
- **Somaliland — Cancer:** First assessed in this re-check. Somaliland has no cancer registry; the Needle Hospital clinic uses paper files (ecancer 2025). DHIS2 is the HMIS platform, but no cancer data in it was found. [Patterns of cancer in Needle Hospital, Hargeisa, Somaliland, July 2022–June 2023 (ecancer 19:1904)](https://ecancer.org/en/journal/article/1904-patterns-of-cancer-in-needle-hospital-hargeisa-somaliland-from-july-2022-to-june-2023/pdf)

[↑ Summary table](#summary-table)

---

### South Sudan

_Republic of South Sudan_

**All NCDs — Maturity:** Aggregate · **Evidence:** Confirmed † (driven by cardiometabolic)

#### South Sudan — Cardiometabolic (diabetes / hypertension / CVD)

**Maturity:** Aggregate · **Evidence:** Confirmed †

DHIS2 is the national HMIS and a logical host for any NCD data, but no source confirms operational diabetes/hypertension modules.

> **Re-check (2 November 2026):** Upgraded from HMIS-only · Indicative. The MoH 2024/25 performance report tabulates CVD, diabetes and hypertension service counts by state for 2021–2025, sourced from DHIS2, and states hypertension detection is 'reported through DHIS2' (public facilities only, incomplete).
>
> - [Annual Health Sector Performance Report 2024/25 – Republic of South Sudan MoH (Tables 23, 25, 26)](https://www.afro.who.int/sites/default/files/2025-12/South%20Sudan%20Annual%20health%20sector%20performance%20report_%282024_25%29%20%281%29_0.pdf) (2025)
> - [Knowledge Management Series – Hypertension in South Sudan: A Growing Public Health Crisis (WHO AFRO)](https://www.afro.who.int/sites/default/files/2026-04/Knowledge%20Management%20Series%20-%20Hypertension%20in%20South%20Sudan%20A%20Growing%20Public%20Health%20Crisis%20.pdf) (2026)

<details>
<summary>Original cardiometabolic search profile: summary and results</summary>

**Summary**
South Sudan has used DHIS2 as its electronic HMIS since 2018, scaled to all counties, with active expansion to integrate LMIS, e-TB, EMR and IDSR modules. However, NCD/cardiometabolic surveillance has not been prioritized within the MoH budget, and the main NCD-control challenge is the lack of strategic information on prevalence and risk factors. DHIS2 exists nationally but no public evidence shows cardiometabolic indicators operationalized in it.

Original single-token verdict (superseded by the two-axis rating above): LIKELY
DHIS2 is the national HMIS and a logical host for any NCD data, but no source confirms operational diabetes/hypertension modules.

**Search Results**

*English query results*
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

</details>

#### South Sudan — Cancer

**Maturity:** Aggregate (partial) · **Evidence:** Confirmed †

DHIS2 is national HMIS since 2018, but cancer infrastructure is extremely limited and search surfaced no cancer capture anywhere.

> **Re-check (2 November 2026):** Upgraded from HMIS-only · Absent. The 2025 Health Statistics Abstract (Table 9) gives cervical cancer OPD cases by state for 2021–24, sourced from DHIS2; cancer also appears in DHIS2 inpatient diagnoses. There is no national cancer database, hence partial.
>
> - [2025 Health Statistics Abstract – Republic of South Sudan (MoH/NBS/WHO), Table 9](https://www.afro.who.int/sites/default/files/2025-12/2025%20Health%20Statistical%20Abstract_A4%20%282%29%20%281%29_1.pdf) (2025)

<details>
<summary>Original cancer search profile: summary and results</summary>

**Summary**
South Sudan has used DHIS2 as its electronic health information management system since 2018, scaled up to all counties and selected health facilities. The Boma Health Initiative leverages DHIS2 to strengthen community health. However, South Sudan faces severe health system constraints as one of the world's most fragile states, and there is no evidence of cancer-specific data collection, cancer registries, or cancer surveillance through DHIS2.

Original single-token verdict (superseded by the two-axis rating above): NO EVIDENCE
DHIS2 is the national HMIS in South Sudan since 2018, but no cancer-specific modules, registries, or screening data collection were identified. Cancer infrastructure remains extremely limited.

**Search Results**

*English query results*
1. **DHIS2 International Consultant for South Sudan Ministry of Health (UNGM)** — https://www.ungm.org/Public/Notice/121449
   UN procurement notice for DHIS2 consultant supporting South Sudan MoH.

2. **South Sudan launches DHIS2-based TVET-MIS** — Reference to DHIS2 use for education management information system in South Sudan.

</details>

#### South Sudan — Chronic respiratory disease (COPD / asthma)

**Maturity:** Aggregate · **Evidence:** Confirmed †

DHIS2 is confirmed as the national HMIS, but CRD (COPD/asthma) capture is not documented and NCD integration is only recommended — the link is inferential.

> **Re-check (2 November 2026):** Upgraded from HMIS-only · Indicative. Performance report Table 24 gives asthma OPD cases by state for 2021–25 (23,810 rising to 39,558), sourced from DHIS2. The 2025 Abstract lists COPD and asthma in DHIS2 inpatient case-fatality tables.
>
> - [Annual Health Sector Performance Report 2024/25 – Republic of South Sudan MoH (Table 24)](https://www.afro.who.int/sites/default/files/2025-12/South%20Sudan%20Annual%20health%20sector%20performance%20report_%282024_25%29%20%281%29_0.pdf) (2025)
> - [2025 Health Statistics Abstract – Republic of South Sudan (MoH/NBS/WHO)](https://www.afro.who.int/sites/default/files/2025-12/2025%20Health%20Statistical%20Abstract_A4%20%282%29%20%281%29_1.pdf) (2025)

<details>
<summary>Original CRD search profile: summary and results</summary>

**Summary**
South Sudan has used DHIS2 as its national routine HMIS since 2018, scaled to all counties and selected facilities, and also for IDSR/EWARS surveillance. NCD integration into DHIS2 is being recommended (e.g., hypertension indicators), but CRD activity itself is thin and no source documents asthma/COPD data captured in DHIS2. The platform is confirmed national HMIS; the CRD link is not established.

Original single-token verdict (superseded by the two-axis rating above): LOW EVIDENCE
DHIS2 is confirmed as the national HMIS, but CRD (COPD/asthma) capture is not documented and NCD integration is only recommended — the link is inferential.

**Search Results**

*English query results*
1. **Health Service Monitoring in South Sudan: Routine Health Information Systems (World Bank)** — https://documents1.worldbank.org/curated/en/399871623734227498/pdf/Routine-Health-Information-Systems.pdf [BROKEN: 404]
   Details DHIS2 implementation as South Sudan's routine HMIS, including data quality strengthening.
2. **Heath Service Monitoring in South Sudan Vol. 2 — RHIS and DHIS2 Implementation (Policy Commons)** — https://policycommons.net/artifacts/1806474/heath-service-monitoring-in-south-sudan-vol-2/2539967/
   Confirms MoH use of DHIS2 as the electronic HMIS scaled to all counties since 2018.
3. **Hypertension in South Sudan: A Growing Public Health Crisis (WHO AFRO)** — https://www.afro.who.int/sites/default/files/2026-04/Knowledge%20Management%20Series%20-%20Hypertension%20in%20South%20Sudan%20A%20Growing%20Public%20Health%20Crisis%20.pdf
   Recommends integrating NCD (hypertension) indicators into HMIS/DHIS2 and standardizing reporting.
4. **Integrated disease surveillance and response in humanitarian context: South Sudan experience — PMC** — https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9474851/
   Notes emphasis on DHIS2 and EWARS for community and event-based surveillance under IDSR third edition.
5. **DHIS2 International Consultant for South Sudan Ministry of Health (UNGM)** — https://www.ungm.org/Public/Notice/121449
   Procurement notice confirming ongoing MoH investment in DHIS2 as the national platform.
6. **Management of chronic lung diseases in Sudan and Tanzania: health system readiness — PMC** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8310588/
   Regional CRD health-system readiness study providing context for chronic lung disease care gaps.

</details>

[↑ Summary table](#summary-table)

---

### Tanzania

_United Republic of Tanzania_

**All NCDs — Maturity:** Tracker (pilot) · **Evidence:** Indicative † (driven by cancer)

#### Tanzania — Cardiometabolic (diabetes / hypertension / CVD)

**Maturity:** Aggregate · **Evidence:** Confirmed

Multiple peer-reviewed sources confirm national DHIS2 routinely captures hypertension and diabetes epidemiological data; no patient-level capture named.

> **Re-check (2 November 2026):** Kept. In Oct 2025 the MoH and the Tanzania Diabetes Association presented a GoT-HoMIS NCD module (diabetes and hypertension, piloted in Pwani and Lindi) that feeds indicators to DHIS2 dashboards automatically. Patient-level data stay in that EMR, not in a DHIS2 Tracker.
>
> - [Tanzania races to digital health initiative in fight against NCDs (Daily News)](https://dailynews.co.tz/tanzania-races-to-digital-health-initiative-in-fight-against-ncds/) (2025)

<details>
<summary>Original cardiometabolic search profile: summary and results</summary>

**Summary**
Tanzania introduced DHIS2 in 2013 as its national HMIS and the Ministry of Health hosts its operational instance at dhis.moh.go.tz. Multiple peer-reviewed sources confirm DHIS2 is the routine reporting platform used by the Tanzanian Ministry of Health to capture epidemiological data on NCDs including hypertension and diabetes, and stakeholders involved in NCD/PEN Plus implementation emphasise DHIS2 as central to NCD monitoring. Care-cascade and HIV-NCD integration studies further document substantial cardiometabolic activity feeding routine systems.

Original single-token verdict (superseded by the two-axis rating above): CONFIRMED
Tanzania's national HMIS is DHIS2 (Ministry of Health portal at dhis.moh.go.tz) and it is explicitly used for routine reporting of NCD indicators including hypertension and diabetes; SWOC analyses ahead of PEN Plus implementation reference DHIS2 as the system to strengthen for NCD data.

**Search Results**

*English query results*
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

</details>

#### Tanzania — Cancer

**Maturity:** Tracker (pilot) · **Evidence:** Indicative †

Tanzania has comprehensive DHIS2 deployment for routine health information, but cancer registration operates through separate pilot registries without documented DHIS2 integration.

> **Re-check (2 November 2026):** Upgraded from HMIS-only. A KCMC hospital pilot of the DHIS2 Oncology Tracker is under way. In Feb 2026 the University of Dar es Salaam DHIS2 Lab ran a national cancer registry workshop that demonstrated the DHIS2 Oncology module integrated with CanReg5. Not yet national.
>
> - [National Cancer Registry Workshop: DHIS2 Oncology Module and CanReg5 (UDSM DHIS2 Lab)](https://dhis2.udsm.ac.tz/?p=2615) (2026)

<details>
<summary>Original cancer search profile: summary and results</summary>

**Summary**
Tanzania has a complete national DHIS2 implementation covering all health facilities. A pilot population-based cancer registry exists in Dar es Salaam, and UCSF has supported a project to evaluate cancer registry completeness. However, cancer-specific integration with the DHIS2 platform has not been documented, with cancer registration operating through separate systems.

Original single-token verdict (superseded by the two-axis rating above): LOW
Tanzania has comprehensive DHIS2 deployment for routine health information, but cancer registration operates through separate pilot registries without documented DHIS2 integration.

**Search Results**

*English query results*
1. **Dar es Salaam Population-Based Cancer Registry** — https://pubmed.ncbi.nlm.nih.gov/33045164/
   Description of the pilot population-based cancer registry in Dar es Salaam, documenting establishment, methodology, and initial findings.

2. **UCSF Global Cancer Program — Tanzania** — https://cancer.ucsf.edu/global
   UCSF global cancer program page describing collaborative work in Tanzania to evaluate and strengthen cancer registry completeness.

3. **WHO Cancer Country Profile — Tanzania** — https://www.who.int/publications/m/item/cancer-tza
   WHO summary of cancer burden, infrastructure, and national cancer control status in Tanzania.

4. **GLOBOCAN Fact Sheet — Tanzania** — https://gco.iarc.fr/today/data/factsheets/populations/834-united-republic-of-tanzania-fact-sheets.pdf
   IARC GLOBOCAN estimated cancer incidence and mortality data for the United Republic of Tanzania.

5. **African Cancer Registry Network (AFCRN) — Tanzania** — https://afcrn.org/index.php/activities/membership
   AFCRN membership listing documenting Tanzania's participation in regional cancer registration network activities.

6. **Cancer control in Tanzania: challenges and opportunities** — https://pubmed.ncbi.nlm.nih.gov/30220331/
   Review of cancer control challenges in Tanzania including gaps in registration, diagnosis, and treatment infrastructure.

7. **Ocean Road Cancer Institute — Dar es Salaam** — https://www.orci.or.tz/
   Tanzania's national cancer treatment and referral center, which contributes data to cancer registration efforts.

</details>

#### Tanzania — Chronic respiratory disease (COPD / asthma)

**Maturity:** Aggregate (partial) · **Evidence:** Confirmed †

National DHIS2 HMIS with PEN-Plus CRD care, but studies report CRD variables missing/incomplete in DHIS2; no confirmed CRD dataset.

> **Re-check (2 November 2026):** Upgraded from HMIS-only · Indicative. Tanzania’s national HMIS runs on DHIS2, and MoH HMIS outpatient diagnosis tables by council (2013–2014) report Bronchial Asthma counts. Partial: COPD is not a separate diagnosis, and recent studies describe CRD variables in DHIS2 as incomplete.
>
> - [OPD diagnoses 5 and above years by Council - 2013 (Tanzania MoH HMIS, open.africa)](https://open.africa/dataset/idadi-ya-magonjwa-kutoka-idara-ya-wagonjwa-wa-nje-kwa-mikoa/resource/3caf3ff3-642c-41de-ae52-1f0ca6ad21cc) (2013)
> - [OPD diagnoses 5 and above years by Council - 2014 (Tanzania MoH HMIS, open.africa)](https://africaopendata.org/dataset/idadi-ya-magonjwa-kutoka-idara-ya-wagonjwa-wa-nje-kwa-mikoa/resource/27f37323-de56-4f76-9ea1-6d2e1e201f2e) (2014)

<details>
<summary>Original CRD search profile: summary and results</summary>

**Summary**
Tanzania completed a nationwide DHIS2 rollout in 2013 and uses it as the national HMIS, and it is scaling WHO PEN/PEN-Plus for NCDs including chronic respiratory disease. Studies explicitly note that NCD reporting in DHIS2 faces challenges because variables for specific diseases — including chronic respiratory diseases — are missing or incomplete, indicating CRD is partially represented but not robustly captured.

Original single-token verdict (superseded by the two-axis rating above): MODERATE
DHIS2 is Tanzania's national HMIS and CRD/NCD care activity exists (PEN-Plus, NCD clinics), but the surfaced evidence describes CRD variables as missing/incomplete in DHIS2 rather than a confirmed dedicated CRD dataset.

**Search Results**

*Swahili query results*
1. **Streamlining Disease Surveillance System implementation in Tanzania: Lessons Learnt** — https://pmc.ncbi.nlm.nih.gov/articles/PMC6606137/
   Describes Tanzania's electronic IDSR built on DHIS2 as the backbone of national surveillance reporting.
2. **Improvements in malaria surveillance through the electronic IDSR (eIDSR) system in mainland Tanzania, 2013–2021** — https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9641756/
   Documents the eIDSR/DHIS2 reporting infrastructure and its nationwide reach.

*English query results*
3. **Clinical Audit for Integration of Communicable and Non-Communicable Diseases at the Primary Health Care Level in Tanzania** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11364185/
   Notes health professionals' challenges reporting comprehensive NCD data on DHIS2, including that variables for specific diseases such as chronic respiratory diseases were missing.
4. **Transforming NCD Care Through the Regional PEN-Plus Strategy in Tanzania (WHO AFRO)** — https://www.afro.who.int/countries/united-republic-of-tanzania/news/transforming-ncd-care-through-regional-pen-plus-strategy-advancing-integrated-services-chronic-and
   Tanzania has advanced to national scale-up of WHO PEN for NCDs, the policy vehicle under which CRD care is delivered.
5. **Are Tanzanian health facilities ready to provide management of chronic respiratory diseases? A national survey** — https://journals.plos.org/plosone/article?id=10.1371%2Fjournal.pone.0210350
   National survey showing very limited facility readiness (guidelines, trained staff, equipment, medicines) for CRD management.
6. **Prevalence, risk factors and clinical correlates of COPD in a rural setting in Tanzania** — https://publications.ersnet.org/content/erj/51/2/1700182
   Rural study estimating COPD prevalence at ~17.5%, underscoring a high CRD burden.
7. **Management of chronic lung diseases in Sudan and Tanzania: how ready are the country health systems?** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8310588/
   Health-system readiness analysis; notes CLD diagnosis often via TB clinics with follow-up at NCD clinics.
8. **Stakeholders' perspectives on management and prevention of NCDs in rural Tanzania (pre-PEN-Plus SWOC analysis)** — https://journals.plos.org/globalpublichealth/article?id=10.1371%2Fjournal.pgph.0005701
   Qualitative assessment of NCD (including CRD) management prior to PEN-Plus implementation.

</details>

[↑ Summary table](#summary-table)

---

### Zambia

_Republic of Zambia_

**All NCDs — Maturity:** Aggregate (partial) · **Evidence:** Confirmed (driven by cancer)

#### Zambia — Cardiometabolic (diabetes / hypertension / CVD)

**Maturity:** Aggregate · **Evidence:** Indicative †

Zambia's HMIS is widely known to be DHIS2-based and would routinely capture aggregate NCD indicators; explicit cardiometabolic-specific DHIS2 Tracker deployment was not confirmed in these search results.

> **Re-check (2 November 2026):** Maturity upgraded from HMIS-only. The MoH NHSP 2022–2026 M&E Framework lists indicator 2.3.8 'Incidence of NCDs' (hypertension, diabetes) with a 2021 HMIS baseline and HMIS (DHIS2-based) as source. It does not name DHIS2 directly, so evidence stays Indicative.
>
> - [Health Sector Monitoring and Evaluation Framework, NHSP 2022-2026 (Zambia Ministry of Health)](https://washinhcf.org/wp-content/uploads/2025/04/Zambia-Monitoring-and-Evaluation-Framework-2022-to-2026-for-the-NHSP.pdf) (c.2023)

<details>
<summary>Original cardiometabolic search profile: summary and results</summary>

**Summary**
Zambia has a national NCD surveillance system implemented by the Ministry of Health, with WHO STEPS surveys in 2017 and a 2023 adapted-STEPS survey across 149 ART clinics in 52 districts. The TASKPEN trial in Lusaka is piloting a WHO PEN-based integrated HIV-NCD care package addressing hypertension, diabetes and dyslipidemia. Zambia is a long-standing DHIS2 country for its national HMIS, though search results did not surface a published account of DHIS2 Tracker for NCDs specifically.

Original single-token verdict (superseded by the two-axis rating above): LIKELY
Zambia's HMIS is widely known to be DHIS2-based and would routinely capture aggregate NCD indicators; explicit cardiometabolic-specific DHIS2 Tracker deployment was not confirmed in these search results.

**Search Results**

*English query results*
1. **Prevalence and risk factors of hypertension and diabetes among PLHIV in Zambia (J Int AIDS Soc, 2025)** — https://onlinelibrary.wiley.com/doi/10.1002/jia2.70051
   National cross-sectional facility-based survey using adapted WHO STEPS in 149 ART clinics.

2. **Zambia Country Report (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9562810/
   Country-level NCD surveillance and STEPS 2017 data.

3. **TASKPEN trial protocol, Lusaka, Zambia (ResearchGate)** — https://www.researchgate.net/publication/381230973
   WHO PEN-based integrated HIV-NCD care stepped-wedge trial.

</details>

#### Zambia — Cancer

**Maturity:** Aggregate (partial) · **Evidence:** Confirmed

Named WHO Global Platform confirms childhood cancer medicine data reported through DHIS2 since 2025; childhood-only scope warrants partial.

> **Re-check (2 November 2026):** Kept. The MoH M&E Framework adds a cervical screening rate with a 2021 HMIS baseline. The cancer registry (ZNCR) and SmartCerv remain separate from DHIS2, so 'partial' stands.
>
> - [Health Sector Monitoring and Evaluation Framework, NHSP 2022-2026 (Zambia Ministry of Health)](https://washinhcf.org/wp-content/uploads/2025/04/Zambia-Monitoring-and-Evaluation-Framework-2022-to-2026-for-the-NHSP.pdf) (c.2023)
> - [Implementing SmartCerv for cervical cancer patient data management in Zambia (CHAI)](https://www.clintonhealthaccess.org/policy/implementing-smartcerv-for-cervical-cancer-patient-data-management-in-zambia/) (2023)

<details>
<summary>Original cancer search profile: summary and results</summary>

**Summary**
Zambia has a complete national DHIS2 implementation and is one of 12 countries participating in the WHO Global Platform for Childhood Cancer Medicines, reporting through DHIS2 since 2025. The country has cancer registries and strong digital health infrastructure, and is an IARC-HISP collaboration reference country, positioning it well for expanded cancer data integration through DHIS2.

Original single-token verdict (superseded by the two-axis rating above): MODERATE
Zambia actively reports childhood cancer medicine data through DHIS2 as part of the WHO Global Platform (started 2025), has complete national DHIS2 deployment, established cancer registries, and serves as an IARC-HISP collaboration reference country for cancer data systems.

**Search Results**

*English query results*
1. **WHO Global Platform for Childhood Cancer Medicines** — https://www.who.int/initiatives/the-global-platform-on-access-to-childhood-cancer-medicines
   Zambia is one of 12 countries reporting childhood cancer medicine data through DHIS2 as part of this WHO initiative, with reporting starting in 2025.

2. **IARC-HISP Centre Collaboration** — https://hisp.uio.no/iarc-hisp-collaboration/
   Zambia is a reference country in the IARC-HISP Centre collaboration focused on strengthening cancer registration through digital health tools.

3. **Zambia National Cancer Registry** — https://www.afcrn.org/membership/members/zambia
   Cancer registry operations in Zambia contributing to the African Cancer Registry Network.

4. **Zambia Cancer Profile — GLOBOCAN** — https://gco.iarc.fr/today/data/factsheets/populations/894-zambia-fact-sheets.pdf
   IARC GLOBOCAN fact sheet providing cancer incidence and mortality estimates for Zambia.

5. **Zambia National Cancer Control Strategic Plan** — https://www.iccp-portal.org/plans/zambia-national-cancer-control-strategic-plan
   Zambia's national strategy for cancer prevention, treatment, data management, and digital health integration.

6. **WHO Global Initiative for Childhood Cancer — Zambia** — https://www.who.int/initiatives/the-global-initiative-for-childhood-cancer
   WHO initiative supporting Zambia's childhood cancer outcomes improvement through better data systems and treatment access.

7. **Digital Health in Zambia** — https://www.moh.gov.zm/digital-health/
    Zambia Ministry of Health digital health strategy supporting DHIS2 and cancer data system strengthening.

</details>

#### Zambia — Chronic respiratory disease (COPD / asthma)

**Maturity:** HMIS-only · **Evidence:** Indicative

Live national DHIS2 HMIS plus asthma/PEN-Plus care activity, but no explicit asthma/COPD indicators reported through DHIS2.

> **Re-check (2 November 2026):** Kept. No asthma or COPD indicator found in the NHSP 2022–2026 M&E Framework or other Zambian sources. PEN-Plus in Zambia targets T1D, sickle cell and RHD rather than CRD.

<details>
<summary>Original CRD search profile: summary and results</summary>

**Summary**
DHIS2 is unambiguously Zambia's national HMIS — the Ministry of Health runs a live instance at dhis2.moh.gov.zm capturing facility-level morbidity and mortality data, with CDC and partner support. CRD activity exists nationally (paediatric asthma care improvement initiatives, PEN-Plus clinics treating severe NCDs, participation in regional asthma/COPD research), but no source explicitly documents asthma/COPD/CRD indicators being reported through DHIS2.

Original single-token verdict (superseded by the two-axis rating above): MODERATE
DHIS2 is Zambia's national HMIS and national asthma/PEN-Plus activity exists, but no explicit DHIS2 CRD data capture surfaced.

**Search Results**

*English query results*
1. **Zambia Ministry of Health HMIS (DHIS2 login)** — https://dhis2.moh.gov.zm/hmis/ [BROKEN: unreachable]
   Live national DHIS2 instance operated by the Zambia MoH, the country's main source of routine health statistics including disease morbidity and mortality.
2. **CDC in Zambia — Global Health** — https://www.cdc.gov/global-health/countries/zambia.html
   Describes CDC support for Zambia's epidemiology, disease and mortality surveillance and health information systems, including SmartCare, DHIS2 and IDSR.
3. **Improving paediatric asthma care in Zambia** — https://pmc.ncbi.nlm.nih.gov/articles/PMC4645426/
   Documents a national partnership between Zambian physicians, government and industry to update asthma guidelines and improve childhood asthma care.
4. **Open for Care: Zambia Cuts the Ribbon on Fourth and Fifth PEN-Plus Clinics** — https://www.ncdipoverty.org/blog/zambia-fourth-and-fifth-clinics
   NCDI Poverty Network report on Zambia expanding PEN-Plus clinics, which deliver integrated care for severe chronic NCDs including severe asthma.
5. **Chronic respiratory disease observatory for Africa (CHEST-Africa): study protocol** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11256067/
   Multi-country African study protocol on the prevalence, determinants and economic impact of asthma and COPD, relevant to regional CRD surveillance capacity.
6. **Zambia Programmatic Data 2019 (Direct from DHIS2) — AIDS Data Repository** — https://adr.unaids.org/dataset/zambia-dhis2-programmatic-data-2019
   Example of national program data extracted directly from Zambia MoH DHIS2 instances, confirming DHIS2 as the routine reporting backbone.
7. **Identifying barriers to the production and use of routine health information in Western Province, Zambia** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10566315/
   Study of routine health information (HMIS/DHIS2) data quality and use challenges in Zambia.

*Bemba query results*
(No Bemba-language sources exist online; this query returned the same English-language material listed above.)

</details>

[↑ Summary table](#summary-table)

---
