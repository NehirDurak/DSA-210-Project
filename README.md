# DSA-210-Project - Correlating FGM/C Prevalence with Religious Demographics

# Introduction 
Female Genital Mutilation/Cutting (FGM/C) is a crime that has been ongoing in 2025, affecting millions of women worldwide. While this practice is mostly driven by social norms, it is also associated with specific religious factors and education levels. This project  investigates into the statistical relationship between the prevalence of FGM/C in various countries in Africa and their corresponding religious influences while controlling for socio-economic and demographic factors such as education, income level,urbanization, region.

# Motivation
Even the existence of FGM/C is a significant human rights issue.Effective policy and intervention strategies to eradicate it depend on a correct understanding of its root causes. The motivation for this project is to apply the data science pipeline to a complex social problem.

# Data Sources and Collection
To build a robust analysis, there will be various datasets from multiple organizations.

* Primary Source: [MICS (Multiple Indicator Cluster Surveys)](https://mics.unicef.org/): UNICEF based microdata surveys including FGM modules for certain countries This website allows me to investigate all african countries collected data such as FGM
The MICS portal was used to filter available datasets based on:

Geographic Focus: African continent, specifically targeting nations with documented FGM/C prevalence.

Survey Recency: Priority was given to MICS6 (2017–2021) and MICS7 (2022–Present) rounds to ensure the analysis reflects current demographic trends.

Module Availability: Datasets were screened to ensure they contained both the Child Protection (FGM/C) module and comprehensive Sociodemographic (Religion/Ethnicity) indicators.

*[UNICEF – FGM Data / Country Profiles:       National FGM prevalence (%), disaggregated by age, region, and education level](https://data.unicef.org/wp-content/uploads/2019/10/XLS_FGM-Girls-prevalence-database_Mar-2025.xlsx?client_id=1697529985.1761937585&session_id=1087509450)

* [Pew Research Center: Religious Composition by Country      Percentage of major religions by country](https://www.pewresearch.org/religion/feature/religious-composition-by-country-2010-2020/)

 For further analysis I may also use these datasets
* [Fertility Rates](https://databank.worldbank.org/indicator/NY.GDP.PCAP.CD/1ff4a498/Popular-Indicators#advancedDownloadOptions)
*  [Prevalence of HIV](https://databank.worldbank.org/indicator/NY.GDP.PCAP.CD/1ff4a498/Popular-Indicators#advancedDownloadOptions)
*  [life expectancy ](https://databank.worldbank.org/indicator/NY.GDP.PCAP.CD/1ff4a498/Popular-Indicators#)

  

# Methodology
To investigate the correlation between religious demographics and FGM/C prevalence, this study utilizes a stratified sampling strategy. Rather than analyzing countries at random, five nations were selected to represent distinct archetypes of religious composition, geographic location, and cultural history.These countries are listed below:
### 1. Nigeria 🇳🇬 (The "Split" Demographic)
Selection Criterion: Religious Parity.

Rationale: Nigeria offers a unique natural experiment due to its near-even demographic split between a predominantly Muslim North and a predominantly Christian South. This allows for a comparative analysis of FGM/C prevalence across two major world religions within a single national legislative framework, helping to isolate the variable of religious affiliation.

### 2. Guinea 🇬🇳 (The Hyper-Prevalence Case)

**Selection Criterion:** Near-Universal Prevalence across all religious demographics.

**Rationale:** Ranking second globally in FGM/C prevalence, Guinea presents a unique environment where the practice is virtually ubiquitous (~96-97%). Unlike regions where FGM is isolated to specific ethnic enclaves, our analysis reveals that high prevalence rates are sustained across **all major religious groups** (both Muslim and Christian). This uniformity—consistently high regardless of the specific faith and often contrasting with non-religious groups—suggests that in Guinea, FGM operates as a deeply entrenched national social norm that transcends theological differences.

### 3. Kenya 🇰🇪 (The Ethnic Variable)
Selection Criterion: Ethnic Diversity & Lower National Prevalence.

Rationale: Unlike nations with universal prevalence, Kenya has immense variation in FGM/C rates between regions. It is selected to test the "Ethnicity Hypothesis"—specifically, whether FGM/C rates cluster more significantly around tribal/ethnic lines (e.g., Somali, Kisii, Masai) rather than religious groups, suggesting that culture may be a stronger driver than faith.

### 4. Sierra Leone 🇸🇱 (The Secret Society Factor)
Selection Criterion: Non-Religious Cultural Drivers.

Rationale: West Africa presents unique cultural dynamics where FGM/C is often linked to rites of passage within secret societies (such as the Bondo Society). Sierra Leone is included to examine drivers of the practice that may exist entirely outside of, or parallel to, organized Abrahamic religions (Islam/Christianity).

# Exploratory Data Analysis (EDA)
To ensure the statistical validity of the results, a standardized EDA pipeline was applied to each of the five country datasets (Kenya, Nigeria, Egypt, Ethiopia, Sierra Leone).

### Data Wrangling & Merging
A critical challenge with UNICEF MICS data is the separation of variables across different files.
* **The Problem:** FGM/C status is recorded in the *Women’s Questionnaire* (`wm.sav`), while Religious Affiliation is often recorded in the *Household Questionnaire* (`hh.sav`).
* **The Solution:** A merging algorithm was implemented to link individual women to their household heads using composite keys (`Cluster Number` + `Household Number`).
* **Data Cleaning:**
    * **Standardization:** Variable labels (e.g., `FG3`, `HC1A`) were mapped to human-readable names (`FGM_Status`, `Religion`).
    * **Null Handling:** Respondents with "Missing", "Don't Know", or "Undetermined" values for FGM status were excluded to prevent skewing the binary classification.
    * **Sample Filtering:** In datasets with extreme class imbalance (e.g., Kenya), religious groups with statistically negligible sample sizes (n < 30) were excluded from comparative analysis to ensure robustness.
      for example Kenya had a sample size 2 of non-religious people.


###  Bivariate Analysis Strategy
For each country, we analyzed the relationship between the categorical variables using the following methods:

1.  **Cross-Tabulation:** Calculated the prevalence (%) of FGM within each religious denomination (Muslim, Catholic, Protestant, Coptic, Traditional, etc.) and education .
2.  **Chi-Square Test of Independence ($\chi^2$):**
    * *Null Hypothesis ($H_0$):* There is no association between religious affiliation and FGM prevalence.
    * *Alternative Hypothesis (H_1):* Statistical dependence exists between the two variables.
    * *Significance Level:* $\alpha = 0.05$.
3.  **Control Variable Testing:** Where applicable, similar tests were run against **Ethnicity/Tribe** and **Region** to detect confounding variables (e.g., checking if FGM tracks with the Somali tribe rather than the Islamic faith in Kenya).

###  Visualization
Bar charts were generated to visualize the conditional probability $P(\text{FGM} | \text{Religion})$. These visualizations highlight the variance—or lack thereof—between different faith groups within the same national border.

