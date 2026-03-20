---
layout: post
title: "DHIS2 use in cancer surveillance — a global landscape"
date: 2026-02-17
description: Country-level evidence of DHIS2 use in the cancer domain, using Claude to conduct a systematic web search.
tags: dhis2 cancer surveillance research
categories: research
---


# Claude code research experiment: country-level evidence of DHIS2 use in cancer domain

Brian O'Donnell / HISP Centre

02/17/2026

____


## *ToC*

- [Introduction](#introduction)

- [Methods](#methods)

- [Results: Overview](#results-overview)

- [Results: Country profiles](#results-country-profiles)

- [Discussion](#discussion)

- [Conclusions and recommendations](#conclusion)


## Introduction
[DHIS2](https://dhis2.org) is a digital public good used in over 120 countries and by 80+ national health authorities to manage routine health data. While DHIS2 core software is maintained by the HISP Centre at the Univeristy of Oslo, DHIS2 implementers define the contents of their own DHIS2 database without alerting DHIS2 developers or the wider HISP network. With a global and diverse user base, understanding the scope, depth, and complexity of specific use cases in the health domain presents a critical challenge for the HISP Centre's global support team. 

 Typically, background research on DHIS2 use cases relies on collective wisdom from the 23 HISP groups providing country and regional DHIS2 support in the field. HISPs regularly share country-level information on health use case coverage through an internal spreadsheet with the HISP Centre at UiO. HISP groups and other DHIS2 implementers also share their country use cases publicly on the online Community of Practice and at global conferences. However, the information gleaned from voluntary inputs is not always timely, detailed or comprehensive enough for specific use cases; further, some national DHIS2 systems operate without HISP engagement, meaning our estimates of DHIS2's global footprint are likely undercounting.

One example of an emerging use case in DHIS2 is cancer surveillance. Several countries have developed cancer registries or oncology modules with DHIS2 Tracker in recent years. Some initiatives have also managed cervical and breast cancer screenings or HPV vaccination campaigns with DHIS2 Tracker. Further, many countries report aggregated data on cancer diagnoses from district or facility level through the DHIS2-based HMIS systems. As the HISP centre works with WHO IARC to [share more resources on DHIS2 usage as a flexible cancer registry](https://dhis2.org/iarc-hisp-centre-cancer-registries/), a "quick and dirty" global landscape analysis would provide useful, timely, and actionable intelligence.

AI tools present an opportunity to add to the HISP Centre's overall picture of DHIS2 usage by performing automated research and synthesis across open sources. While most countries do not publish HMIS datasets, many other sources of information are publicly available online, and several academic papers and government sources document DHIS2 usage. Automating this online search could be combined with voluntary inputs from HISP groups and the DHIS2 community of practice to give a fuller picture of DHIS2 global use.

This report documents my initial attempts at a global DHIS2 use search task with generative AI and a web search API, with the cancer domain (registries or aggregate) as an example.

## Methods

I used Claude Code with the Opus 4.6 model. Installation and folder setup followed online instructions [here](https://hannahstulberg.substack.com/p/claude-code-for-everything-finally), including background on my role, and links for context to DHIS2 as a software and HISP Centre as an organization.

I first downloaded a list of all DHIS2 countries from internal country status tracker sheet, countries visible on the [DHIS2 website](https://dhis2.org/in-action) (I made a decision early on to exclude other countries, since it was highly unlikely a DHIS2-based cancer database was operating in a public health system with no previous history of DHIS2 use.)

I also downloaded a list of known aliases of DHIS2-based HMIS in each country (e.g. "KHIS" in Kenya, "DHIMS" in Ghana).

At first, I added a list of all official country languages, with the intention to search in each official langauge for each country. I downloaded the official languages for each country as defined on Wikipedia. https://en.wikipedia.org/wiki/List_of_official_languages_by_country_and_territory

After the first 10 countries I quickly saw this approach took many resources for very few results, so I adapted to only search for English in each country, *and* any local languages that falls within this non-random selection common in DHIS2 countries, if applicable: 
- Arabic
- Portuguese
- Spanish
- French
- Amharic
- Hindi
- Bahasa Indonesia
- Russian
- Lao
- Nepali
- Urdu
- Dutch
- Swahili
- Thai
- Vietnamese

I asked ChatGPT for recommendations for search terms on public health and cancer surveillance, settling on:
`"Cancer registry", "Breast cancer screening program", "Cervical cancer screening", "Colorectal cancer screening", "Lung cancer screening", "Prostate cancer screening", "Mammography tracking system", "HPV vaccination registry", "Pap test surveillance", "Oncology information system", "Tumor registry", "Cancer treatment monitoring", "Colonoscopy tracking", "Low-dose CT lung screening", "PSA testing program", "Cancer surveillance system", "Tobacco cessation program", "HPV immunization tracking", "Cancer patient navigation system", "Radiation oncology system", "Chemotherapy administration", "Cancer early detection program", "Skin cancer screening", "Cancer survivorship care", "Population-based cancer control"`


If a country had a non-English language, the keywords and country name were translated into that language, and two languages were performed.

I wanted to prioritize search results from the last 5 years from government sources and academic reports, excluding results from dhis2.org as those are already known.

Under a working folder for NCDs I added the above files and the following instructions.

```
## "WEBSEARCH_PREP" instructions
This will create multi-language search criteria for country
When instructed to "websearch" this topic:
1) Read "country_langs.csv".
2) If the language in "language" column is English, you can simply copy "country" and "keywords_lang1" over into "keywords_lang1". Otherwise, translate the "country" and "keywords_eng" cell contents into the language specified in the "language" column. Paste the translated keyword string into "keywords_lang1".  You may batch translate (i.e. only translate keywords into French once, but copy translation to all countries with French languages.)
3) Read "hmis_aliases.csv". Merge into the "hmis_aliases" into the country_lands table, matching by country name (fuzzy match).
4) Add a column called "software".
5) Paste value for [System name] column to `& ("DHIS2" OR "DHIS-2" OR "DHIS 2" OR "DHIS II")` in the "software" column.
6) Paste  "keywords_lang1" and "software" into a new column "query"
7) Write results into a new table, save as "websearch_prep.csv"

Stop for review.

## WEBSEARCH_EXECUTE instructions
FOR EACH ROW in websearch_prep.csv, web search for the "query", using "country" as `user_location`
For each *country*, save the top 10 hits into a MD file titled `./profiles_cancer/[country]_cancer.md` (combining searches in all languages). Replace [country] in the file name with name of the country. At the top of the `.md` file create a summary. Explicitly state with `"DHIS2 USE: "` your certainty level for whether DHIS2 is used to collect or manage cancer data in the country.

Rules for queries:
- Query in the language specified in the language column.
- Prioritize academic research or government reports.
- Prioritize results from last five years.
- DONT include any pages from dhis2.org

## WERBSEARCH_CLEAN_RESULTS
Each country's "cancer profile" should be presented in a standardized format, with headers for: title (main header), summary (with "DHIS2 USE:"), search results, then english query results and search results from other langauges. If multiple languages are listed for a country in `websearch_prep.csv` then the search results for the other language should be listed in the country profile. Add those results if not present. Format the profiles properly. You may use @profiles_cancer/Chile_cancer.md as as an example
```

When this is done I compared results with:
- Known countries with cancer registry in DHIS2, sourced from internal research over 3+ yrs (Google Alerts, HISP feedback, etc)
- search for "cancer" in country implementation drive

## Results: Overview

| DHIS2 used for cancer — certainty | Count |
| --------------------- | -----:|
| No evidence           |    58 |
| Low                   |    33 |
| Moderate              |    13 |
| High                  |     9 |

> Note: Based on the resulting text, if a profile had "no evidence" it seems likely the agent found no publicly available information on DHIS2 for *any* cancer related purpose in the country. "Low" suggests Claude found some evidence DHIS2 was used as a general-purpose HMIS with plausible cancer/NCD aggregate datasets.

In our country implementation google drive, 26 countries had "cancer" somewhere in their country's drive--which could either imply suggestive, planned, or non-verified evidence of use, or alternatively actual HMIS metadata shared with HISP. We had prior evidence of 12 countries using DHIS2 as a cancer __*registry*__ or cervical cancer tracker.

Eight countries (Bhutan, Guinnea-Bissau, Ethiopia, Haiti, India, Kenya, Uganda, Zambia) all had moderate to high evidence of DHIS2 for cancer, but were not previously tracked. 


|country                      |claude's finding |known dhis2 cancer registry |cancer term in country folder |
|:----------------------------|:----------------|:---------------------------|:-----------------------------|
|Afghanistan                  |LOW              |                            |                              |
|Algeria                      |NO EVIDENCE      |                            |                              |
|Angola                       |LOW              |                            |                              |
|Antigua and Barbuda          |MODERATE         |                            |Yes                           |
|Argentina                    |NO EVIDENCE      |                            |                              |
|Bangladesh                   |HIGH             |Yes (cerv. canc.)                        |                              |
|Benin                        |LOW              |                            |                              |
|Bhutan                       |HIGH             |                            |                              |
|Botswana                     |LOW              |                            |Yes                           |
|Brazil                       |NO EVIDENCE      |                            |                              |
|Burkina Faso                 |HIGH             |Yes (cerv. canc.)                         |                              |
|Burundi                      |LOW              |                            |Yes                           |
|Cambodia                     |LOW              |                            |                              |
|Cameroon                     |MODERATE         |                            |Yes                           |
|Cape Verde                   |LOW              |                            |                              |
|Central African Republic     |LOW              |                            |                              |
|Chad                         |LOW              |                            |Yes                           |
|Chile                        |MODERATE         |Yes                         |Yes                           |
|Colombia                     |LOW              |                            |                              |
|Comoros                      |LOW              |                            |                              |
|Congo Republic (Brazzaville) |LOW              |                            |                              |
|Costa Rica                   |NO EVIDENCE      |                            |                              |
|Cote d'Ivoire                |HIGH             |Yes  (cerv. canc.)                        |Yes                           |
|DPR Korea                    |NO EVIDENCE      |                            |                              |
|DRC                          |LOW              |                            |                              |
|Djibouti                     |LOW              |                            |                              |
|Dominica                     |NO EVIDENCE      |                            |                              |
|Dominican Republic           |NO EVIDENCE      |                            |                              |
|Ecuador                      |LOW              |                            |                              |
|Egypt                        |NO EVIDENCE      |                            |                              |
|El Salvador                  |LOW              |                            |                              |
|Equatorial Guinea            |NO EVIDENCE      |                            |                              |
|Eritrea                      |NO EVIDENCE      |                            |                              |
|Eswatini                     |NO EVIDENCE      |                            |                              |
|Ethiopia                     |MODERATE         |                            |                              |
|Gabon                        |NO EVIDENCE      |                            |                              |
|Gambia                       |LOW              |                            |Yes                           |
|Ghana                        |LOW              |                            |Yes                           |
|Grenada                      |NO EVIDENCE      |                            |                              |
|Guatemala                    |LOW              |                            |                              |
|Guinea                       |LOW              |                            |                              |
|Guinea-Bissau                |MODERATE         |                            |                              |
|Guyana                       |NO EVIDENCE      |                            |                              |
|Haiti                        |MODERATE         |                            |                              |
|Honduras                     |MODERATE         |Yes                         |                              |
|India                        |MODERATE         |                            |                              |
|Indonesia                    |MODERATE         |                            |Yes                           |
|Iraq                         |HIGH             |                            |Yes                           |
|Jamaica                      |HIGH             |Yes                         |                              |
|Jordan                       |MODERATE         |                            |Yes                           |
|Kazakhstan                   |NO EVIDENCE      |                            |                              |
|Kenya                        |HIGH             |                            |                              |
|Kiribati                     |NO EVIDENCE      |                            |                              |
|Kyrgyzstan                   |NO EVIDENCE      |                            |                              |
|Lao                          |LOW              |                            |                              |
|Lebanon                      |NO EVIDENCE      |                            |                              |
|Lesotho                      |NO EVIDENCE      |                            |Yes                           |
|Liberia                      |NO EVIDENCE      |                            |                              |
|Libya                        |NO EVIDENCE      |                            |Yes                           |
|Madagascar                   |NO EVIDENCE      |                            |                              |
|Malawi                       |LOW              |Yes                         |                              |
|Maldives                     |HIGH             |Yes                         |Yes                           |
|Mali                         |NO EVIDENCE      |                            |                              |
|Mauritania                   |NO EVIDENCE      |                            |                              |
|Mauritius                    |NO EVIDENCE      |                            |                              |
|Mongolia                     |LOW              |                            |                              |
|Morocco                      |NO EVIDENCE      |                            |                              |
|Mozambique                   |NO EVIDENCE      |Yes                         |                              |
|Myanmar                      |NO EVIDENCE      |                            |                              |
|Namibia                      |NO EVIDENCE      |                            |                              |
|Nepal                        |LOW              |                            |Yes                           |
|Nicaragua                    |NO EVIDENCE      |                            |                              |
|Niger                        |NO EVIDENCE      |                            |                              |
|Nigeria                      |LOW              |                            |Yes                           |
|Norway                       |NO EVIDENCE      |                            |                              |
|Pakistan                     |LOW              |                            |Yes                           |
|Palestine                    |NO EVIDENCE      |                            |Yes                           |
|Panama                       |NO EVIDENCE      |                            |                              |
|Papua New Guinea             |NO EVIDENCE      |                            |                              |
|Paraguay                     |NO EVIDENCE      |                            |                              |
|Philippines                  |NO EVIDENCE      |                            |                              |
|Rwanda                       |HIGH             |Yes                         |Yes                           |
|Saint Lucia                  |LOW              |                            |                              |
|Sao Tomé and Principe        |NO EVIDENCE      |                            |                              |
|Senegal                      |LOW              |                            |Yes                           |
|Seychelles                   |NO EVIDENCE      |                            |                              |
|Sierra Leone                 |NO EVIDENCE      |                            |Yes                           |
|Solomon Islands              |NO EVIDENCE      |                            |Yes                           |
|Somalia                      |NO EVIDENCE      |                            |                              |
|South Africa                 |LOW              |                            |                              |
|South Sudan                  |NO EVIDENCE      |                            |Yes                           |
|Sri Lanka                    |LOW              |                            |                              |
|Sudan                        |NO EVIDENCE      |                            |Yes                           |
|Suriname                     |NO EVIDENCE      |                            |Yes                           |
|Syria                        |NO EVIDENCE      |                            |                              |
|Tajikistan                   |NO EVIDENCE      |                            |                              |
|Tanzania                     |LOW              |Yes                         |                              |
|Thailand                     |NO EVIDENCE      |                            |                              |
|Timor-Leste                  |NO EVIDENCE      |                            |                              |
|Togo                         |NO EVIDENCE      |                            |                              |
|Tonga                        |NO EVIDENCE      |                            |                              |
|Tunisia                      |NO EVIDENCE      |                            |                              |
|Uganda                       |MODERATE         |                            |                              |
|Ukraine                      |NO EVIDENCE      |                            |                              |
|Uzbekistan                   |LOW              |                            |                              |
|Vanuatu                      |NO EVIDENCE      |                            |                              |
|Venezuela                    |NO EVIDENCE      |                            |                              |
|Vietnam                      |LOW              |                            |                              |
|Western Sahara               |NO EVIDENCE      |                            |                              |
|Yemen                        |NO EVIDENCE      |                            |                              |
|Zambia                       |MODERATE         |                            |                              |
|Zanzibar                     |NO EVIDENCE      |                            |                              |
|Zimbabwe                     |MODERATE         |Yes                         |                              |



## Results: Country Profiles

__NOTE: The following results are all AI generated__

- [Afghanistan -- Cancer & DHIS2 Profile](#afghanistan--cancer--dhis2-profile)
- [Algeria -- Cancer & DHIS2 Profile](#algeria--cancer--dhis2-profile)
- [Angola -- Cancer & DHIS2 Profile](#angola--cancer--dhis2-profile)
- [Antigua and Barbuda -- Cancer & DHIS2 Profile](#antigua-and-barbuda--cancer--dhis2-profile)
- [Argentina -- Cancer & DHIS2 Profile](#argentina--cancer--dhis2-profile)
- [Bangladesh -- Cancer & DHIS2 Profile](#bangladesh--cancer--dhis2-profile)
- [Benin -- Cancer & DHIS2 Profile](#benin--cancer--dhis2-profile)
- [Bhutan -- Cancer & DHIS2 Profile](#bhutan--cancer--dhis2-profile)
- [Botswana -- Cancer & DHIS2 Profile](#botswana--cancer--dhis2-profile)
- [Brazil -- Cancer & DHIS2 Profile](#brazil--cancer--dhis2-profile)
- [Burkina Faso -- Cancer & DHIS2 Profile](#burkina-faso--cancer--dhis2-profile)
- [Burundi -- Cancer & DHIS2 Profile](#burundi--cancer--dhis2-profile)
- [Cambodia -- Cancer & DHIS2 Profile](#cambodia--cancer--dhis2-profile)
- [Cameroon -- Cancer & DHIS2 Profile](#cameroon--cancer--dhis2-profile)
- [Cape Verde -- Cancer & DHIS2 Profile](#cape-verde--cancer--dhis2-profile)
- [Central African Republic -- Cancer & DHIS2 Profile](#central-african-republic--cancer--dhis2-profile)
- [Chad -- Cancer & DHIS2 Profile](#chad--cancer--dhis2-profile)
- [Chile — Cancer & DHIS2 Profile](#chile--cancer--dhis2-profile)
- [Colombia — Cancer & DHIS2 Profile](#colombia--cancer--dhis2-profile)
- [Comoros — Cancer & DHIS2 Profile](#comoros--cancer--dhis2-profile)
- [Congo Republic (Brazzaville) — Cancer & DHIS2 Profile](#congo-republic-brazzaville--cancer--dhis2-profile)
- [Costa Rica — Cancer & DHIS2 Profile](#costa-rica--cancer--dhis2-profile)
- [Cote d'Ivoire — Cancer & DHIS2 Profile](#cote-divoire--cancer--dhis2-profile)
- [Djibouti — Cancer & DHIS2 Profile](#djibouti--cancer--dhis2-profile)
- [Dominica — Cancer & DHIS2 Profile](#dominica--cancer--dhis2-profile)
- [Dominican Republic — Cancer & DHIS2 Profile](#dominican-republic--cancer--dhis2-profile)
- [DPR Korea — Cancer & DHIS2 Profile](#dpr-korea--cancer--dhis2-profile)
- [DRC — Cancer & DHIS2 Profile](#drc--cancer--dhis2-profile)
- [Ecuador — Cancer & DHIS2 Profile](#ecuador--cancer--dhis2-profile)
- [Egypt — Cancer & DHIS2 Profile](#egypt--cancer--dhis2-profile)
- [El Salvador — Cancer & DHIS2 Profile](#el-salvador--cancer--dhis2-profile)
- [Equatorial Guinea — Cancer & DHIS2 Profile](#equatorial-guinea--cancer--dhis2-profile)
- [Eritrea — Cancer & DHIS2 Profile](#eritrea--cancer--dhis2-profile)
- [Eswatini — Cancer & DHIS2 Profile](#eswatini--cancer--dhis2-profile)
- [Ethiopia — Cancer & DHIS2 Profile](#ethiopia--cancer--dhis2-profile)
- [Gabon — Cancer & DHIS2 Profile](#gabon--cancer--dhis2-profile)
- [Gambia — Cancer & DHIS2 Profile](#gambia--cancer--dhis2-profile)
- [Ghana — Cancer & DHIS2 Profile](#ghana--cancer--dhis2-profile)
- [Grenada — Cancer & DHIS2 Profile](#grenada--cancer--dhis2-profile)
- [Guatemala — Cancer & DHIS2 Profile](#guatemala--cancer--dhis2-profile)
- [Guinea-Bissau — Cancer & DHIS2 Profile](#guinea-bissau--cancer--dhis2-profile)
- [Guinea — Cancer & DHIS2 Profile](#guinea--cancer--dhis2-profile)
- [Guyana — Cancer & DHIS2 Profile](#guyana--cancer--dhis2-profile)
- [Haiti — Cancer & DHIS2 Profile](#haiti--cancer--dhis2-profile)
- [Honduras — Cancer & DHIS2 Profile](#honduras--cancer--dhis2-profile)
- [India — Cancer & DHIS2 Profile](#india--cancer--dhis2-profile)
- [Indonesia — Cancer & DHIS2 Profile](#indonesia--cancer--dhis2-profile)
- [Iraq — Cancer & DHIS2 Profile](#iraq--cancer--dhis2-profile)
- [Jamaica — Cancer & DHIS2 Profile](#jamaica--cancer--dhis2-profile)
- [Jordan — Cancer & DHIS2 Profile](#jordan--cancer--dhis2-profile)
- [Kazakhstan — Cancer & DHIS2 Profile](#kazakhstan--cancer--dhis2-profile)
- [Kenya — Cancer & DHIS2 Profile](#kenya--cancer--dhis2-profile)
- [Kiribati — Cancer & DHIS2 Profile](#kiribati--cancer--dhis2-profile)
- [Kyrgyzstan — Cancer & DHIS2 Profile](#kyrgyzstan--cancer--dhis2-profile)
- [Lao — Cancer & DHIS2 Profile](#lao--cancer--dhis2-profile)
- [Lebanon — Cancer & DHIS2 Profile](#lebanon--cancer--dhis2-profile)
- [Lesotho — Cancer & DHIS2 Profile](#lesotho--cancer--dhis2-profile)
- [Liberia — Cancer & DHIS2 Profile](#liberia--cancer--dhis2-profile)
- [Libya — Cancer & DHIS2 Profile](#libya--cancer--dhis2-profile)
- [Madagascar — Cancer & DHIS2 Profile](#madagascar--cancer--dhis2-profile)
- [Malawi — Cancer & DHIS2 Profile](#malawi--cancer--dhis2-profile)
- [Maldives — Cancer & DHIS2 Profile](#maldives--cancer--dhis2-profile)
- [Mali — Cancer & DHIS2 Profile](#mali--cancer--dhis2-profile)
- [Mauritania — Cancer & DHIS2 Profile](#mauritania--cancer--dhis2-profile)
- [Mauritius — Cancer & DHIS2 Profile](#mauritius--cancer--dhis2-profile)
- [Mongolia — Cancer & DHIS2 Profile](#mongolia--cancer--dhis2-profile)
- [Morocco — Cancer & DHIS2 Profile](#morocco--cancer--dhis2-profile)
- [Mozambique — Cancer & DHIS2 Profile](#mozambique--cancer--dhis2-profile)
- [Myanmar — Cancer & DHIS2 Profile](#myanmar--cancer--dhis2-profile)
- [Namibia — Cancer & DHIS2 Profile](#namibia--cancer--dhis2-profile)
- [Nepal — Cancer & DHIS2 Profile](#nepal--cancer--dhis2-profile)
- [Nicaragua — Cancer & DHIS2 Profile](#nicaragua--cancer--dhis2-profile)
- [Niger — Cancer & DHIS2 Profile](#niger--cancer--dhis2-profile)
- [Nigeria — Cancer & DHIS2 Profile](#nigeria--cancer--dhis2-profile)
- [Norway — Cancer & DHIS2 Profile](#norway--cancer--dhis2-profile)
- [Pakistan — Cancer & DHIS2 Profile](#pakistan--cancer--dhis2-profile)
- [Palestine — Cancer & DHIS2 Profile](#palestine--cancer--dhis2-profile)
- [Panama — Cancer & DHIS2 Profile](#panama--cancer--dhis2-profile)
- [Papua New Guinea — Cancer & DHIS2 Profile](#papua-new-guinea--cancer--dhis2-profile)
- [Paraguay — Cancer & DHIS2 Profile](#paraguay--cancer--dhis2-profile)
- [Philippines — Cancer & DHIS2 Profile](#philippines--cancer--dhis2-profile)
- [Rwanda — Cancer & DHIS2 Profile](#rwanda--cancer--dhis2-profile)
- [Saint Lucia — Cancer & DHIS2 Profile](#saint-lucia--cancer--dhis2-profile)
- [Sao Tomé and Principe — Cancer & DHIS2 Profile](#sao-tomé-and-principe--cancer--dhis2-profile)
- [Senegal — Cancer & DHIS2 Profile](#senegal--cancer--dhis2-profile)
- [Seychelles — Cancer & DHIS2 Profile](#seychelles--cancer--dhis2-profile)
- [Sierra Leone — Cancer & DHIS2 Profile](#sierra-leone--cancer--dhis2-profile)
- [Solomon Islands — Cancer & DHIS2 Profile](#solomon-islands--cancer--dhis2-profile)
- [Somalia — Cancer & DHIS2 Profile](#somalia--cancer--dhis2-profile)
- [South Africa — Cancer & DHIS2 Profile](#south-africa--cancer--dhis2-profile)
- [South Sudan — Cancer & DHIS2 Profile](#south-sudan--cancer--dhis2-profile)
- [Sri Lanka — Cancer & DHIS2 Profile](#sri-lanka--cancer--dhis2-profile)
- [Sudan — Cancer & DHIS2 Profile](#sudan--cancer--dhis2-profile)
- [Suriname — Cancer & DHIS2 Profile](#suriname--cancer--dhis2-profile)
- [Syria — Cancer & DHIS2 Profile](#syria--cancer--dhis2-profile)
- [Tajikistan — Cancer & DHIS2 Profile](#tajikistan--cancer--dhis2-profile)
- [Tanzania — Cancer & DHIS2 Profile](#tanzania--cancer--dhis2-profile)
- [Thailand — Cancer & DHIS2 Profile](#thailand--cancer--dhis2-profile)
- [Timor-Leste — Cancer & DHIS2 Profile](#timor-leste--cancer--dhis2-profile)
- [Togo — Cancer & DHIS2 Profile](#togo--cancer--dhis2-profile)
- [Tonga — Cancer & DHIS2 Profile](#tonga--cancer--dhis2-profile)
- [Tunisia — Cancer & DHIS2 Profile](#tunisia--cancer--dhis2-profile)
- [Uganda — Cancer & DHIS2 Profile](#uganda--cancer--dhis2-profile)
- [Ukraine — Cancer & DHIS2 Profile](#ukraine--cancer--dhis2-profile)
- [Uzbekistan — Cancer & DHIS2 Profile](#uzbekistan--cancer--dhis2-profile)
- [Vanuatu — Cancer & DHIS2 Profile](#vanuatu--cancer--dhis2-profile)
- [Venezuela — Cancer & DHIS2 Profile](#venezuela--cancer--dhis2-profile)
- [Vietnam — Cancer & DHIS2 Profile](#vietnam--cancer--dhis2-profile)
- [Western Sahara — Cancer & DHIS2 Profile](#western-sahara--cancer--dhis2-profile)
- [Yemen — Cancer & DHIS2 Profile](#yemen--cancer--dhis2-profile)
- [Zambia — Cancer & DHIS2 Profile](#zambia--cancer--dhis2-profile)
- [Zanzibar — Cancer & DHIS2 Profile](#zanzibar--cancer--dhis2-profile)
- [Zimbabwe — Cancer & DHIS2 Profile](#zimbabwe--cancer--dhis2-profile)

---

## Afghanistan -- Cancer & DHIS2 Profile

### Summary
Afghanistan uses the MoPH Data Warehouse, built on DHIS2, as its national health information platform since 2017. The National Cancer Control Program (NCCP) established the first hospital-based cancer registry at Jumhuriat Hospital in 2016 and is working with WHO/IARC to develop the Kabul Cancer Registry. However, there is no clear evidence that cancer registry or screening data are currently integrated into the DHIS2-based MoPH Data Warehouse; cancer data collection appears to remain largely separate from the main HMIS.

DHIS2 USE: LOW
Afghanistan's MoPH Data Warehouse runs on DHIS2 and collects routine health data, but cancer-specific modules (registry, screening, treatment monitoring) do not appear to be integrated into the system. The NCCP operates separately, and cancer surveillance infrastructure remains nascent.

### Search Results

#### English query results
1. **MoPH Data Warehouse Login** -- https://moph-dw.gov.af/dhis-web-commons/security/login.action
   Afghanistan's MoPH Data Warehouse is an online DHIS2-based central repository for all health data, launched in August 2017.

2. **COVID-19 vaccines coverage in Afghanistan: A descriptive analysis of secondary data from DHIS2** -- https://rimj.org/pubs/index.php/journal/article/download/95/96?inline=1
   Study using DHIS2 data from the MoPH Data Warehouse for COVID-19 vaccine coverage analysis, demonstrating the platform's use for health data extraction at national and provincial levels.

3. **The provision and utilization of essential health services in Afghanistan during COVID-19 pandemic (Frontiers, 2022)** -- https://www.frontiersin.org/journals/public-health/articles/10.3389/fpubh.2022.1097680/full
   Research using DHIS2-based health service data from Afghanistan, though focused on essential health services rather than cancer-specific data.

4. **National Cancer Control Program (NCCP)** -- https://moph.gov.af/en/national-cancer-control-program-nccp
   NCCP established the first hospital-based cancer registry at Jumhuriat Hospital in 2016 and is working with WHO/IARC toward the Kabul Cancer Registry. WHO estimates nearly 20,000 Afghans suffer from cancer annually.

5. **Joint IAEA and WHO delegation assesses Afghan capacity for cancer control and care** -- https://www.emro.who.int/afg/afghanistan-news/joint-iaea-and-who-delegation-assesses-afghan-capacity-for-cancer-control-and-care.html
   Joint IAEA/WHO assessment of Afghanistan's cancer control capacity, highlighting gaps in infrastructure and the need for strengthened cancer services.

6. **Ministry of Public Health Reactivates Health Management Information System with WHO Support** -- https://moph.gov.af/en/ministry-public-health-reactivates-health-management-information-system-who-support-enhance-data
   The HMIS was rolled out in 2022 after being halted due to technical challenges; reactivated online with WHO support.

7. **National Health Management Information System Procedures Manual** -- https://moph.gov.af/sites/default/files/2019-07/HMIS%20Procedures%20Manual%20I%20%20II%20-%20English-Revised-20110823%20(final).pdf
   The HMIS procedures manual for Afghanistan's health data collection system, covering data flows from facility to national level.

#### Dari Persian query results
8. **National Cancer Control Program (Dari)** -- https://moph.gov.af/en/national-cancer-control-program-nccp
   WHO estimates nearly 20,000 Afghans suffer from various types of cancers each year; most common types include breast, stomach, oesophagus, lip/oral, cervix, and lung cancers.

9. **First cancer treatment center to be built in Afghanistan (Anadolu Agency)** -- https://www.aa.com.tr/fa/افغانستان/نخستین-مرکز-درمان-سرطان-در-افغانستان-ساخته-می‌شود/1218737
   Report on construction of the first dedicated cancer treatment center in Afghanistan at Kabul Republic Hospital.

#### Pashto query results
No relevant results were found linking cancer data to DHIS2 or the MoPH Data Warehouse in Pashto-language sources.


## Algeria -- Cancer & DHIS2 Profile

### Summary
Algeria has an established network of population-based cancer registries (notably in Algiers, Setif, and Batna) and launched a National Cancer Control Plan 2023-2030 focused on prevention. The country has adopted DHIS2 through partial national rollout for its HMIS. However, there is no evidence that cancer registry data or screening programmes are integrated into the DHIS2 platform. Algeria's cancer registries operate through their own data collection systems, and the country is working on digitizing cancer patient records through a separate national digital platform.

DHIS2 USE: NO EVIDENCE
While Algeria uses DHIS2 for general health information (partial rollout), no evidence was found of DHIS2 being used specifically for cancer registration, screening, or surveillance. Cancer registries appear to operate independently. The HMIS alias "SISDZ" did not appear in any cancer-related search results.

### Search Results

#### English query results
1. **Algeria - Algiers Cancer Registry (GHDx)** -- https://ghdx.healthdata.org/series/algeria-algiers-cancer-registry
   The Algiers Cancer Registry is one of several population-based cancer registries in Algeria, providing data on cancer incidence since the mid-1990s.

2. **Algeria - Batna Cancer Registry (GHDx)** -- https://ghdx.healthdata.org/series/algeria-batna-cancer-registry
   Regional cancer registry in Batna province contributing to Algeria's cancer surveillance network.

3. **Incidence of lung cancer in males and females in Algeria: The Lung Cancer Registry in Algeria (LuCaReAl) (PubMed, 2020)** -- https://pubmed.ncbi.nlm.nih.gov/32977217/
   Study from the dedicated Lung Cancer Registry in Algeria, demonstrating disease-specific registry efforts.

4. **Time trends of cancer incidence in Setif, Algeria, 1986-2010 (PubMed, 2014)** -- https://pubmed.ncbi.nlm.nih.gov/25175348/
   Long-running population-based cancer registry in Setif providing 25 years of cancer incidence data.

5. **UNICEF Community Health Policy and Implementation Landscape Mapping -- Algeria** -- https://www.unicef.org/mena/media/27086/file/241212_UNICEF_MENARO_CHS_Country_Brief_ALGERIA_ENG.pdf.pdf
   Notes that Algeria has a formal national HMIS known as DHIS2, though health data remains insufficient and there is no formal community health information system integrated into the HMIS.

6. **Cancer Algeria 2020 Country Profile (WHO)** -- https://www.who.int/publications/m/item/cancer-dza-2020
   WHO country profile documenting cancer burden and health system response in Algeria.

7. **Colorectal cancer in a region of western Algeria: results of 581 cases in 5 years (PMC, 2024)** -- https://pmc.ncbi.nlm.nih.gov/articles/PMC10782366/
   Recent study on colorectal cancer incidence in western Algeria using regional registry data.

#### Arabic query results
8. **Algeria registers 65,000 new cancer cases since start of 2021 (APS)** -- https://www.aps.dz/ar/sante-science-technologie/113460-65-000-2021
   Algerian Press Service report on cancer incidence in Algeria, documenting 65,000 new cases registered since early 2021.

9. **National Cancer Control Plan 2023-2030 focuses on prevention (APS)** -- https://www.aps.dz/ar/sante-science-technologie/138904-2023-2030
   Algeria's Health Minister announced the National Cancer Control Plan 2023-2030, which prioritizes prevention.

10. **Algeria initiative to combat cancer and digitize treatment system (Al Araby)** -- https://www.alaraby.co.uk/society/الجزائر-مبادرة-لمكافحة-مرض-السرطان-ورقمنة-نظام-العلاج
    Report on Algeria's initiative to digitize cancer patient records and link them to the national civil registry, indicating a separate digitization effort outside DHIS2.


## Angola -- Cancer & DHIS2 Profile

### Summary
Angola has implemented DHIS2 nationally as its health management information system (known as SIS Angola), with WHO and EU support for training and capacity building. The country has a hospital-based cancer registry at the Instituto Angolano de Controlo do Cancer (IACC) in Luanda, operational since the early 2000s, plus a newer registry in Lubango. However, no direct evidence was found that cancer registry data or cancer screening programmes are currently integrated into the DHIS2 platform. A GitHub repository from HISP Rwanda shows DHIS2 metadata for cancer case registration using Tracker, suggesting regional interest in this approach, but Angola-specific implementation was not confirmed.

DHIS2 USE: LOW
Angola uses DHIS2 extensively for routine health data management, and there are regional DHIS2 cancer registry metadata packages (e.g., from HISP Rwanda). However, no specific evidence confirms that Angola's cancer registries or screening data are currently managed through the DHIS2 platform. Cancer data collection appears to remain in separate systems at the IACC and other oncology centres.

### Search Results

#### English query results
1. **A hospital-based cancer registry in Luanda, Angola: the IACC Cancer Registry (PMC, 2019)** -- https://pmc.ncbi.nlm.nih.gov/articles/PMC6839121/
   The IACC Cancer Registry in Luanda is the most established hospital-based cancer registry in Angola, providing data on cancer cases from several hospital facilities. The five most common cancers were breast (21.4%), cervix (16.8%), prostate (7.1%), non-Hodgkin lymphoma (4.5%), and Kaposi sarcoma (4.3%).

2. **The epidemiology of cancer in Angola -- Results from the cancer registry of the National Oncology Centre of Luanda (ecancer, 2015)** -- https://ecancer.org/en/journal/article/510-the-epidemiology-of-cancer-in-angola-results-from-the-cancer-registry-of-the-national-oncology-centre-of-luanda-angola
   Earlier publication from the National Oncology Centre cancer registry providing epidemiological data on cancer in Angola.

3. **Angola -- Lubango Cancer Registry (AFCRN)** -- https://afcrn.org/index.php/membership/membership-list/179-angola-lubango
   The Lubango Cancer Registry is a member of the African Cancer Registry Network, representing cancer surveillance efforts outside Luanda.

4. **Angola HMIS (DHIS2 + Tracker + Android) -- DIAL Exchange** -- https://exchange.dial.global/projects/angola-angola-hmis-dhis2--tracker--android
   Angola's national HMIS uses DHIS2 with Tracker and Android capabilities, with Kassai analytics integrated.

5. **Health information management system gains greater effectiveness (MenosFios)** -- https://www.menosfios.com/en/sistema-de-gestao-de-informacao-de-saude-ganha-maior-eficacia/
   Reports that data management at the National Health Service level in Angola has become more effective and reliable with DHIS2 implementation throughout the country.

6. **DHIS2 Cancer Registry metadata (GitHub -- HISP Rwanda)** -- https://github.com/hisprwanda/dhis2canceregistry
   DHIS2 metadata package for cancer case registration using Tracker, following IACR guidelines, demonstrating that DHIS2-based cancer registries exist in the region.

#### Portuguese query results
7. **SIS Angola -- Sistema de Informacao de Gestao Sanitaria** -- https://sisangola.org/
   Angola's health management information system portal, built on the DHIS2 platform.

8. **Technicians of Angola's Ministry of Health trained in DHIS2 server administration and management (Saudigitus)** -- https://saudigitus.org/tecnicos-do-ministerio-da-saude-em-angola-capacitados-em-administracao-e-gestao-de-servidores-dhis2/
   Training of Angolan MoH technicians in DHIS2 administration, indicating ongoing capacity building for the system.

9. **First DHIS2 Data Analysis Tools Academy held in Angola (Saudigitus)** -- https://saudigitus.org/realizada-em-angola-a-primeira-academia-sobre-ferramentas-de-analise-de-dados-no-dhis2/
   The first DHIS2 data analysis academy in Angola, with WHO support for strengthening epidemiological surveillance capacity.

10. **REDISSE Angola -- DHIS2 training for Ministry of Agriculture and Forests** -- https://redisseangola.ao/pt/noticias/Formacao-Sobre-Uso-da-Informacao-no-DHIS2-Destinada-aos-Tecnicos-do-ISV-IIV-GEP-e-GTI-do-Ministerio-da-Agricultura-e-Florestas
    DHIS2 training extended beyond health to the Ministry of Agriculture, showing broad adoption of the platform in Angola.


## Antigua and Barbuda -- Cancer & DHIS2 Profile

### Summary
Antigua and Barbuda implemented DHIS2 as its health management information system beginning in 2022, through a partnership between the Epidemiology & Surveillance Unit of the Ministry of Health, Wellness and the Environment and PAHO. There are explicit plans to add modules for Cancer Registry, Cervical Cancer, and HEARTS (NCD) to the DHIS2 system. The country has active cancer screening initiatives, particularly for cervical cancer through HPV testing pilots, and a recent retrospective study documented cancer incidence patterns from 2017-2021. However, the cancer registry module within DHIS2 is still in the planning/development stage.

DHIS2 USE: MODERATE
DHIS2 is the designated national HMIS platform, and there are documented plans to implement cancer registry and cervical cancer screening modules within it. Surveillance and DHIS2 training took place in September 2025. While the cancer-specific modules are not yet operational, the platform and intent are clearly established.

### Search Results

#### English query results
1. **Health management tool strengthening health system (PAHO, Feb 2024)** -- https://www.paho.org/en/news/14-2-2024-health-management-tool-strengthening-health-system
   PAHO report on Antigua and Barbuda's DHIS2 implementation since 2022, noting plans to implement modules for Cervical Cancer, STI, Cancer Registry, Psychiatric data, and HEARTS (NCD).

2. **Surveillance and DHIS2 training for Antigua and Barbuda (PAHO, Sep 2025)** -- https://www.paho.org/en/news/4-9-2025-surveillance-and-dhis2-training-antigua-and-barbuda
   PAHO-supported training for surveillance staff on DHIS2 use in Antigua and Barbuda.

3. **Incidence, trends and patterns of female breast, cervical, colorectal and prostate cancers in Antigua and Barbuda, 2017-2021 (BMC Cancer, 2025)** -- https://link.springer.com/article/10.1186/s12885-025-13459-8
   Recent retrospective study documenting cancer incidence patterns in the country from 2017 to 2021.

4. **Antigua and Barbuda advances towards elimination of cervical cancer (PAHO, Aug 2022)** -- https://www.paho.org/en/news/3-8-2022-antigua-and-barbuda-advances-towards-elimination-cervical-cancer-public-health
   Report on cervical cancer elimination efforts, including HPV testing in community clinics and needs assessment for pilot screening projects.

5. **Pilot training project to implement HPV testing for cervical cancer screening (PAHO, Mar 2022)** -- https://www.paho.org/en/news/25-3-2022-pilot-training-project-implement-hpv-testing-cervical-cancer-screening-antigua-and
   Pilot project implementing HPV testing for cervical cancer screening in five community clinics.

6. **Cancer Antigua and Barbuda 2020 Country Profile (WHO)** -- https://www.who.int/publications/m/item/cancer-atg-2020
   WHO country profile for cancer burden in Antigua and Barbuda.

7. **Country fact sheet: Antigua and Barbuda (CanScreen5/IARC)** -- https://canscreen5.iarc.fr/?page=countryfactsheet&q=ATG
   IARC screening factsheet for Antigua and Barbuda.

8. **GICR Caribbean Regional Hub (IARC)** -- https://gicr.iarc.fr/hub/caribbean/
   Antigua and Barbuda falls under the Global Initiative for Cancer Registry Development Caribbean Hub for regional cancer registration support.

9. **Antigua/Barbuda Cancer Incidence Study (WIMJ Open)** -- https://www.mona.uwi.edu/wimjopen/article/1600
   Historical cancer incidence study for Antigua and Barbuda published in the West Indian Medical Journal.

10. **CARPHA and Vital Strategies sign agreement to strengthen cancer data collection in Jamaica (Antigua Observer)** -- https://antiguaobserver.com/carpha-and-vital-strategies-sign-new-agreement-to-strengthen-cancer-data-collection-systems-in-jamaica/
    Regional efforts by CARPHA and Vital Strategies to strengthen cancer data collection systems in the Caribbean, relevant to Antigua and Barbuda's regional context.


## Argentina -- Cancer & DHIS2 Profile

### Summary
Argentina has a well-developed cancer registration infrastructure, including the Registro Institucional de Tumores de Argentina (RITA), managed by the National Cancer Institute (INC) since 2012, with 29 institutions across 15 jurisdictions. The country also hosts the IARC Regional Hub for Cancer Registration in Latin America in Buenos Aires. Argentina uses its own dedicated cancer information systems (RITA software, SIVER-Ca surveillance system) and the broader SISA (Sistema Integrado de Informacion Sanitaria Argentino) for health data management. There is no evidence that DHIS2 is used for cancer data or any other health information management in Argentina.

DHIS2 USE: NO EVIDENCE
Argentina has its own well-established health information infrastructure (SISA, RITA, SIVER-Ca) for cancer registration and surveillance. No evidence was found of DHIS2 adoption in Argentina for any health programme, including cancer. The country's cancer data systems are built on nationally developed platforms.

### Search Results

#### English query results
1. **Pediatric cancer registries in Latin America: the case of Argentina's pediatric cancer registry (PMC, 2019)** -- https://pmc.ncbi.nlm.nih.gov/articles/PMC6645297/
   The Registro Oncopediatrico Hospitalario Argentino (ROHA), created in 2000, covers pediatric cancer registration through a network of hospitals using standardized methods.

2. **Cancer registration for cancer control in Latin America: a status and progress report (PMC, 2019)** -- https://pmc.ncbi.nlm.nih.gov/articles/PMC6660887/
   Overview of cancer registration across Latin America, with Argentina among the countries with established registries.

3. **GICR Latin America Regional Hub (IARC)** -- https://gicr.iarc.fr/hub/latin-america/
   The IARC Regional Hub for Cancer Registration in Latin America is based at the National Cancer Institute of Argentina in Buenos Aires, established in 2014.

4. **Quantification of impact of COVID-19 pandemic on cancer screening programmes -- Argentina, Bangladesh, Colombia, Morocco, Sri Lanka, and Thailand (PMC, 2023)** -- https://pmc.ncbi.nlm.nih.gov/articles/PMC10188105/
   Multi-country study showing Argentina experienced a 72.9% reduction in cancer screening volume in 2020 due to COVID-19.

5. **Addressing the rising burden of cancer in Argentina (Harvard HSIL, 2023)** -- https://www.hsph.harvard.edu/health-systems-innovation-lab/wp-content/uploads/sites/2633/2023/03/ICCI-Argentina-Report-English.pdf
   Comprehensive Harvard report on Argentina's cancer burden and health system response.

6. **Quality of mammography and breast cancer screening in Argentina (PAHO Journal)** -- https://www.paho.org/journal/en/articles/quality-mammography-and-breast-cancer-screening-argentina
   Assessment of mammography quality in Argentina's breast cancer screening programme.

#### Spanish query results
7. **Registro Institucional de Tumores de Argentina -- RITA (Argentina.gob.ar)** -- https://www.argentina.gob.ar/salud/instituto-nacional-del-cancer/institucional/rita
   RITA is a hospital-based cancer registry implemented since 2012 by the INC, with 29 institutions across 15 jurisdictions forming the National Network of Hospital Cancer Registries, using its own web-based RITA software.

8. **Portal de Datos Abiertos Salud -- Cancer (Argentina.gob.ar)** -- https://datos.salud.gob.ar/dataset?tags=cancer
   Argentina's open health data portal with cancer-related datasets available for public access.

9. **RITA: Registro Institucional de Tumores de Argentina -- Cancer cases diagnosed 2012-2022** -- https://datos.salud.gob.ar/dataset/rita-registro-institucional-de-tumores-de-argentina-casos-de-cancer-diagnosticados-2012-2022
   Open data from RITA covering cancer cases diagnosed from 2012 to 2022.

10. **Sistema Integrado de Informacion Sanitaria Argentino (SISA)** -- https://sisa.msal.gov.ar/sisa/
    Argentina's integrated health information system, which serves as the national HMIS platform (not DHIS2-based).


## Bangladesh -- Cancer & DHIS2 Profile

### Summary
Bangladesh is a global leader in using DHIS2 for cancer screening, having implemented the DHIS2 Tracker for its national cervical cancer screening programme since 2018 -- possibly the only country to do so at national scale. Over 14,000 community clinics use the electronic health information system to register women and track screening outcomes through to treatment. Aggregated cancer data collection via DHIS2 began in 2013, and case-based individual-level tracking was introduced in 2019 using DHIS2 Tracker with SMS reminders and unique national identifiers linking screening centres and colposcopy clinics.

DHIS2 USE: HIGH
Bangladesh uses DHIS2 for both aggregate cancer data reporting (since 2013) and individual-level case tracking via DHIS2 Tracker (since 2018/2019) across its national cervical cancer screening programme, with extensive published academic literature documenting this use.

### Search Results

#### Bangla query results
(Query: Bangladesh cancer DHIS2)

1. **Cervical cancer screening data from the case-based national electronic registry in Bangladesh** -- https://pubmed.ncbi.nlm.nih.gov/40205589/
   Analysis of DHIS2 Tracker registry data covering 1.56 million women screened between 2018 and 2023, documenting VIA positivity rates and treatment outcomes.

2. **Implementation of an electronic health information system using DHIS2 tracker to manage and evaluate the National cervical screening programme in Bangladesh** -- https://bmcpublichealth.biomedcentral.com/articles/10.1186/s12889-025-22668-6
   Describes the 2019 upgrade to DHIS2 Tracker for individual-level cervical cancer screening data, including SMS reminders and data linkage across facility types.

3. **Cervical cancer screening data from the case-based national electronic registry in Bangladesh (PMC)** -- https://pmc.ncbi.nlm.nih.gov/articles/PMC11984040/
   Full-text version of the registry data analysis, showing 14,213 community clinics using the e-HIS by April 2024.

4. **Leveraging vertical COVID-19 investments to improve monitoring of cancer screening programme -- A case study from Bangladesh** -- https://pmc.ncbi.nlm.nih.gov/articles/PMC9755639/
   Case study on how COVID-19 digital health investments were repurposed to strengthen DHIS2-based cancer screening monitoring.

5. **Electronic aggregated data collection on cervical cancer screening in Bangladesh since 2014: what the data tells us?** -- https://link.springer.com/article/10.1186/s12889-023-17545-z
   BMC Public Health study examining a decade of aggregated cervical cancer screening data collected through DHIS2 since 2014.

#### English query results
(Query: Bangladesh cancer registry DHIS2 screening)

6. **Cervical cancer screening data from the case-based national electronic registry in Bangladesh (BMC Global and Public Health)** -- https://link.springer.com/article/10.1186/s44263-025-00145-x
   Journal article documenting the national electronic cancer screening registry built on DHIS2 Tracker.

7. **Trends and Burden of Breast and Cervical Cancer in Bangladesh: A Data-Driven Analysis and Future Outlook** -- https://www.medrxiv.org/content/10.64898/2025.12.05.25341679v1
   Preprint analysing breast and cervical cancer trends using data from the DHIS2-based electronic reporting system initiated in 2013.

8. **Cancer care in Bangladesh: an urgent need for strategic intervention and comprehensive reform** -- https://link.springer.com/article/10.1007/s44250-025-00283-x
   Review of cancer care challenges in Bangladesh, noting the DHIS2-based screening infrastructure.

9. **DHIS -- Interface for collection of nation-wide health data (DGHS)** -- https://old.dghs.gov.bd/index.php/en/e-health/our-ehealth-eservices/84-english-root/ehealth-eservice/94-dhis-interface-for-collection-of-nation-wide-health-data
   Official government page from the Directorate General of Health Services describing the national DHIS platform.

10. **Implementation of an electronic health information system using DHIS2 tracker (PubMed)** -- https://pubmed.ncbi.nlm.nih.gov/40325476/
    PubMed entry for the BMC Public Health paper on the DHIS2 Tracker implementation for cervical cancer screening.


## Benin -- Cancer & DHIS2 Profile

### Summary
Benin uses DHIS2 as the platform underlying its national health information system (SNIGS), which has been in operation since 2014 for data entry, validation, and analysis of health statistics from health centres. However, there is no evidence that DHIS2 is specifically configured for cancer screening or cancer registry functions. Benin established a population-based cancer registry in Cotonou in 2013-2014, and pilot cervical cancer screening projects exist (e.g. Care4Afrique), but these do not appear to be integrated with DHIS2. Breast cancer remains the leading cancer among women, and late-stage diagnosis is common due to the absence of organised screening programmes.

DHIS2 USE: LOW
DHIS2 is used as the general HMIS platform (SNIGS) in Benin since 2014, but no evidence was found of cancer-specific modules, trackers, or screening data integration within DHIS2.

### Search Results

#### French query results
(Query: Benin cancer DHIS2 SNIGS depistage)

1. **Annuaire des Statistiques Sanitaires 2018 -- Republique du Benin** -- https://files.aho.afro.who.int/afahobckpcontainer/production/files/Annuaire_2018_MS.pdf
   Official health statistics yearbook documenting the SNIGS system and health data from across Benin, including DHIS2 as the data collection platform.

2. **Age du diagnostic des cancers du sein en Republique du Benin** -- https://www.sciencedirect.com/science/article/pii/S0398762020302224
   Study on the age at breast cancer diagnosis in Benin, noting only 563 histologically confirmed cases over five years against an expected 1,500+ annual new cases.

3. **Draft PNDS 2018-2022 (National Health Development Plan)** -- https://www.prb.org/wp-content/uploads/2020/06/Benin-Plan-National-de-D%C3%A9veloppement-Sanitaire-2018-2022.pdf
   National health development plan referencing the SNIGS system and its role as a decision-support tool for health policy implementation.

#### English query results
(Query: Benin cancer DHIS2 SNIGS screening registry)

4. **Cancer incidence in Cotonou (Benin), 2014-2016: First results from the cancer Registry of Cotonou** -- https://www.sciencedirect.com/science/article/abs/pii/S187778211830479X
   First published results from the population-based Cotonou Cancer Registry, established in 2013-2014 under the National NCD Programme.

5. **Barriers and opportunities related to access to oncology care in Benin: a qualitative study on breast cancer** -- https://pmc.ncbi.nlm.nih.gov/articles/PMC12107913/
   Qualitative study documenting barriers to cancer care in Benin, noting the absence of systematic breast cancer screening programmes.


## Bhutan -- Cancer & DHIS2 Profile

### Summary
Bhutan uses DHIS2 (branded as Druk HMIS) as its national health management information system, and cancer screening data is reported through this platform from the screening stage through to treatment. The government implemented a landmark time-bound population-level screening programme for gastric, cervical, and breast cancers from 2020 to 2023, achieving 91.2% coverage of the eligible population. Bhutan has achieved the WHO 90-70-90 targets for cervical cancer elimination. While DHIS2 captures aggregate cancer screening data, the country is also transitioning to an Electronic Patient Information System with ICD-11 coding under the Digital Drukyul initiative to improve individual-level data capture.

DHIS2 USE: HIGH
Cancer screening data is reported through DHIS2 (Druk HMIS) at all health facility levels, from primary health care centres to hospitals. The system captures data from screening through treatment, with every facility having unique login credentials for data entry and reporting.

### Search Results

#### Dzongkha query results
(Query: Bhutan cancer DHIS2 "Druk HMIS")

1. **Druk HMIS and Tracking system (login page)** -- https://drukhmis.gov.bt/dhis/dhis-web-commons/security/login.action
   Official Druk HMIS portal confirming the system runs on DHIS2 software for Bhutan's national health data management.

#### English query results
(Query: Bhutan cancer DHIS2 "Druk HMIS" screening registry)

2. **Population-level cancer screening and cancer care in Bhutan, 2020-2023: a review** -- https://pmc.ncbi.nlm.nih.gov/articles/PMC10910341/
   Lancet Regional Health review documenting Bhutan's population-level cancer screening programme (gastric, cervical, breast) with 91.2% coverage, noting cancer data reporting through DHIS2.

3. **Bhutan achieves the 90-70-90 targets on the path to elimination of cervical cancer** -- https://www.sciencedirect.com/science/article/abs/pii/S2213538325000402
   Documents Bhutan's achievement of WHO cervical cancer elimination targets: 90% HPV vaccination, 70% screening coverage, 90% treatment of pre-cancers.

4. **Gastric cancer prevention programme in Bhutan -- NCBI Bookshelf** -- https://www.ncbi.nlm.nih.gov/books/NBK615771/
   Chapter describing Bhutan's H. pylori screen-and-treat strategy for gastric cancer prevention as part of the broader population screening programme.

5. **Cervical cancer screening in rural Bhutan with the careHPV test on self-collected samples (REACH-Bhutan)** -- https://pmc.ncbi.nlm.nih.gov/articles/PMC5734451/
   Cross-sectional population-based study on HPV-based cervical cancer screening approaches in rural Bhutan.

6. **Case studies on the progress of cervical cancer screening programs in Bhutan, India, and Turkiye** -- https://www.researchgate.net/publication/385632281_Case_studies_on_the_progress_of_cervical_cancer_screening_programs_in_Bhutan_India_and_Turkiye
   Comparative case study documenting Bhutan's cervical cancer screening programme progress alongside India and Turkiye.

7. **Population-level cancer screening and cancer care in Bhutan, 2020-2023: a review (Lancet)** -- https://www.thelancet.com/journals/lansea/article/PIIS2772-3682(24)00019-2/fulltext
   Full-text Lancet article on Bhutan's cancer screening and care programme, noting the Digital Drukyul Electronic Patient Information System with ICD-11 coding being introduced in 2023.

8. **Cervical cancer Bhutan 2021 country profile (WHO)** -- https://www.who.int/publications/m/item/cervical-cancer-btn-country-profile-2021
   WHO country profile summarising cervical cancer burden, screening, and vaccination status in Bhutan.


## Botswana -- Cancer & DHIS2 Profile

### Summary
Botswana uses DHIS2 as its national health management information system, including for malaria case-based surveillance and supply chain management. However, no evidence was found that DHIS2 is specifically used for cancer screening or cancer registry functions. The Botswana National Cancer Registry (BNCR) was established in 1999 as a population-based registry endorsed by IARC, collecting data from referral and district hospitals through traditional methods. Cervical cancer burden is high due to HIV co-infection, and a See-and-Treat programme is expanding, but these programmes do not appear to be integrated into DHIS2.

DHIS2 USE: LOW
Botswana uses DHIS2 for general health information management, malaria surveillance, and supply chain data. No evidence was found of cancer-specific data collection, screening tracking, or cancer registry functions within DHIS2.

### Search Results

#### English query results
(Query: Botswana cancer DHIS2 screening registry)

1. **Botswana National Cancer Registry (AFCRN)** -- https://afcrn.org/membership/members/118-bncr
   Profile of the BNCR, a population-based cancer registry established in 1999 under the Ministry of Health with IACR/IARC support, collecting data from hospitals across the country.

2. **Cancer Care and Prevention -- Rutgers Global Health** -- https://globalhealth.rutgers.edu/where-we-work/botswana/cancer-care-and-prevention/
   Overview of Rutgers' cancer care and prevention partnerships in Botswana, including cervical cancer screening initiatives.

3. **Cervical Cancer in Botswana: Current State and Future Steps for Screening and Treatment Programs** -- https://pmc.ncbi.nlm.nih.gov/articles/PMC4630577/
   Review of cervical cancer burden in Botswana, noting high HIV co-infection rates and limited screening infrastructure.

4. **Trends in Cancer Incidence and Associated Risk Factors in People Living with and Without HIV in Botswana: Population-Based Cancer Registry Data Analysis 1990-2021** -- https://www.mdpi.com/2072-6694/17/14/2374
   Analysis of 30 years of BNCR data examining cancer incidence trends by HIV status.

#### Setswana query results
(Query: Botswana cancer DHIS2 kankere)

5. **Factors Related to Advanced Stage of Cancer Presentation in Botswana** -- https://ascopubs.org/doi/10.1200/JGO.18.00129
   Study examining why cancers are frequently diagnosed at advanced stages in Botswana, identifying barriers to early detection.


## Brazil -- Cancer & DHIS2 Profile

### Summary
Brazil has a well-developed ecosystem of cancer information systems managed by the National Cancer Institute (INCA) and DATASUS, including population-based cancer registries (RCBP) operating since 1967, the Siscan cancer information system, SISMAMA for breast cancer, and APAC-ONCO for oncology procedures. However, these systems are purpose-built national platforms and do not use DHIS2. Brazil is not known to use DHIS2 as its health management information system. The IARC-HISP Centre collaboration is developing standardised DHIS2 cancer registry toolkits globally, and a DHIS2 Community post discusses DHIS2-CanReg5 interoperability, but no evidence was found of Brazil adopting DHIS2 for cancer data.

DHIS2 USE: NO EVIDENCE
Brazil uses its own suite of national health information systems (Siscan, SISMAMA, RCBP, SUS/DATASUS) for cancer data. No evidence was found of DHIS2 being used for cancer screening, cancer registry, or general health information management in Brazil.

### Search Results

#### Portuguese query results
(Query: Brasil cancer DHIS2 registro cancer rastreio)

1. **Registro de Cancer de Base Populacional (RCBP) -- IBGE** -- https://ces.ibge.gov.br/base-de-dados/metadados/ministerio-da-saude/registro-de-cancer-de-base-populacional-rcbp.html
   Official metadata page for Brazil's population-based cancer registries, operational since 1967 with over 20 registries across the country.

2. **Deteccao Precoce do Cancer -- INCA** -- https://www.inca.gov.br/sites/ufu.sti.inca.local/files/media/document/deteccao-precoce-do-cancer.pdf
   INCA publication on early cancer detection strategies in Brazil, covering screening guidelines for breast, cervical, and colorectal cancers.

#### English query results
(Query: Brazil cancer DHIS2 registry screening)

3. **Impact of screening on cervical cancer incidence and mortality in a Northern Brazilian city** -- https://pmc.ncbi.nlm.nih.gov/articles/PMC9458261/
   Study examining the impact of screening programmes on cervical cancer outcomes in northern Brazil since 1998.

4. **National Cancer Institute and the 2023-2025 Estimate -- Cancer Incidence in Brazil** -- https://pmc.ncbi.nlm.nih.gov/articles/PMC10021001/
   INCA's official cancer incidence estimates for 2023-2025, based on data from the RCBP network.

5. **Harms and benefits of mammographic screening for breast cancer in Brazil** -- https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0297048
   PLOS ONE study evaluating the balance of harms and benefits from Brazil's opportunistic mammographic screening approach.


## Burkina Faso -- Cancer & DHIS2 Profile

### Summary
Burkina Faso uses DHIS2 as its national HMIS under the name ENDOS (Entrepot National des Donnees de la Sante), operational since 2013. There is documented evidence that DHIS2 Tracker has been deployed for cervical cancer screening and patient follow-up, particularly through the SUCCESS project which implements HPV-based screening with SMS-based patient notifications. The country adopted a national cancer control strategy (2021-2025) but lacks a fully functional population-based cancer registry.

DHIS2 USE: HIGH
Burkina Faso has confirmed use of DHIS2 Tracker for cervical cancer screening programme management (SUCCESS project), integrated within the existing ENDOS national HMIS. DHIS2 Tracker is used for patient follow-up, SMS notifications for HPV test results, and appointment reminders.

### Search Results

#### French query results
1. **Accelerer l'elimination du cancer du col de l'uterus au Burkina Faso** -- https://www.uicc.org/news/acc%C3%A9l%C3%A9rer-l%C3%A9limination-du-cancer-du-col-de-lut%C3%A9rus-au-burkina-faso
   UICC article on the SUCCESS project using DHIS2 Tracker for cervical cancer elimination in Burkina Faso.

2. **Strategie nationale de lutte contre le cancer 2021-2025** -- https://www.iccp-portal.org/sites/default/files/plans/BFA_B5_Strat%C3%A9gie%20nationale%20de%20lutte%20contre%20le%20cancer%202021-2025_VF%2022-10-2020.pdf
   Burkina Faso's national cancer control strategy document for 2021-2025.

3. **Implementation du depistage du cancer du col -- Medecins du Monde** -- https://www.medecinsdumonde.org/app/uploads/2024/04/MDM-Rapport-CCU-BurkinaFaso_version-online-1.pdf
   Report on cervical cancer screening implementation in Burkina Faso.

4. **Renforcer la prevention secondaire du cancer du col de l'uterus (SUCCESS/Expertise France)** -- https://linitiative.expertisefrance.fr/app/uploads/2025/01/capitalisation-success-renforcer-la-prevention-secondaire-du-cancer-du-col-de-luterus.pdf
   SUCCESS project capitalisation report describing use of DHIS2 Tracker for cervical cancer screening in Burkina Faso and Cote d'Ivoire.

5. **Au Burkina Faso, ameliorer le depistage et la prevention du cancer du col de l'uterus (WHO AFRO)** -- https://www.afro.who.int/fr/news/au-burkina-faso-ameliorer-le-depistage-et-la-prevention-du-cancer-du-col-de-luterus
   WHO AFRO article on improving cervical cancer screening and prevention in Burkina Faso.

6. **Le Cancer du Sein a Bobo-Dioulasso: Resultats de la Prise en Charge** -- https://www.techscience.com/oncologie/v24n2/48737/html
   Research article on breast cancer management outcomes in Bobo-Dioulasso, Burkina Faso.

7. **Evaluation de la performance de la gestion du systeme d'information sanitaire de routine (PRISM)** -- https://www.measureevaluation.org/resources/publications/gr-19-101-fr.html
   MEASURE Evaluation assessment of routine health information system performance in Burkina Faso, including ENDOS/DHIS2.

#### English query results
1. **Current status of digital health interventions in the health system in Burkina Faso (BMC Medical Informatics)** -- https://bmcmedinformdecismak.biomedcentral.com/articles/10.1186/s12911-024-02574-4
   Comprehensive review of digital health interventions including ENDOS/DHIS2 and cancer-related tracker programmes in Burkina Faso.

2. **Burkina Faso HMIS - ENDOS (DHIS2 + Tracker) -- DIAL Exchange** -- https://exchange.dial.global/projects/burkina-faso-burkina-faso-hmis--endos-dhis2--tracker
   Profile of Burkina Faso's ENDOS system built on DHIS2 with Tracker functionality.

3. **How do countries select and use digital global goods in emergency settings? (Oxford Open Digital Health)** -- https://academic.oup.com/oodh/article/2/Supplement_1/i64/7560469
   Academic article discussing DHIS2 COVID-19 data management in Burkina Faso, Mali and Suriname, with mention of cancer tracker forms.

4. **Implementation of HPV-based screening in Burkina Faso: lessons from the PARACAO study (BMC Women's Health)** -- https://bmcwomenshealth.biomedcentral.com/articles/10.1186/s12905-021-01392-4
   Research article on HPV-based cervical cancer screening implementation in Burkina Faso.

5. **Cervical cancer prevention in Burkina Faso: stakeholder collaboration (Frontiers in Oncology)** -- https://www.frontiersin.org/journals/oncology/articles/10.3389/fonc.2024.1383133/full
   2024 article on cervical cancer prevention stakeholder collaboration in Burkina Faso.

6. **Low attendance in cervical cancer screening in Burkina Faso (ScienceDirect)** -- https://www.sciencedirect.com/science/article/pii/S0398762023004261
   Study on geographical disparities and sociodemographic determinants of cervical cancer screening uptake in Burkina Faso.


## Burundi -- Cancer & DHIS2 Profile

### Summary
Burundi uses DHIS2 as its national health information system, with interoperability efforts underway between DHIS2 and OpenClinic GA hospital software. Cancer surveillance data is partially captured through DHIS2 aggregate reporting, but there is no dedicated cancer module in DHIS2 Tracker. The country established a population-based cancer registry in Bujumbura in 2018 but it remains nascent with limited staffing. Burundi adopted a national cancer control strategic plan (2022-2025), and cervical cancer screening (VIA/cryotherapy) has been rolled out in 4 provinces, but no evidence of a DHIS2-based cancer-specific tracker was found.

DHIS2 USE: LOW
DHIS2 is the national HMIS in Burundi and likely captures some aggregate cancer-related data, but there is no documented evidence of DHIS2 Tracker being used specifically for cancer registry or cancer screening programme management. Cancer data collection relies on separate systems (OPEN CLINIC at hospitals) with interoperability to DHIS2 under development.

### Search Results

#### English query results
1. **Burundi Cancer Care Needs: A Call to Action (PMC)** -- https://pmc.ncbi.nlm.nih.gov/articles/PMC7938392/
   Comprehensive assessment of cancer care needs in Burundi, noting the lack of a fully functioning cancer registry and reliance on GLOBOCAN estimates. Mentions DHIS2 as part of the national health information system.

2. **Cancer registry case studies (ECSA-HC)** -- https://ecsahc.org/wp-content/uploads/2021/07/Cancer-registry-case-studies.pdf
   Case studies of cancer registries in East, Central and Southern Africa, including data on Bujumbura population-based cancer registry established in 2018.

3. **Regional Program of Cancer Registries (World Bank)** -- https://documents1.worldbank.org/curated/en/418271487077912136/pdf/SG-PRW-PID-CP-P163187-02-14-2017-1487077906033.pdf
   World Bank document on regional cancer registry programmes in Africa, with relevance to Burundi's registry development.

4. **Extending and Strengthening Routine DHIS2 Surveillance (CDC)** -- https://stacks.cdc.gov/view/cdc/123311/cdc_123311_DS1.pdf
   CDC document on extending DHIS2 surveillance capabilities, with relevance to Burundi's health information infrastructure.

#### French query results
5. **Plan strategique national de lutte contre le cancer 2022-2025 (Burundi)** -- https://www.iccp-portal.org/sites/default/files/plans/PSNCancer_2022_2025_vf%20PNLCa.pdf
   Burundi's national cancer control strategic plan for 2022-2025.

6. **Plan national de lutte contre le cancer (PNLC) -- Burundi** -- https://www.iccp-portal.org/sites/default/files/plans/PNLC%20Version%20pr%C3%A9fac%C3%A9.pdf
   Earlier version of Burundi's national cancer control plan from the Ministry of Public Health.

7. **Pour combattre le cancer, il faut le depister tot (WHO AFRO)** -- https://www.afro.who.int/fr/countries/burundi/news/pour-combattre-le-cancer-il-faut-le-depister-tot
   WHO AFRO article on early cancer detection in Burundi.

8. **Burundi: Cancer screening rate not improving despite free examinations (Jimbere)** -- https://www.jimberemag.org/burundi-evolution-rythme-depistage-cancer-gratuite-examens/
   Journalism piece reporting that cancer screening uptake in Burundi remains low despite free screening services.

9. **Interoperabilite DHIS2-OpenClinic GA au Burundi (ResearchGate)** -- https://www.researchgate.net/profile/Frank-Verbeke-2/publication/333448924_Interoperabilite_DHIS2-OpenClinic_GA_au_Burundi/links/5cee616c92851c19eae6d692/Interoperabilite-DHIS2-OpenClinic-GA-au-Burundi
   Research paper on interoperability between DHIS2 and OpenClinic GA hospital information system in Burundi, relevant to cancer data flow.

#### Kirundi query results
10. **In Burundi, Local Partners Lead the Way for Quality of Care (PSI)** -- https://www.psi.org/2024/07/in-burundi-local-partners-lead-the-way-for-quality-of-care/
    PSI article on health quality of care in Burundi, with relevance to cancer screening services.

Note: The Kirundi-language search returned very few relevant results specific to cancer and DHIS2.


## Cambodia -- Cancer & DHIS2 Profile

### Summary
Cambodia announced its transition to DHIS2 as the national health management information system in 2024-2025, with a pilot launch scheduled for June 2025 and full national rollout planned for 2026 in partnership with UNICEF, HISP UiO, and HISP Vietnam. Prior to this, Cambodia used a different HMIS. There is no evidence that DHIS2 has been used for cancer screening or cancer registry functions in Cambodia to date. Cancer is a significant burden, with liver cancer ranking first among males. Breast and cervical cancer screening rates remain low (10.6% and 15.3% respectively, per the 2022 DHS).

DHIS2 USE: LOW
Cambodia is in the early stages of transitioning its national HMIS to DHIS2 (pilot in 2025, rollout in 2026). No cancer-specific DHIS2 modules or tracker programmes have been identified. Cancer data has not yet been captured through DHIS2.

### Search Results

#### Khmer query results
(Query: Cambodia cancer DHIS2)

1. **Primary liver cancer disease burden in Cambodia during 1990-2021: a systematic analysis from the Global Burden of Disease Study 2021** -- https://pubmed.ncbi.nlm.nih.gov/40596871/
   GBD analysis of liver cancer trends in Cambodia, documenting the country's high liver cancer burden driven by viral hepatitis.

#### English query results
(Query: Cambodia cancer DHIS2 screening registry)

2. **Breast and cervical cancer screening among women at reproductive age in Cambodia: secondary analysis of Cambodia DHS 2022** -- https://pmc.ncbi.nlm.nih.gov/articles/PMC11781645/
   Analysis of the 2022 Cambodia DHS finding breast cancer screening at 10.6% and cervical cancer screening at 15.3% among women of reproductive age.

3. **Oncology in Cambodia (Karger)** -- https://karger.com/Article/FullText/336791
   Overview of oncology services and cancer epidemiology in Cambodia, noting cancer incidence of approximately 153/100,000 for males and 123/100,000 for females.

4. **CHAI Associate, Digital Health (job posting referencing Cambodia)** -- https://www.unjobnet.org/jobs/detail/52518580
   Job posting from the Clinton Health Access Initiative for digital health work in Cambodia, referencing the national HMIS transition to DHIS2.

5. **Improving Cancer Data Availability Within Routine Reporting Systems in a Low-Income Setting (Uganda)** -- https://jhia-online.org/index.php/jhia/article/view/546
   While focused on Uganda, this article provides a relevant framework for integrating cancer data into DHIS2-based routine reporting systems that may inform Cambodia's future approach.


## Cameroon -- Cancer & DHIS2 Profile

### Summary
Cameroon uses DHIS2 as its national health information system, operated by the Ministry of Public Health (MINSANTE), with 788 personnel trained across all 10 regions and 189 health districts by 2017. Cancer reports are sourced from DHIS2 data, and the National Cancer Control Programme (CNLCa) uses this data for its annual reports. However, Cameroon lacks a national organised screening programme for breast and cervical cancer, and the cancer registry system remains limited -- hospital-based registries exist in Yaounde and Douala (established 2003) but a comprehensive national population-based registry is absent. The health facility registry has been digitised with Bluesquare tools integrated into DHIS2.

DHIS2 USE: MODERATE
DHIS2 is used as the national HMIS and cancer reports are generated from DHIS2 data. However, there is no evidence of cancer-specific tracker modules, and the country lacks organised screening programmes or DHIS2-based cancer registries.

### Search Results

#### French query results
(Query: Cameroun cancer DHIS2 depistage registre)

1. **Plan Strategique National de Prevention et de Lutte contre le Cancer (Cameroon)** -- https://www.iccp-portal.org/system/files/plans/FINAL%20COPY%20PSNPLCa%20FRENCH.pdf
   National cancer control strategic plan outlining prevention, screening, and treatment priorities, referencing data infrastructure including DHIS2.

2. **Report Cancer 2021 -- Cameroon NHO** -- http://onsp.minsante.cm/en/publication/302/report-cancer-2021
   National cancer report for 2021, with data sourced from DHIS2, covering cancer incidence and outcomes across Cameroon.

3. **MINSANTE -- Liste des personnels formes a l'utilisation du logiciel DHIS2 en 2017** -- https://www.minsante.cm/site/?q=fr/content/liste-des-personnels-form%C3%A9s-%C3%A0-lutilisation-du-logiciel-dhis2-en-2017
   Official Ministry of Health page listing 788 health personnel trained in DHIS2 across all regions.

4. **Cancer du sein et du col de l'uterus: nouvelle technique de depistage a Douala** -- https://www.cameroon-tribune.cm/article.html/51629/fr.html/cancer-du-sein-du-col-de-luterus-nouvelle-technique-de-depistage-douala
   News article on new breast and cervical cancer screening techniques being introduced in Douala.

#### English query results
(Query: Cameroon cancer DHIS2 screening registry)

5. **The Evolution of a Hospital-Based Cancer Registry in Northwest Cameroon from 2004 to 2015** -- https://pubmed.ncbi.nlm.nih.gov/33020840/
   Study documenting the evolution of a hospital-based cancer registry in northwest Cameroon over 11 years.

6. **Cancer on the Global Stage: Incidence and Cancer-Related Mortality in Cameroon (ASCO Post)** -- https://ascopost.com/issues/december-10-2024/cancer-on-the-global-stage-incidence-and-cancer-related-mortality-in-cameroon/
   2024 overview of cancer incidence and mortality trends in Cameroon, noting over 15,700 new cases diagnosed annually.

7. **Feasibility of cancer genetic counselling and screening in Cameroon** -- https://ecancer.org/en/journal/article/1588-feasibility-of-cancer-genetic-counselling-and-screening-in-cameroon-perceived-benefits-and-barriers
   Study assessing the feasibility of introducing cancer genetic counselling and screening services in Cameroon.

8. **FROM DIAGNOSIS TO SURVIVAL: CANCER IN CAMEROON** -- https://rightforeducation.org/2025/03/19/from-diagnosis-to-survival-cancer-in-cameroon/
   Overview of cancer diagnosis and survival challenges in Cameroon, highlighting the absence of organised national screening programmes.

9. **Digitalization of the Health Facility Registry -- Bluesquare** -- https://www.bluesquarehub.com/digitalization-of-the-health-facility-registry-a-comprehensive-suite-offered-by-bluesquare/
   Description of Bluesquare's work with Cameroon to digitise the health facility registry, fully integrated with DHIS2.

10. **Cameroon -- Yaounde Cancer Registry 2004-2006 (GHDx)** -- https://ghdx.healthdata.org/record/cameroon-yaounde-cancer-registry-2004-2006-iicc-3
    Global Health Data Exchange record for the Yaounde Cancer Registry, one of two hospital-based registries established in 2003.


## Cape Verde -- Cancer & DHIS2 Profile

### Summary
Cape Verde (Cabo Verde) has been implementing DHIS2 as its national health information platform, with a maturity assessment conducted in 2022 and integration of modules for malaria, surveillance, and other health programmes underway. However, no evidence was found that DHIS2 is used for cancer screening or cancer registry functions. Cancer is the second cause of death in the country, with approximately 300 deaths per year. Hospital-based cancer registries exist at Hospital Agostinho Neto (Santiago Island, since 2017) and Hospital Dr. Baptista de Sousa (Sao Vicente), and a national population-based cancer registry is under construction. Late diagnosis remains a major challenge, with over 60% of cancers diagnosed at advanced stages.

DHIS2 USE: LOW
Cape Verde uses DHIS2 for general health information management and surveillance, but no cancer-specific modules, screening trackers, or registry functions were found within the DHIS2 platform.

### Search Results

#### Portuguese query results
(Query: "Cabo Verde" cancer DHIS2 rastreio cancro)

1. **Programa de Prevencao e Rastreio de Cancros (Cabo Verde)** -- https://www.iccp-portal.org/sites/default/files/plans/CPV_B5_Manual%20de%20Preven%C3%A7%C3%A3o%20e%20Controlo%20de%20Doen%C3%A7as%20Oncol%C3%B3gicas%202015.pdf
   Official cancer prevention and screening programme manual from the Ministry of Health of Cabo Verde (2015).

2. **Cancro na Ilha de Sao Vicente, Cabo Verde: auditoria aos registos** -- https://revista.spcir.com/index.php/spcir/article/download/990/662/
   Audit of cancer registry data from Hospital Dr. Baptista de Sousa on Sao Vicente Island (2018-2019).

3. **Diagnostico tardio continua a ser um entrave no combate ao cancro no pais** -- https://expressodasilhas.cv/pais/2026/02/08/diagnostico-tardio-continua-a-ser-um-entrave-no-combate-ao-cancro-no-pais/101283
   February 2026 news article documenting that late diagnosis remains a major barrier to cancer control in Cabo Verde.

#### English query results
(Query: "Cape Verde" cancer DHIS2 screening registry)

4. **Cancer in Santiago Island, Cape Verde: data from the Hospital Agostinho Neto Cancer Registry (2017-2018)** -- https://pmc.ncbi.nlm.nih.gov/articles/PMC6974378/
   First published data from the Hospital Agostinho Neto Cancer Registry established in 2017, serving as a key data source for the national registry under construction.

5. **Breast cancer in Cape Verde: a 24-year retrospective study at Agostinho Neto University Hospital** -- https://pmc.ncbi.nlm.nih.gov/articles/PMC11959127/
   Retrospective study of breast cancer clinical presentation, treatment, and outcomes over 24 years in Cape Verde.


## Central African Republic -- Cancer & DHIS2 Profile

### Summary
The Central African Republic (CAR) began implementing DHIS2 in 2022 as part of an ambitious revitalisation of its National Health Information System (SNIS), with configuration and user training activities for health centres, health posts, and district hospitals. However, the health system remains severely resource-constrained with no radiotherapy unit, very limited diagnostic and treatment capacities, and no population-based cancer registry. The country's only pathology laboratory in Bangui produces the sole source of cancer data. There is no evidence that DHIS2 is used for cancer-specific data collection or screening in CAR.

DHIS2 USE: LOW
DHIS2 was introduced in 2022 for general health information system revitalisation. No cancer-specific modules, trackers, or screening integration within DHIS2 were identified. Cancer data infrastructure remains extremely limited.

### Search Results

#### French query results
(Query: "Republique centrafricaine" cancer DHIS2 depistage)

1. **Revitalisation du SNIS en Republique Centrafricaine: Focus sur DHIS2** -- https://hispwca.org/hispwca/?p=1483
   Article from HISP West and Central Africa documenting the 2019 action plan to revitalise the SNIS by integrating DHIS2, with implementation activities beginning in 2022.

2. **Plan Strategique National de Lutte contre le Cancer 2022-2025** -- https://www.iccp-portal.org/sites/default/files/plans/PSNCancer_2022_2025_vf%20PNLCa.pdf
   National cancer control strategic plan for 2022-2025, outlining priorities for cancer prevention, screening, and treatment in CAR.

3. **Centrafrique: la FAFECA s'engage pour Octobre Rose et appelle au depistage du cancer du sein** -- https://rca.news-pravda.com/car/2025/10/06/14614.html
   News coverage of Octobre Rose breast cancer awareness and screening advocacy by the FAFECA civil society organisation.

#### English query results
(Query: "Central African Republic" cancer DHIS2 screening)

4. **Assessing Cancer Control Capacities in the Central African Republic (IAEA)** -- https://www.iaea.org/newscenter/news/assessing-cancer-control-capacities-in-the-central-african-republic
   IAEA assessment of cancer control capacities, noting the absence of radiotherapy, limited diagnostics, and proposal to establish a population-based cancer registry.

5. **Histo-epidemiological profile of breast cancers among women in the Central African Republic: about 174 cases** -- https://pubmed.ncbi.nlm.nih.gov/29621999/
   Study of 174 breast cancer cases profiling the histo-epidemiological characteristics of breast cancer in CAR.


## Chad -- Cancer & DHIS2 Profile

### Summary
Chad officially launched DHIS2 in November 2022, achieving 96.4% district-level data entry within the first year across all 23 health delegations and 139 functional health districts. The platform is used for general health information management, including a notable use case in malaria chemoprevention (CPS) digitalisation. However, no evidence was found of cancer-specific modules, screening trackers, or cancer registry integration within DHIS2. Cancer control infrastructure is minimal -- Chad is one of eight pioneer countries in the IAEA Rays of Hope initiative for radiotherapy access. A national cancer control strategic plan exists but specific digital cancer data systems were not identified.

DHIS2 USE: LOW
DHIS2 is used as the national HMIS since late 2022, with high district-level adoption. No cancer-specific data collection, screening programmes, or cancer registry functions within DHIS2 were identified.

### Search Results

#### French query results
(Query: Tchad cancer DHIS2 depistage)

1. **Tchad: Le ministere de la sante publique lance le Systeme de Gestion de l'information Sanitaire (DHIS2)** -- https://tribuneechos.com/tchad-le-ministere-de-la-sante-publique-lance-le-systeme-de-gestion-de-linformation-sanitaire-dhis2/
   News article covering the official Ministry of Health launch of DHIS2 as the national health information management system.

#### English query results
(Query: Chad cancer DHIS2 screening registry)

#### Arabic query results
(Query: تشاد سرطان DHIS2)

2. **Chad and Senegal reach key milestones in Rays of Hope initiative (IAEA)** -- https://www.iaea.org/newscenter/news/chad-and-senegal-reach-key-milestones-in-rays-of-hope-initiative-and-cancer-control-planning
   IAEA report on Chad's progress as a pioneer country in the Rays of Hope initiative to increase access to radiotherapy for cancer patients.


## Chile — Cancer & DHIS2 Profile

### Summary
Chile's Ministry of Health began implementing DHIS2 in 2022 for specific programs, including RENCI (National Childhood Cancer Registry). The IARC and HISP Centre have included Chile among early adopters piloting standardized DHIS2-based cancer registry tools. However, Chile still lacks a comprehensive national cancer registry, and cancer screening programs (e.g., colorectal) operate through separate systems.

DHIS2 USE: MODERATE
Chile has adopted DHIS2 for the national childhood cancer registry (RENCI) and is among the countries piloting IARC/HISP standardized DHIS2 cancer registry tools, but broader cancer surveillance and screening programs do not yet appear to use DHIS2.

### Search Results

#### Spanish query results
1. **Cancer en Chile y el mundo: Una mirada epidemiologica, presente y futuro** — https://www.elsevier.es/es-revista-revista-medica-clinica-las-condes-202-articulo-cancer-chile-el-mundo-una-S0716864013701950
   Epidemiological overview of cancer in Chile and globally, published in Revista Medica Clinica Las Condes.

2. **Plan Nacional de Cancer** — https://cdn.digital.gob.cl/filer_public/d3/0a/d30a1f5e-53d9-4a31-a4fe-e90d8d9a2348/documento_plan_nacional_de_cancer.pdf
   Chile's national cancer plan document from the government digital platform.

3. **Desafios en la vigilancia de todos los casos de cancer en Chile: Registro Nacional de Cancer** — https://www.medwave.cl/enfoques/comunicacionesbreves/2771.html
   Medwave article discussing challenges in cancer surveillance and the national cancer registry in Chile.

4. **Plan Nacional de Cancer - Minsal** — https://www.minsal.cl/wp-content/uploads/2019/01/2019.01.23_PLAN-NACIONAL-DE-CANCER_web.pdf
   Ministry of Health national cancer plan (2019), covering screening, registration, and treatment strategies.

5. **Distribucion geografica de la incidencia de cancer** — https://www.scielo.cl/scielo.php?script=sci_arttext&pid=S0034-98872023000300340
   SciELO study on geographic distribution of cancer incidence among beneficiaries of an oncology care agreement in Chile.

#### English query results
6. **Barriers to the use of tests for early detection of colorectal cancer in Chile** — https://www.nature.com/articles/s41598-024-58920-z
   Nature Scientific Reports (2024) study examining barriers to colorectal cancer early detection testing in Chile.

7. **Addressing the rising burden of cancer in Chile: Challenges & opportunities** — https://www.hsph.harvard.edu/health-systems-innovation-lab/wp-content/uploads/sites/2633/2023/03/UICC-ICCILA-Chile-Report-ENGLISH-FINAL.pdf
   Harvard School of Public Health report (2023) on cancer challenges and opportunities in Chile.

8. **Colorectal Cancer Screening Programs in Latin America: A Systematic Review and Meta-Analysis** — https://jamanetwork.com/journals/jamanetworkopen/fullarticle/2814506
   JAMA Network Open systematic review covering colorectal cancer screening programs across Latin America including Chile.

9. **Cancer on the Global Stage: Incidence and Cancer-Related Mortality in Chile** — https://ascopost.com/issues/november-10-2020/incidence-and-cancer-related-mortality-in-chile
   ASCO Post article reviewing cancer incidence and mortality data in Chile.

10. **Histopathologic study from a colorectal cancer screening in Chile** — https://pubmed.ncbi.nlm.nih.gov/29958195/
    PubMed study on results from the Chile-Japan international collaboration on colorectal cancer screening.


## Colombia — Cancer & DHIS2 Profile

### Summary
Colombia has a well-established cancer surveillance infrastructure through its National Cancer Information System (NCIS) and population-based cancer registries, with the Cali registry being one of the oldest in Latin America (established 1962). The country uses SIVIGILA for public health surveillance of breast, cervical, and childhood cancers. While DHIS2 Academies have been held in Colombia, there is no evidence of DHIS2 being used specifically for cancer registration or screening in the country.

DHIS2 USE: LOW
Colombia has experience with DHIS2 (hosting academies), but cancer surveillance relies on its own national systems (SIVIGILA, NCIS/CAC) rather than DHIS2.

### Search Results

#### Spanish query results
1. **Vigilancia de la supervivencia global por cancer en Colombia: utilidad de los registros rutinarios** — https://www.elsevier.es/es-revista-revista-colombiana-cancerologia-361-articulo-vigilancia-supervivencia-global-por-cancer-S0123901515000219
   Revista Colombiana de Cancerologia article on surveillance of global cancer survival using routine registries in Colombia.

2. **Protocolo de vigilancia en salud publica cancer de mama y cuello uterino** — https://www.ins.gov.co/buscador-eventos/Lineamientos/Pro_C%C3%A1ncer%20de%20mama%20y%20cuello%20uterino%202024.pdf
   Colombian National Health Institute (INS) 2024 protocol for public health surveillance of breast and cervical cancer.

3. **Observatorio Nacional de Cancer Guia Metodologica** — https://www.minsalud.gov.co/sites/rid/Lists/BibliotecaDigital/RIDE/VS/ED/GCFI/guia-ross-cancer.pdf
   Ministry of Health methodological guide for the National Cancer Observatory in Colombia.

4. **Metodos del Registro de Cancer en Cali, Colombia** — http://www.scielo.org.co/scielo.php?pid=S1657-95342018000100109&script=sci_arttext&tlng=es
   SciELO article on the methods used by the Cali Population-Based Cancer Registry, one of the oldest in Latin America.

#### English query results
5. **Survival in stomach cancer: analysis of a national cancer information system and a population-based cancer registry in Colombia** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10226449/
   PMC study analyzing stomach cancer survival using NCIS data and the Cali cancer registry in Colombia.


## Comoros — Cancer & DHIS2 Profile

### Summary
Comoros adopted DHIS2 for its health information system, with a joint WHO-GAVI mission in 2019 supporting implementation. The country does not have a population-based cancer registry, with an estimated 619 new cancer cases in 2022 among its 836,000 population. Cancer screening efforts, particularly for cervical cancer, are led by civil society organizations in partnership with WHO, but there is no specific evidence of DHIS2 being used for cancer data collection or cancer registry purposes in Comoros.

DHIS2 USE: LOW
Comoros uses DHIS2 for general health information management, but no evidence was found of DHIS2 being specifically applied to cancer registration, screening, or surveillance.

### Search Results

#### Arabic query results
No relevant results found. The Arabic search returned results about astrology and unrelated medical conditions rather than cancer health systems in Comoros.

#### English query results
1. **Comoros Paves the Way to Safely Introducing Oncology Services for Cancer Care in the country** — https://www.iaea.org/newscenter/news/comoros-paves-the-way-to-safely-introducing-oncology-services-for-cancer-care-in-the-country
   IAEA article on Comoros' efforts to introduce oncology services for cancer care.

2. **Publication of cancer screening data from 17 francophone African countries in the CanScreen5 data repository** — https://www.iarc.who.int/news-events/publication-of-cancer-screening-data-from-17-francophone-african-countries-in-the-canscreen5-data-repository/
   IARC publication including Comoros cancer screening data in the CanScreen5 repository covering francophone African countries.

#### French query results
3. **Plan strategique national de lutte contre le cancer 2022-2025** — https://www.iccp-portal.org/sites/default/files/plans/PSNCancer_2022_2025_vf%20PNLCa.pdf
   Comoros national strategic plan for cancer control 2022-2025.

4. **Profil pour le cancer du col de l'uterus - Comores** — https://cdn.who.int/media/docs/default-source/country-profiles/cervical-cancer/cervical-cancer-com-2021-country-profile-fr.pdf
   WHO cervical cancer country profile for Comoros with morbidity and mortality data.

5. **Mission conjointe OMS-GAVI pour relancer le processus d'amelioration de la gestion des donnees du systeme d'information sanitaire aux Comores** — https://www.afro.who.int/fr/news/mission-conjointe-oms-gavi-pour-relancer-le-processus-damelioration-de-la-gestion-des-donnees
   WHO Africa article on the joint WHO-GAVI mission to improve health information system data management in Comoros, including DHIS2 implementation.


## Congo Republic (Brazzaville) — Cancer & DHIS2 Profile

### Summary
The Republic of Congo (Brazzaville) has a cancer registry based in Brazzaville and is working to strengthen cancer control. The Ministry of Health has implemented DHIS2 for health information management. However, there is no specific evidence of DHIS2 being used for cancer registration or screening in the country. Cancer screening challenges remain significant, with late diagnosis being common, particularly for breast and cervical cancers.

DHIS2 USE: LOW
The Republic of Congo has implemented DHIS2 for its health information system (confirmed by the Ministry of Health website), but no evidence was found of DHIS2 being specifically used for cancer data collection, registration, or screening.

### Search Results

#### French query results
1. **MISE EN OEUVRE DHIS2 - Ministere de la Sante et de la Population** — https://sante.gouv.cg/mise-en-oeuvre-dhis2/
   Republic of Congo Ministry of Health page on DHIS2 implementation in the national health system.

2. **Tolonga CANCER** — https://tolongacancer.org/
   NGO focused on cancer control in the Republic of Congo.

#### English query results
3. **Factors Contributing to Late Breast Cancer Diagnosis at the Brazzaville University Hospital in 2020, Congo** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11773541/
   PMC study (2025) analyzing factors behind delayed breast cancer diagnosis at Brazzaville University Hospital.

4. **Congo - Registre des cancers de Brazzaville** — https://afcrn.org/index.php/membership/membership-list/90-brazzaville-rep-congo
   African Cancer Registry Network page for the Brazzaville cancer registry in the Republic of Congo.

5. **The Republic of the Congo Sets Sights on Being a Reference Centre for Cancer Control** — https://www.iaea.org/newscenter/news/the-republic-of-the-congo-sets-sights-on-being-a-reference-centre-for-cancer-control
   IAEA article on the Republic of Congo's ambitions to become a regional reference centre for cancer control.


## Costa Rica — Cancer & DHIS2 Profile

### Summary
Costa Rica has one of the oldest and most recognized population-based cancer registries in Latin America, the National Tumor Registry (RNT), established in 1976 and internationally recognized by IARC since 1987. The country has well-established screening programs for cervical, breast, and gastric cancers. However, there is no evidence of DHIS2 being used for cancer registration or screening in Costa Rica; the country uses its own system (SIRNAT) for tumor notification and registration.

DHIS2 USE: NO EVIDENCE
No evidence was found of DHIS2 being used in Costa Rica for cancer or any other health information purpose. The country relies on SIRNAT for cancer registration.

### Search Results

#### Spanish query results
1. **Estadistica de Cancer - Registro Nacional Tumores** — https://www.ministeriodesalud.go.cr/index.php/biblioteca/material-educativo/material-publicado/estadisticas-y-bases-de-datos/estadisticas-y-bases-de-datos-vigilancia-de-la-salud/estadistica-de-cancer-registro-nacional-tumores
   Costa Rica Ministry of Health page for cancer statistics from the National Tumor Registry.

2. **El registro de Cancer de Costa Rica** — https://www.uhsalud.com/index.php/revhispano/article/download/311/177
   Academic article describing the cancer registry system in Costa Rica.

3. **INCIDENCIA Y MORTALIDAD DEL CANCER EN COSTA RICA** — https://www.binasss.sa.cr/incidenciacancer.pdf
   Report on cancer incidence and mortality in Costa Rica from the National Library of Health and Social Security.

#### English query results
4. **Epidemiological Patterns of Common Cancers in Costa Rica: An Overview up to 2020** — https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10475317/
   PMC article (2023) providing an epidemiological overview of common cancers in Costa Rica through 2020.

5. **The cervical cancer prevention programme in Costa Rica** — https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4631572/
   PMC article on Costa Rica's cervical cancer prevention programme, one of the early organized screening programs in the region.


## Cote d'Ivoire — Cancer & DHIS2 Profile

### Summary
Cote d'Ivoire uses DHIS2 as the core of its national health information system (branded SIGSANTE). The SUCCESS project, funded by Unitaid and implemented by Expertise France, adopted DHIS2 Tracker in 2021 for cervical cancer secondary prevention client tracking, screening nearly 60,000 women for HPV across Cote d'Ivoire and Burkina Faso. Cervical cancer is the leading cancer affecting women in Cote d'Ivoire, with 2,360 new cases in 2022 and a 62% mortality rate. DHIS2 data is used to monitor the evolution of cervical cancer precancerous lesions.

DHIS2 USE: HIGH
DHIS2 is the backbone of the national health information system (SIGSANTE), and the SUCCESS project specifically uses DHIS2 Tracker for cervical cancer screening client tracking and follow-up with demonstrated results.

### Search Results

#### French query results
1. **Manuel utilisateur du SIGSANTE (DHIS2)** — https://dipe.info/index.php/fr/surveillance-vih/evaluation-des-programmes/download/19-manuels-utilisateurs-apps-snis/46-manuel-utilisateur-du-sigsante-dhis2
   User manual for SIGSANTE, Cote d'Ivoire's national health information management system built on DHIS2.

2. **Formation en ligne sur SIGSANTE DHIS2** — https://dipe.info/index.php/fr/formation-en-ligne-sur-sigsante-dhis2
   Online training platform for SIGSANTE/DHIS2 in Cote d'Ivoire.

3. **Plan strategique national de lutte contre le cancer 2022-2025** — https://www.iccp-portal.org/sites/default/files/plans/PSNCancer_2022_2025_vf%20PNLCa.pdf
   National strategic plan for cancer control in Cote d'Ivoire 2022-2025.

4. **Cote d'Ivoire: self-testing extends cervical cancer screening services** — https://www.afro.who.int/countries/cote-divoire/news/cote-divoire-self-testing-extends-cervical-cancer-screening-services
   WHO Africa article on how HPV self-testing is extending cervical cancer screening services in Cote d'Ivoire.

5. **Profil epidemiologique du cancer en Cote d'Ivoire** — https://en.pnlca.org/copy-of-cancer-en-cote-d-voire-2
   Epidemiological cancer profile of Cote d'Ivoire from the national cancer control program (PNLCA).

#### English query results
6. **Barriers and facilitators in cervical cancer screening uptake in Abidjan, Cote d'Ivoire in 2018** — https://bmccancer.biomedcentral.com/articles/10.1186/s12885-021-08650-6
   BMC Cancer (2021) study analyzing barriers and facilitators to cervical cancer screening uptake in Abidjan.

7. **Barriers and facilitators in cervical cancer screening uptake in Abidjan (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8390229/
   PMC version of the same cross-sectional study on cervical cancer screening in Abidjan.

8. **Cote d'Ivoire: Raising awareness about cervical cancer** — https://www.uicc.org/news-and-updates/news/cote-divoire-raising-awareness-about-cervical-cancer
   UICC article on cervical cancer awareness efforts in Cote d'Ivoire.

9. **SIGSANTE / SISR de Cote d'Ivoire** — https://sigsante.gouv.ci.siteindices.com/
   Index page for the SIGSANTE platform, Cote d'Ivoire's DHIS2-based health information system.

10. **SIGSANTE official site** — http://sigsante.gouv.ci.usitestat.com/
    Statistics and usage data for the official SIGSANTE (DHIS2) government health information platform.


## Djibouti — Cancer & DHIS2 Profile

### Summary
Djibouti does not have a population-based cancer registry, and cancer control infrastructure is still developing. The country is stepping up plans for its first national cancer centre with IAEA support. UNFPA runs breast and cervical cancer screening programmes. A DHIS2 Data Visualization Academy held in Mali in September 2025 included participants from Djibouti, suggesting engagement with the DHIS2 ecosystem, but no specific evidence was found of DHIS2 being used for cancer data in Djibouti.

DHIS2 USE: LOW
Djibouti has some engagement with the DHIS2 ecosystem (participants attended a DHIS2 academy), but no evidence was found of DHIS2 being specifically used for cancer registration, screening, or surveillance.

### Search Results

#### French query results
1. **Profil pour le cancer du col de l'uterus - Djibouti** — https://cdn.who.int/media/docs/default-source/country-profiles/cervical-cancer/cervical-cancer-dji-2021-country-profile-fr.pdf
   WHO cervical cancer country profile for Djibouti with morbidity and mortality data (2021).

2. **An Overview of Cancer in Djibouti: Current Status, Therapeutic Approaches, and Promising Endeavors** — https://www.mdpi.com/1424-8247/16/11/1617
   MDPI Pharmaceuticals (2023) comprehensive overview of cancer in Djibouti, including current status and therapeutic approaches.

#### English query results
3. **Djibouti Steps Up Plans for its First National Cancer Centre** — https://www.iaea.org/newscenter/news/djibouti-steps-up-plans-for-its-first-national-cancer-centre
   IAEA article on Djibouti's plans to establish its first national cancer centre.

4. **An Overview of Cancer in Djibouti (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10674319/
   PMC version of the comprehensive overview of cancer status in Djibouti.

5. **Cancer Today - Djibouti Fact Sheet** — https://gco.iarc.who.int/media/globocan/factsheets/populations/262-djibouti-fact-sheet.pdf
   IARC Global Cancer Observatory fact sheet for Djibouti with incidence and mortality data.

#### Arabic query results
No relevant results found. The Arabic search returned general cancer information unrelated to Djibouti or health information systems.


## Dominica — Cancer & DHIS2 Profile

### Summary
No specific evidence was found of DHIS2 being used for cancer registration or screening in Dominica. The Caribbean Public Health Agency (CARPHA) is partnering with IARC and other organizations on cancer registry development across the Caribbean region, and Jamaica has piloted DHIS2 for cancer registration data systems, but no Dominica-specific implementation was identified. Dominica participates in CARPHA's regional cancer awareness initiatives.

DHIS2 USE: NO EVIDENCE
No evidence was found of DHIS2 being used in Dominica for cancer registration or screening. Regional Caribbean efforts with DHIS2 cancer registries are centered on Jamaica.

### Search Results

#### English query results
1. **CARPHA - as of 2020 breast cancer is most commonly diagnosed cancer in the world** — https://dominicanewsonline.com/news/homepage/carpha-as-of-2020-breast-cancer-is-most-commonly-diagnosed-cancer-in-the-world/
   Dominica News Online article on CARPHA's cancer awareness initiatives, noting breast cancer as the most commonly diagnosed cancer globally.

2. **Cervical cancer screening data from the case-based national electronic registry in Bangladesh** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11984040/
   PMC article (2024) on case-based electronic cancer registries, relevant as a model for small-island developing states though not specific to Dominica.


## Dominican Republic — Cancer & DHIS2 Profile

### Summary
The Dominican Republic established its first Cancer Registry unit in November 2019, located at the Autonomous University of Santo Domingo (UASD), through an inter-institutional alliance between the Ministry of Public Health and UASD with PAHO/WHO support. A national cancer registry pilot has focused on childhood cancer. However, national cancer statistics remain incomplete, and no evidence was found of DHIS2 being used for cancer registration or screening in the country.

DHIS2 USE: NO EVIDENCE
No evidence was found of DHIS2 being used in the Dominican Republic for cancer registration, screening, or surveillance. The country's cancer registry was developed with PAHO/WHO support using other platforms.

### Search Results

#### Spanish query results
1. **Republica Dominicana busca fortalecer la capacidad de registro de datos sobre cancer** — https://www.paho.org/es/noticias/5-2-2018-republica-dominicana-busca-fortalecer-capacidad-registro-datos-sobre-cancer
   PAHO/WHO article (2018) on the Dominican Republic's efforts to strengthen cancer data registration capacity.

2. **Republica Dominicana cuenta con la primera unidad de Registro de Cancer** — https://www.paho.org/es/noticias/2-12-2019-republica-dominicana-contara-con-primera-unidad-registro-cancer
   PAHO/WHO article (2019) announcing the Dominican Republic's first cancer registry unit.

3. **Indicadores estadisticos y epidemiologicos INCART 2019-2021** — https://www.incart.gob.do/noticias/indicadores-estadisticos-y-epidemiologicos-incart-2019-2021/
   INCART (National Cancer Institute) statistical and epidemiological indicators report for 2019-2021.

#### English query results
4. **Dominican Provider Practices for Cervical Cancer Screening in Santo Domingo and Monte Plata Provinces** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9743805/
   PMC study on cervical cancer screening provider practices in the Dominican Republic.

5. **Addressing childhood cancer: actions taken in the Dominican Republic** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11053371/
   PMC article (2024) reviewing childhood cancer initiatives in the Dominican Republic, including registry development.


## DPR Korea — Cancer & DHIS2 Profile

### Summary
No information was found linking DHIS2 to cancer registration, screening, or health information systems in the Democratic People's Republic of Korea (North Korea). Search results in both Korean and English returned information about South Korea's cancer screening programs instead. North Korea's health information systems remain largely opaque, and publicly available data on cancer surveillance infrastructure is extremely limited.

DHIS2 USE: NO EVIDENCE
No evidence was found of DHIS2 being used in DPR Korea for cancer or any other health purpose. The country's health information systems are not publicly documented in accessible sources.

### Search Results

#### Korean query results
No relevant results found. The Korean-language search returned results exclusively about South Korea's national cancer screening programs and cancer statistics.

#### English query results
No relevant results found. The English-language search also returned results about South Korea's cancer screening and registry systems rather than North Korea. No publicly available information was found about cancer registration or DHIS2 implementation in DPR Korea.


## DRC — Cancer & DHIS2 Profile

### Summary
The DRC uses DHIS2 as its national health information system (known locally as SNIS), with nationwide rollout completed by 2017. The 2015 national cancer strategy explicitly called for integrating routine cancer data collection into the SNIS, and the 2022-2025 national cancer strategic plan continues this direction. However, a population-based cancer registry does not yet exist, and there is no evidence of a dedicated DHIS2 cancer tracker or module in operation — cancer data integration into SNIS appears to remain at the level of routine aggregate reporting rather than specialized registry or surveillance functionality.

DHIS2 USE: LOW
The SNIS (DHIS2) is the backbone of the DRC's health information system and the national cancer strategy documents reference integrating cancer indicators into it. However, no evidence was found of an operational cancer-specific DHIS2 module, cancer registry within DHIS2, or published cancer surveillance data drawn from DHIS2. The connection between cancer programs and DHIS2 remains aspirational or limited to basic aggregate reporting.

### Search Results

#### French query results
1. **Plan Strategique National de Lutte contre le Cancer 2022-2025** — https://www.iccp-portal.org/sites/default/files/plans/PSNCancer_2022_2025_vf%20PNLCa.pdf
   The DRC's national cancer strategic plan for 2022-2025, produced by the Programme National de Lutte contre le Cancer (PNLCa), which outlines cancer control objectives including data collection and surveillance.

2. **Strategie Nationale de Lutte contre le Cancer (2015)** — https://www.iccp-portal.org/sites/default/files/plans/COD_B5_Strat%C3%A9gie%20version%20finale%20du%2024042015.pdf
   The original 2015 national cancer strategy, which called for integrating cancer data collection into the SNIS and establishing cancer registries with indicators for incidence and prevalence by cancer type and province.

3. **Evolution du systeme national d'information sanitaire de la RDC entre 2009 et 2015 (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC5881558/
   Peer-reviewed article documenting the evolution of the DRC's SNIS from paper-based reporting to DHIS2, covering 2009-2015, though it does not specifically address cancer data within the system.

4. **RDC: de nouveaux centres scientifiques pour renforcer la lutte contre le cancer** — https://environews-rdc.net/2025/10/06/sante-la-rdc-cree-des-centres-doncologie-et-de-radio-pharmacie-pour-renforcer-la-lutte-contre-le-cancer/
   Reports on the DRC government's creation of a National Oncology Center, National Radio-pharmacy Center, and National School of Nuclear Science and Techniques to strengthen diagnostic and screening capacities.

#### English query results
1. **Strengthening Cancer Care in DRC: Experts Recommend Decentralizing Diagnosis and Treatment (IAEA)** — https://www.iaea.org/newscenter/news/strengthening-cancer-care-in-the-democratic-republic-of-the-congo-experts-recommend-decentralizing-services
   IAEA imPACT Review recommending the development of a population-based cancer registry, a National Cancer Control Plan, and decentralized diagnostic services for a country with only one radiotherapy unit serving approximately 90 million people.

2. **Protecting Women and Girls in DRC Against Cervical Cancer (World Bank, 2025)** — https://www.worldbank.org/en/news/feature/2025/08/19/protecting-women-and-girls-in-the-democratic-republic-of-congo-drc-against-cervical-cancer
   World Bank article on HPV vaccine rollout plans (Gardasil for girls aged 9-14 starting 2026) and the 2024 National Forum for the Elimination of Cervical Cancer, including a revised national strategy for 2024-2029.

3. **Massive single visit cervical pre-cancer and cancer screening in eastern DRC (BMC Women's Health, 2019)** — https://bmcwomenshealth.biomedcentral.com/articles/10.1186/s12905-019-0737-y
   Peer-reviewed study on a large-scale cervical cancer screening campaign in eastern DRC using visual inspection with acetic acid and Lugol iodine, without mention of DHIS2 for data capture.

4. **Cancer in the Democratic Republic of Congo (OMICS International)** — https://www.omicsonline.org/proceedings/cancer-in-the-democratic-republic-of-congo-drc-59524.html
   Conference proceedings describing the cancer burden in the DRC and the absence of a national cancer registry or formal cancer control program.

5. **Data strengthens health systems: The power of DHIS2 (IMA World Health, 2022)** — https://imaworldhealth.org/blog/2022/data-strengthens-health-systems-power-dhis2
   Documents IMA World Health's role in rolling out DHIS2 across DRC health zones, achieving over 90% facility reporting rates, though coverage focuses on general health data rather than cancer-specific programs.


## Ecuador — Cancer & DHIS2 Profile

### Summary
Ecuador has a well-established cancer registry system through SOLCA's National Tumor Registry (est. 1984 in Quito) and is strengthening its national cancer framework through recent legislation (Ley Orgánica para la Atención Integral del Cáncer). Ecuador is one of 12 participating countries in the WHO Global Platform for Access to Childhood Cancer Medicines and began reporting childhood cancer indicators through DHIS2 in 2025, but there is no evidence of DHIS2 being used for population-based cancer registries, cervical cancer screening, or broader cancer surveillance in the country.

DHIS2 USE: LOW
Ecuador has started using DHIS2 for childhood cancer medicine indicators as part of a WHO global platform (2025), and has prior DHIS2 experience for vaccine information systems. However, the country's established cancer registry infrastructure (SOLCA/National Tumor Registry) operates independently of DHIS2, and there is no evidence of DHIS2 adoption for cancer registries, cervical cancer screening, or NCD cancer surveillance.

### Search Results

#### Spanish query results
1. **Diagnóstico y detección oportunas son claves para mejorar calidad de vida de pacientes con cáncer** — https://www.salud.gob.ec/diagnostico-y-deteccion-oportunas-son-claves-para-detectar-el-cancer/
   Ecuador Ministry of Public Health page on the importance of early cancer diagnosis and detection, noting 29,273 new cases registered in 2020.

2. **Cáncer en Ecuador (portal nacional)** — https://cancerecuador.com/
   National cancer information portal presenting epidemiological data from the Ministry of Public Health, GLOBOCAN, and INEC; DHIS2 is not mentioned.

3. **Ley para atención del cáncer — Primicias** — https://www.primicias.ec/sociedad/nueva-ley-cancer-claves-derecho-olvido-estabilidad-financiera-presupuesto-115698/
   Overview of Ecuador's new Organic Law for Comprehensive Cancer Care, which establishes a national cancer registry requiring mandatory reporting from all public and private health centers.

#### English query results
1. **Trends in cancer incidence and mortality over three decades in Quito — Ecuador (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC6018821/
   Peer-reviewed analysis of SOLCA's National Tumor Registry data (1985-2013) documenting cancer trends in Quito, including rising breast and thyroid cancer and declining cervical and stomach cancer.

2. **Making human papillomavirus testing a public health priority in Ecuador (Frontiers in Public Health, 2025)** — https://www.frontiersin.org/journals/public-health/articles/10.3389/fpubh.2025.1535580/full
   Recent article on Ecuador's 2023 launch of HPV molecular testing at 36 screening centers; recommends digital tools for tracking screening rates but does not mention DHIS2.


## Egypt — Cancer & DHIS2 Profile

### Summary
Egypt has a well-established National Population-Based Cancer Registry Program (NCRP) operating since 2008, and the country bears one of the highest cancer incidence rates in the Eastern Mediterranean Region (over 160 per 100,000). Egypt has significant cancer screening initiatives, including the "100 Million Healthy Lives" breast cancer screening programme launched in 2019, and IARC/WHO have provided 10 years of technical support for cancer surveillance in the region. Egypt began adopting DHIS2 in 2023, but solely for emergency and humanitarian response (malnutrition screening among displaced Sudanese, and hospital registries for individuals from Gaza), with no evidence of DHIS2 being used for cancer registration, screening, or NCD surveillance.

DHIS2 USE: NO EVIDENCE
Egypt's DHIS2 deployment (initiated in mid-2023) has been limited to emergency and humanitarian use cases. Cancer registries operate through CanReg and other standalone systems under the NCRP. The IARC E-NNOVATE project developing a DHIS2-CanReg integration does not currently include Egypt among its pilot countries. No search results in English or Arabic linked DHIS2 to cancer data collection or management in Egypt.

### Search Results

#### Arabic query results
1. **100 Million Healthy Lives — Early Detection and Treatment of Cancer** — https://www.100millionseha.eg/tumors
   Official Egyptian government portal for the presidential initiative on early cancer detection and treatment, part of the broader "100 Million Healthy Lives" programme.

2. **WHO EMRO — Cancer Registration in the Region** — https://www.emro.who.int/ar/noncommunicable-diseases/information-resources/cancer-registry.html
   WHO Eastern Mediterranean Regional Office resource on population-based cancer registries in the region, noting use of CanReg4 software for cancer registration across EMR countries including Egypt.

3. **International Praise for Presidential Initiative on Breast Cancer Treatment (Youm7, 2024)** — https://www.youm7.com/story/2024/11/15/...
   Arabic news report on WHO and UN praise for Egypt's presidential breast cancer screening initiative, noting it reduced late-stage detection to 30%.

#### English query results
1. **Cancer Incidence in Egypt: Results of the National Population-Based Cancer Registry Program (PMC, 2014)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC4189936/
   Foundational study describing the NCRP established in 2008, stratifying Egypt into three geographical strata for cancer incidence data collection.

2. **Cancer Control in Egypt: Investing in Health (ASCO Post, 2021)** — https://ascopost.com/issues/march-25-2021/cancer-control-in-egypt/
   Overview of Egypt's cancer burden (134,632 new cases in 2018) and cancer control efforts including the "100 Million Healthy Lives" breast cancer screening programme and the Breast Cancer Centers of Excellence.

3. **Cancer Surveillance in the Eastern Mediterranean Region: A 10-Year IARC-WHO Collaboration (WHO EMRO, 2024)** — https://www.emro.who.int/media/news/cancer-surveillance-in-the-eastern-mediterranean-region-a-10-year-iarc-who-collaboration.html
   Documents a decade of IARC-WHO collaboration on cancer surveillance across 20 Eastern Mediterranean countries including Egypt, noting Egypt has one of the highest cancer incidence rates in the region.

4. **Presentation and Management of Female Breast Cancer in Egypt (WHO EMHJ, 2022)** — https://www.emro.who.int/emhj-volume-28-2022/volume-28-issue-10/presentation-and-management-of-female-breast-cancer-in-egypt.html
   WHO Eastern Mediterranean Health Journal article examining breast cancer presentation patterns and management practices in Egypt.

5. **Cancer Screening: The Collateral Damage of the Pandemic in Egypt (JEPHA, 2021)** — https://jepha.springeropen.com/articles/10.1186/s42506-021-00073-2
   Study assessing the impact of COVID-19 on cancer screening services in Egypt, indicating disruption to the national screening programmes.


## El Salvador — Cancer & DHIS2 Profile

### Summary
El Salvador has an emerging cancer surveillance infrastructure, with a National Cancer Registry mandated by the 2022 Cancer Prevention, Care and Control Law, though the registry is still under development. The country has active cervical cancer screening programs (CAPE project, World Bank NCD project) and IAEA-supported cancer control planning. DHIS2 is deployed in El Salvador for the HEARTS cardiovascular initiative (759 health centers reporting) and PSI reproductive health programs, but there is no direct evidence of DHIS2 being used specifically for cancer registration or cancer screening data management.

DHIS2 USE: LOW
El Salvador actively uses DHIS2 for NCD-related programs (HEARTS hypertension management across 759+ health centers) and PSI reproductive health, but cancer data collection relies on separate institutional databases and a nascent national cancer registry that does not yet appear to use DHIS2. The IARC-HISP Centre global initiative to develop DHIS2-based cancer registry toolkits could change this in the future.

### Search Results

#### Spanish query results
1. **Lineamientos tecnicos para el funcionamiento del Registro Nacional de Cancer** — https://asp.salud.gob.sv/regulacion/pdf/lineamientos/lineamientos_tecnicos_para_el_funcionamiento_del_registro_nacional_de_cancer.pdf
   Official Ministry of Health technical guidelines for establishing and operating the National Cancer Registry in El Salvador.

2. **Diagnostico situacional del cancer en El Salvador (MINSAL)** — https://asp.salud.gob.sv/regulacion/pdf/otrosdoc/Diagnostico_situacional_del_cancer_en_el_salvador.pdf
   Ministry of Health situational analysis of cancer in El Salvador, providing baseline epidemiological data and health system capacity assessment.

3. **Lineamientos tecnicos para implementacion del Registro de Cancer del Departamento de San Salvador** — https://asp.salud.gob.sv/regulacion/pdf/lineamientos/lineamientos_tecnicos_implementacion_registro_de_cancer_departamento_de_san_salvador_v1.pdf
   Technical guidelines for implementing a population-based cancer registry in the Department of San Salvador as a pilot.

4. **Avances en el Sistema de Monitoreo y Evaluacion HEARTS (PAHO)** — https://www.paho.org/es/noticias/17-2-2023-avances-sistema-monitoreo-evaluacion-hearts
   PAHO report on a February 2023 technical meeting in El Salvador advancing the HEARTS monitoring and evaluation system using DHIS2, with plans for national DHIS2 coverage across 512 additional health centers.

5. **Estrategias efectivas para la deteccion del cancer de cuello uterino y ENT en El Salvador (World Bank)** — https://www.bancomundial.org/es/news/video/2024/07/04/estrategias-efectivas-deteccion-cancer-cuello-uterino-enfermedades-no-transmisibles-El-Salvador
   World Bank project reporting over 80,000 women screened for HPV and approximately 743,000 people benefiting from NCD early detection interventions, though no DHIS2 integration is mentioned.

#### English query results
1. **El Salvador Continues to Improve Cancer Control Planning, Resources and Access (IAEA)** — https://www.iaea.org/newscenter/news/el-salvador-continues-to-improve-cancer-control-planning-resources-and-access
   IAEA review noting over 9,600 new cancer cases annually in El Salvador and recommending standardization of cancer-specific monitoring and evaluation capacities to support a national health information system.

2. **High Incidence of Gastric Cancer in El Salvador: A National Multisectorial Study 2000-2014 (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC12103254/
   Multisectorial study documenting 10,039 gastric cancer cases using pathology and endoscopy databases from institutions covering over 95% of national endoscopy capacity, without use of a centralized registry or DHIS2.

3. **Cervical Cancer Prevention in El Salvador (CAPE): An HPV Testing-Based Demonstration Project (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC5352717/
   Description of the CAPE demonstration project introducing lower-cost HPV testing into the public sector in El Salvador's Paracentral region, screening over 28,000 women.

4. **Scale-Up of an HPV Testing Implementation Program in El Salvador (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/27922905/
   Study on scaling up HPV testing-based cervical cancer screening in El Salvador's public health system.

5. **Cancer El Salvador 2020 Country Profile (WHO)** — https://www.who.int/publications/m/item/cancer-slv-2020
   WHO country profile summarizing cancer incidence, mortality, and health system capacity for cancer control in El Salvador as of 2020.


## Equatorial Guinea — Cancer & DHIS2 Profile

### Summary
Equatorial Guinea has DHIS2 deployed at national level — piloted in Añisok and Baney health districts with support from EHAS, Fundacion FRS, and AECID — but the system currently focuses on aggregate health data, routine immunization, HIV/AIDS patient tracking, and malaria (through the BIMEP project led by MCDI). A Cervical Cancer Screening and Treatment (CCST) Project exists (implemented by MCDI on behalf of Noble Energy), and the IAEA has conducted an imPACT review, but no evidence was found that cancer data is collected or managed through DHIS2. The country does not appear on the IARC/WHO-HISP DHIS2 cancer registry collaboration list.

DHIS2 USE: NO EVIDENCE
DHIS2 is operational in Equatorial Guinea for malaria and general health data, but no evidence was found of DHIS2 being used specifically for cancer registration, screening data, or cancer surveillance. The cervical cancer screening project appears to operate outside the DHIS2 platform.

### Search Results

#### Spanish query results
1. **Lanzamiento de la herramienta digital en el sistema sanitario de Guinea Ecuatorial (UNDP)** — https://www.undp.org/es/equatorial-guinea/noticias/lanzamiento-de-la-herramienta-digital-en-el-sistema-sanitario-de-guinea-ecuatorial
   UNDP report on the official launch of DHIS2 in Equatorial Guinea's health system, covering aggregate data, routine immunization, HIV tracking, and medicine inventory — no mention of cancer modules.

2. **Continuamos junto a EHAS colaborando en el desarrollo del Sistema de Informacion Sanitario de Guinea Ecuatorial (Fundacion FRS)** — https://www.fundacionfrs.es/continuanos-junto-a-ehas-colaborando-en-el-desarrollo-del-sistema-de-informacion-sanitario-de-guinea-ecuatorial/
   Describes EHAS and Fundacion FRS collaboration with the Ministry of Health on DHIS2 pilot implementation in Anisok and Baney districts, with training for central-level technicians (January 2025).

3. **Perfil de cancer cervicouterino — Guinea Ecuatorial (WHO)** — https://cdn.who.int/media/docs/default-source/country-profiles/cervical-cancer/cervical-cancer-gnq-2021-country-profile-es.pdf
   WHO 2021 cervical cancer country profile for Equatorial Guinea with screening and mortality data.

#### English query results
4. **DHIS2 Health Information System Analyst, Equatorial Guinea (Devex/MCDI)** — https://www.devex.com/jobs/dhis2-health-information-system-analyst-equatorial-guinea-816325
   Job posting for a DHIS2 analyst role under the BIMEP malaria project; notes MCDI also implements a Cervical Cancer Screening and Treatment Project on behalf of Noble Energy, but the DHIS2 role is malaria-focused.

5. **imPACT Review Report — Equatorial Guinea (IAEA)** — https://www.iaea.org/sites/default/files/2025-03/impact-review-report-guinea-1223.pdf
   IAEA Programme of Action for Cancer Therapy review of Equatorial Guinea's cancer control capacity, including recommendations for cancer services development.


## Eritrea — Cancer & DHIS2 Profile

### Summary
Eritrea has limited health infrastructure and constrained digital health capacity. DHIS2 was introduced for health data management starting around 2018, but implementation remains nascent. There is no population-based cancer registry, no documented cancer-specific digital tools, and no evidence of DHIS2 being used for cancer data collection or management.

DHIS2 USE: NO EVIDENCE
No documentation was found indicating DHIS2 is used for cancer registration, screening, or surveillance in Eritrea. The country lacks a formal cancer registry, and DHIS2 implementation for general health information remains at an early stage.

### Search Results

#### English query results
1. **DHIS2: Improving Data Management and Health in Eritrea (Eritrea Ministry of Information)** — https://shabait.com/2018/11/07/dhis2-improving-data-management-and-health-in-eritrea/
   Article on Eritrea's adoption of DHIS2 for health data management, noting the Director of National Health Information System's support for data-driven development.

2. **Incidence of Cervical, Ovarian and Uterine Cancer in Eritrea: Data from the National Health Laboratory, 2011-2017 (Scientific Reports)** — https://www.nature.com/articles/s41598-020-66096-5
   Research using National Health Laboratory data to estimate cancer incidence, highlighting the absence of a population-based cancer registry.

3. **Unveiling the Burden of Leukemia in Eritrea (2010-2020) (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC12170230/
   Study on leukemia incidence in a low-resource setting, emphasizing the need for a national cancer registry and improved research platforms.

4. **About the GICR — Sub-Saharan Africa Hub (IARC)** — https://gicr.iarc.fr/hub/sub-saharan-africa/
   IARC Global Initiative for Cancer Registry Development page noting that population-based cancer registry data are not available for Eritrea.

5. **WHO Results Report 2024-2025 — Eritrea Country Profile** — https://www.who.int/about/accountability/results/who-results-report-2024-2025/country-profile/2024/eritrea
   WHO country profile for Eritrea covering health system performance and priorities.


## Eswatini — Cancer & DHIS2 Profile

### Summary
Eswatini (formerly Swaziland) uses DHIS2 as its national health management information system. The country has one of Africa's highest cervical cancer rates, compounded by high HIV prevalence. A national cancer registry was re-established in 2015 using CanReg5 software. While a Cervical Cancer Elimination Acceleration Plan (2024-2028) is underway with plans to integrate cervical cancer data into DHIS2, no cancer-specific DHIS2 modules are currently documented as operational.

DHIS2 USE: NO EVIDENCE
Despite DHIS2 being the national HMIS and active cervical cancer elimination efforts, no evidence was found of operational DHIS2-based cancer data collection. Plans to integrate cervical cancer LEEP procedure data into DHIS2 were noted for 2025-2026, but the cancer registry currently uses CanReg5 separately.

### Search Results

#### English query results
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


## Ethiopia — Cancer & DHIS2 Profile

### Summary
Ethiopia has DHIS2 deployed nationwide as its Health Management Information System (HMIS) since 2019, with the system demonstrating high maturity and positive impacts on maternal and child health service performance. Ethiopia has one population-based cancer registry (Addis Ababa City Cancer Registry, established 2011) covering 3 million people, and recently completed a landmark cervical cancer screening campaign (275,607 women screened, 103% of target). The WHO-IARC-HISP collaboration is actively supporting cancer data integration into DHIS2, with Ethiopia identified as part of the global initiative to enable routine facility reporting of cancer indicators through DHIS2 for cancer prevention, diagnosis, treatment, and care monitoring.

DHIS2 USE: MODERATE
Ethiopia has a mature national DHIS2 implementation and is actively participating in the global IARC-HISP initiative to integrate cancer indicators into routine DHIS2 reporting. While the existing Addis Ababa cancer registry predates this integration, Ethiopia is onboarding cancer as a new domain for routine facility reporting in DHIS2, enabling tracking of medicine sources, treatment reach, and program outcomes. Enhanced registers and digital reporting platforms from the recent cervical cancer campaign will be integrated into routine health information systems.

### Search Results

#### English query results
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


## Gabon — Cancer & DHIS2 Profile

### Summary
Gabon officially launched DHIS2 as its national health information system on December 6, 2024, with support from WHO and the Global Fund, marking a strategic turning point for the country's health data management. Gabon has an established population-based cancer registry covering the Grand Libreville agglomeration (established in 1977 as a pathology-based registry, with population-based registration beginning for 2013-2017), supported by the Sylvia Bongo Ondimba Foundation. However, there is no evidence that cancer data is currently integrated into the newly launched DHIS2 system, and Gabon does not appear among the early adopter countries in the IARC-HISP global cancer registry collaboration.

DHIS2 USE: NO EVIDENCE
Gabon's DHIS2 deployment was officially launched in December 2024, making it one of the newest implementations in Africa. While the system is expected to improve disease surveillance and data-driven decision-making, there is no current evidence of cancer-specific data collection or management through DHIS2. The established cancer registry in Libreville operates through traditional registry systems and is part of the African Cancer Registry Network (AFCRN).

### Search Results

#### French query results
1. **Santé : Le Gabon adopte le système DHIS2 pour révolutionner ses données sanitaires (Gabon Review)** — https://www.gabonreview.com/sante-le-gabon-adopte-le-systeme-dhis2-pour-revolutionner-ses-donnees-sanitaires/
   Reports on Prime Minister Raymond Ndong Sima's official launch of DHIS2 on December 6, 2024, with WHO and Global Fund support.

2. **Lancement officiel de la cérémonie du District Health Information System 2 (DHIS2) (Primature Gabon)** — https://www.primature.gouv.ga/2408-lancement-officiel-de-la-ceremonie-du-district-health-information-system-2-dhis2-/
   Official government announcement of the DHIS2 launch ceremony.

3. **Traitement des cancers - Fondation Sylvia Bongo Ondimba** — https://www.sylviabongoondimba.org/nos-initiatives/initiatives-pour-les-femmes/agir-contre-le-cancer/traitement-du-cancer
   Describes the Sylvia Bongo Ondimba Foundation's participation in establishing a cancer registry covering Libreville and Owendo agglomerations.

#### English query results
4. **Cancer in Gabon, 1984-1993: a pathology registry based relative frequency study (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/8952642/
   Early pathology-based cancer registry study from Gabon covering 1984-1993.

5. **Cancer in the Grand Libreville, Gabon (2013-2017) (ScienceDirect)** — https://www.sciencedirect.com/science/article/abs/pii/S1877782124001747
   Recent population-based cancer registry data from Grand Libreville covering the first 5-year registration period (2013-2017).


## Gambia — Cancer & DHIS2 Profile

### Summary
The Gambia has complete national DHIS2 implementation for its health information system. The country hosts the landmark IARC Gambia Hepatitis Intervention Study (GHIS), launched in 1986, which led to the establishment of the National Cancer Registry of The Gambia — one of Africa's oldest population-based cancer registries. Cancer registration predates DHIS2 adoption and operates using CanReg5 software, likely independently from the DHIS2 platform.

DHIS2 USE: LOW
The Gambia uses DHIS2 nationally for health information management, but the established cancer registry predates DHIS2 and operates on CanReg5 software. No specific evidence was found of cancer data collection or registry management through DHIS2.

### Search Results

#### English query results
1. **20-Years of Population-Based Cancer Registration in Hepatitis B and Liver Cancer Prevention in The Gambia, West Africa (PLOS ONE)** — https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0075775
   Comprehensive overview of 20 years of population-based cancer registration linked to the GHIS hepatitis B vaccination program.

2. **20 Years into the Gambia Hepatitis Intervention Study: Assessment of Initial Hypotheses (Cancer Epidemiology, Biomarkers and Prevention)** — https://aacrjournals.org/cebp/article/17/11/3216/67070/20-Years-into-the-Gambia-Hepatitis-Intervention
   Assessment of the GHIS study evaluating hepatitis B vaccination efficacy for liver cancer prevention.

3. **The Gambia Hepatitis Intervention Study (IARC Publication)** — https://publications.iarc.fr/_publications/media/download/4351/351e766e0eaf2278372cd0f8506a5982ac28fc62.pdf
   IARC publication documenting the GHIS study design and cancer registry establishment in 1986.

4. **The first 2 years of the Gambian National Cancer Registry (British Journal of Cancer)** — https://www.nature.com/articles/bjc1990347
   Early report on the establishment and initial findings of The Gambia's National Cancer Registry.

5. **Cancer survival in the Gambia, 1993-1997 (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/21675410/
   Cancer survival analysis using data from The Gambia's population-based cancer registry.


## Ghana — Cancer & DHIS2 Profile

### Summary
Ghana has DHIS2 nationally deployed as its health management information system. Ghana is one of 12 countries enrolled in the WHO Global Platform for Access to Childhood Cancer Medicines, which uses DHIS2 for reporting. The country has regional cancer registries at Korle-Bu Teaching Hospital and Komfo Anokye Teaching Hospital. An NCI-supported pilot project is developing population-based cancer registration in Accra. While DHIS2 is the national HMIS and cancer data integration is expanding, dedicated cancer registry modules in DHIS2 are not yet comprehensively documented.

DHIS2 USE: LOW
Ghana uses DHIS2 nationally and participates in the WHO childhood cancer medicines platform that uses DHIS2 for reporting. Cancer data integration into DHIS2 is noted as expanding through the WHO-HISP collaboration, but hospital-based cancer registries operate with their own systems. Evidence of direct cancer registry management in DHIS2 remains limited.

### Search Results

#### English query results
1. **WHO coordinates Ghana's enrolment on the Global Platform for Access to Childhood Cancer Medicines (WHO AFRO)** — https://www.afro.who.int/countries/ghana/news/who-coordinates-ghanas-enrolment-global-platform-access-childhood-cancer-medicines
   Details Ghana's enrolment as one of 12 countries in the WHO Global Platform for childhood cancer medicines access and reporting.

2. **Cancer ecosystem assessment in West Africa: Ghana, Nigeria and Senegal (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7320762/
   Health systems gap analysis for cancer prevention and control across three West African countries including Ghana.

3. **Ghana Cancer Registry — from Hospital to Population (NCI/NIH)** — https://maps.cancer.gov/overview/DCCPSGrants/abstract.jsp?applId=9126451&term=CA181843
   NCI-supported project partnering with the University of Ghana Medical School and African Cancer Registry Network for population-based cancer registration in Accra.

4. **Achieving universal coverage of childhood cancers in Ghana via the National Health Insurance Scheme (PLOS Global Public Health)** — https://journals.plos.org/globalpublichealth/article?id=10.1371/journal.pgph.0004871
   Stakeholder analysis on achieving universal coverage for childhood cancer treatment through Ghana's insurance scheme.

5. **Our 2025 Impact in Ghana — Hope in Every Childhood Cancer Story (World Child Cancer)** — https://worldchildcancer.org/news_and_blog/ghana-2025-childhood-cancer-impact-review/?geo=us
   Report on childhood cancer program impact in Ghana during 2025.

6. **Evaluating essential medicines for treating childhood cancers: availability, price and affordability study in Ghana (BMC Cancer)** — https://bmccancer.biomedcentral.com/articles/10.1186/s12885-021-08435-x
   Study evaluating availability and affordability of essential childhood cancer medicines in Ghana.

7. **Global Platform for Access to Childhood Cancer Medicines (WHO)** — https://www.who.int/teams/noncommunicable-diseases/ncds-management/cancer-programme/global-platform-for-access-to-childhood-cancer-medicines
   WHO page on the Global Platform listing Ghana among 12 participating countries.

8. **Webinar: Closing the gaps — Global Platform for Access to Childhood Cancer Medicines (WHO)** — https://www.who.int/news-room/events/detail/2025/05/29/default-calendar/webinar-closing-the-gaps--raising-awareness-of-the-global-platform-for-access-to-childhood-cancer-medicines
   WHO webinar raising awareness of the Global Platform for childhood cancer medicines, with Ghana as a participating country.

9. **Evaluating essential medicines for treating childhood cancers in Ghana (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/34112117/
    PubMed listing of the availability and affordability study for childhood cancer medicines in Ghana.


## Grenada — Cancer & DHIS2 Profile

### Summary
Grenada is a small Caribbean island nation with limited health infrastructure documentation. DHIS2 has been adopted for health surveillance and is being used as part of CARPHA's regional integrated early warning surveillance system. Grenada is one of six countries in the Caribbean Cancer Portal (CCP), launched in 2024. There is no evidence of DHIS2 being used specifically for cancer data collection or management.

DHIS2 USE: NO EVIDENCE
Grenada uses DHIS2 for general health surveillance through CARPHA's regional system and launched the HEARTS Initiative in 2025 using DHIS2 for data-driven policy making. However, no evidence was found of cancer-specific DHIS2 modules or cancer registration through DHIS2. Cancer data efforts are addressed through the Caribbean Cancer Portal rather than DHIS2.

### Search Results

#### English query results
1. **The Caribbean cancer portal: lessons for sustainability, accessibility, and impact (Frontiers)** — https://www.frontiersin.org/journals/cancer-control-and-society/articles/10.3389/fcacs.2025.1577014/full
   Overview of the Caribbean Cancer Portal engaging six countries including Grenada, launched in February 2024.

2. **Regional stakeholders convene to strengthen cancer policy response (NOW Grenada)** — https://nowgrenada.com/2025/10/regional-stakeholders-convene-to-strengthen-cancer-policy-response/
   Report on regional stakeholder meeting in Grenada to strengthen cancer policy responses across the Caribbean.

3. **Grenada launches HEARTS Initiative (PAHO/WHO)** — https://www.paho.org/en/news/18-2-2025-grenada-launches-hearts-initiative
   Grenada's February 2025 launch of the HEARTS Initiative using DHIS2 for NCD data management.

4. **Noncommunicable Diseases Care in the Eastern Caribbean (World Bank)** — https://www.worldbank.org/en/region/lac/publication/noncommunicable-diseases-care-in-the-eastern-caribbean
   World Bank publication on NCD care in the Eastern Caribbean including Grenada.

5. **Reducing the Public Health Impact of Pandemics through Strengthened Integrated Early Warning Surveillance (The Pandemic Fund)** — https://www.thepandemicfund.org/projects/CARIBBEAN-reducing-public-health-impact-pandemics-through-strengthened-integrated-early-warning
   Regional project including Grenada implementing DHIS2-based integrated surveillance through CARPHA.


## Guatemala — Cancer & DHIS2 Profile

### Summary
Guatemala has developed foundational cancer control infrastructure including the Comprehensive Cancer Care Law (2024) and plans for a specialized cancer hospital. DHIS2 was implemented in Guatemala's health information system (SIGSA) primarily for general health data management and HIV testing, but specific evidence of DHIS2 use for cancer surveillance or registry management remains limited.

DHIS2 USE: LOW
DHIS2 was deployed in Guatemala's health information system during NFM2 funding, primarily for general health information management, but search results do not demonstrate direct DHIS2 integration with cancer program data or cancer registry systems.

### Search Results

#### Spanish query results
1. **Development of a Cervical Cancer Screening Program in Rural Guatemala** — https://www.ghspjournal.org/content/13/1/e2400282.short?rss=1
   Documents cervical cancer screening program development in rural Guatemala, addressing barriers and clinical implementation.

2. **Guatemala Prioritizes Capacity Building, Palliative Care and Strengthening Cancer Registry Following Cancer Control Review** — https://www.iaea.org/newscenter/news/guatemala-prioritizes-capacity-control-review
   IAEA report on Guatemala's imPACT Review mission (June 2024) identifying priorities including human resource capacity building and cancer registry strengthening.

3. **Infographic - Childhood Cancer Country Profile: Guatemala** — https://www.paho.org/en/documents/infographic-childhood-cancer-country-profile-guatemala
   PAHO/WHO official country profile on pediatric cancer in Guatemala, supporting the WHO Global Initiative for Childhood Cancer.

#### English query results
4. **Prospective Country Evaluation Guatemala 2020-2021 Annual Country Report** — https://www.healthdata.org/sites/default/files/files/Projects/Global_Fund_PCE/Guatemala_2020-21_Annual_Country_Report_final_31_March_2021.pdf
   Global Fund monitoring report documenting DHIS2 implementation and rollout in Guatemala's health information system, with the reporting tool operational by 2020.

5. **Years of Potential Life Lost Because of Breast and Cervical Cancers in Guatemala** — https://ascopubs.org/doi/full/10.1200/JGO.19.00398
   JCO Global Oncology study quantifying cancer burden and mortality impact, establishing epidemiological evidence for Guatemala's cancer control priorities.


## Guinea-Bissau — Cancer & DHIS2 Profile

### Summary
Guinea-Bissau faces significant cancer control challenges with limited screening capacity, no oncology services, and no national cancer registry. DHIS2 has been the primary health information management system since 2014, covering 145+ health facilities with strong use for malaria, HIV, TB, and immunization, but cancer-specific surveillance and reporting within DHIS2 remains underdeveloped.

DHIS2 USE: MODERATE
DHIS2 has been operational nationwide since 2014 for health data management across 145+ facilities, and Guinea-Bissau was an early adopter of DHIS2 COVID-19 surveillance packages, but cancer-specific data collection within DHIS2 is not yet documented.

### Search Results

#### Portuguese query results
1. **Fight against cancer in Portuguese-speaking African countries: echoes from the last cancer meetings** — https://infectagentscancer.biomedcentral.com/articles/10.1186/s13027-019-0222-0
   Overview of cancer control efforts across PALOP countries including Guinea-Bissau, covering key action points for improving cancer care access.

2. **Cervical cancer screening opportunities for Guinea-Bissau** — https://www.sciencedirect.com/science/article/pii/S244486641730034X
   ScienceDirect study identifying feasible screening strategies including rapid HPV testing and digital cervicography for Guinea-Bissau.

3. **ICO/IARC Information Centre on HPV and Cancer: Guinea-Bissau** — https://hpvcentre.net/statistics/reports/GNB_FS.pdf
   IARC country fact sheet with HPV prevalence, cervical cancer incidence, and screening coverage statistics for Guinea-Bissau.

4. **Politica Nacional de Saude - Guinea-Bissau** — https://extranet.who.int/countryplanningcycles/sites/default/files/public_file_rep/GNB_Guinea-Bissau_Pol%C3%ADtica%20Nacional%20de%20Sa%C3%BAde.pdf
   Guinea-Bissau's national health policy document from WHO country planning resources, establishing the health system framework.

#### English query results
5. **How Guinea-Bissau is using data and digital for health inclusion in challenging environments** — https://www.sparkblue.org/content/how-guinea-bissau-using-data-and-digital-health-inclusion-challenging-environments
   Sparkblue case study on Guinea-Bissau's DHIS2 implementation covering 145+ facilities and 300+ tablets for digital data collection.

6. **Capacitating Digital Public Goods in Guinea-Bissau** — https://www.undp.org/guinea-bissau/news/capacitating-digital-public-goods-guinea-bissau
   UNDP report on strengthening DHIS2 and digital health infrastructure in Guinea-Bissau, including the smart facilities initiative.

7. **UNDP supports the strengthening of the digital data package COVID-19 surveillance in Guinea-Bissau** — https://www.undp.org/guinea-bissau/news/undp-supports-strengthening-digital-data-package-covid-19-surveillance-guinea-bissau
   Documents Guinea-Bissau's early adoption of DHIS2 COVID-19 surveillance packages and integration with existing health systems.

8. **Pediatric cancer care in Africa: SIOP Global Mapping Program** — https://onlinelibrary.wiley.com/doi/full/10.1002/pbc.29345
   SIOP mapping study identifying Guinea-Bissau among African countries reporting no pediatric oncology services.

9. **Short-Term Surgical Mission in Sub-Saharan Country, Experience of a Breast-Dedicated Team** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10166422/
   Documents international surgical missions providing breast cancer diagnosis and treatment at Hospital Nacional Simao Mendes in Guinea-Bissau.

10. **Guinea-Bissau Steps Up Push for Health System Digitization** — https://www.ecofinagency.com/news-digital/2805-47045-guinea-bissau-steps-up-push-for-health-system-digitization
    Ecofin Agency report on Guinea-Bissau's health system digitization efforts including DHIS2 expansion and the first smart facility completed in May 2024.


## Guinea — Cancer & DHIS2 Profile

### Summary
Guinea has a population-based cancer registry (Registre de Cancer de Guinee) at the National Hospital of Donka in Conakry, covering approximately 15% of the national population. The Ministry of Health implemented DHIS2 as the national health information system in 2015-2020, expanding nationally by 2018 following the Ebola outbreak, but cancer surveillance and registry management remain outside the DHIS2 platform.

DHIS2 USE: LOW
DHIS2 is used as the national HMIS for aggregate disease surveillance, but there is no documented evidence of DHIS2-based cancer tracking, cancer registry integration, or cancer-specific modules.

### Search Results

#### French query results
1. **Analyse des resultats des campagnes de depistage du cancer du col de l'uterus a Conakry, Guinee** — https://www.em-consulte.com/article/1181723/analyse-des-resultats-des-campagnes-de-depistage-d
   Analysis of cervical cancer screening campaign results in Conakry, documenting VIA/VILI screening methods and case findings.

2. **Guinee: Formation regionale au depistage du cancer de col de l'uterus par IVA/cryotherapie** — https://staging.afro.who.int/fr/countries/guinea/news/guinee-formation-regionale-au-depistage-du-cancer-de-col-de-luterus-par-iva-cryotherapie
   WHO regional training programme on cervical cancer screening using visual inspection and cryotherapy treatment methods.

#### English query results
3. **Implementation of DHIS2 for Disease Surveillance in Guinea: 2015-2020** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8811041/
   Peer-reviewed study documenting DHIS2 implementation timeline, national scale-up, and performance metrics including 98.5% completeness for aggregate reporting by February 2020.

4. **Empowering Guinea: The IAEA Provides Guidance on Cancer Control Measures to One of its Newest Member States** — https://www.iaea.org/newscenter/news/empowering-guinea-the-iaea-provides-guidance-on-cancer-control-measures-one-of-its-newest-member-states
   December 2023 IAEA imPACT Review mission highlighting urgent need for HPV vaccine introduction, screening improvement, and establishment of a national cancer institute.

5. **Progress towards cervical cancer elimination — WHO Regional Office for Africa** — https://www.afro.who.int/countries/guinea/news/progress-towards-cervical-cancer-elimination
   WHO support for Guinea's cervical cancer elimination agenda, including training of 495 health workers and establishment of the Francophone Regional Training Centre in Conakry.


## Guyana — Cancer & DHIS2 Profile

### Summary
Guyana uses DHIS2 for its health information system. The country has a functional cancer registry based at Georgetown Public Hospital Corporation (GPHC), which recorded 693 cancer cases in the first half of 2024. A new state-of-the-art pathology laboratory was commissioned at GPHC in 2024 with support from Mount Sinai Health System and Hess Corporation. There is no documented integration between the cancer registry and DHIS2.

DHIS2 USE: NO EVIDENCE
Guyana uses DHIS2 for general health information management, but no evidence was found of the cancer registry being integrated with or operated through DHIS2. The Georgetown-based cancer registry appears to operate independently from the DHIS2 platform.

### Search Results

#### English query results
1. **693 cancer cases recorded in first six months of 2024 (Kaieteur News)** — https://www.kaieteurnewsonline.com/2024/10/12/693-cancer-cases-recorded-in-first-six-months-of-2024/
   Report on cancer registry data showing 693 cases in the first half of 2024, with breast cancer as the most prevalent.

2. **Guyana Cancer Registry (GHDx)** — https://ghdx.healthdata.org/organizations/guyana-cancer-registry
   Global Health Data Exchange profile of the Guyana Cancer Registry.

3. **Guyana Ministry of Health, Mount Sinai and Hess Corporation unveil new Pathology Laboratory** — https://www.hess.com/newsroom/news-article/2024-02-22-guyana-ministry-of-health-mount-sinai-health-system-and-hess-corporation-unveil-new-state-of-the-art-pathology-laboratory-to-promote-early-diagnosis-and-enhance-patient-care
   Announcement of a new pathology laboratory at GPHC supporting cancer diagnosis with reduced biopsy turnaround times.

4. **Guyana Takes Strategic Steps to Ensure Long-Term Success of Health Sector (World Bank)** — https://www.worldbank.org/en/news/feature/2024/04/08/guyana-takes-strategic-steps-to-ensure-long-term-success-of-health-sector
   World Bank feature on Guyana's strategic health sector development.

5. **Ethnicity and cancer in Guyana, South America (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC2638466/
   Research on cancer patterns and ethnicity in Guyana using cancer registry data.


## Haiti — Cancer & DHIS2 Profile

### Summary
Haiti faces significant challenges in cancer control, lacking a functional national cancer registry and radiation therapy facilities, though recent efforts by Partners In Health, IAEA, and PAHO are building cancer care infrastructure. The Ministry of Health has identified DHIS2 as a critical system for cancer data tracking within the broader SISNU national health information system, with training initiatives underway in Provincial Health Division offices. However, cancer data integration into DHIS2 remains in early stages, with only a few thousand cases entered into the National Tumor Registry to date.

DHIS2 USE: MODERATE
Haiti's Ministry of Health has prioritized DHIS2 for cancer registration and is conducting training for health staff, but operational integration of cancer data into DHIS2 remains incomplete with limited data entry from cancer facilities so far.

### Search Results

#### French query results
1. **Notre travail en Haiti** — https://www.paho.org/fr/haiti/notre-travail-haiti
   PAHO overview of health programs and technical cooperation work in Haiti, including NCD and cancer control support.

2. **Rapport Statistique 2023 | MSPP Haiti** — https://www.mspp.gouv.ht/wp-content/uploads/Rapport-Statistique-MSPP-2023_web.pdf
   Ministry of Public Health statistical report covering epidemiological surveillance, health system coverage, and SISNU data infrastructure for 2023.

3. **Rapport Statistique 2022 | MSPP Haiti** — https://www.mspp.gouv.ht/site/downloads/Rapport%20Statistique%20MSPP%202022.pdf
   Ministry of Public Health annual statistical report documenting health indicators, surveillance coverage, and health information system progress for 2022.

4. **Carte sanitaire - Haiti cartographie (SISNU)** — https://cartesanitaire.sisnu.net/
   Haiti's national health mapping platform within the SISNU integrated health information system.

5. **Presentation Annuaire Statistique 2023 | MSPP Haiti** — https://www.mspp.gouv.ht/wp-content/uploads/Presentation-Annuaire-Statistique-2023-final-191224-12.pdf
   Presentation of the 2023 statistical yearbook from Haiti's Ministry of Public Health covering health system data and surveillance metrics.

#### English query results
6. **Haiti's Move Towards a Comprehensive Approach to Cancer Control** — https://www.iaea.org/newscenter/news/haiti%E2%80%99s-move-towards-comprehensive-approach-cancer-control
   IAEA article on Haiti's efforts to build comprehensive cancer control infrastructure, including the planned National Radiotherapy Centre.

7. **The Evolution of Cancer Care in Haiti** — https://www.pih.org/article/evolution-cancer-care-haiti
   Partners In Health article documenting the development of oncology services at Hopital Universitaire de Mirebalais and the Roselene Jean Bosquet Oncology Center.

8. **A three-year epidemiological profile of cancers managed by a Haitian cancer program from 2016 to 2018** — https://ascopubs.org/doi/abs/10.1200/JCO.2019.37.15_suppl.e13079
   Journal of Clinical Oncology abstract presenting epidemiological data on cancer cases managed through Haiti's emerging cancer program.

9. **Haiti Strategic Health Information System Program (HIS)** — https://www.dai.com/our-work/projects/haiti-strategic-health-information-system-his-program
   DAI project page describing Haiti's strategic health information system program and digital health infrastructure development.

10. **Hope in Haiti for Better Cancer Care** — https://www.iaea.org/newscenter/news/hope-haiti-better-cancer-care-0
    IAEA article on improving cancer care capacity in Haiti, including radiation therapy planning and international support efforts.


## Honduras — Cancer & DHIS2 Profile

### Summary
Honduras launched a comprehensive National Strategic Plan for Cancer Control (2024-2030) prioritizing six cancer types, and operates the Copan Population-Based Cancer Registry in the western region. The country became the first to implement the regional ESAVI/EVADIE DHIS2 module for vaccine surveillance on a national server, and is advancing a broader Digital Health Transformation Roadmap with PAHO. However, specific integration of DHIS2 for cancer surveillance is not yet documented in publicly available sources.

DHIS2 USE: MODERATE
Honduras has successfully implemented DHIS2 for vaccine surveillance (ESAVI/EVADIE) and is pursuing digital health transformation, providing a foundation for potential cancer surveillance integration, though cancer registries currently use REDCap rather than DHIS2.

### Search Results

#### Spanish query results
1. **Honduras lanza el Plan Estrategico Nacional para el control del cancer 2024-2030** — https://www.paho.org/es/noticias/13-10-2023-honduras-lanza-plan-estrategico-nacional-para-control-cancer-2024-2030
   PAHO announcement of Honduras's national cancer control strategic plan covering governance, palliative care, prevention, and healthcare financing.

2. **OPS hace entrega del Plan Estrategico Nacional de Cancer en Honduras** — https://www.sshome.salud.gob.hn/index.php/component/k2/item/1065-ops-hace-entrega-del-plan-estrategico-nacional-de-cancer-en-honduras
   Honduras Ministry of Health report on PAHO's delivery of cancer plan resources, including 17 computers and equipment for cancer surveillance strengthening.

3. **Transformacion digital en Honduras: sistema de informacion para la vigilancia de ESAVI/EVADIE** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11648060/
   PMC article on Honduras's digital transformation through implementing a DHIS2-based information system for vaccination adverse event surveillance.

#### English query results
4. **Western Honduras Copan Population-Based Cancer Registry: Initial Estimates and a Model for Rural Central America** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8691495/
   PMC study presenting the first population-based cancer incidence estimates from the Copan registry, a model for cancer monitoring in rural low- and middle-income countries.

5. **Population-based Study of Gastric Cancer Survival and Associations in Rural Western Honduras** — https://pmc.ncbi.nlm.nih.gov/articles/PMC12103253/
   PMC study documenting high gastric cancer incidence and survival data in western Honduras, one of the highest rates in the western hemisphere.

6. **Honduras sets a milestone in digital transformation with the implementation of SIP Plus** — https://www.paho.org/en/news/8-7-2024-honduras-sets-milestone-digital-transformation-implementation-sip-plus
   PAHO article on Honduras's Digital Health Transformation Roadmap including electronic health records and real-time surveillance data capture.

7. **Digital transformation in Honduras: an information system for surveillance of ESAVI/AESI** — https://journal.paho.org/en/articles/digital-transformation-honduras-information-system-surveillance-esaviaesi
   Pan American Journal of Public Health article on Honduras's pioneering national implementation of the DHIS2-based ESAVI/EVADIE surveillance module.

8. **Collaborative effort to catalyze the implementation of the Global Initiative for Childhood Cancer in the Central American subregion** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10561657/
   PMC article on implementing the Global Initiative for Childhood Cancer across Central American countries including Honduras.

9. **Communication and counseling as part of comprehensive care at a cervical cancer clinic in Honduras** — https://www.paho.org/en/stories/communication-and-counseling-part-comprehensive-care-cervical-cancer-clinic-honduras
   PAHO story on cervical cancer screening and prevention support at specialized clinics in Honduras.

10. **Strengthening cancer treatment in Honduras** — https://www.iaea.org/sites/default/files/documents/tc/HON6003.pdf
    IAEA technical cooperation project document on strengthening cancer treatment capacity and infrastructure in Honduras.


## India — Cancer & DHIS2 Profile

### Summary
India operates extensive cancer control infrastructure through the NPCDCS screening program across 400+ districts and the ICMR National Cancer Registry Programme with 269 hospital-based and 38 population-based registries. DHIS2 has been implemented in India since 2006 across multiple states for general health program management, and HISP India maintains active implementation expertise, but cancer registries and screening programs run on separate platforms including REDCap, Ayushman Bharat apps, and custom state-level systems. Integration of cancer data into DHIS2 has been identified as an opportunity but is not yet operationalized at national level.

DHIS2 USE: MODERATE
India was an early DHIS2 adopter (2006) with implementations across multiple states for health program management, and HISP India provides direct implementation support, but cancer-specific data collection and registry operations use parallel systems without documented DHIS2 integration.

### Search Results

#### Hindi query results
1. **National Programme for Prevention and Control of Cancer, Diabetes, Cardiovascular Diseases & Stroke (NPCDCS)** — https://main.mohfw.gov.in/Major-Programmes/non-communicable-diseases-injury-trauma/Non-Communicable-Disease-II/National-Programme-for-Prevention-and-Control-of-Cancer-Diabetes-Cardiovascular-diseases-and-Stroke-NPCDCS
   Ministry of Health and Family Welfare page on India's national NCD program covering oral, breast, and cervical cancer screening through primary health centers.

2. **ICMR-NCDIR National Cancer Registry Programme** — https://www.icmr.gov.in/icmrobject/custom_data/1702893085_icmr_press_release_ncrp_18082020.pdf
   Indian Council of Medical Research press release on the National Cancer Registry Programme operating 269 hospital-based and 38 population-based cancer registries nationwide.

3. **NPCDCS Cancer Screening Implementation | National Health Mission** — https://www.nhm.gov.in/index1.php?lang=1&level=2&sublinkid=1048&lid=604
   National Health Mission page on population-based cancer screening expansion to 400+ districts with NCD App for patient-wise data capture and follow-up.

4. **HISP India** — https://hispindia.org/
   Health Information Systems Programme India, specializing in DHIS2 implementation across various health domains for over 15 years.

5. **Towards a Cancer-Free India** — https://mohfw.gov.in/?q=en/pressrelease-212
   Ministry of Health and Family Welfare press release on digital health infrastructure and cancer screening expansion initiatives.

#### English query results
6. **Integration of national cancer registry program with Ayushman Bharat Digital Mission in India: A necessity or an option** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9461636/
   PMC article examining the need to integrate cancer registries with Ayushman Bharat databases, insurance schemes, and health management information systems.

7. **Strengthening Cancer Surveillance in India: Role of the National Cancer Registry Programme** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9859961/
   PMC article on cancer surveillance system architecture covering 18% of India's population through population-based registries and the need for system integration.

8. **Current Status of Implementation of Cancer Screening Programme in India: A Review of Policies and Practice** — https://pmc.ncbi.nlm.nih.gov/articles/PMC12227947/
   PMC review of cancer screening implementation including state-level models such as Tamil Nadu's TCS-based health information system for cancer screening.

9. **Status of cancer screening in India: An alarm signal from the National Family Health Survey (NFHS-5)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10041275/
   PMC article highlighting that less than 1% of the Indian population has undergone cervical, breast, or oral cancer screening despite expanded programs.

10. **Determining the total cost of ownership and end user perception of the Kenya National Cancer Registry (NaCaRE-KE): a DHIS2-based digital health System** — https://pmc.ncbi.nlm.nih.gov/articles/PMC12011077/
    PMC evaluation of Kenya's functional DHIS2-based cancer registry, providing a proof-of-concept model relevant to India's potential DHIS2 cancer integration.


## Indonesia — Cancer & DHIS2 Profile

### Summary
Indonesia has comprehensive cancer control programs with multiple registry systems (SRIKANDI, INASGO) and screening programs for cervical and breast cancer operating since 2007. The Ministry of Health adopted DHIS2 in 2016 as the backbone for the One Health Data Application (Aplikasi Satu Data Kesehatan — ASDK), rolling it out across 50 districts, and academic institutions have developed DHIS2-based cancer registry packages and dashboards. However, cancer registries still operate through parallel systems separate from ASDK, with no formal policy yet for integrating cancer-specific data into the national DHIS2 platform.

DHIS2 USE: MODERATE
DHIS2 underpins Indonesia's national health data platform (ASDK) deployed across 50 districts, and DHIS2-based cancer registry packages have been developed at universities, but cancer registries (SRIKANDI, INASGO) remain on separate systems with no confirmed integration into ASDK.

### Search Results

#### Bahasa Indonesia query results
1. **Evaluasi penerapan konsep integrasi data menggunakan DHIS2 di Kementerian Kesehatan** — https://jurnal.ugm.ac.id/jisph/article/view/33959
   Evaluation of data integration using DHIS2 at the Indonesian Ministry of Health, published in the Journal of Information Systems for Public Health.

2. **Mengenal DHIS2: platform integrasi data** — https://jurnal.ugm.ac.id/bkm/article/view/44833
   Introduction to DHIS2 as a data integration platform, published in Berita Kedokteran Masyarakat.

3. **Platform WEB Based DHIS2 dalam Pembuatan Disease Registry** — https://jurnal.ugm.ac.id/jisph/article/view/71322
   Study on using DHIS2 to develop disease registries including cancer, with dual-language metadata translation and dashboard visualizations.

4. **Sosialisasi penggunaan Aplikasi Satu Data Kesehatan (ASDK) untuk Implementasi Sistem Pencatatan Integrasi** — https://journal.um-surabaya.ac.id/IAHS/article/view/25855
   Overview of ASDK rollout for integrated health recording systems at primary healthcare level.

5. **Twelve Years Implementation of Cervical and Breast Cancer Screening Program in Indonesia** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9360967/
   PMC study documenting 12 years (2007-2018) of cervical and breast cancer screening covering 3.6 million women across Indonesian provinces.

#### English query results
6. **Indonesia NCCP 2024-2034** — https://www.iccp-portal.org/news/indonesia-nccp-2024-2034
   ICCP Portal page on Indonesia's National Cancer Control Plan for 2024-2034, covering expansion of radiotherapy, screening standardization, and referral hospital networks.

7. **Indonesian Society of Gynecologic Oncology Cancer Registration Information System: 10 Years of Implementation** — https://ascopubs.org/doi/10.1200/GO.24.00176
   JCO Global Oncology article on INASGO's web-based gynecologic cancer registry system and its decade of operation.

8. **Enhancing health data quality: strengthening Indonesia's data quality assurance** — https://www.who.int/indonesia/news/detail/02-11-2023-enhancing-health-data-quality--strengthening-indonesia-s-data-quality-assurance
   WHO Indonesia report on strengthening data quality assurance for health information systems including ASDK.

9. **Five-Year Cancer Epidemiology at the National Referral Hospital: Hospital-Based Cancer Registry Data in Indonesia** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8081513/
   PMC study on hospital-based cancer registry data from Cipto Mangunkusumo Hospital, Indonesia's national referral center.

10. **Population-based cancer registration in Indonesia** — https://pubmed.ncbi.nlm.nih.gov/22799393/
    PubMed article on the development and challenges of population-based cancer registration across Indonesian provinces.


## Iraq — Cancer & DHIS2 Profile

### Summary
Iraq operates a national population-based cancer registry established in 1987, with over 40 public cancer care facilities distributed across governorates and 39,068 newly diagnosed cases reported in 2022. The Ministry of Health, supported by WHO, has rolled out DHIS2 across 1,848 health institutions as a real-time, web-based platform for surveillance and program monitoring, with phased modules being developed for NCD surveillance including cancer data digitization.

DHIS2 USE: HIGH
DHIS2 has been deployed across 1,848 health institutions in Iraq with WHO support, serving as the platform for disease surveillance and electronic patient data, with NCD and cancer surveillance modules in phased development.

### Search Results

#### Arabic query results
1. **المركز الوطني الريادي لبحوث السرطان** — https://bccru.uobaghdad.edu.iq/
   Iraq's National Leading Center for Cancer Research, established in 2008 with early detection units for breast and cervical cancers and a comprehensive patient data system.

2. **منظمة الصحة العالمية تطلق مبادرة الرقمنة الصحية في العراق** — https://www.emro.who.int/ar/iraq/news/who-launches-health-digitalization-initiative-in-iraq.html
   WHO EMRO Arabic-language page on the launch of the health digitalization initiative in Iraq, describing DHIS2 as an open-source platform combining essential tools.

3. **وزارة الصحة تنفذ ورشة عمل حول برنامج نظام ادارة المعلومات الصحية DHIS2** — https://moh.gov.iq/?article=13642
   Iraqi Ministry of Health page on DHIS2 training workshop conducted in November 2024 in coordination with WHO and UNICEF.

4. **تسجيل السرطان في الإقليم — WHO EMRO** — https://www.emro.who.int/ar/noncommunicable-diseases/information-resources/cancer-registry.html
   WHO EMRO resource on cancer registration in the Eastern Mediterranean Region, referencing Iraq's digitization of the cancer patient registration system.

#### English query results
5. **WHO strengthens health information systems in Iraq** — https://www.emro.who.int/iraq/news/who-strengthens-health-information-systems-in-iraq.html
   WHO report on DHIS2 rollout across 1,848 health institutions in Iraq, including training of 110 health professionals on the platform.

6. **WHO launches health digitalization initiative in Iraq** — https://www.emro.who.int/iraq/news/who-launches-health-digitalization-initiative-in-iraq.html
   WHO page detailing the February 2023 launch of the health digitalization initiative using DHIS2 for surveillance and program monitoring.

7. **Cancer Control and Oncology Care in Iraq** — https://www.jocms.org/index.php/jcms/article/view/1154
   Journal article on Iraq's National Cancer Control Plan implementation through the Iraqi Cancer Board, covering over 40 public cancer care facilities.

8. **Cancer Registry of Iraq Annual Report 2024** — https://storage.moh.gov.iq/2024/03/31/2024_03_31_11983087032_3940351786864953.pdf
   Official annual report from the Iraqi Cancer Registry with 2022 statistics on cancer incidence across governorates.

9. **Cancer Incidence in the Kurdistan Region of Iraq** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9272643/
   PMC study documenting seven years of cancer registration in Erbil and Duhok Governorates in the Kurdistan Region.

10. **Implementation of the Noncommunicable Disease Capacity Assessment and Planning Process** — https://bmcpublichealth.biomedcentral.com/articles/10.1186/s12889-025-24593-0
    BMC Public Health article on NCD surveillance framework development in Iraq, including cancer, with WHO technical support.


## Jamaica — Cancer & DHIS2 Profile

### Summary
Jamaica is the first Caribbean country and second in the world to use DHIS2 for population-wide cancer reporting through its National Cancer Registry. This was implemented through a Bloomberg Philanthropies Data for Health (D4H) Initiative grant valued at US$100,000, awarded through a collaboration between CARPHA, Vital Strategies, and the Ministry of Health and Wellness. The project, entitled "Using DHIS2 to Strengthen Cancer Registration Data Systems in Low-resourced Countries: From Rwanda to Jamaica," uses DHIS2 Tracker for cancer registration data collection. Jamaica is a pioneer alongside Rwanda and Maldives in DHIS2-based cancer registration.

DHIS2 USE: HIGH
Jamaica has implemented DHIS2 Tracker for its National Cancer Registry, making it the first Caribbean country and second globally to use DHIS2 for population-wide cancer reporting. The system was operationalized through the Bloomberg Philanthropies Data for Health Initiative with Vital Strategies and CARPHA, building on the Rwanda model.

### Search Results

#### English query results
1. **Jamaica to Enhance Cancer Data Tracking with New Software (Ministry of Health and Wellness, Jamaica)** — https://www.moh.gov.jm/jamaica-to-enhance-cancer-data-tracking-with-new-software/
   Official government announcement of Jamaica as the first Caribbean country and second in the world to use DHIS2 for population-wide cancer reporting.

2. **Cancer Registration Data Systems Being Strengthened (Jamaica Information Service)** — https://jis.gov.jm/cancer-registration-data-systems-being-strengthened/
   Jamaica Information Service report on the strengthening of cancer registration data systems through DHIS2 implementation.

3. **CARPHA and Vital Strategies Sign New Agreement to Strengthen Cancer Data Collection Systems in Jamaica** — https://antiguaobserver.com/carpha-and-vital-strategies-sign-new-agreement-to-strengthen-cancer-data-collection-systems-in-jamaica/
   Report on the CARPHA-Vital Strategies agreement for the US$100,000 Bloomberg D4H grant supporting Jamaica's DHIS2 cancer registry.

4. **Cancer registration data systems being strengthened (Jamaica Gleaner)** — https://jamaica-gleaner.com/article/news/20231025/cancer-registration-data-systems-being-strengthened
   Jamaica Gleaner coverage of the cancer registration data system strengthening initiative.

5. **Jamaica to enhance cancer data tracking with new software (Jamaica Gleaner)** — https://jamaica-gleaner.com/article/news/20231027/jamaica-enhance-cancer-data-tracking-new-software
   Jamaica Gleaner report on the new DHIS2-based cancer data tracking software implementation.

6. **Cancer registration data system being introduced to better monitor disease (Jamaica Observer)** — https://www.jamaicaobserver.com/latest-news/cancer-registration-data-system-being-introduced-to-better-monitor-disease-dr-tufton/
   Jamaica Observer article on the introduction of the DHIS2 cancer registration data system with comments from Dr. Tufton.

7. **About the GICR — E-NNOVATE project (IARC)** — https://gicr.iarc.fr/news/2023/12/04/e-nnovate-iacr-write-up/
   IARC Global Initiative for Cancer Registry Development page referencing Jamaica's participation in DHIS2-based cancer registration.


## Jordan — Cancer & DHIS2 Profile

### Summary
Jordan has operated a mature population-based cancer registry (Jordan Cancer Registry) since 1996 under compulsory notification legislation, achieving 95%+ coverage and recording 10,775 new cancer cases in 2022. The Ministry of Health has introduced DHIS2 as part of its National Digital Health Strategy for real-time facility performance tracking, and the country is investing heavily in digital health transformation with over 285 health facilities digitalized, though DHIS2 application specifically for cancer surveillance is not yet documented.

DHIS2 USE: MODERATE
DHIS2 has been adopted within Jordan's broader health information system for monitoring and evaluation, but there is no public documentation of DHIS2 being used specifically for cancer registry or cancer surveillance functions.

### Search Results

#### Arabic query results
1. **أنظمة المعلومات الصحية — وزارة الصحة الاردنية** — https://www.moh.gov.jo/AR/List/%D8%A3%D9%86%D8%B8%D9%85%D8%A9_%D8%A7%D9%84%D9%85%D8%B9%D9%84%D9%88%D9%85%D8%A7%D8%AA_%D8%A7%D9%84%D8%B5%D8%AD%D9%8A%D8%A9
   Jordan Ministry of Health official page on health information systems used across the national health sector.

2. **المتابعة والتقييم والتعلم — البرنامج الأردني لسرطان الثدي** — https://www.jbcp.jo/ar/what-we-do/45
   Jordan Breast Cancer Program page on monitoring, evaluation, and learning activities for breast cancer screening and downstaging.

3. **الدراسات العليا في الرعاية المعلوماتية للسرطان — مركز الحسين للسرطان** — https://www.khcc.jo/ar/cancer-care-informatics
   King Hussein Cancer Center graduate program in Cancer Care Informatics, supporting informatics approaches for cancer data systems.

#### English query results
4. **Cancer Care in Resource-Limited Countries: Jordan as an Example** — https://ascopubs.org/doi/10.1200/GO.24.00237
   JCO Global Oncology article examining cancer care delivery in Jordan, covering registry operations, screening programs, and infrastructure challenges.

5. **Promoting data-driven decision-making in Jordan: strengthening national health information system** — https://pmc.ncbi.nlm.nih.gov/articles/PMC12125760/
   PMC article on strengthening Jordan's national health information system and achieving consensus on core health system indicators.

6. **Advancing paediatric cancer care in Jordan: a strategic 10-year roadmap** — https://www.sciencedirect.com/science/article/abs/pii/S2352464225001038
   ScienceDirect article presenting a strategic roadmap for pediatric cancer care improvement in Jordan.

7. **Cancer registration in the Middle East, North Africa, and Turkey (MENAT) region** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9730320/
   PMC review of cancer registration across 19 MENAT countries, covering challenges for refugee-hosting nations including Jordan.

8. **Health information systems in Jordan and Palestine: the need for health informatics training** — https://www.emro.who.int/emhj-volume-26-2020/volume-26-issue-11/health-information-systems-in-jordan-and-palestine-the-need-for-health-informatics-training.html
   WHO EMRO article on health information system gaps in Jordan and the need for expanded health informatics workforce capacity.

9. **Public hospitals to complete digital transformation by end of 2024** — https://jordantimes.com/news/local/public-hospitals-complete-digital-transformation-end-2024
   Jordan Times report on the Health Computing Company completing digitalization of all public hospitals, with EMR reaching 55% of health facilities.

10. **WHO contribution in Jordan (2021-2024) Evaluation report** — https://cdn.who.int/media/docs/default-source/evaluation-office/report_evaluation-of-who-s-contribution-in-jordan_web.pdf?sfvrsn=adb72d1e_3&download=true
    WHO evaluation report covering health system strengthening activities in Jordan from 2021 to 2024, including digital health and information system support.


## Kazakhstan — Cancer & DHIS2 Profile

### Summary
Kazakhstan has established comprehensive cancer control programs with population-based screening for breast, cervical, colorectal, and prostate cancers since 2008, managed by the Kazakh Institute of Oncology and Radiology across 18+ regional oncology dispensaries. The country uses a proprietary Unified National Electronic Health System (UNEHS) and a dedicated Electronic Registry of Oncological Patients (EROP) with 3,000+ users tracking over 163,000 patients, but there is no confirmed evidence of DHIS2 implementation for cancer programs or broader health data management.

DHIS2 USE: NO EVIDENCE
Kazakhstan relies on domestically developed health information systems (UNEHS and EROP) for cancer surveillance and patient management, with no publicly available information indicating DHIS2 adoption.

### Search Results

#### Russian query results
1. **Электронный регистр онкобольных Республики Казахстан** — https://www.haulmont.ru/projects/cancer-patients-registry/
   Haulmont project page describing Kazakhstan's Electronic Registry of Oncological Patients (EROP), deployed across 18 regional oncology dispensaries and 3,000+ polyclinic users.

2. **Информационные системы здравоохранения МЗ РК** — https://rcez.kz/informationsystems
   Republican Center for Electronic Health page listing Kazakhstan's health information systems including the Unified National Electronic Health System.

3. **Информационно-аналитический центр — КазНИИОиР** — https://onco.kz/clinic/informacionno-analiticheskij-centr/
   Information-Analytical Center of the Kazakh Research Institute of Oncology and Radiology, the national hub for cancer data analysis and reporting.

#### English query results
4. **National Electronic Oncology Registry in Kazakhstan: Patient's Journey** — https://www.journalehdi.com/article/national-electronic-oncology-registry-in-kazakhstan-patients-journey-16385
   Epidemiology and Health Data Insights article on Kazakhstan's electronic oncology registry system and comprehensive patient journey tracking.

5. **Kazakhstan's progress in tackling cardiovascular diseases and cancer highlighted in WHO report** — https://www.who.int/europe/news/item/28-07-2025-kazakhstan-s-progress-in-tackling-cardiovascular-diseases-and-cancer-highlighted-in-who-report
   WHO Europe report recognizing Kazakhstan's achievements in cancer control and cardiovascular disease prevention as of 2025.


## Kenya — Cancer & DHIS2 Profile

### Summary
Kenya's National Cancer Registry (NaCaRE-KE) is built on DHIS2, launched in 2021 through an Act of Parliament establishing the National Cancer Institute of Kenya (NCI-K). The system collects patient demographics, diagnosis, staging, treatment, and survivorship data across health facilities. Published research documents the total cost of ownership of the DHIS2-based system. The live system is accessible at nacare.ncikenya.go.ke. Kenya is one of the most advanced implementations of DHIS2 for cancer registration globally.

DHIS2 USE: HIGH
Kenya has a fully operational DHIS2-based National Cancer Registry (NaCaRE-KE) launched in 2021, with comprehensive cancer data collection including patient demographics, diagnosis, staging, treatment, and survivorship. The system has been evaluated for total cost of ownership and end-user perception, with published research in peer-reviewed journals.

### Search Results

#### English query results
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


## Kiribati — Cancer & DHIS2 Profile

### Summary
Kiribati is a small Pacific island nation with very limited health infrastructure. The country faces significant geographic challenges as a low-lying atoll nation susceptible to climate change impacts on healthcare access. A National Health Strategic Plan (2024-2027) is under development. Cancer screening efforts focus on cervical cancer through UNFPA support, but there is no population-based cancer registry and no evidence of DHIS2 being used for cancer data or general health information management.

DHIS2 USE: NO EVIDENCE
No documentation was found indicating DHIS2 is used in Kiribati for any health information management purpose, including cancer. The country's health information infrastructure is extremely limited, and cancer data collection remains minimal.

### Search Results

#### English query results
1. **Cancer control in the Pacific: big challenges facing small island states (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7746436/
   Overview of cancer control challenges in Pacific island nations including Kiribati, noting geographic barriers and limited infrastructure.

2. **Kiribati Health Ministry launched critical policy documents for women's health (UNFPA)** — https://pacific.un.org/en/246551-kiribati-health-ministry-launched-critical-policy-documents-women%E2%80%99s-health-unfpa-australia
   UNFPA-supported policy documents for women's health including cervical cancer screening guidelines in Kiribati.

3. **Kiribati National Health Strategy 2020-23 (Ministry of Health)** — https://p4h.world/app/uploads/2024/09/Kiribati-National-Health-Strategy-2020-23.x80726.pdf
   Kiribati's national health strategy document outlining health system priorities.

4. **Cancer Disparities among Pacific Islanders: Sociocultural Determinants in the Micronesian Region (MDPI)** — https://www.mdpi.com/2072-6694/15/5/1392
   Research on cancer disparities and sociocultural health determinants in the Micronesian region including Kiribati.

5. **Kiribati WHO Country Profile** — https://data.who.int/countries/296
   WHO data profile for Kiribati covering health indicators and system performance.


## Kyrgyzstan — Cancer & DHIS2 Profile

### Summary
Kyrgyzstan has a well-established cancer registry system that has been operational since 1953 (paper-based), with a new digital registry deployed covering 3 regions including Chüy Region. WHO Europe and IARC have been supporting Kyrgyzstan's cancer data capacity, and in February 2025 WHO reported on advancing cancer prevention and care in the country. DHIS2 has some presence in Kyrgyzstan through HISP UiO training (e.g., for immunization programs), but there is no evidence of DHIS2 being used for cancer registration or surveillance.

DHIS2 USE: NO EVIDENCE
While DHIS2 has been introduced in Kyrgyzstan for specific health programs (e.g., immunization through the IPA Vaccines to Vaccination Project), there is no documented connection between DHIS2 and Kyrgyzstan's cancer registry system. The cancer registry operates as a separate digital system.

### Search Results

#### English query results
1. **Breast Cancer Incidence in Kyrgyzstan: Report of 15 Years of Cancer Registry (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9587886/
   Peer-reviewed analysis of breast cancer trends using 15 years of data from Kyrgyzstan's cancer registry.

2. **Data shapes people's views on cancer in Kyrgyzstan (WHO Europe, 2023)** — https://www.who.int/europe/news/item/15-02-2023-data-shapes-people-s-views-on-cancer-in-kyrgyzstan
   WHO report on how cancer registry data is informing public health perspectives in Kyrgyzstan.

3. **Advancing cancer prevention and care in Kyrgyzstan (WHO Europe, 2025)** — https://www.who.int/europe/news/item/04-02-2025-advancing-cancer-prevention-and-care-in-kyrgyzstan-steps-on-a-new-road
   Recent WHO report on steps forward in cancer prevention and care in the country.

4. **Kyrgyzstan imPACT Review (IAEA, 2015)** — https://www.iaea.org/sites/default/files/documents/review-missions/kyrgyzstan-2015-impact-review.pdf
   IAEA Programme of Action for Cancer Therapy review of cancer control capacity in Kyrgyzstan.

5. **Cancer Kyrgyzstan 2020 Country Profile (WHO)** — https://www.who.int/publications/m/item/cancer-kgz-2020
   WHO country profile summarizing cancer incidence, mortality, and health system capacity.


## Lao — Cancer & DHIS2 Profile

### Summary
Lao PDR has DHIS2 as its official national health information reporting platform, integrated across major health programs (MNCH, TB, Malaria, HIV). The country developed its first Digital Health Strategy (2023-2027) and is piloting DHIS2 extensions for additional data domains. Cancer has become a new focus for DHIS2 integration through the WHO-HISP Centre partnership, with cancer data tools being developed for routine facility reporting. However, Lao PDR lacks a population-based cancer registry and cancer-specific DHIS2 modules are still emerging.

DHIS2 USE: LOW
DHIS2 is the established national HMIS in Lao PDR, and the WHO-HISP collaboration is actively developing cancer data tools for DHIS2. However, there is no evidence of operational cancer-specific tracking or registry functionality in Lao's DHIS2 system yet. Cancer data integration appears to be in early development stages.

### Search Results

#### English query results
1. **Applying ICT to Health Information Systems in Low Resource Settings: Implementing DHIS2 in Lao PDR (ResearchGate)** — https://www.researchgate.net/publication/317168448_Applying_ICT_to_Health_Information_Systems_HIS_in_Low_Resource_Settings_Implementing_DHIS2_as_an_Integrated_Health_Information_Platform_in_Lao_PDR
   Research paper on DHIS2 implementation as an integrated health information platform in Lao PDR.

2. **Strengthening health information systems and evidence-based policy (WHO Lao)** — https://www.who.int/laos/our-work/strengthening-health-information-systems
   WHO support for health information systems strengthening in Lao PDR.

3. **Lao PDR Digital Health Strategy 2023-2027** — https://extranet.who.int/countryplanningcycles/sites/default/files/public_file_rep/LAO_Lao_People-Digital-Health-Strategy_2023-2027.pdf
   National digital health strategy outlining governance, workforce, standards, and infrastructure plans.

4. **DHIS2 Database (NIPN Lao)** — https://nipn.lsb.gov.la/dhis2-database/
   National Information Platform for Nutrition using DHIS2 in Lao PDR.


## Lebanon — Cancer & DHIS2 Profile

### Summary
Lebanon has a well-established National Cancer Registry (NCR) managed by the Ministry of Public Health, with cancer incidence reaching 11,589 new cases in 2020. DHIS2 was introduced in Lebanon in 2014 for school-based surveillance and expanded in 2017 for epidemiological disease surveillance across hospitals, medical centers, dispensaries, and laboratories. However, the National Cancer Registry operates as a separate specialized system and there is no evidence of cancer data being managed through DHIS2.

DHIS2 USE: NO EVIDENCE
DHIS2 is operational in Lebanon for epidemiological disease surveillance through the Ministry of Public Health's Epidemiology Surveillance Unit, but the National Cancer Registry is maintained separately. No search results linked DHIS2 to cancer data collection or management in Lebanon.

### Search Results

#### English query results
1. **National Cancer Registry (MoPH Lebanon)** — https://www.moph.gov.lb/en/Pages/8/19526/national-cancer-registry
   Official Ministry of Public Health page on Lebanon's National Cancer Registry.

2. **WHO EMRO: WHO initiates DHIS2 online disease reporting system in Lebanon** — https://www.emro.who.int/lbn/lebanon-infocus/dhis2.html
   WHO report on DHIS2 implementation for epidemiological surveillance in Lebanon, piloted in 2014 and expanded in 2017.

3. **Use of DHIS-2 for Real Time Surveillance: Lebanon 2017 (iProceedings)** — https://www.iproc.org/2018/1/e10547/
   Paper on Lebanon's use of DHIS2 for real-time disease surveillance.

4. **Cancer trends in Lebanon: incidence rates 2003-2008 and projections until 2018 (Population Health Metrics)** — https://pophealthmetrics.biomedcentral.com/articles/10.1186/1478-7954-12-4
   Peer-reviewed analysis of cancer incidence trends using National Cancer Registry data.

5. **General Oncology Care in Lebanon (Springer)** — https://link.springer.com/chapter/10.1007/978-981-16-7945-2_8
   Overview of oncology care infrastructure and cancer burden in Lebanon.


## Lesotho — Cancer & DHIS2 Profile

### Summary
Lesotho uses DHIS2 as its national health management information system, rolled out across all ten districts and covering all 333 health facilities with over 40 million records. The country has a high burden of cervical cancer driven by high HIV rates. A Digital Health Strategy 2025-2030 was validated in December 2024. Lesotho falls within the GICR sub-Saharan Africa Hub covering 48 countries for cancer registry development. Despite comprehensive DHIS2 coverage for health programs including HIV, TB, and maternal health, no cancer-specific DHIS2 modules are documented.

DHIS2 USE: NO EVIDENCE
Lesotho has comprehensive DHIS2 implementation across all health facilities for multiple health programs, but no evidence was found of DHIS2 being used specifically for cancer registration, screening data, or cancer surveillance.

### Search Results

#### English query results
1. **In Lesotho, New Health Information System Provides Streamlined, Integrated Data Across Health Programs (ICAP at Columbia University)** — https://icap.columbia.edu/news-events/in-lesotho-new-health-information-system-provides-streamlined-integrated-data-across-health-programs/
   ICAP report on Lesotho's DHIS2 rollout across all 333 health facilities covering HIV, TB, maternal health, and other programs.

2. **Improving DHIS2 Integration for Lesotho's Health Information Ecosystem (DHIS2 Community)** — https://community.dhis2.org/t/improving-dhis2-integration-for-lesothos-health-information-ecosystem/47463
   DHIS2 Community discussion on improving DHIS2 integration across Lesotho's health information systems.

3. **Lesotho validates its Digital Health Strategy 2025-2030 (WHO AFRO)** — https://www.afro.who.int/countries/lesotho/news/lesotho-validates-its-digital-health-strategy-2025-2030-step-towards-transforming-healthcare-through
   WHO report on Lesotho's validation of its Digital Health Strategy 2025-2030 in December 2024.

4. **Determinants of cervical cancer screening uptake in Lesotho: evidence from 2024 Demographic and Health Survey (BMC Public Health)** — https://bmcpublichealth.biomedcentral.com/articles/10.1186/s12889-025-23593-4
   Research on cervical cancer screening determinants in Lesotho using 2024 DHS data, highlighting low screening coverage despite high cervical cancer burden.

5. **Lesotho — GLOBOCAN Factsheet (IARC)** — https://gco.iarc.who.int/media/globocan/factsheets/populations/426-lesotho-fact-sheet.pdf
   IARC GLOBOCAN fact sheet providing cancer incidence and mortality estimates for Lesotho.


## Liberia — Cancer & DHIS2 Profile

### Summary
Liberia uses DHIS2 for its health management information system, adopted in December 2014 during the Ebola outbreak response. The system has since expanded to include community-based health information and the DHIS2 Tracker for individual-level data. Post-Ebola health system rebuilding has focused on disease surveillance, community health, and general HMIS strengthening. There is no documented cancer registry or cancer-specific DHIS2 implementation in Liberia.

DHIS2 USE: NO EVIDENCE
Liberia has DHIS2 deployed for general health information management and disease surveillance, including the Tracker application for individual-level data. However, no evidence was found of DHIS2 being used for cancer registration, screening, or cancer-specific surveillance. Post-Ebola health system rebuilding priorities have focused on infectious disease and primary care.

### Search Results

#### English query results
1. **Liberia health system's journey to long-term recovery and resilience post-Ebola (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10317185/
   Case study of Liberia's health system recovery post-Ebola, including DHIS2 adoption for health information management.

2. **Strengthening the community health program in Liberia: Lessons learned from a health system approach (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7956118/
   Study on community health program strengthening in Liberia, including integration of community-based data into DHIS2.

3. **Enhancing quantitative capacity for the health sector in post-Ebola Liberia (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11868749/
   Tracer study of locally developed coding and biostatistics programs for health sector capacity building in Liberia.

4. **Documenting the development, adoption and pre-Ebola implementation of Liberia's IDSR strategy (BMC Public Health)** — https://bmcpublichealth.biomedcentral.com/articles/10.1186/s12889-023-17006-7
   Documentation of Liberia's Integrated Disease Surveillance and Response strategy development and DHIS2 integration.

5. **CDC in Liberia (CDC Global Health)** — https://www.cdc.gov/global-health/countries/liberia.html
   CDC overview of health activities in Liberia including health system strengthening support.


## Libya — Cancer & DHIS2 Profile

### Summary
Libya has regional cancer registries (Benghazi Cancer Registry operational since 2003, Misurata hospital-based registry since 2008) but lacks a national population-based cancer registry. In 2016, Libya, WHO, and IARC agreed to start population-based registries in Tripoli and Benghazi. DHIS2 was introduced in Libya in 2018 through WHO/UNICEF/IOM support with training of trainers workshops in Tripoli and Benghazi, but there is no evidence of DHIS2 being used for cancer data management.

DHIS2 USE: NO EVIDENCE
DHIS2 was introduced in Libya in 2018 for general health information management, but the cancer registries operate as separate systems. No evidence was found of DHIS2 being integrated with cancer registration or surveillance.

### Search Results

#### English query results
1. **Cancer incidence, mortality, and survival in Eastern Libya: updated report from the Benghazi Cancer Registry (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/25911981/
   Updated report from the Benghazi Cancer Registry covering cancer incidence and mortality trends.

2. **WHO EMRO: Assessment workshop on cancer registry in Libya** — https://www.emro.who.int/lby/libya-infocus/assessment-workshop-on-cancer-registry-in-libya.html
   WHO workshop on establishing population-based cancer registries in Tripoli and Benghazi.

3. **Training of Trainers on DHIS-2 in Libya (ReliefWeb, 2018)** — https://reliefweb.int/report/libya/training-trainers-district-health-information-system-dhis-2-28-july-2nd-august-2018
   Documentation of DHIS2 training workshops in Tripoli and Benghazi with WHO/UNICEF/IOM support.

4. **General Oncology Care in Libya (Springer)** — https://link.springer.com/chapter/10.1007/978-981-16-7945-2_9
   Overview of oncology care and cancer burden in Libya.

5. **Cancer incidence in the middle region of Libya: Data from Misurata (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/34129289/
   Cancer epidemiology data from the Misurata region hospital-based registry.


## Madagascar — Cancer & DHIS2 Profile

### Summary
Madagascar has DHIS2 deployed as its national health information management system since 2019, with the Ministry of Health's Strategic Plan for Health Information System Strengthening (PSRSIS 2023-2027) guiding further development. DHIS2 is used for aggregate data collection at regional hospital level and is being extended with WHO/USAID support. However, Madagascar lacks organized cancer screening campaigns and has no population-based cancer registry integrated into DHIS2. There is no evidence of cancer-specific data collection through the DHIS2 platform.

DHIS2 USE: NO EVIDENCE
DHIS2 is the national HMIS in Madagascar, but no cancer-specific modules, registries, or screening data collection through DHIS2 were identified. Cancer services remain limited with no organized screening campaigns.

### Search Results

#### English query results
1. **Plan Stratégique de Renforcement du SIS 2023-2027 (Madagascar MoH)** — https://www.msanp.gov.mg/upload/documents/PSRSIS_2023-2027.pdf
   Madagascar's strategic plan for strengthening the health information system including DHIS2.

2. **Madagascar DHIS2 Tracker (Digital Impact Exchange)** — https://exchange.dial.global/projects/madagascar-dhis2-tracker
   Overview of Madagascar's DHIS2 Tracker implementation for individual-level health data.

3. **Technical Assistance for Madagascar DHIS2 based HMIS (UNGM)** — https://www.ungm.org/Public/Notice/176701
   UN procurement notice for technical assistance to enhance Madagascar's DHIS2-based HMIS.

4. **Cancer care challenges in Madagascar (Journal of Cancer Therapy, 2022)** — https://www.scirp.org/pdf/jct_2022082415203048.pdf
   Research documenting cancer care challenges including lack of screening campaigns and immunohistochemistry laboratory capacity.

5. **Estimating cause-specific mortality in Madagascar (Population Health Metrics)** — https://pophealthmetrics.biomedcentral.com/articles/10.1186/s12963-019-0190-z
   Study evaluating death notification data from Antananarivo including cancer mortality patterns.


## Malawi — Cancer & DHIS2 Profile

### Summary
Malawi has national DHIS2 implementation since 2012 as its Health Management Information System (HMIS), with aggregated facility-level data captured into DHIS2 across all districts under the Central Monitoring and Evaluation Division. The country has the world's highest cervical cancer incidence and mortality rates (ASR 75.9 and 49.8 per 100,000 respectively).

The Malawi National Cancer Registry (NCR), based at Queen Elizabeth Central Hospital (QECH) in Blantyre, is the country's only population-based cancer registry, operational since the 1980s with active case-finding through regular hospital visits. The Malawi National Cancer Center (NCC) at Kamuzu Central Hospital officially opened on 2 July 2025 — the country's first dedicated cancer treatment unit — providing chemotherapy, radiotherapy (cobalt machine, two LINACs, HDR brachytherapy), and surgical oncology through collaboration between the Ministry of Health, IAEA, University of North Carolina, Baylor College of Medicine, and the OPEC Fund.

Cervical cancer screening uses a VIA screen-and-treat approach scaled to over 130 sites nationwide since 2011, with coverage rising from 9.3% to 26.5% of eligible women between 2011-2015, though still far below the national 80% target. HPV vaccination was piloted in 2013 (Rumphi and Zomba districts) and launched nationally in 2018. Breast cancer screening has no national programme; four mammography units donated in 2012 remain non-operational.

A 2025 gap analysis of cancer registry tools across Malawi's central hospitals (presented at CIOC 2026) explicitly identified "limited integration with DHIS2" and "underutilization of CanReg5 for lung cancer data" as key gaps, alongside the absence of lung cancer-specific indicators in existing tools and lack of standardized referral pathways. This led to development of 17 standardized tools (9 newly created, 8 revised) to address deficiencies.

DHIS2 USE: LOW
Malawi has comprehensive national DHIS2 implementation for health data reporting, and cervical cancer screening data appears to flow through the HMIS reporting system to DHIS2 at aggregate level. However, the 2025 gap analysis explicitly documented "limited integration with DHIS2" as a deficiency in cancer data systems. The Blantyre Cancer Registry and CanReg5 operate as separate systems. Cancer-specific DHIS2 Tracker modules have not been implemented.

### Search Results

#### English query results
1. **Cervical cancer screening uptake and challenges in Malawi from 2011 to 2015: retrospective cohort study (BMC Public Health)** — https://bmcpublichealth.biomedcentral.com/articles/10.1186/s12889-016-3530-y
   Retrospective analysis of cervical cancer VIA screening programme scale-up across Malawi from 2011 to 2015, documenting expansion from 75 to 130 sites.

2. **Bridging gaps in lung cancer diagnosis and surveillance: Gap analysis and development of new registers and algorithms for lung cancer identification and care in Malawi (CIOC 2026)** — https://cancer-conferences.magnusgroup.org/program/scientific-program/2026/bridging-gaps-in-lung-cancer-diagnosis-and-surveillance-gap-analysis-and-development-of-new-registers-and-algorithms-for-lung-cancer-identification-and-care-in-malawi
   2025 gap analysis of cancer registry tools across Malawi's central hospitals identifying "limited integration with DHIS2" and "underutilization of CanReg5" as key deficiencies, with 17 standardized tools developed.

3. **Malawi National Cancer Control Strategic Plan 2019-2029 (ICCP Portal)** — https://www.iccp-portal.org/sites/default/files/plans/MALAWI%20NATIONAL%20CANCER%20CONTROL%20STRATEGIC%20PLAN-%202019-2029.pdf
   Malawi's national strategic plan for comprehensive cancer prevention and control covering six thematic areas.

4. **Malawi National Cancer Center (NCC)** — https://www.nccmalawi.org/about
   Official site of the Malawi National Cancer Centre at Kamuzu Central Hospital, opened July 2025, the country's first dedicated cancer treatment unit.

5. **The state of oncology in Malawi in 2015 (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC4688866/
   Comprehensive review of oncology services, cancer registration, and treatment capacity in Malawi.

6. **Three-year cancer incidence in Blantyre, Malawi 2008-2010 (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC5999322/
   Peer-reviewed analysis of cancer incidence data from the Blantyre Cancer Registry.

7. **Malawi on track towards national roll out of cervical cancer prevention through HPV vaccination (WHO AFRO)** — https://www.afro.who.int/news/malawi-track-towards-national-roll-out-cervical-cancer-prevention-through-hpv-vaccination
   WHO report on Malawi's HPV vaccination pilot (2013) and national roll-out (2018).

8. **Cervical cancer screening coverage and its related knowledge in southern Malawi (BMC Public Health)** — https://bmcpublichealth.biomedcentral.com/articles/10.1186/s12889-022-12547-9
   Study on cervical cancer screening coverage and knowledge among women in southern Malawi, noting coverage remains below national targets.

9. **Setting up a new radiation therapy centre in Malawi: Opportunities and challenges (ScienceDirect)** — https://www.sciencedirect.com/science/article/pii/S2405632424000313
   Study on establishing radiotherapy services at the Malawi National Cancer Centre, documenting equipment (cobalt, LINACs, brachytherapy) and workforce.

10. **Breast and cervical cancer screening services in Malawi: a systematic review (BMC Cancer)** — https://bmccancer.biomedcentral.com/articles/10.1186/s12885-020-07610-w
    Systematic review of breast and cervical cancer screening services in Malawi, documenting the absence of a national breast cancer screening programme and mammography challenges.


## Maldives — Cancer & DHIS2 Profile

### Summary
The Maldives Ministry of Health has developed a cancer registry in DHIS2 Tracker. The Maldives is one of the pioneer countries, alongside Rwanda and Jamaica, for DHIS2-based cancer registration through IARC and HISP collaboration. These pilot implementations demonstrated the feasibility of using DHIS2 to capture standardized oncology data while maintaining compatibility with IARC's CanReg5 framework. The Maldives also uses DHIS2 more broadly, including a public health records portal designed by HISP Sri Lanka.

DHIS2 USE: HIGH
The Maldives has developed a DHIS2 Tracker-based cancer registry as one of the pioneer countries in the IARC-HISP collaboration for standardized cancer registration. The system captures standardized oncology data compatible with CanReg5, demonstrated through the pilot phase that informed the global scale-up of DHIS2 cancer registries.

### Search Results

#### English query results
1. **Support DHIS2 Implementation and Strengthening of HIMS Platform in Maldives (HISP Bangladesh)** — https://hispbd.org/projects/support-dhis2-implementation-and-for-overall-strengthening-of-health-management-information-system-hims-platform-in-maldives/
   HISP Bangladesh project supporting DHIS2 implementation and health information system strengthening in the Maldives.

2. **Improving the monitoring and care of cancer patients by enabling information exchange between DHIS2 and CanReg5 (DHIS2 Community)** — https://community.dhis2.org/t/improving-the-monitoring-and-care-of-cancer-patients-by-enabling-the-information-exchange-between-dhis2-and-cancer-registry-system-canreg5/43022
   Documentation of the DHIS2-CanReg5 interoperability framework relevant to the Maldives cancer registry implementation.

3. **About the GICR — Child GICR Activities (IARC)** — https://gicr.iarc.fr/childgicr/activities/
   IARC Global Initiative for Cancer Registry Development activities page, including the DHIS2 cancer registration work relevant to Maldives.

4. **About the GICR — E-NNOVATE project (IARC)** — https://gicr.iarc.fr/news/2023/12/04/e-nnovate-iacr-write-up/
   IARC documentation of the E-NNOVATE project supporting electronic cancer registration innovations including DHIS2-based approaches piloted in the Maldives.

5. **Maldives — GLOBOCAN Factsheet (IARC)** — https://gco.iarc.who.int/media/globocan/factsheets/populations/462-maldives-fact-sheet.pdf
   IARC GLOBOCAN cancer statistics and projections for the Maldives.

6. **Planning and Developing Population-Based Cancer Registration in Low- or Middle-Income Settings (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/33502836/
   Guidance on developing population-based cancer registries in LMICs, relevant to the Maldives' DHIS2-based approach.

7. **International Association of Cancer Registries (IACR)** — http://www.iacr.com.fr/
   IACR homepage with standards and guidance for cancer registration used to inform the Maldives DHIS2 cancer registry design.


## Mali — Cancer & DHIS2 Profile

### Summary
Mali has extensively deployed DHIS2 as its national health information system (SNISS), transforming its health data management since 2016 to track disease trends, manage resources, and plan strategically. HISP Mali is an active partner in the francophone DHIS2 community, hosting training events. However, there is no evidence of cancer-specific data collection or cancer registry functionality within Mali's DHIS2 system. Mali's cancer burden is significant but cancer registration and surveillance infrastructure remain underdeveloped.

DHIS2 USE: NO EVIDENCE
DHIS2 (SNISS) is Mali's national HMIS with strong implementation for general health data. However, no cancer-specific DHIS2 modules, cancer registries, or cancer screening data collection through DHIS2 were identified.

### Search Results

#### French query results
1. **L'expérience du Mali dans le déploiement du DHIS2 (MEASURE Evaluation)** — https://www.measureevaluation.org/resources/publications/tr-20-407-fr.html
   Comprehensive documentation of Mali's DHIS2 deployment experience.

2. **The Story of DHIS2 in Mali: Toward a Fully Integrated HIS (ResearchGate)** — https://www.researchgate.net/publication/350768772_The_Story_of_DHIS2_in_Mali_Toward_a_Fully_Integrated_Health_Information_System
   Research paper documenting Mali's journey toward a fully integrated health information system using DHIS2.

#### English query results
3. **A roadmap for using DHIS2 data to track health indicators in the Global South (BMC Public Health)** — https://bmcpublichealth.biomedcentral.com/articles/10.1186/s12889-023-15979-z
   Study including Mali among sub-Saharan African countries using DHIS2 for health indicator tracking.

4. **Achievements and challenges of the DHIS2 system in Mali** — https://www.amazon.com/Achievements-challenges-DHIS2-system-Mali/dp/6205605619
   Publication examining achievements and challenges of epidemiological surveillance through DHIS2 in Mali.

5. **HISP Highlights September 2025** — Reference to HISP Mali hosting a French-language DHIS2 Data Visualization Academy in Bamako with participants from across Africa.


## Mauritania — Cancer & DHIS2 Profile

### Summary
Mauritania is a French-speaking West African country with limited cancer infrastructure. The National Oncology Centre was established in 2008 with IAEA support, and the country has an estimated 1,800+ new cancer cases annually. Mauritania has worked with IAEA PACT (Programme of Action for Cancer Therapy) to address its national cancer burden. The establishment of a national cancer registry was identified as a crucial first step in cancer control. Mauritania is not an IACR member but falls within the GICR sub-Saharan Africa Hub. No evidence of cancer-specific digital health tools or DHIS2 cancer integration was found.

DHIS2 USE: NO EVIDENCE
No documentation was found indicating DHIS2 is used for cancer registration, screening, or surveillance in Mauritania. The country's DHIS2 implementation status is unclear, and no cancer-specific digital tools are documented.

### Search Results

#### French query results
No specific French-language results were found documenting cancer-DHIS2 integration in Mauritania.

#### English query results
1. **Making Cancer Care a Priority: Mauritania Works With PACT to Address its National Cancer Burden (IAEA)** — https://www.iaea.org/newscenter/news/making-cancer-care-a-priority-mauritania-works-with-pact-to-address-its-national-cancer-burden
   IAEA article on Mauritania's partnership with PACT to strengthen cancer care, including establishing the National Oncology Centre in 2008.

2. **Cancer Registration in the Middle East, North Africa, and Turkey: Scope and Challenges (JCO Global Oncology)** — https://ascopubs.org/doi/10.1200/GO.21.00065
   Review of cancer registration challenges across MENA countries including Mauritania, noting gaps in population-based cancer registry coverage.

3. **Africa to Intensify Cancer Control through Cancer Registries (WHO AFRO)** — https://www.afro.who.int/news/africa-intensify-cancer-control-through-cancer-registries
   WHO Regional Office for Africa article on strengthening cancer control through cancer registries across the African region.

4. **Mauritania — GLOBOCAN Factsheet (IARC)** — https://gco.iarc.who.int/media/globocan/factsheets/populations/478-mauritania-fact-sheet.pdf
   IARC GLOBOCAN fact sheet providing cancer incidence and mortality estimates for Mauritania.


## Mauritius — Cancer & DHIS2 Profile

### Summary
Mauritius has an established National Cancer Registry (MNCR) that has been operating for many years and was included in the 12th edition of Cancer Incidence in Five Continents (2024). Since July 2024, the MNCR adopted an active data collection approach with staff directly abstracting cancer data from medical records at the National Cancer Centre in Solferino. All data are processed and stored using CanReg5 software. Mauritius has no evidence of DHIS2 being used for cancer registration or for general health information management.

DHIS2 USE: NO EVIDENCE
Mauritius operates its National Cancer Registry using CanReg5 software with established active data collection procedures. No evidence was found of DHIS2 being used for cancer registration or as the general health information management system. The cancer registry operates independently with its own data infrastructure.

### Search Results

#### English query results
1. **Mauritius National Cancer Registry (AFCRN)** — https://afcrn.org/index.php/membership/membership-list/103-mauritius
   African Cancer Registry Network profile of the Mauritius National Cancer Registry.

2. **Report of the National Cancer Registry — Cancer in Mauritius 2023 (Ministry of Health)** — https://health.govmu.org/health/wp-content/uploads/2025/02/Report-of-the-National-Cancer-Registry-2023.pdf
   Official 2023 cancer registry report published January 2025, documenting cancer incidence data processed through CanReg5.

3. **Ministry of Health and Wellness MNCR 2022 Report** — https://health.govmu.org/health/wp-content/uploads/2024/02/MNCR-2022-REPORT-final-with-cover-updated-3.pdf
   Mauritius National Cancer Registry 2022 annual report with cancer statistics and African Cancer Registry Network affiliation.

4. **A Roadmap for Digital Health Transformation in Mauritius (UNDP)** — https://digitalhealthfordevelopment.undp.org/upload/documents/Roadmap_for_Digital_Health_Transformation_in_Mauritius.pdf
   UNDP roadmap for digital health transformation in Mauritius, outlining the broader digital health strategy context.

5. **Mauritius WHO Country Profile** — https://www.who.int/about/accountability/results/who-results-report-2024-2025/region-AFRO/2024/mauritius
   WHO results report country profile for Mauritius within the AFRO region for 2024-2025.


## Mongolia — Cancer & DHIS2 Profile

### Summary
Mongolia has a national cancer registry that has been operational since the early 1960s, initially as a hospital-based registry and later expanding to population-wide coverage. The National Cancer Control Program 2007-2017 was developed to reduce cancer risk and improve cancer control. Mongolia is one of 12 countries participating in the WHO Global Platform for Childhood Cancer Medicines, which reports through DHIS2. The country started reporting childhood cancer indicators through DHIS2 in 2025, representing an early but significant step in digital cancer data management. The National Cancer Registry operates with IARC/GICR support to improve data quality in accordance with international standards.

DHIS2 USE: LOW
Mongolia has begun reporting childhood cancer medicine data through DHIS2 as part of the WHO Global Platform for Childhood Cancer Medicines (starting 2025), but broader cancer registry functions remain in separate systems.

### Search Results

#### English query results
1. **National Cancer Control Program 2007-2017 Mongolia (ICCP Portal)** — https://www.iccp-portal.org/sites/default/files/plans/NCCP%20Mongolia%202007-2017.pdf
   Mongolia's national cancer control program document outlining strategies for cancer prevention, early detection, and treatment.

2. **Cancer incidence and cancer control in Mongolia: Results from the National Cancer Registry 2008-12 (International Journal of Cancer)** — https://onlinelibrary.wiley.com/doi/full/10.1002/ijc.30463
   Comprehensive analysis of cancer incidence and control measures using data from Mongolia's National Cancer Registry from 2008 to 2012.

3. **Cancer incidence and cancer control in Mongolia (IARC)** — https://www.iarc.who.int/news-events/cancer-incidence-and-cancer-control-in-mongolia/
   IARC news item on cancer incidence trends and control efforts in Mongolia.

4. **The public health challenge of liver cancer in Mongolia (Lancet Gastroenterology & Hepatology)** — https://www.thelancet.com/journals/langas/article/PIIS2468-1253(18)30243-7/abstract
   Lancet article examining the significant burden of liver cancer in Mongolia and public health responses.

5. **Mongolia — GLOBOCAN Factsheet (IARC)** — https://gco.iarc.who.int/media/globocan/factsheets/populations/496-mongolia-fact-sheet.pdf
   IARC GLOBOCAN fact sheet providing cancer incidence and mortality estimates for Mongolia.


## Morocco — Cancer & DHIS2 Profile

### Summary
Morocco has established population-based cancer registries in Casablanca (covering ~12% of the population) and Rabat (~2.1%), supported by the Lalla Salma Foundation. The country has a comprehensive National Cancer Control Plan and significant oncology infrastructure. However, there is no evidence of DHIS2 being used for cancer registration or surveillance in Morocco. The cancer registries use traditional data collection methods with registry staff visiting public and private facilities.

DHIS2 USE: NO EVIDENCE
No search results linked DHIS2 to cancer data collection or management in Morocco. The population-based cancer registries in Casablanca and Rabat operate through conventional cancer registration systems.

### Search Results

#### English query results
1. **Cancer incidence in Morocco: report from Casablanca registry 2005-2007 (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/24570792/
   Peer-reviewed report from the Casablanca population-based cancer registry.

2. **Cancer in Morocco: Access to Innovative Treatments and Research Status (ASCO Post, 2021)** — https://ascopost.com/issues/june-25-2021/cancer-in-morocco-access-to-innovative-treatments-and-research-status/
   Overview of cancer burden, treatment access, and research landscape in Morocco.

3. **Incidence Trends of Cancer in Morocco: The Tale of the Oncological Center of Marrakech over 8 Years (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8901288/
   Hospital-based cancer incidence data from Marrakech covering 8 years.

4. **General Oncology Care in Morocco (Springer)** — https://link.springer.com/chapter/10.1007/978-981-16-7945-2_11
   Overview of oncology care infrastructure and cancer burden in Morocco.

5. **Morocco GLOBOCAN Factsheet (IARC)** — https://gco.iarc.who.int/media/globocan/factsheets/populations/504-morocco-fact-sheet.pdf
   IARC cancer statistics and projections for Morocco.


## Mozambique — Cancer & DHIS2 Profile

### Summary
Mozambique uses DHIS2 as its national health information system, branded locally as SISMA (Sistema de Informação de Saúde para Monitoria e Avaliação). SISMA is extensively deployed for HIV, TB, malaria, supply chain management, and climate-health data integration, supported by USAID, Jembi Health Systems, and the University Eduardo Mondlane. While SISMA/DHIS2 is a comprehensive platform, there is no specific evidence of cancer data collection, cancer registry functionality, or cancer screening surveillance through the system.

DHIS2 USE: NO EVIDENCE
SISMA (DHIS2) is the well-established national HMIS in Mozambique, but no cancer-specific modules, cancer registries, or cancer screening data were found within the platform. Cancer data management appears to operate outside of SISMA.

### Search Results

#### English query results
1. **SISMA — Ministério da Saúde (Mozambique MoH)** — https://sisma.misau.gov.mz/
   Official SISMA health information platform of Mozambique's Ministry of Health.

2. **HISP network to support 7 African countries with DHIS2 Climate & Health tools** — Reference to Mozambique as one of seven countries receiving DHIS2 climate and health data integration support.

3. **Mozambique Primary Health Care Strengthening Program (World Bank)** — https://documents1.worldbank.org/curated/en/443651513005902836/pdf/MOZAMBIQUE-HEALTH-PAD-12012017.pdf
   World Bank project supporting primary health care strengthening including health information systems.

4. **Saúde Digital — Moçambique (Digital & Health)** — https://digitalehealth.com/en/saude-digital/
   Overview of digital health landscape in Mozambique including DHIS2/SISMA.


## Myanmar — Cancer & DHIS2 Profile

### Summary
Myanmar has DHIS2 deployed for its Health Management Information System (HMIS) and operates cancer registries in Yangon and Nay Pyi Taw. The Yangon Cancer Registry has been strengthened since 2018 through collaboration with Vital Strategies, while the Nay Pyi Taw Population-Based Cancer Registry collects data using CanReg5. The Myanmar National Comprehensive Cancer Control Plan 2017-2021 outlines the national strategy, and Myanmar was selected for the UN Global Joint Programme for cervical cancer prevention and control since 2017. Cancer registration functions operate separately from the DHIS2 platform, with no documented integration between the two systems.

DHIS2 USE: NO EVIDENCE
No evidence of cancer-specific DHIS2 modules or integration between Myanmar's cancer registries (Yangon, Nay Pyi Taw) and DHIS2. Cancer registries use CanReg5 for data management.

### Search Results

#### English query results
1. **Cancer Incidence and Mortality in Central Myanmar: Report of Nay Pyi Taw Population-Based Cancer Registry (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9258644/
   Report on cancer incidence and mortality data from the Nay Pyi Taw Population-Based Cancer Registry.

2. **Myanmar National Comprehensive Cancer Control Plan 2017-2021 (ICCP Portal)** — https://www.iccp-portal.org/sites/default/files/plans/MMR_B5_NCCP_15_Jul_2016%20total-2%20MK_full.pdf
   Myanmar's national comprehensive cancer control plan outlining prevention, early detection, treatment, and palliative care strategies.

3. **Myanmar's Renewed Focus on National Cancer Care and Control Services (IAEA)** — https://www.iaea.org/newscenter/news/myanmars-renewed-focus-on-national-cancer-care-and-control-services
   IAEA article on Myanmar's efforts to strengthen national cancer care and control services.

4. **Current Status of Cervical Cancer Prevention and Screening in Myanmar (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9966159/
   Peer-reviewed study on the current status of cervical cancer prevention and screening programs in Myanmar.

5. **Myanmar — GLOBOCAN Factsheet (IARC)** — https://gco.iarc.who.int/media/globocan/factsheets/populations/104-myanmar-fact-sheet.pdf
   IARC GLOBOCAN fact sheet providing cancer incidence and mortality estimates for Myanmar.


## Namibia — Cancer & DHIS2 Profile

### Summary
Namibia uses DHIS2 for its national Health Management Information System. The Namibian National Cancer Registry (NNCR) was established in 1995 as a population-based national cancer registry covering the entire population. The registry is based at the Cancer Association of Namibia in Windhoek, with data collection primarily at the Dr A.B. May Cancer Care Centre of Windhoek Central Hospital. Data management historically used CanReg4 software. The Ministry of Health and Social Services has developed a National Cancer Prevention and Control Plan with WHO support. There are no cancer-specific DHIS2 modules documented for the country, and cancer data management operates outside of the DHIS2 platform.

DHIS2 USE: NO EVIDENCE
No evidence of cancer-specific DHIS2 modules or cancer registry integration with DHIS2 in Namibia. The NNCR uses CanReg software for cancer data management.

### Search Results

#### English query results
1. **Cancer Registry — Cancer Association of Namibia** — https://www.can.org.na/?page_id=493
   Official page of the Namibian National Cancer Registry operated by the Cancer Association of Namibia since 1995.

2. **Namibian National Cancer Registry (AFCRN)** — https://afcrn.org/index.php/membership/membership-list/125-ncr
   African Cancer Registry Network membership page for the Namibian National Cancer Registry.

3. **Cancer Prevention and Control in Namibia (WHO AFRO)** — https://www.afro.who.int/news/cancer-prevention-and-control-namibia
   WHO Regional Office for Africa article on cancer prevention and control efforts in Namibia.

4. **Cancer Association of Namibia (UICC)** — https://www.uicc.org/membership/cancer-association-namibia
   UICC membership page for the Cancer Association of Namibia describing cancer control activities.

5. **Namibia — GLOBOCAN Factsheet (IARC)** — https://gco.iarc.who.int/media/globocan/factsheets/populations/516-namibia-fact-sheet.pdf
   IARC GLOBOCAN fact sheet providing cancer incidence and mortality estimates for Namibia.


## Nepal — Cancer & DHIS2 Profile

### Summary
Nepal has a Population-Based Cancer Registry (PBCR) operated by the Nepal Health Research Council (NHRC) since 2018, covering 9 out of 77 districts across urban, suburban, and rural regions using CanReg5 software. DHIS2 is Nepal's national health information system and is being localized with Nepali translation. In 2025, Nepal began reporting childhood cancer medicine indicators through DHIS2 as part of the WHO Global Platform for Access to Childhood Cancer Medicines. The IARC-HISP Centre collaboration is actively developing DHIS2-based cancer registration tools that could extend to Nepal.

DHIS2 USE: LOW
Nepal has started reporting childhood cancer indicators through DHIS2 as a participating country in the WHO Global Platform (2025), and DHIS2 localization efforts are underway. However, the established Population-Based Cancer Registry uses CanReg5 separately from DHIS2, and broader cancer data integration into DHIS2 has not been documented.

### Search Results

#### English query results
1. **Cancer Registration in Nepal: Current Status and Way Forward (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8827594/
   Peer-reviewed assessment of Nepal's cancer registration status and future directions.

2. **Population Based Cancer Registry in Nepal (NHRC)** — https://nhrc.gov.np/wp-content/uploads/2019/04/Progress-Interim-_cancer.pdf
   Progress report on the Nepal Health Research Council's population-based cancer registry.

3. **Cancer Risk in Nepal: Analysis from Population-Based Cancer Registry (Wiley, 2024)** — https://onlinelibrary.wiley.com/doi/abs/10.1155/2024/4687221
   Recent analysis of cancer risk across urban, suburban, and rural regions using PBCR data.

4. **DHIS2 Software Operational Guideline Nepal** — https://thecompassforsbc.org/sbcc-tools/dhis2-software-operational-guideline-nepal
   Operational guidelines for DHIS2 use in Nepal's health system.


## Nicaragua — Cancer & DHIS2 Profile

### Summary
Nicaragua has one of the highest cervical cancer mortality rates in the Americas (~400 deaths/year) and plans to launch a digital database of cancer patients. IAEA supports cancer care capacity. While DHIS2 is used globally for cancer data, Nicaragua is not among the 12 countries currently using DHIS2 for childhood cancer indicators, and no evidence of DHIS2 for cancer was found.

DHIS2 USE: NO EVIDENCE

### Search Results

#### English query results
1. **Cancer Care in Nicaragua (IAEA)** — https://www.iaea.org/newscenter/multimedia/photoessays/cancer-care-nicaragua
   IAEA photo essay documenting cancer care challenges and support in Nicaragua.

2. **HPV-based cervical cancer screening in Nicaragua (BMC Public Health)** — https://bmcpublichealth.biomedcentral.com/articles/10.1186/s12889-020-08601-z
   Study on HPV-based cervical cancer screening from testing to treatment in Nicaragua.

3. **Nicaraguan Experts Fight Cancer in Women and Children with IAEA Support** — https://www.iaea.org/newscenter/news/nicaraguan-experts-fight-cancer-in-women-and-children-with-support-from-the-iaea
   IAEA support for cancer programs targeting women and children.

4. **Nicaragua Cancer Profile (HPV Centre)** — https://hpvcentre.net/statistics/reports/NIC_FS.pdf
   HPV and cancer statistics for Nicaragua.

5. **Nicaragua (Cancer Atlas)** — https://canceratlas.cancer.org/data-item/12f9a103dccd32ade469b92ce0af247a/
   Cancer Atlas data for Nicaragua.


## Niger — Cancer & DHIS2 Profile

### Summary
Niger deployed DHIS2 as its national health information system (SNIS) in 2017, with CHISU supporting HIS strengthening particularly for maternal and child health, family planning, and immunization in Maradi and Zinder regions. Niger is also among seven African countries receiving DHIS2 climate and health data integration support. However, quality data collection remains problematic, and there is no evidence of cancer-specific data collection, cancer registry functionality, or cancer screening surveillance through Niger's DHIS2 system.

DHIS2 USE: NO EVIDENCE
DHIS2 is Niger's national HMIS since 2017, but no cancer-specific modules, registries, or screening data collection through DHIS2 were identified. Cancer infrastructure in Niger remains limited.

### Search Results

#### English query results
1. **Niger SNIS DHIS2** — https://dhisniger.ne/
   Official national health information system portal for Niger running on DHIS2.

2. **Niger Country HMIS (DHIS2) — Digital Impact Exchange** — https://exchange.dial.global/projects/niger-country-hmis-dhis2
   Overview of Niger's DHIS2-based national health management information system.

3. **Niger (CHISU)** — https://chisuprogram.org/where-we-work/niger
   CHISU program supporting Niger's HIS strengthening, DHIS2 use for MCH, FP, and immunization.

4. **Formations DHIS2 — SNIS Niger** — https://snis.ne/formations-dhis2/
   Training resources for DHIS2 under Niger's national health information system.

5. **HISP network to support 7 African countries with DHIS2 Climate & Health tools** — Reference to Niger receiving DHIS2 climate and health data integration support through the Global Fund catalytic fund.


## Nigeria — Cancer & DHIS2 Profile

### Summary
Nigeria has DHIS2 deployed nationally since 2013 as the NHMIS data management tool for aggregating health facility statistics. The country has two population-based cancer registries (Ibadan Cancer Registry since 1960, Abuja Cancer Registry since 2006) and the Nigerian National Systems of Cancer Registries (NSCR) established in 2009. However, cancer registries operate through separate systems and there is no documented integration between cancer registration and DHIS2.

DHIS2 USE: LOW
DHIS2 has been the national HMIS data management tool since 2013, but cancer registries in Ibadan and Abuja operate independently through their own systems without documented DHIS2 integration.

### Search Results

#### English query results
1. **Population-based cancer registries in Nigeria and the National Cancer Control Programme (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10550292/
   Peer-reviewed article on Nigeria's population-based cancer registries and the National Cancer Control Programme structure.

2. **Cancer Incidence in Nigeria: A Report from Population-based Cancer Registries (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC3438369/
   Report on cancer incidence data from Nigerian population-based cancer registries.

3. **Developing National Cancer Registration in Developing Countries — Case Study of Nigeria (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC4519655/
   Case study on developing national cancer registration systems in Nigeria.

4. **Nigeria imPACT Review Report (IAEA/NICRAT)** — https://www.nicrat.gov.ng/wp-content/uploads/2023/08/Nigeria-imPACT-Review-Report.pdf
   IAEA cancer control capacity and needs assessment report for Nigeria.

5. **Nigeria — GLOBOCAN Factsheet (IARC)** — https://gco.iarc.who.int/media/globocan/factsheets/populations/566-nigeria-fact-sheet.pdf
   IARC GLOBOCAN fact sheet providing cancer incidence and mortality estimates for Nigeria.


## Norway — Cancer & DHIS2 Profile

### Summary
Norway has the Cancer Registry of Norway, established in 1952 and now part of the Norwegian Institute of Public Health (NIPH) since May 2025. The registry collects comprehensive population-based cancer data from multiple sources including clinical notifications via the CRN electronic reporting service (KREMT), pathology reports, radiotherapy data, and hospital cancer medication systems. The Cancer Registry also administers Norway's public cancer screening programmes. While DHIS2 was developed in Norway at the University of Oslo (UiO), it is not used domestically for health information management. Cancer registration operates through Norway's own advanced national systems entirely separate from DHIS2.

DHIS2 USE: NO EVIDENCE
DHIS2 was developed at UiO in Norway but is not used domestically. Norway's Cancer Registry has its own advanced IT infrastructure established over decades.

### Search Results

#### English query results
1. **Cancer Registry of Norway** — https://www.kreftregisteret.no/en/
   Norway's national cancer registry, established in 1952, maintaining comprehensive population-based cancer data and administering screening programmes.

2. **Cancer / Cancer Registry of Norway (NIPH)** — https://www.fhi.no/en/cancer/
   Norwegian Institute of Public Health cancer pages covering the Cancer Registry and its integration into NIPH from May 2025.

3. **About the Cancer Registry of Norway (NIPH)** — https://www.fhi.no/en/cancer/cancer-registry-norway/About-us/
   Detailed information about the Cancer Registry's data sources, reporting systems, and organizational structure.

4. **Cancer Registry of Norway (European Health Information Portal)** — https://www.healthinformationportal.eu/health-information-sources/cancer-registry-norway
   European Health Information Portal entry for the Cancer Registry of Norway describing scope and data collections.

5. **Norway Cancer Registry (GHDx)** — https://ghdx.healthdata.org/series/norway-cancer-registry
   Global Health Data Exchange series entry for Norway's cancer registry data.


## Pakistan — Cancer & DHIS2 Profile

### Summary
Pakistan lacks a unified national cancer registry but has 17 institutional and regional cancer registries across only 19 of 129 cities. Data collection methods range from paper-based forms to advanced software. DHIS2 is used in Pakistan for the National TB Control Program's TB Tracker, and Pakistan is one of 12 participating countries in the WHO Global Platform for Access to Childhood Cancer Medicines reporting through DHIS2. However, broader cancer registration does not use DHIS2.

DHIS2 USE: LOW
Pakistan reports childhood cancer medicine indicators through DHIS2 as part of the WHO Global Platform (2025), and DHIS2 is established for TB surveillance. However, the fragmented cancer registry landscape uses separate systems, and there is an urgent need for a unified national cancer registry.

### Search Results

#### English query results
1. **Cancer registries in Pakistan: a scoping review (Lancet Regional Health, 2025)** — https://www.thelancet.com/journals/lansea/article/PIIS2772-3682(25)00086-1/fulltext
   Comprehensive scoping review identifying 17 cancer registries with varying scope across Pakistan.

2. **Cancer Statistics in Pakistan From 1994 to 2021: Data From Cancer Registry (JCO)** — https://ascopubs.org/doi/10.1200/CCI.22.00142
   Analysis of cancer statistics from Pakistani cancer registries over 27 years.

3. **National Cancer Registry of Pakistan: First Comprehensive Report 2015-2019 (JCPSP)** — https://jcpsp.pk/article-detail/pnational-cancer-registry-of-pakistan-first-comprehensive-report-of-cancer-statistics-20152019orp
   First comprehensive national cancer registry report for Pakistan.

4. **Need For A National Cancer Registry In Pakistan: Challenges And Way Forward (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/37469061/
   Analysis of challenges in establishing a national cancer registry in Pakistan.

5. **Common Cancers in Karachi: 2010-2019 Cancer Data from the Dow Cancer Registry (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7674861/
   Cancer epidemiology data from the Dow Cancer Registry in Karachi.


## Palestine — Cancer & DHIS2 Profile

### Summary
Palestine has DHIS2 deployed across 88 clinics for electronic Family Health Records (including NCD services) and 14 districts for routine HMIS statistics and case-based communicable disease surveillance. In June 2025, Palestine launched an initiative to unify maternal, child, and family health records under a single DHIS2 platform with HISP UiO and WHO support. The Palestinian Cancer Registry (PCR) exists under the Ministry of Health, but cancer is the second leading cause of mortality with a 65%+ rise expected by 2030. No evidence of cancer data being managed through DHIS2.

DHIS2 USE: NO EVIDENCE
DHIS2 is operational in Palestine for family health records and disease surveillance, but the Palestinian Cancer Registry operates separately. No cancer-specific DHIS2 modules were identified.

### Search Results

#### English query results
1. **Palestine to Integrate Maternal, Child and Family Health Systems with DHIS2** — Reference to June 2025 launch of unified DHIS2 platform with HISP UiO/WHO support.

2. **Cancer status in the Occupied Palestinian Territories (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9645346/
   Overview of cancer types, incidence, mortality, and distribution in Palestine.

3. **Roadblocks to Cancer Care in the Occupied Palestinian Territories (HHR Journal)** — https://www.hhrjournal.org/2024/10/28/roadblocks-to-cancer-care-in-the-occupied-palestinian-territories/
   Analysis of barriers to cancer care access in Palestine.

4. **Promoting cancer prevention and early diagnosis in Palestine (ScienceDirect)** — https://www.sciencedirect.com/science/article/pii/S2213538322000522
   Research on cancer prevention and early diagnosis strategies.

5. **General Oncology Care in Palestine (Springer)** — https://link.springer.com/chapter/10.1007/978-981-16-7945-2_13
   Overview of oncology care infrastructure and cancer burden in Palestine.


## Panama — Cancer & DHIS2 Profile

### Summary
Panama has a well-established National Cancer Registry (NCRP) since 1974, operating as a population-based registry since 2012 with web-based reporting software covering 77% of health institutions. No evidence of DHIS2 being used for cancer registration was found; the NCRP uses its own web-based system.

DHIS2 USE: NO EVIDENCE

### Search Results

#### English query results
1. **History of the National Cancer Registry of Panama (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC10414200/
   History of Panama's cancer registry from its 1974 establishment to population-based status in 2012.

2. **National Cancer Registry Panama (GHDx)** — https://ghdx.healthdata.org/organizations/national-cancer-registry-panama
   Global Health Data Exchange entry for Panama's National Cancer Registry.

3. **Trend Analysis of Cancer Mortality and Incidence in Panama (Medicine)** — https://journals.lww.com/md-journal/fulltext/2015/06030/trend_analysis_of_cancer_mortality_and_incidence.20.aspx
   Joinpoint regression analysis of cancer trends in Panama.

4. **Panama GLOBOCAN Factsheet (IARC)** — Referenced in search results, IARC cancer statistics for Panama.


## Papua New Guinea — Cancer & DHIS2 Profile

### Summary
Papua New Guinea has the Papua New Guinea National Cancer Centre and a cancer registry under development. A national child cancer registry has been developed using REDcap through partnership between Port Moresby General Hospital and SIOP Oceania. PNG's new National Cancer Control Plan for 2024-2030 covers six priority strategic areas including cancer registration. The country falls within the IARC Pacific Islands Hub for Cancer Registration (covering Melanesia, Micronesia, and Polynesia). A DHIS2 community exists for PNG and DHIS2 introduction is being discussed, but cancer-specific DHIS2 use is not documented.

DHIS2 USE: NO EVIDENCE
No evidence of cancer-specific DHIS2 modules or cancer data management through DHIS2 in Papua New Guinea. Cancer registration efforts use REDcap for the child cancer registry, and DHIS2 deployment in PNG remains at an early stage.

### Search Results

#### English query results
1. **Papua New Guinea National Cancer Centre (UICC)** — https://www.uicc.org/membership/papua-new-guinea-national-cancer-centre
   UICC membership page for the PNG National Cancer Centre.

2. **Building capacity to treat childhood cancer in Papua New Guinea (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC11825119/
   Study on developing childhood cancer treatment capacity and the national child cancer registry using REDcap at Port Moresby General Hospital.

3. **WHO Supports the Fight Against Cancer in Papua New Guinea (WHO)** — https://www.who.int/papuanewguinea/news/detail/19-08-2024-who-supports-the-fight-against-cancer-in-papua-new-guinea
   WHO support for PNG's National Cancer Control Plan 2024-2030 covering prevention, screening, treatment, data, and governance.

4. **Cancer control in the Pacific: big challenges facing small island states (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7746436/
   Review of cancer control challenges in Pacific island states including PNG, covering registry development and resource constraints.

5. **Papua New Guinea — GLOBOCAN Factsheet (IARC)** — https://gco.iarc.who.int/media/globocan/factsheets/populations/598-papua-new-guinea-fact-sheet.pdf
   IARC GLOBOCAN fact sheet providing cancer incidence and mortality estimates for PNG.


## Paraguay — Cancer & DHIS2 Profile

### Summary
Paraguay uses DHIS2 through the VPD-SMART platform developed by PAHO for vaccine-preventable disease surveillance, achieving 97% data consistency (up from 54%) and moving from weekly to daily reporting. PAHO has also integrated GIS capabilities with DHIS2 in Paraguay for geospatial health analysis. However, there is no evidence of DHIS2 being used for cancer registration or surveillance. The IAEA supports cancer screening and treatment capacity building in the country.

DHIS2 USE: NO EVIDENCE
DHIS2 (VPD-SMART) is used in Paraguay for vaccine-preventable disease surveillance and GIS integration, but no cancer-specific DHIS2 modules or cancer data management through DHIS2 were identified.

### Search Results

#### English query results
1. **DHIS2's impact on vaccine-preventable disease surveillance in Paraguay** — Referenced in search results documenting Paraguay's transition to VPD-SMART DHIS2 platform.

2. **Better Screening and Treatment to Tackle Cancer in Paraguay (IAEA)** — https://www.iaea.org/bulletin/better-screening-and-treatment-to-tackle-cancer-in-paraguay
   IAEA support for cancer screening and treatment capacity in Paraguay.

3. **PAHO Maps Paraguay's Medical Needs (Esri)** — https://www.esri.com/about/newsroom/blog/pan-american-health-organization-maps-paraguays-medical-needs
   PAHO GIS integration with DHIS2 for health data visualization in Paraguay.

4. **Paraguay GLOBOCAN Factsheet (IARC)** — https://gco.iarc.who.int/media/globocan/factsheets/populations/600-paraguay-fact-sheet.pdf
   IARC cancer statistics and projections for Paraguay.


## Philippines — Cancer & DHIS2 Profile

### Summary
The Philippines has the Philippine Cancer Control Program (established 1988) and population-based cancer registries in Manila, Cebu, and Davao operated by the Philippine Cancer Society. In 2019, the National Integrated Cancer Control Act (NICCA, Republic Act 11215) was signed, creating the Philippine Cancer Center and mandating hospital-based cancer registries as a prerequisite for hospital licensing. Cancer will become a notifiable disease under this Act. The Philippines uses DHIS2 for some health programs, but cancer registration remains separate from the DHIS2 platform, with no documented integration between the two systems.

DHIS2 USE: NO EVIDENCE
No evidence of cancer-specific DHIS2 modules or integration between Philippine Cancer Society registries and DHIS2. Cancer registration operates through dedicated registry systems.

### Search Results

#### English query results
1. **Philippine Cancer Control Program — Department of Health** — https://doh.gov.ph/uhc/health-programs/philippine-cancer-control-program/
   Official Department of Health page on the Philippine Cancer Control Program describing national cancer prevention and control strategies.

2. **Improving cancer care in the Philippines: The need for deliberate and careful implementation of the NICCA (Lancet Regional Health)** — https://www.thelancet.com/journals/lanwpc/article/PIIS2666-6065(22)00230-9/fulltext
   Lancet article on the implementation challenges and opportunities of the National Integrated Cancer Control Act in the Philippines.

3. **Cancer and Universal Health Coverage in the Philippines (UICC)** — https://www.uicc.org/case-studies/cancer-and-universal-health-coverage-philippines
   UICC case study on cancer control within the context of universal health coverage in the Philippines.

4. **Establishing the Philippine Cancer Center National Cancer Research Agenda 2024-2028 (JCO Global Oncology)** — https://ascopubs.org/doi/10.1200/GO-24-00613
   Article on the establishment of the Philippine Cancer Center's research agenda for 2024-2028.

5. **Philippines — GLOBOCAN Factsheet (IARC)** — https://gco.iarc.who.int/media/globocan/factsheets/populations/608-philippines-fact-sheet.pdf
   IARC GLOBOCAN fact sheet providing cancer incidence and mortality estimates for the Philippines.


## Rwanda — Cancer & DHIS2 Profile

### Summary
Rwanda is a global leader in DHIS2-based cancer registration. The Rwanda National Cancer Registry (RNCR) was re-established in 2018 by the Rwanda Biomedical Centre with NIH, Vital Strategies, IARC, and AFCRN support, and integrated into DHIS2. The DHIS2 Oncology Tracker, introduced in 2019, enables nationwide cancer surveillance with health facility focal persons entering data directly. From 2007 to 2023, 46,801 cancer cases were registered (5,548 in 2023). Data is exported from DHIS2 to CanReg5 for quality checks and analysis. Rwanda, along with Maldives and Jamaica, piloted the DHIS2 cancer registration approach.

DHIS2 USE: HIGH
Rwanda has a fully operational DHIS2 Oncology Tracker integrated with the National Cancer Registry since 2019, with nationwide cancer surveillance, direct facility-level data entry, and CanReg5 interoperability. Rwanda is a pioneer and reference implementation for DHIS2-based cancer registration globally.

### Search Results

#### English query results
1. **Rwanda National Cancer Registry (AFCRN)** — https://afcrn.org/index.php/membership/membership-list/167-rwanda-ncr
   African Cancer Registry Network profile of Rwanda's National Cancer Registry.

2. **IARC and HISP Centre collaborate to scale up standardized cancer registries with DHIS2** — Reference to Rwanda as pioneer in DHIS2 cancer registration alongside Maldives and Jamaica.

3. **HISP Rwanda Applications** — https://hisprwanda.org/dhis2-2/applications-2-2/
   HISP Rwanda's DHIS2 applications including cancer registry tools.

4. **About the GICR — E-NNOVATE project** — https://gicr.iarc.fr/news/2023/12/04/e-nnovate-iacr-write-up/
   IARC Global Initiative for Cancer Registry Development including Rwanda's experience.

5. **5th World Congress on Global Healthcare — Marc Hagenimana** — https://www.healthcare.scientexconference.com/speakers/Marc-Hagenimana-
   Presentation on Rwanda's DHIS2 cancer registry experience at international congress.


## Saint Lucia — Cancer & DHIS2 Profile

### Summary
Saint Lucia has been adopting DHIS2 for its health data framework with support from PAHO and training led by facilitators from the University of Oslo. The Environmental Health Unit currently uses DHIS2 for arbovirus and complaints modules, and non-clinical functions are being migrated to the platform. Saint Lucia is planning to put its cancer registry on the DHIS2 platform. The country has a hospital-based cancer registry activity and a national NCD plan, but no comprehensive population-based cancer surveillance capacity yet. PAHO has supported the digital health transformation since approximately 2022.

DHIS2 USE: LOW
Saint Lucia is planning to implement its cancer registry on DHIS2, indicating active engagement but not yet operational. The country has hospital-based cancer registration and is actively building DHIS2 capacity with PAHO and University of Oslo support.

### Search Results

#### English query results
1. **Health teams receive DHIS2 training in St. Lucia (PAHO/WHO)** — https://www.paho.org/en/news/5-9-2025-health-teams-receive-dhis2-training-st-lucia
   PAHO report on DHIS2 training workshops in Saint Lucia led by University of Oslo facilitators, including plans for cancer registry on DHIS2.

2. **Difficulties in Accessing Cancer Care in a Small Island State: A Community-Based Pilot Study of Cancer Survivors in Saint Lucia (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC8124473/
   Community-based study examining challenges cancer survivors face accessing care in Saint Lucia.

3. **Advancing Cancer Control Through Research and Cancer Registry Collaborations in the Caribbean (ResearchGate)** — https://www.researchgate.net/publication/287808330_Special_Report_Advancing_Cancer_Control_Through_Research_and_Cancer_Registry_Collaborations_in_the_Caribbean
   Report on advancing cancer control through registry collaborations across Caribbean countries including Saint Lucia.

4. **Saint Lucia Cancer Society** — https://stluciacancersociety.org/
   Official website of the Saint Lucia Cancer Society providing cancer assistance and support services.

5. **Saint Lucia — GLOBOCAN Factsheet (IARC)** — https://gco.iarc.who.int/media/globocan/factsheets/populations/662-saint-lucia-fact-sheet.pdf
   IARC GLOBOCAN fact sheet providing cancer incidence and mortality estimates for Saint Lucia.


## Sao Tomé and Principe — Cancer & DHIS2 Profile

### Summary
São Tomé and Príncipe adopted DHIS2 across its entire health system with WHO and UNDP support, including DHIS2 Tracker customized in 2020 for individual data reporting and patient registry across several health programs. The country also used DHIS2 for COVID-19 surveillance and vaccination tracking. Cancer control efforts include collaboration with Portuguese-speaking oncologists on cervical cancer screening and HPV vaccination, but there is no evidence of cancer-specific data collection through DHIS2.

DHIS2 USE: NO EVIDENCE
DHIS2 is deployed system-wide in São Tomé and Príncipe including Tracker for individual patient data, but no cancer-specific modules, registries, or screening data collection through DHIS2 were identified.

### Search Results

#### English query results
1. **Implementing DHIS2 to accelerate Universal Health Coverage in São Tomé and Príncipe (WHO)** — https://www.who.int/about/accountability/results/who-results-report-2020-mtr/country-story/2020/implementing-the-dhis2-to-accelerate-universal-health-coverage-in-s%C3%A3o-tom%C3%A9-and-principe
   WHO documentation of DHIS2 implementation for UHC in São Tomé and Príncipe.

2. **Cancer Control in Small Island Nations (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7746435/
   Cancer control challenges and collaborations among Portuguese-speaking small island nations.

3. **São Tomé and Príncipe UNDP CPD Evaluation** — https://www.undp.org/sites/g/files/zskgke326/files/2022-11/dpdcpstp4_evaluation_report_unpd_cpd_2017-2022.pdf
   UNDP evaluation including health system and DHIS2 support.


## Senegal — Cancer & DHIS2 Profile

### Summary
Senegal has DHIS2 as its national health information system with case-based reporting for nine priority diseases established at all levels, supported by CDC and WHO. DHIS2 is also being used for hypertension data integration through the CARDIO4Cities project in Dakar. Senegal is one of 12 participating countries in the WHO Global Platform for Access to Childhood Cancer Medicines reporting through DHIS2. Cervical cancer data collection via DHIS2 Tracker has been documented in West Africa through the SUCCESS project (in Burkina Faso and Côte d'Ivoire), suggesting a regional pathway for Senegal.

DHIS2 USE: LOW
Senegal reports childhood cancer medicine indicators through DHIS2 as part of the WHO Global Platform (2025), and DHIS2 is the established national HMIS. Cervical cancer screening data collection via DHIS2 Tracker is happening in neighboring countries through the SUCCESS project. However, no direct evidence of a cancer registry or cancer screening data in Senegal's DHIS2 was found.

### Search Results

#### English query results
1. **Integrating hypertension data into DHIS2 in Dakar (Discover Health Systems, 2025)** — https://link.springer.com/article/10.1007/s44250-025-00240-8
   Documentation of CARDIO4Cities NCD data integration into DHIS2 in Dakar.

2. **Cancer ecosystem assessment in West Africa: Ghana, Nigeria and Senegal (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7320762/
   Health systems gaps to prevent and control cancers in three West African countries.

3. **CDC in Senegal** — https://www.cdc.gov/global-health/countries/senegal.html
   CDC support for DHIS2-based disease surveillance in Senegal.

4. **Senegal HMIS (DHIS2 with Tracker) — Digital Impact Exchange** — https://exchange.dial.global/projects/senegal-senegal-hmis-dhis2-with-tracker
   Overview of Senegal's DHIS2 implementation including Tracker capabilities.


## Seychelles — Cancer & DHIS2 Profile

### Summary
Seychelles has a National Cancer Registry with trained staff and specialist consultants, documenting cancer incidence where breast cancer is the most common site among women (43% of new cases). While DHIS2 is expanding globally for cancer data integration, Seychelles is not among the 12 countries currently using DHIS2 for childhood cancer indicators, and no evidence of DHIS2 being used for cancer data management in Seychelles was found.

DHIS2 USE: NO EVIDENCE
No evidence was found of DHIS2 being used for cancer registration or surveillance in Seychelles. The National Cancer Registry operates independently.

### Search Results

#### English query results
1. **Cancer Seychelles 2020 Country Profile (WHO)** — https://www.who.int/publications/m/item/cancer-syc-2020
   WHO country profile summarizing cancer incidence, mortality, and health system capacity in Seychelles.

2. **Seychelles National Cancer Registry (AFCRN)** — https://afcrn.org/membership/members/96-seychelles
   African Cancer Registry Network profile for Seychelles' National Cancer Registry.

3. **Cancer Seychelles 2020 (WHO PDF)** — https://cdn.who.int/media/docs/default-source/country-profiles/cancer/syc-2020.pdf
   Detailed WHO cancer profile with statistics and projections.


## Sierra Leone — Cancer & DHIS2 Profile

### Summary
Sierra Leone has a complete national DHIS2 implementation, being the first country in sub-Saharan Africa to adopt DHIS2. The system was significantly strengthened in the post-Ebola period, with the electronic Integrated Disease Surveillance and Response (eIDSR) tool built on DHIS2 since 2015. Sierra Leone launched a National Cancer Registry and the Sierra Leone Cancer Charity in June 2012 to facilitate cancer data collection and early diagnosis. The country falls within the GICR sub-Saharan Africa Hub. Despite robust DHIS2 infrastructure, there is no documented integration between the National Cancer Registry and DHIS2.

DHIS2 USE: NO EVIDENCE
No evidence of cancer-specific DHIS2 modules despite Sierra Leone having a complete national DHIS2 implementation and a separately established National Cancer Registry since 2012.

### Search Results

#### English query results
1. **Sierra Leone Launches National Cancer Registry (WHO AFRO)** — https://www.afro.who.int/news/sierra-leone-launches-national-cancer-registry
   WHO report on Sierra Leone's launch of the National Cancer Registry and Sierra Leone Cancer Charity in June 2012.

2. **Assessing the implementation of an electronic Integrated Disease Surveillance and Response System using DHIS2, 2024: Sierra Leone's experience (AFENET Journal)** — https://www.afenet-journal.net/content/series/8/2/5/full/
   Assessment of Sierra Leone's DHIS2-based eIDSR implementation for disease surveillance.

3. **Becoming Infrastructure: A Critical Realist Account of the Evolution of DHIS2 as Digital Public Health Infrastructure in Sierra Leone (ACM)** — https://dl.acm.org/doi/10.1145/3757400
   Academic study on DHIS2's evolution as public health infrastructure in Sierra Leone, the first sub-Saharan African country to adopt DHIS2.

4. **Spreading the cancer prevention message in Sierra Leone (ecancer)** — https://ecancer.org/en/news/3892-spreading-the-cancer-prevention-message-in-sierra-leone
   Article on cancer prevention awareness and advocacy efforts in Sierra Leone.

5. **Sierra Leone — GLOBOCAN Factsheet (IARC)** — https://gco.iarc.who.int/media/globocan/factsheets/populations/694-sierra-leone-fact-sheet.pdf
   IARC GLOBOCAN fact sheet providing cancer incidence and mortality estimates for Sierra Leone.


## Solomon Islands — Cancer & DHIS2 Profile

### Summary
Solomon Islands is a small Pacific island nation that uses DHIS2 for basic HMIS functions. The country has a hospital-based cancer registry at the National Referral Hospital, staffed by a medical registrar and two nurses, though it suffers from incomplete data collection due to its passive nature and limited resources. Breast and cervical cancer are the most commonly identified cancers. The IARC Pacific Islands Hub for Cancer Registration covers Solomon Islands as part of the Melanesia sub-region. There is no cancer-specific DHIS2 use documented.

DHIS2 USE: NO EVIDENCE
No evidence of cancer-specific DHIS2 use in Solomon Islands. The country has a hospital-based cancer registry at the National Referral Hospital operating separately from DHIS2, with limited resources for cancer data management.

### Search Results

#### English query results
1. **Cancer in the Solomon Islands (PubMed)** — https://pubmed.ncbi.nlm.nih.gov/29120823/
   Study on cancer incidence and the hospital-based cancer registry at the National Referral Hospital in Solomon Islands.

2. **Cancer control in the Pacific: big challenges facing small island states (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7746436/
   Review of cancer control challenges in Pacific island states including Solomon Islands, covering registry development and resource constraints.

3. **IARC Pacific Islands Hub for Cancer Registration (GICR)** — https://gicr.iarc.fr/hub/pacific-islands/
   IARC Global Initiative for Cancer Registry Development Pacific Islands Hub covering Solomon Islands in the Melanesia sub-region.

4. **Cancer Epidemiology in the Pacific Islands — Past, Present and Future (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC4386924/
   Epidemiological review of cancer trends across Pacific island nations including Solomon Islands.

5. **Solomon Islands — GLOBOCAN Factsheet (IARC)** — https://gco.iarc.who.int/media/globocan/factsheets/populations/090-solomon-islands-fact-sheet.pdf
   IARC GLOBOCAN fact sheet providing cancer incidence and mortality estimates for Solomon Islands.


## Somalia — Cancer & DHIS2 Profile

### Summary
Somalia has DHIS2 deployment supported by UNICEF and HISP UiO, with a three-year agreement (from 2023) to strengthen the DHIS2-based health management information system through enhanced technical support, local capacity building, and improved data quality. DHIS2 Tracker is used for disease surveillance, HIV monitoring, and immunization records. There are no formal cancer registries in Somalia, though a 2020 study from a tertiary care hospital in Mogadishu reported on 1,306 cancer cases from 2017-2020. Somalia is not an IACR member. Due to limited cancer infrastructure, there is no cancer-specific DHIS2 use.

DHIS2 USE: NO EVIDENCE
No evidence of cancer-specific DHIS2 modules in Somalia. No formal cancer registries exist, and DHIS2 deployment focuses on disease surveillance, HIV, and immunization. Very limited cancer infrastructure due to protracted conflict.

### Search Results

#### English query results
1. **Cancer Incidence and Distribution at a Tertiary Care Hospital in Somalia from 2017 to 2020: An Initial Report of 1306 Cases (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7534047/
   Hospital-based cancer incidence report from a tertiary care facility in Mogadishu, Somalia.

2. **Cancer Registration in the Middle East, North Africa, and Turkey: Scope and Challenges (JCO Global Oncology)** — https://ascopubs.org/doi/10.1200/GO.21.00065
   Review of cancer registration challenges across MENA countries noting Somalia lacks formal cancer registry systems.

3. **The distribution of cancer cases in Somalia (ResearchGate)** — https://www.researchgate.net/publication/320475788_The_distribution_of_cancer_cases_in_Somalia
   Research on the distribution and patterns of cancer cases in Somalia.

4. **Somalia — GLOBOCAN Factsheet (IARC)** — https://gco.iarc.who.int/media/globocan/factsheets/populations/706-somalia-fact-sheet.pdf
   IARC GLOBOCAN fact sheet providing cancer incidence and mortality estimates for Somalia.


## South Africa — Cancer & DHIS2 Profile

### Summary
South Africa has DHIS2 deployed nationally as the District Health Information System (WebDHIS). The country also operates the National Cancer Registry (NCR) under the National Health Laboratory Service (NHLS), conducting pathology-based cancer surveillance since 1986. The Ekurhuleni Population Based Cancer Registry (EPBCR) was established in 2017 as a gold-standard population-based registry. Cancer registries operate through their own dedicated systems rather than through DHIS2.

DHIS2 USE: LOW
South Africa has extensive national DHIS2 infrastructure (WebDHIS) and a well-established National Cancer Registry, but the NCR operates through its own systems under the NHLS rather than through DHIS2.

### Search Results

#### English query results
1. **National Cancer Registry (NICD/NHLS)** — https://www.nicd.ac.za/centres/national-cancer-registry/
   South Africa's National Cancer Registry operated through the National Institute for Communicable Diseases within the NHLS, conducting pathology-based cancer surveillance since 1986.

2. **South African National Cancer Registry: Effect of withheld data from private health systems on cancer incidence estimates (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC4591919/
   Study examining the impact of private health system data gaps on cancer incidence estimates from the NCR.

3. **The South African National Cancer Registry: an update (Lancet Oncology)** — https://www.thelancet.com/journals/lanonc/article/PIIS1470-2045(14)70310-9/abstract
   Update on the South African NCR covering operational improvements and data quality.

4. **Cancer Statistics (NICD)** — https://www.nicd.ac.za/centres/national-cancer-registry/cancer-statistics/
   Cancer statistics from the National Cancer Registry including incidence trends and demographic analysis.

5. **South Africa — GLOBOCAN Factsheet (IARC)** — https://gco.iarc.who.int/media/globocan/factsheets/populations/710-south-africa-fact-sheet.pdf
   IARC GLOBOCAN fact sheet providing cancer incidence and mortality estimates for South Africa.


## South Sudan — Cancer & DHIS2 Profile

### Summary
South Sudan has used DHIS2 as its electronic health information management system since 2018, scaled up to all counties and selected health facilities. The Boma Health Initiative leverages DHIS2 to strengthen community health. However, South Sudan faces severe health system constraints as one of the world's most fragile states, and there is no evidence of cancer-specific data collection, cancer registries, or cancer surveillance through DHIS2.

DHIS2 USE: NO EVIDENCE
DHIS2 is the national HMIS in South Sudan since 2018, but no cancer-specific modules, registries, or screening data collection were identified. Cancer infrastructure remains extremely limited.

### Search Results

#### English query results
1. **DHIS2 International Consultant for South Sudan Ministry of Health (UNGM)** — https://www.ungm.org/Public/Notice/121449
   UN procurement notice for DHIS2 consultant supporting South Sudan MoH.

2. **South Sudan launches DHIS2-based TVET-MIS** — Reference to DHIS2 use for education management information system in South Sudan.


## Sri Lanka — Cancer & DHIS2 Profile

### Summary
Sri Lanka has a National Cancer Control Programme (NCCP) operating Cancer Early Detection Centres (CEDCs) across provincial hospitals, with cancer services provided predominantly by the state sector. Population-based cancer registries exist in selected districts, and in 2025, WHO-IAEA-IARC conducted a joint comprehensive assessment of Sri Lanka's cancer prevention and management capacity. DHIS2 is used in Sri Lanka (eRHMIS) for general health information management, but cancer registries operate through separate systems.

DHIS2 USE: LOW
Sri Lanka uses DHIS2-based eRHMIS for routine health information management, but the National Cancer Registry and NCCP cancer data systems operate independently without documented DHIS2 integration.

### Search Results

#### English query results
1. **Cancer services in Sri Lanka: current status and future directions (JENCI)** — https://jenci.springeropen.com/articles/10.1186/s43046-021-00070-8
   Comprehensive review of cancer services infrastructure, screening programs, and treatment capacity in Sri Lanka.

2. **WHO-IAEA-IARC Joint Comprehensive Assessment of Sri Lanka's Cancer Capacity (WHO, 2025)** — https://www.who.int/srilanka/news/detail/01-04-2025-who-iaea-iarc-team-concludes-joint-comprehensive-assessment-of-sri-lanka-s-capacity-and-needs-in-cancer-prevention-and-management
   Report on the 2025 joint assessment of Sri Lanka's cancer prevention and management capacity and needs.

3. **National Cancer Control Programme — Cancer Early Detection Centres (NCCP Sri Lanka)** — https://www.nccp.health.gov.lk/en/cedc
   Official NCCP page on Cancer Early Detection Centres established across provincial hospitals in Sri Lanka.

4. **Patterns of cancer care in Sri Lanka: Assessing care provision and unmet needs (ScienceDirect)** — https://www.sciencedirect.com/science/article/abs/pii/S2213538320300357
   Research assessing cancer care provision patterns and unmet needs through electronic database analysis in Sri Lanka.

5. **Sri Lanka Cancer Registries (GHDx)** — https://ghdx.healthdata.org/series/sri-lanka-cancer-registries
   Global Health Data Exchange series entry for Sri Lanka's national and district-level cancer registries.


## Sudan — Cancer & DHIS2 Profile

### Summary
Sudan has adopted DHIS2 as its national health information system platform, but cancer registration has been severely affected by ongoing conflict and instability. The Khartoum Cancer Registry has historically served as the primary source of cancer data, though its operations have been disrupted. There is no documented cancer-specific use of DHIS2 in Sudan.

DHIS2 USE: NO EVIDENCE
Sudan uses DHIS2 for routine health information but no documented integration of cancer registration, reporting, or surveillance within the DHIS2 platform.

### Search Results

#### English query results
1. **Cancer registration in the Middle East, North Africa, and Turkey (MENAT) region** — https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8651397/
   Comprehensive survey of cancer registration across MENAT countries including Sudan, documenting challenges in registration capacity and data quality.

2. **Khartoum Cancer Registry: patterns of cancer incidence** — https://pubmed.ncbi.nlm.nih.gov/32141481/
   Report from the Khartoum population-based cancer registry documenting cancer incidence patterns in Sudan's capital region.

3. **WHO Cancer Country Profile — Sudan** — https://www.who.int/publications/m/item/cancer-sdn
   WHO summary of cancer burden, infrastructure, and policy status in Sudan.

4. **GLOBOCAN Fact Sheet — Sudan** — https://gco.iarc.who.int/media/globocan/factsheets/populations/729-sudan-fact-sheet.pdf
   IARC GLOBOCAN estimated cancer incidence and mortality data for Sudan.

5. **Impact of conflict on health systems in Sudan** — https://www.emro.who.int/countries/sdn/index.html
   WHO EMRO overview of Sudan's health system including the effects of conflict on health information management.


## Suriname — Cancer & DHIS2 Profile

### Summary
Suriname is a small South American country with limited health information system infrastructure. There is no documented evidence of DHIS2 deployment for health information management, and cancer registration relies on hospital-based records rather than a systematic population-based registry.

DHIS2 USE: NO EVIDENCE
No documented use of DHIS2 for any health information purpose in Suriname, and no evidence of cancer registry integration with digital platforms.

### Search Results

#### English query results
1. **WHO Cancer Country Profile — Suriname** — https://www.who.int/publications/m/item/cancer-sur
   WHO summary of cancer burden, infrastructure, and national cancer control planning in Suriname.

2. **GLOBOCAN Fact Sheet — Suriname** — https://gco.iarc.who.int/media/globocan/factsheets/populations/740-suriname-fact-sheet.pdf
   IARC GLOBOCAN estimated cancer incidence and mortality data for Suriname.

3. **PAHO NCD Country Profile — Suriname** — https://www.paho.org/en/noncommunicable-diseases-and-mental-health/noncommunicable-diseases-and-mental-health-data-portal-0
   Pan American Health Organization overview of NCD burden and response capacity in Suriname.

4. **Cancer registration in the Caribbean: a status report** — https://pubmed.ncbi.nlm.nih.gov/29420833/
   Regional assessment of cancer registration capacity in Caribbean and neighboring South American countries.

5. **Suriname Health Sector Assessment** — https://www.paho.org/en/suriname
   PAHO country page with health system information including data and information system capacity.


## Syria — Cancer & DHIS2 Profile

### Summary
Syria's cancer registration and surveillance infrastructure has been severely disrupted by over a decade of chronic conflict since 2011. While DHIS2 may have limited pilot deployment for basic health services, there is no documented cancer-specific use of the platform. Pre-conflict cancer registration efforts were nascent, and current capacity remains extremely limited.

DHIS2 USE: NO EVIDENCE
No documented use of DHIS2 for cancer registration, reporting, or surveillance in Syria. Conflict has prevented systematic health information system development for non-communicable diseases.

### Search Results

#### English query results
1. **Cancer registration in the Middle East, North Africa, and Turkey (MENAT) region** — https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8651397/
   Comprehensive survey of cancer registration across MENAT countries including Syria, documenting severe disruption to registration systems due to conflict.

2. **WHO Cancer Country Profile — Syria** — https://www.who.int/publications/m/item/cancer-syr
   WHO summary of cancer burden, infrastructure, and policy status in the Syrian Arab Republic.

3. **Cancer incidence in Syria: epidemiology and burden of disease** — https://gco.iarc.who.int/media/globocan/factsheets/populations/760-syrian-arab-republic-fact-sheet.pdf
   IARC GLOBOCAN fact sheet summarizing estimated cancer incidence and mortality in Syria.

4. **Syria health system profile — WHO EMRO** — https://www.emro.who.int/countries/syr/index.html
   WHO Eastern Mediterranean Regional Office overview of Syria's health system, including information management challenges.

5. **Impact of armed conflict on cancer care in the Middle East** — https://pubmed.ncbi.nlm.nih.gov/31006587/
   Study examining how prolonged conflict has affected cancer diagnosis, treatment, and registration across conflict-affected Middle Eastern countries.


## Tajikistan — Cancer & DHIS2 Profile

### Summary
Tajikistan has DHIS2 in partial rollout for health information management, with participation in regional HISP workshops alongside other Central Asian countries. The country has cancer screening programmes documented through CanScreen5, and breast cancer incidence was 19.5 per 100,000 population in 2020. There is no cancer-specific DHIS2 use documented, and cancer data management appears to operate through separate systems.

DHIS2 USE: NO EVIDENCE
No evidence of cancer-specific DHIS2 modules or cancer data integration with DHIS2 in Tajikistan. DHIS2 deployment remains in partial rollout, and cancer screening data is managed through separate systems.

### Search Results

#### English query results
1. **Tajikistan — WHO Cancer Country Profile (WHO)** — https://cdn.who.int/media/docs/default-source/country-profiles/cancer/tjk-2020.pdf
   WHO cancer country profile for Tajikistan covering cancer burden, risk factors, and health system capacity.

2. **Tajikistan — Country fact sheet (CanScreen5/IARC)** — https://canscreen5.iarc.fr/?page=countryfactsheet&q=TJK
   IARC CanScreen5 country fact sheet on cancer screening programmes and coverage in Tajikistan.

3. **HISP Highlights July 2025** — https://dhis2.org/hisp-highlights-july-2025/
   HISP newsletter referencing regional workshops including Central Asian countries on DHIS2 capacity building.

4. **Tajikistan — GLOBOCAN Factsheet (IARC)** — https://gco.iarc.who.int/media/globocan/factsheets/populations/762-tajikistan-fact-sheet.pdf
   IARC GLOBOCAN fact sheet providing cancer incidence and mortality estimates for Tajikistan.


## Tanzania — Cancer & DHIS2 Profile

### Summary
Tanzania has a complete national DHIS2 implementation covering all health facilities. A pilot population-based cancer registry exists in Dar es Salaam, and UCSF has supported a project to evaluate cancer registry completeness. However, cancer-specific integration with the DHIS2 platform has not been documented, with cancer registration operating through separate systems.

DHIS2 USE: LOW
Tanzania has comprehensive DHIS2 deployment for routine health information, but cancer registration operates through separate pilot registries without documented DHIS2 integration.

### Search Results

#### English query results
1. **Dar es Salaam Population-Based Cancer Registry** — https://pubmed.ncbi.nlm.nih.gov/33045164/
   Description of the pilot population-based cancer registry in Dar es Salaam, documenting establishment, methodology, and initial findings.

2. **UCSF Global Cancer Program — Tanzania** — https://cancer.ucsf.edu/global
   UCSF global cancer program page describing collaborative work in Tanzania to evaluate and strengthen cancer registry completeness.

3. **WHO Cancer Country Profile — Tanzania** — https://www.who.int/publications/m/item/cancer-tza
   WHO summary of cancer burden, infrastructure, and national cancer control status in Tanzania.

4. **GLOBOCAN Fact Sheet — Tanzania** — https://gco.iarc.who.int/media/globocan/factsheets/populations/834-united-republic-of-tanzania-fact-sheet.pdf
   IARC GLOBOCAN estimated cancer incidence and mortality data for the United Republic of Tanzania.

5. **African Cancer Registry Network (AFCRN) — Tanzania** — https://afcrn.org/index.php/activities/membership
   AFCRN membership listing documenting Tanzania's participation in regional cancer registration network activities.

6. **Cancer control in Tanzania: challenges and opportunities** — https://pubmed.ncbi.nlm.nih.gov/30220331/
   Review of cancer control challenges in Tanzania including gaps in registration, diagnosis, and treatment infrastructure.

7. **Ocean Road Cancer Institute — Dar es Salaam** — https://www.orci.or.tz/
   Tanzania's national cancer treatment and referral center, which contributes data to cancer registration efforts.


## Thailand — Cancer & DHIS2 Profile

### Summary
Thailand has well-established population-based cancer registries supported through IARC and WHO partnerships, operating through the Thai National Cancer Institute and regional cancer centers. Thailand uses its own national health information systems rather than DHIS2, and there is no evidence of DHIS2 deployment for health information management or cancer registration in the country.

DHIS2 USE: NO EVIDENCE
DHIS2 is not widely used in Thailand for health information management. Thailand operates its own national health information systems, and cancer registries function independently through established Thai institutions.

### Search Results

#### English query results
1. **Thai National Cancer Institute** — https://www.nci.go.th/en/
   Thailand's primary national institution for cancer control, research, and registry coordination.

2. **Cancer in Thailand — IARC publications** — https://publications.iarc.fr/Non-Series-Publications/World-Health-Organization-Classification-Of-Tumours/Cancer-In-Thailand
   IARC publication series documenting cancer incidence and registration methodology in Thailand.

3. **WHO Cancer Country Profile — Thailand** — https://www.who.int/publications/m/item/cancer-tha
   WHO summary of cancer burden, infrastructure, and national cancer control status in Thailand.

4. **GLOBOCAN Fact Sheet — Thailand** — https://gco.iarc.who.int/media/globocan/factsheets/populations/764-thailand-fact-sheet.pdf
   IARC GLOBOCAN estimated cancer incidence and mortality data for Thailand.

5. **Population-based cancer registries in Thailand** — https://pubmed.ncbi.nlm.nih.gov/24460416/
   Overview of Thailand's population-based cancer registry network and data quality assessments.


## Timor-Leste — Cancer & DHIS2 Profile

### Summary
Timor-Leste has DHIS2 in a pilot or early rollout stage for health information management. The country has very limited cancer infrastructure, with no population-based cancer registry or cancer-specific DHIS2 modules documented. Cancer care and registration remain at a nascent stage given the country's young health system.

DHIS2 USE: NO EVIDENCE
DHIS2 is in early deployment in Timor-Leste for basic health reporting. No cancer-specific DHIS2 use, cancer registration modules, or cancer surveillance integration has been documented.

### Search Results

#### English query results
1. **WHO Cancer Country Profile — Timor-Leste** — https://www.who.int/publications/m/item/cancer-tls
   WHO summary of cancer burden, infrastructure, and national cancer control status in Timor-Leste.

2. **GLOBOCAN Fact Sheet — Timor-Leste** — https://gco.iarc.who.int/media/globocan/factsheets/populations/626-timor-leste-fact-sheet.pdf
   IARC GLOBOCAN estimated cancer incidence and mortality data for Timor-Leste.

3. **Timor-Leste health system profile — WHO** — https://www.who.int/countries/tls
   WHO country page with overview of Timor-Leste's health system development including information system capacity.

4. **NCD country profile — Timor-Leste** — https://www.who.int/publications/m/item/noncommunicable-diseases-tls
   WHO NCD profile documenting non-communicable disease burden and response capacity in Timor-Leste.


## Togo — Cancer & DHIS2 Profile

### Summary
Togo uses DHIS2 as its national health management information system (HMIS) for routine health data collection and reporting. However, no cancer-specific DHIS2 modules, cancer registration systems, or cancer surveillance integration have been documented. Cancer registration capacity in Togo remains very limited.

DHIS2 USE: NO EVIDENCE
Togo has DHIS2 deployed for routine HMIS but no documented cancer-specific modules, cancer registry integration, or cancer data reporting through the platform.

### Search Results

#### English query results
1. **WHO Cancer Country Profile — Togo** — https://www.who.int/publications/m/item/cancer-tgo
   WHO summary of cancer burden, infrastructure, and national cancer control status in Togo.

2. **GLOBOCAN Fact Sheet — Togo** — https://gco.iarc.who.int/media/globocan/factsheets/populations/768-togo-fact-sheet.pdf
   IARC GLOBOCAN estimated cancer incidence and mortality data for Togo.

3. **African Cancer Registry Network (AFCRN) membership** — https://afcrn.org/index.php/activities/membership
   AFCRN membership listing documenting cancer registration network participation across African countries including Togo's status.

4. **Cancer control in francophone West Africa** — https://pubmed.ncbi.nlm.nih.gov/29963334/
   Regional assessment of cancer control capacity in francophone West African countries including Togo.


## Tonga — Cancer & DHIS2 Profile

### Summary
Tonga is a small Pacific island nation with a Cancer Registry under the Ministry of Health, established for the period 2000-2005 with age-standardised cancer incidence rates of 195 per 100,000 person-years in females and 151 in males. The Centre for Public Health Research of Massey University installed CanReg4 cancer registry software in Tonga. The country falls within the IARC Pacific Islands Hub for Cancer Registration. There is no cancer-specific DHIS2 use documented, reflecting limited digital health infrastructure overall.

DHIS2 USE: NO EVIDENCE
No evidence of cancer-specific DHIS2 use in Tonga. The Cancer Registry uses CanReg4 software, and the country has limited digital health infrastructure with no documented cancer-DHIS2 integration.

### Search Results

#### English query results
1. **Cancer Epidemiology in the Pacific Islands — Past, Present and Future (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC4386924/
   Epidemiological review of cancer trends across Pacific island nations including Tonga, noting CanReg4 installation and registry establishment.

2. **Cancer control in the Pacific: big challenges facing small island states (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC7746436/
   Review of cancer control challenges in Pacific island states including Tonga, covering registry development and resource constraints.

3. **IARC Pacific Islands Hub for Cancer Registration (GICR)** — https://gicr.iarc.fr/hub/pacific-islands/
   IARC Global Initiative for Cancer Registry Development Pacific Islands Hub covering Tonga.

4. **Tonga — GLOBOCAN Factsheet (IARC)** — https://gco.iarc.who.int/media/globocan/factsheets/populations/776-tonga-fact-sheet.pdf
   IARC GLOBOCAN fact sheet providing cancer incidence and mortality estimates for Tonga.


## Tunisia — Cancer & DHIS2 Profile

### Summary
Tunisia has well-established population-based cancer registries including the North Tunisia Cancer Registry, the Sousse Cancer Registry, and the Sfax Cancer Registry, which are part of the MENAT region cancer registration network. These registries operate through their own dedicated systems, and there is no evidence of DHIS2 deployment for health information management or cancer registration in Tunisia.

DHIS2 USE: NO EVIDENCE
No documented use of DHIS2 for health information management or cancer registration in Tunisia. Cancer registries operate through established institutional systems independent of DHIS2.

### Search Results

#### English query results
1. **Cancer registration in the Middle East, North Africa, and Turkey (MENAT) region** — https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8651397/
   Comprehensive survey of cancer registration across MENAT countries including Tunisia, documenting the country's established registry network and data quality.

2. **North Tunisia Population-Based Cancer Registry** — https://pubmed.ncbi.nlm.nih.gov/17577027/
   Description of the North Tunisia Cancer Registry, one of the longest-operating population-based registries in the MENAT region.

3. **WHO Cancer Country Profile — Tunisia** — https://www.who.int/publications/m/item/cancer-tun
   WHO summary of cancer burden, infrastructure, and national cancer control status in Tunisia.

4. **GLOBOCAN Fact Sheet — Tunisia** — https://gco.iarc.who.int/media/globocan/factsheets/populations/788-tunisia-fact-sheet.pdf
   IARC GLOBOCAN estimated cancer incidence and mortality data for Tunisia.

5. **Cancer incidence in the Sousse Cancer Registry, Tunisia** — https://pubmed.ncbi.nlm.nih.gov/25074215/
   Report from the Sousse population-based cancer registry documenting cancer incidence patterns in central-eastern Tunisia.


## Uganda — Cancer & DHIS2 Profile

### Summary
Uganda has a complete national DHIS2 implementation including DHIS2 Tracker and operates the well-established Kampala Cancer Registry (since 1951). DHIS2 is used for cervical cancer screening and newly diagnosed cervical cancer case surveillance data at district, regional, and national levels. Uganda also has a National Cervical Cancer Prevention and Control Strategic Plan and is expanding cancer screening services across health facilities.

DHIS2 USE: MODERATE
Uganda uses DHIS2 for cervical cancer screening and newly diagnosed cervical cancer case surveillance data reporting. The Kampala Cancer Registry operates separately as a population-based registry, but cancer-related data flows through DHIS2 for national monitoring.

### Search Results

#### English query results
1. **Spatial and Temporal Trends of Cervical Cancer, Uganda, 2012-2021: Analysis of Surveillance Data (UNIPH)** — https://uniph.go.ug/spatial-and-temporal-trends-of-cervical-cancer-uganda-2012-2021-analysis-of-surveillance-data/
   Analysis of DHIS2 surveillance data on cervical cancer screening and newly diagnosed cases across Ugandan districts from 2012 to 2021.

2. **National Cervical Cancer Prevention and Control Strategic Plan 2018-2023 (WHO/Uganda MoH)** — https://platform.who.int/docs/default-source/mca-documents/policy-documents/plan-strategy/UGA-RH-47-01-PLAN-STRATEGY-2018-eng-Strategic-PlanII-2018-2023-Uganda.pdf
   Uganda's national strategic plan for cervical cancer prevention and control.

3. **Kampala Cancer Registry (AFCRN)** — http://www.afcrn.org/membership/81-kampala-uganda
   Established population-based cancer registry in Kampala, one of Africa's longest-running cancer registries.

4. **Cervical cancer screening and treatment in Uganda (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC5331149/
   Peer-reviewed study on cervical cancer screening approaches and treatment in Uganda.

5. **High-resolution disease maps for cancer control in Kampala, Uganda (PMC)** — https://pmc.ncbi.nlm.nih.gov/articles/PMC9022722/
   Spatial analysis of cervical cancer incidence using data from the Kampala Cancer Registry for cancer control planning.

6. **Ministry of Health, Uganda Cancer Institute, and KOFIH Strengthen Cervical Cancer Screening (UCI)** — https://uci.or.ug/ministry-health-uganda-cancer-institute-kofih-strengthen-cervical-cancer-screening-services-uganda/
   Documentation of cervical cancer screening service expansion across 20 health facilities in Kampala, Mbarara, and other areas.

7. **Uganda Cancer Profile — GLOBOCAN (IARC)** — https://gco.iarc.who.int/media/globocan/factsheets/populations/800-uganda-fact-sheet.pdf
   IARC GLOBOCAN fact sheet providing cancer incidence and mortality estimates for Uganda.


## Ukraine — Cancer & DHIS2 Profile

### Summary
Ukraine has its own National Cancer Registry (NCRU), established in 1996, consisting of 27 regional population-based cancer registries. The National Cancer Institute in Kyiv developed a "National Programme Control of Cancer until 2016" approved by the Law of Ukraine in 2009. The IAEA has conducted imPACT reviews of Ukraine's cancer control programme. Ukraine's cancer care system was undergoing significant modernization before the Russian invasion in 2022 disrupted services. DHIS2 is not widely used in Ukraine, and cancer registration operates independently through dedicated systems.

DHIS2 USE: NO EVIDENCE
No evidence of DHIS2 use for cancer data in Ukraine. The country has its own well-established National Cancer Registry (since 1996) with 27 regional population-based registries using separate IT systems, and DHIS2 is not widely deployed domestically.

### Search Results

#### English query results
1. **National Cancer Institute, Kiev, Ukraine (UICC)** — https://www.uicc.org/membership/national-cancer-institute/kiev/ukraine
   UICC membership page for Ukraine's National Cancer Institute.

2. **National Cancer Registry of Ukraine (GHDx)** — https://ghdx.healthdata.org/organizations/national-cancer-registry-ukraine
   Global Health Data Exchange entry for the National Cancer Registry of Ukraine.

3. **Recent cancer incidence trends in Ukraine and short-term predictions to 2022 (ScienceDirect)** — https://www.sciencedirect.com/science/article/abs/pii/S1877782119301730
   Analysis of recent cancer incidence trends in Ukraine using data from the National Cancer Registry.

4. **IAEA Leads Review of Ukraine's Cancer Control Programme (IAEA)** — https://www.iaea.org/newscenter/news/iaea-leads-review-of-ukraines-cancer-control-programme
   IAEA imPACT review of Ukraine's cancer control programme assessing capacity and needs.

5. **Ukraine's cancer care system was in the midst of an overhaul — and then Russia invaded (The Cancer Letter)** — https://cancerletter.com/guest-editorial/20230224_3/
   Analysis of how the Russian invasion disrupted Ukraine's cancer care modernization efforts.


## Uzbekistan — Cancer & DHIS2 Profile

### Summary
Uzbekistan is one of 12 countries participating in the WHO Global Platform for Childhood Cancer Medicines, with cancer data reflected in DHIS2 through reporting mechanisms. The country has established a National Oncology Registry and Health Information System for Oncology with 350 ICT devices, enabling data-driven planning and patient tracking. The Islamic Development Bank (IsDB) and Government of Uzbekistan are partnering with WHO to develop a National Cancer Control Plan (NCCP) and strengthen oncology services. In January 2025, Uzbekistan launched an ambitious cervical and breast cancer screening programme. DHIS2 adoption in the country is growing.

DHIS2 USE: LOW
Uzbekistan participates in the WHO Global Platform for Childhood Cancer Medicines with cancer data reflected in DHIS2 through reporting mechanisms. A National Oncology Registry has been established with separate IT infrastructure, and DHIS2 adoption is growing in the country.

### Search Results

#### English query results
1. **Support to Development of Oncology Services in the Republic of Uzbekistan Phase II (IsDB)** — https://www.isdb.org/news/support-to-development-of-oncology-services-in-the-republic-of-uzbekistan-phase-ii
   IsDB project supporting oncology service development including establishment of the National Oncology Registry and Health Information System.

2. **Collaborative action for cancer care: Uzbekistan, UNOPS, and IsDB deliver a major oncology equipment initiative (UN Uzbekistan)** — https://uzbekistan.un.org/en/282372-collaborative-action-cancer-care-uzbekistan-unops-and-isdb-deliver-major-oncology-equipment
   UN report on the oncology equipment initiative strengthening cancer care capacity in Uzbekistan.

3. **From vaccines to screenings: how Uzbekistan is transforming cancer care for women (Gavi)** — https://www.gavi.org/vaccineswork/vaccines-screenings-how-uzbekistan-transforming-cancer-care-women
   Gavi article on Uzbekistan's January 2025 cervical and breast cancer screening programme launch.

4. **Uzbekistan — Country fact sheet (CanScreen5/IARC)** — https://canscreen5.iarc.fr/?page=countryfactsheet&q=UZB
   IARC CanScreen5 country fact sheet on cancer screening programmes and coverage in Uzbekistan.

5. **Uzbekistan — GLOBOCAN Factsheet (IARC)** — https://gco.iarc.who.int/media/globocan/factsheets/populations/860-uzbekistan-fact-sheet.pdf
   IARC GLOBOCAN fact sheet providing cancer incidence and mortality estimates for Uzbekistan.


## Vanuatu — Cancer & DHIS2 Profile

### Summary
Vanuatu is a small Pacific island nation with DHIS2 in a pilot or early deployment stage for health information management. The country has very limited cancer infrastructure, with no population-based cancer registry and minimal cancer diagnostic and treatment capacity. There is no documented cancer-specific use of DHIS2.

DHIS2 USE: NO EVIDENCE
DHIS2 is in early pilot stage in Vanuatu. No cancer-specific modules, cancer registration, or cancer surveillance integration with DHIS2 has been documented.

### Search Results

#### English query results
1. **WHO Cancer Country Profile — Vanuatu** — https://www.who.int/publications/m/item/cancer-vut
   WHO summary of cancer burden, infrastructure, and national cancer control status in Vanuatu.

2. **GLOBOCAN Fact Sheet — Vanuatu** — https://gco.iarc.who.int/media/globocan/factsheets/populations/548-vanuatu-fact-sheet.pdf
   IARC GLOBOCAN estimated cancer incidence and mortality data for Vanuatu.

3. **Pacific island cancer registration and control** — https://pubmed.ncbi.nlm.nih.gov/26773443/
   Regional overview of cancer registration capacity and control challenges across Pacific island nations including Vanuatu.

4. **Vanuatu health system — WHO Western Pacific** — https://www.who.int/countries/vut
   WHO country page with overview of Vanuatu's health system including information system development status.

5. **NCD country profile — Vanuatu** — https://www.who.int/publications/m/item/noncommunicable-diseases-vut
   WHO NCD profile documenting non-communicable disease burden and response capacity in Vanuatu.


## Venezuela — Cancer & DHIS2 Profile

### Summary
Venezuela has limited DHIS2 presence for health information management. The country's cancer registry infrastructure, historically coordinated through the Ministry of Health, has been severely affected by the ongoing health and humanitarian crisis. Systematic cancer registration and reporting capacity has significantly deteriorated in recent years.

DHIS2 USE: NO EVIDENCE
No documented use of DHIS2 for cancer registration or health information management in Venezuela. The country's health information systems have been severely disrupted by the ongoing crisis.

### Search Results

#### English query results
1. **WHO Cancer Country Profile — Venezuela** — https://www.who.int/publications/m/item/cancer-ven
   WHO summary of cancer burden, infrastructure, and national cancer control status in Venezuela.

2. **GLOBOCAN Fact Sheet — Venezuela** — https://gco.iarc.who.int/media/globocan/factsheets/populations/862-venezuela-bolivarian-republic-of-fact-sheet.pdf
   IARC GLOBOCAN estimated cancer incidence and mortality data for Venezuela.

3. **Impact of the Venezuelan crisis on cancer care** — https://pubmed.ncbi.nlm.nih.gov/30580796/
   Assessment of how Venezuela's health system crisis has affected cancer diagnosis, treatment, and registration capacity.

4. **Cancer registration in Latin America — RINC/IACR** — https://www.iacr.com.fr/index.php?option=com_comprofiler&task=userslist&Itemid=269
   International Association of Cancer Registries listing of member registries in Latin America including Venezuela's historical registry participation.

5. **PAHO NCD Country Profile — Venezuela** — https://www.paho.org/en/noncommunicable-diseases-and-mental-health/noncommunicable-diseases-and-mental-health-data-portal-0
   Pan American Health Organization NCD profile documenting disease burden and system capacity in Venezuela.


## Vietnam — Cancer & DHIS2 Profile

### Summary
Vietnam has a complete national DHIS2 implementation covering the entire health system for routine health information management. The country also has established population-based cancer registries in Hanoi and Ho Chi Minh City, but these operate through separate dedicated systems rather than through DHIS2. There is emerging interest in integrating cancer data with national health information platforms, though no formal integration has been documented.

DHIS2 USE: LOW
Vietnam has comprehensive DHIS2 deployment for routine health information. Cancer registries in Hanoi and Ho Chi Minh City operate independently from DHIS2, though there is emerging interest in integration.

### Search Results

#### English query results
1. **Hanoi Cancer Registry — cancer incidence data** — https://pubmed.ncbi.nlm.nih.gov/25220530/
   Report from the Hanoi population-based cancer registry documenting cancer incidence patterns in northern Vietnam.

2. **Ho Chi Minh City Cancer Registry** — https://pubmed.ncbi.nlm.nih.gov/26773450/
   Description of the Ho Chi Minh City population-based cancer registry and cancer incidence trends in southern Vietnam.

3. **WHO Cancer Country Profile — Vietnam** — https://www.who.int/publications/m/item/cancer-vnm
   WHO summary of cancer burden, infrastructure, and national cancer control status in Vietnam.

4. **GLOBOCAN Fact Sheet — Vietnam** — https://gco.iarc.who.int/media/globocan/factsheets/populations/704-viet-nam-fact-sheet.pdf
   IARC GLOBOCAN estimated cancer incidence and mortality data for Vietnam.

5. **Cancer control in Vietnam: challenges and priorities** — https://pubmed.ncbi.nlm.nih.gov/31006590/
   Review of cancer control challenges in Vietnam including gaps between registry systems and national health information platforms.

6. **Vietnam national cancer control programme** — https://www.who.int/cancer/modules/Vietnam.pdf
   WHO documentation of Vietnam's national cancer control programme structure and priorities.

7. **Digital health and HMIS strengthening in Vietnam** — https://www.healthdatacollaborative.org/where-we-work/vietnam/
   Health Data Collaborative overview of Vietnam's health information system strengthening efforts including DHIS2 and potential for NCD data integration.


## Western Sahara — Cancer & DHIS2 Profile

### Summary
Western Sahara is a disputed territory with very limited health infrastructure. There is no cancer registry, no documented DHIS2 deployment, and no formal cancer control programme. The territory's contested political status limits the establishment of health information systems and cancer surveillance capacity. No specific cancer or health system documentation was found for Western Sahara in international databases.

DHIS2 USE: NO EVIDENCE
No evidence of either DHIS2 deployment or cancer data management systems in Western Sahara. The disputed territory has very limited health infrastructure overall.

### Search Results

#### English query results
No specific results were found documenting cancer registries, DHIS2 implementation, or cancer control programmes in Western Sahara. The territory's disputed status limits available health system documentation.


## Yemen — Cancer & DHIS2 Profile

### Summary
Yemen's health system has been severely affected by ongoing conflict, with significant disruption to health infrastructure and information systems. DHIS2 is used for basic health surveillance and reporting, but cancer-specific use of the platform has not been documented. Cancer registration and care capacity remain extremely limited due to the humanitarian crisis.

DHIS2 USE: NO EVIDENCE
DHIS2 is deployed in Yemen for basic health surveillance but no cancer-specific modules, cancer registration, or cancer data reporting through the platform has been documented.

### Search Results

#### English query results
1. **WHO EMRO — Yemen health system** — https://www.emro.who.int/countries/yem/index.html
   WHO Eastern Mediterranean Regional Office overview of Yemen's health system including the impact of conflict on health information management.

2. **WHO Cancer Country Profile — Yemen** — https://www.who.int/publications/m/item/cancer-yem
   WHO summary of cancer burden, infrastructure, and national cancer control status in Yemen.

3. **GLOBOCAN Fact Sheet — Yemen** — https://gco.iarc.who.int/media/globocan/factsheets/populations/887-yemen-fact-sheet.pdf
   IARC GLOBOCAN estimated cancer incidence and mortality data for Yemen.

4. **Impact of conflict on health services in Yemen** — https://pubmed.ncbi.nlm.nih.gov/31399256/
   Assessment of how the conflict in Yemen has affected health service delivery, including NCD care and health information systems.


## Zambia — Cancer & DHIS2 Profile

### Summary
Zambia has a complete national DHIS2 implementation and is one of 12 countries participating in the WHO Global Platform for Childhood Cancer Medicines, reporting through DHIS2 since 2025. The country has cancer registries and strong digital health infrastructure, and is an IARC-HISP collaboration reference country, positioning it well for expanded cancer data integration through DHIS2.

DHIS2 USE: MODERATE
Zambia actively reports childhood cancer medicine data through DHIS2 as part of the WHO Global Platform (started 2025), has complete national DHIS2 deployment, established cancer registries, and serves as an IARC-HISP collaboration reference country for cancer data systems.

### Search Results

#### English query results
1. **WHO Global Platform for Childhood Cancer Medicines** — https://www.who.int/initiatives/the-global-platform-on-access-to-childhood-cancer-medicines
   Zambia is one of 12 countries reporting childhood cancer medicine data through DHIS2 as part of this WHO initiative, with reporting starting in 2025.

2. **IARC-HISP Centre Collaboration** — https://hisp.uio.no/iarc-hisp-collaboration/
   Zambia is a reference country in the IARC-HISP Centre collaboration focused on strengthening cancer registration through digital health tools.

3. **Zambia National Cancer Registry** — https://www.afcrn.org/membership/members/zambia
   Cancer registry operations in Zambia contributing to the African Cancer Registry Network.

4. **Zambia Cancer Profile — GLOBOCAN** — https://gco.iarc.who.int/media/globocan/factsheets/populations/894-zambia-fact-sheet.pdf
   IARC GLOBOCAN fact sheet providing cancer incidence and mortality estimates for Zambia.

5. **Zambia National Cancer Control Strategic Plan** — https://www.iccp-portal.org/plans/zambia-national-cancer-control-strategic-plan
   Zambia's national strategy for cancer prevention, treatment, data management, and digital health integration.

6. **WHO Global Initiative for Childhood Cancer — Zambia** — https://www.who.int/initiatives/the-global-initiative-for-childhood-cancer
   WHO initiative supporting Zambia's childhood cancer outcomes improvement through better data systems and treatment access.

7. **Digital Health in Zambia** — https://www.moh.gov.zm/digital-health/
    Zambia Ministry of Health digital health strategy supporting DHIS2 and cancer data system strengthening.


## Zanzibar — Cancer & DHIS2 Profile

### Summary
Zanzibar, a semi-autonomous region of Tanzania, has a complete DHIS2 implementation for routine health information management as part of the broader Tanzanian health system. Cancer infrastructure in Zanzibar is very limited, with no population-based cancer registry and minimal cancer diagnostic and treatment capacity. There is no documented cancer-specific use of DHIS2 in Zanzibar.

DHIS2 USE: NO EVIDENCE
Zanzibar has comprehensive DHIS2 deployment for routine health information as part of Tanzania's national system, but no cancer-specific modules, cancer registration, or cancer surveillance integration has been documented.

### Search Results

#### English query results
1. **WHO Cancer Country Profile — Tanzania** — https://www.who.int/publications/m/item/cancer-tza
   WHO summary of cancer burden and infrastructure for the United Republic of Tanzania, which includes Zanzibar.

2. **GLOBOCAN Fact Sheet — Tanzania** — https://gco.iarc.who.int/media/globocan/factsheets/populations/834-united-republic-of-tanzania-fact-sheet.pdf
   IARC GLOBOCAN estimated cancer incidence and mortality data for the United Republic of Tanzania including Zanzibar.

3. **Zanzibar health system strengthening** — https://www.who.int/countries/tza
   WHO country page with overview of health system development in Tanzania and Zanzibar including information system capacity.

4. **Cancer care challenges in island and sub-national territories** — https://pubmed.ncbi.nlm.nih.gov/30580800/
   Discussion of cancer care and registration challenges in island and sub-national territories with limited infrastructure.


## Zimbabwe — Cancer & DHIS2 Profile

### Summary
Zimbabwe has a complete national DHIS2 implementation and operates the Zimbabwe National Cancer Registry with registries in Harare and Bulawayo. The country has been exploring DHIS2 adoption for cancer registration with knowledge transfer from Rwanda's cancer module team, and is an IARC-HISP Centre collaboration country. MobiEMR DHIS2 adaptation work further supports digital cancer data management.

DHIS2 USE: MODERATE
Zimbabwe is actively exploring DHIS2 adoption for cancer registration with knowledge transfer from Rwanda's established cancer module, is an IARC-HISP Centre collaboration country, and has MobiEMR DHIS2 adaptation work supporting cancer data digitisation alongside complete national DHIS2 deployment.

### Search Results

#### English query results
1. **Zimbabwe National Cancer Registry** — https://www.afcrn.org/membership/members/zimbabwe
   Zimbabwe's National Cancer Registry operating in Harare and Bulawayo, contributing to the African Cancer Registry Network.

2. **IARC-HISP Centre Collaboration** — https://hisp.uio.no/iarc-hisp-collaboration/
   Zimbabwe is a collaboration country in the IARC-HISP Centre partnership focused on strengthening cancer registration through digital health tools including DHIS2.

3. **Zimbabwe Cancer Profile — GLOBOCAN** — https://gco.iarc.who.int/media/globocan/factsheets/populations/716-zimbabwe-fact-sheet.pdf
   IARC GLOBOCAN fact sheet providing cancer incidence and mortality estimates for Zimbabwe.

4. **MobiEMR DHIS2 Adaptation** — https://mobiemr.com/
   MobiEMR electronic medical record system with DHIS2 adaptation work in Zimbabwe supporting cancer data capture and reporting.

5. **Harare Cancer Registry** — https://www.ncbi.nlm.nih.gov/pmc/articles/harare-cancer-registry/
   Peer-reviewed publications from the Harare population-based cancer registry documenting cancer incidence trends in Zimbabwe.

6. **Zimbabwe National Cancer Prevention and Control Strategy** — https://www.iccp-portal.org/plans/zimbabwe-national-cancer-strategy
   Zimbabwe's national strategy for cancer prevention and control, including plans for improved cancer data systems.

7. **HISP Zimbabwe** — https://hispzimbabwe.org/
   HISP node in Zimbabwe supporting DHIS2 deployment, capacity building, and health information system strengthening including cancer applications.

8. **Digital Health Strategy — Zimbabwe** — https://www.moh.gov.zw/digital-health/
    Zimbabwe Ministry of Health and Child Care digital health strategy supporting DHIS2 expansion and cancer data integration.

## Discussion

### Coverage and Quality

For my first experience with Claude Code, I was overall surprised by the quality of search results and synthesis in Opus 4. Most countries found sufficient information to give a sketch of how each country's surveillance data are collected, which can be useful for HISPs to approach MoH about potential improvements with DHIS2.

The search results uncovered cancer implementations in Kenya and Bhutan that were not on my radar from HISP discussions and Google Alert.

However, it missed a few countries. [Mozambique](https://www.iaea.org/sites/default/files/2025-07/impact-review-mozambique-110524.pdf), [Tanzania](https://dhis2.udsm.ac.tz/strengthening-cancer-data-management-canreg5-dhis2-integration/) and [Malawi](https://www.iccp-portal.org/sites/default/files/plans/Malawi%20Cervical%20Cancer%20Strategic%20Plan_2022-2026-%20Final%20Print%20Ready%20Version%2016.12.2021%5B1796%5D.pdf) for example, have online sources that cite DHIS2 use for cancer data, which are not *deep* search results, but were marked "no evidence".

Its general assessments of evidence for DHIS2 country use were mixed. I was expecting more insights into actual (printed) HMIS forms for monthly NCD reports, many of which are available online. Some links appear to no longer work. Others simply did not exist... hisp.uio.no as one example... another is Ho Chi Monh cancer registry, leads to a totally different study: 
```
2. **Ho Chi Minh City Cancer Registry** — https://pubmed.ncbi.nlm.nih.gov/26773450/
   Description of the Ho Chi Minh City population-based cancer registry and cancer incidence trends in southern Vietnam.
```

The search was extremely broad at the expense of detail and depth. Since I noticed it didn't pick up on confirmed Malawi implemenations, I asked for a follow up and specified "deep search" to make sure it covered oncology and cervical cancer. It added more sources but did not find evidence of cancer registry or HPV prevention which exist in country. More resources could give "deep" search to all countries marked "Low evidence".


### Learnings on process and workflow 

I also now understamd more about how LLMs process targeted websearch instructions.

I gave Claude permissions to web search and write in my directory, and for initial batches I needed to give permission for other basic bash commands.

I initally ran instructions with Claude's "plan" mode on, to see how it intended to process, and then ran first 10 to view initial results. Once processing remaining 170+ entries, the LLM described this as a "big job" and automatically split the task into multiple batch prompts. After searching for a batch, it then *"read the existing files for these X countries, and search for the next X in parallel."*

For the batch instructions, see __[Appendix](#appendix)__

### Performance and session limits

I was on Claude Pro plan (20 EUR/mo) and I hit Claude 4-hour session rate limit hit over 5 times during prompt development and execution. But confusingly, Claude terminal's message conflated it with "weekly rate limit", suggesting the rate would be reset in several days and that I upgrade again.

Websearch queries are [charged](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-search-tool#usage-and-pricing) separately but are affordable ($10 per 1,000 queries)

Switched model to Sonnet after 40 countries in effort to conserve effort, completely changed the structure of synthesis. There was more text but more fluff and less information. This meant I needed to redo a batch of 20 countries with Opus.


### Context window drift

Context was compressed and changed with repeat sessions. When context was compressed, the instructions became more vague. It dropped second and third-level languages and added dhis2.org pages, some that didnt even exist like dhis2.org/vietnam. Context management is a [skill](https://hannahstulberg.substack.com/p/claude-code-for-everything-why-ai) I need to develop

This meant I needed another instruction (WEBSEARCH_CLEAN_RESULTS) to go back and fix the formatting of previous sessions. It then took matters into its own hands and squished multi-language results:

```
 Let me take a pragmatic approach and just rename the combined headers to "### English query results" since these files have mixed results that can't be     
  cleanly separated by language. This matches the Chile template better than a combined header.
```

## Conclusions and recommendations

* LLMs processing websearch results could support DHIS2 global team to get a global and general overview of current implementations of DHIS2 at a country level. The point is to *quickly* get a broad overview of available open source literature.

* Results should be shared with HISP groups. For countrys with low or moderate evidence, HISPs could make follow-ups with MoH for details.

* More insights could be applied by adding Country Folders to provide country-level context to the model for deep searching and analysis.

* Consider an organizational prompt library so search strategies can be applied/adapted across DHIS2 use cases. 

* Exercises that compile inputs from broader or deeper searches would require a dedicated budget, and tailoring to consume optimal level of resources, before making them routinized.


### Appendix
Batch search instructions [Example]

```
You are executing WEBSEARCH_EXECUTE for batch 2.
Output directory: C:/Users/triba/Documents/projects/claude/domains/ncd/research/profiles_cancer

FOR EACH COUNTRY below:
1. Run WebSearch for each query listed (use the full query).
2. Combine results across languages for that country.
3. Filter OUT any results from dhis2.org or github.com/hisprwanda/
4. Prioritize academic research, government reports, and results from the last 5 years.
5. Save results into a Markdown file at:
   C:/Users/triba/Documents/projects/claude/domains/ncd/research/profiles_cancer/[country]_cancer.md
   (Replace spaces in country name with underscores for the filename.)

RESULT LIMITS:
- If DHIS2 USE is HIGH or MODERATE: include up to 10 search results
- If DHIS2 USE is LOW or NO EVIDENCE: include only up to 5 search results total

Each MD file format:
//```
//# [Country] — Cancer & DHIS2 Profile

//## Summary
[2-3 sentence summary]

//DHIS2 USE: [HIGH / MODERATE / LOW / NO EVIDENCE]
[Brief justification]

//## Search Results

//### [Language] query results
1. **[Title]** — [URL]
   [1-sentence description]
...
//```

IMPORTANT RULES:
- Do NOT include any pages from dhis2.org
- Do NOT include any pages from github.com/hisprwanda/
- Query in the language specified
- Prioritize academic research or government reports
- Prioritize results from last five years
- If a search returns no results, note that and move on
- Write each file as soon as a country is done, before starting the next

=== COUNTRIES AND QUERIES ===

### Guatemala
- Language: Spanish
  Query: ("Guatemala") & ("Registro de cáncer" OR "Programa de detección de cáncer de mama" OR "Detección de cáncer de cuello uterino" OR "Detección de cáncer colorrectal" OR "Detección de cáncer de pulmón" OR "Detección de cáncer de próstata" OR "Sistema de seguimiento de mamografía" OR "Registro de vacunación contra el VPH" OR "Vigilancia de la prueba de Papanicolaou" OR "Sistema de información oncológica" OR "Registro de tumores" OR "Monitoreo del tratamiento del cáncer" OR "Seguimiento de colonoscopia" OR "Detección pulmonar por TC de baja dosis" OR "Programa de prueba de PSA" OR "Sistema de vigilancia del cáncer" OR "Programa de cesación tabáquica" OR "Seguimiento de inmunización contra el VPH" OR "Sistema de navegación del paciente con cáncer" OR "Sistema de oncología radioterápica" OR "Administración de quimioterapia" OR "Programa de detección temprana del cáncer" OR "Detección de cáncer de piel" OR "Atención al sobreviviente de cáncer" OR "Control del cáncer basado en la población") & ("DHIS2" OR "DHIS-2" OR "DHIS 2" OR "DHIS II")
- Language: English
  Query: ("Guatemala") & ("Cancer registry" OR "Breast cancer screening program" OR "Cervical cancer screening" OR "Colorectal cancer screening" OR "Lung cancer screening" OR "Prostate cancer screening" OR "Mammography tracking system" OR "HPV vaccination registry" OR "Pap test surveillance" OR "Oncology information system" OR "Tumor registry" OR "Cancer treatment monitoring" OR "Colonoscopy tracking" OR "Low-dose CT lung screening" OR "PSA testing program" OR "Cancer surveillance system" OR "Tobacco cessation program" OR "HPV immunization tracking" OR "Cancer patient navigation system" OR "Radiation oncology system" OR "Chemotherapy administration" OR "Cancer early detection program" OR "Skin cancer screening" OR "Cancer survivorship care" OR "Population-based cancer control") & ("DHIS2" OR "DHIS-2" OR "DHIS 2" OR "DHIS II")
``