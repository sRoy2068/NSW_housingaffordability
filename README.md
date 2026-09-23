You’re not far from done. At this point, **do not turn the README into another 20-page technical document**. You already have separate folders for the data model, DAX, preprocessing, forecasting and regional findings. The README should be the **recruiter-facing landing page** that points to those deeper documents.

I’d finish it today with this structure:

# NSW Housing Affordability & Rental Stress Analytics

**Power BI | Python | DAX | Power Query | Excel | Statistical Scenario Modelling**

An end-to-end housing analytics project examining how **property prices, rents, household income, market activity and housing segments interact across Sydney**.

The solution combines historical market analysis with postcode-level investigation and SA4-level scenario forecasting to answer:

> **Where is housing pressure concentrated, what is driving it, how reliable are the observed signals, and how could those pressures evolve under different market scenarios?**

**Historical Period:** 2022-Q1 to 2025-Q3
**Forecast Horizon:** 2025-Q4 to 2026-Q3
**Geography:** Sydney Metro / NSW

---

## 🎯 Project Objectives

Housing affordability cannot be understood from property prices alone.

This project combines **sales prices, weekly rents, household income, transaction activity, rental bond activity and inflation context** to investigate:

* where purchase and rental affordability pressure is highest;
* whether rent and sales-price growth are diverging;
* which suburbs drive regional patterns;
* whether strong growth signals are supported by sufficient market activity;
* which dwelling and bedroom segments contribute most to rental stress; and
* how affordability could change under Low, Base and High future scenarios.

---

## 📊 Dashboard Architecture

The Power BI solution follows a **layered analytical workflow**, moving from regional identification to local diagnosis and scenario analysis.

### 1. Sydney Housing Affordability – Summary

Provides the high-level Sydney Metro view.

Key features include:

* dual maps for **median sales price and median weekly rent**;
* sales and rental affordability KPIs;
* regional affordability comparison;
* indexed rent-versus-sales growth;
* purchase/rental pressure classification.

**[SUMMARY DASHBOARD]**
<img width="569" height="369" alt="image" src="https://github.com/user-attachments/assets/3e9c9412-035e-4967-8ad5-5795b59826ac" />


---

### 2. Housing Affordability Deep Dive

Investigates affordability within a selected SA4 using:

* rent burden;
* price-to-income ratio;
* CPI-adjusted rental pressure;
* suburb-level pressure drivers;
* rental and purchase pressure hotspots.

**[POSTAL LEVEL AFFORDABILITY DASHBOARD]**
<img width="574" height="374" alt="image" src="https://github.com/user-attachments/assets/d8416082-2bba-411b-857c-8de1fbddd69e" />


---

### 3. Postal Level Detail

Drills from SA4 into **postcode and suburb-level behaviour**.

The page helps identify which local areas are driving broader regional trends while comparing:

* sales and rent growth;
* market activity;
* expected growth benchmarks;
* price volatility;
* reliability of observed signals.

**[POSTAL-LEVEL DRILLDOWN]**
<img width="602" height="372" alt="image" src="https://github.com/user-attachments/assets/2a50e285-6fc9-44db-87fb-c6ae6301a0e9" />


---

### 4. Housing Segment Affordability – Deep Dive

Analyses rental affordability by:

* dwelling type;
* bedroom count;
* weekly rent;
* rent burden;
* CPI-adjusted pressure;
* reported rental activity.

Field parameters allow users to dynamically switch between segment perspectives.

**[SEGMENT DASHBOARD]**
<img width="608" height="374" alt="image" src="https://github.com/user-attachments/assets/422ca1fc-41be-42a7-b36c-7806478ba6a6" />


---

### 5. Regional Forecast Outlook

Provides **Low, Base and High scenarios** for future sales prices and weekly rents at SA4 level.

Forecast values are translated into:

* projected price-to-income ratio;
* projected rent burden;
* purchase pressure;
* rental pressure;
* dual-pressure regions.

**[REGIONAL FORECAST DASHBOARD]**
<img width="657" height="376" alt="image" src="https://github.com/user-attachments/assets/0dd48d3a-8a05-4f78-adcc-b60e91a5fde4" />

---

# 🧩 Data Model

The Power BI semantic model follows a **fact-constellation architecture built using dimensional-modelling principles**.

Two conformed dimensions sit at the centre of the historical model:

* `postcode_dim` — shared geographic context;
* `Date_Dim` — shared historical time context.

Independent sales and rental fact tables retain their **natural grain** rather than being flattened into one table.

Examples include:

* postcode × quarter;
* postcode × quarter × dwelling type;
* postcode × quarter × bedroom count;
* postcode × quarter × dwelling type × bedroom count.

This reduces duplication, avoids unnecessary fact-to-fact relationships and allows each analytical component to evolve independently.

### Modularity

The model was deliberately designed to be modular.

For example, the **SA4-level forecasting layer was added later through `forecast_sa4_dim` and `forecast_summary` without restructuring the historical postcode-level sales or rental model**.

This allows new analytical subject areas to be introduced while preserving existing fact-table grain and business logic.

**[INSERT DATA MODEL SCREENSHOT]**

➡️ Detailed documentation: **`Data Model/`**

---

# 🧹 Data Preparation & Quality

Data from multiple sources was cleaned, standardised and validated before being introduced into the semantic model.

Important preparation decisions included:

* resolving missing and inconsistent geographic classifications;
* standardising postcode and SA4 mappings;
* handling suppressed observations;
* ensuring suppressed activity was **not interpreted as zero**;
* retaining small-sample indicators;
* flagging extreme sales-price observations;
* validating reporting periods and table grain;
* cleaning invalid rental observations.

Data-quality checks were retained within the analytical layer so that unreliable observations could be identified rather than silently treated as equally credible.

➡️ Detailed workflow: **`Data Preprocessing/`**

---

# 📐 Analytical Methodology

The dashboard uses several complementary measures of housing-market pressure.

### Purchase Affordability

**Median Sales Price ÷ Annual Household Income**

Used to distinguish high nominal prices from high **income-adjusted purchase pressure**.

### Rental Affordability

**Median Weekly Rent ÷ Median Weekly Household Income**

Used to evaluate rental burden relative to household financial capacity.

### Rent vs Inflation

Current rents are compared with an **inflation-adjusted baseline** to identify rental increases that exceed general price growth.

### Growth & Benchmarking

The model includes:

* QoQ growth;
* growth since 2022-Q1;
* indexed sales and rental trends;
* time-adjusted expected-growth benchmarks.

### Reliability & Volatility

Observed market growth is evaluated alongside:

* transaction activity;
* small-sample behaviour;
* extreme-price observations;
* reported-quarter coverage;
* historical YoY price volatility.

This prevents large percentage movements from automatically being interpreted as strong market signals.

➡️ Selected DAX documentation: **`DAX & Forecasting Methodology/`**

---

# 📈 Forecasting Methodology

The project uses a **scenario-based forecasting framework rather than a single deterministic prediction**.

### Low Scenario

Represents weaker growth based on the lower portion (25th percentile) of historical quarterly growth behaviour.

### Base Scenario

Combines:

* **68% recent four-quarter average growth**
* **32% full-period median growth**

to balance recent momentum with longer-term market behaviour.

### High Scenario

Represents stronger growth based on the upper portion (75th percentile) of the historical quarterly growth distribution.

Forecasts are generated independently for **sales and rent** and subsequently translated into projected affordability measures.

The forecast should therefore be interpreted as:

> **A range of plausible market outcomes rather than an exact prediction of future property values.**

---

---

# 🔎 Key Analytical Findings

The analysis surfaced several recurring market patterns across Sydney:

* rental growth materially outpaced sales-price growth over the analysis period;
* affordability pressure is not one-dimensional — some regions are rent-led, others purchase-led, while some face combined pressure;
* similar rents can result in very different affordability outcomes once household income is considered;
* postcode-level conditions can differ significantly from SA4-level averages;
* larger rental properties can carry a substantial affordability premium, particularly for 3-bedroom and 4+ bedroom households;
* expensive regions can contain very different local affordability structures and market segments;
* property structure matters — high-growth non-strata markets can coexist with much higher-volume strata activity;
* similar headline prices can represent very different markets when transaction volume is considered;
* affordability pressure forms clear geographic clusters rather than being evenly distributed;
* forecast performance should be assessed using both error metrics and directional accuracy against actual outcomes.

➡️ Detailed regional analysis: **Region Wise Findings/**

Detailed regional analysis was conducted for five Sydney SA4 regions:

- Parramatta
- Blacktown
- City & Inner South
- Inner South West
- North Sydney & Hornsby

The analysis moves from the regional position into postcode-level patterns and, where relevant, dwelling-type, bedroom-count and market-activity drivers.

---

## 🏙️ Parramatta

* **Regional headline:** By 2025-Q3, Parramatta recorded a median sales price of approximately **$1.15M**, median weekly rent of **$650**, a **11.8x Price-to-Income ratio**, **36.8% rent burden**, and around **2,682 sales**.

* **Trend:** Rental affordability deteriorated substantially across the analysis period. Across the comparable postcode sample, median rent burden increased from roughly **27.7% in 2022-Q2 to 37.2% by 2025-Q3**, while rental growth remained materially ahead of sales-price growth.

* **Geographic drilldown:** Affordability pressure varied considerably within the region. Postcode areas represented by **Villawood, Bass Hill, Chester Hill and Guildford** showed stronger combined rental and purchase pressure, while areas including **Merrylands, Berala and South Granville** were more strongly rent-led.

* **Segment insight:** The ownership market showed a major split between strata and non-strata property. From 2022-Q1 to 2025-Q3, average postcode median **non-strata prices increased by approximately 27.3%**, compared with only around **1.9% for strata**, despite strata representing roughly **64% of reported sales in 2025-Q3**.

* **Affordability interpretation:** Rental pressure was broad rather than being restricted to the largest properties. **2-bedroom properties represented the core rental market**, while 3BR and particularly 4+BR properties showed substantially greater absolute affordability pressure.

* **Standout:** Postcode 2151, represented as **North Rocks**, recorded a non-strata price of approximately **$2.15M but only 36 non-strata sales**, while **Winston Hills** recorded a similar ~$2.00M price with **179 non-strata sales**. This demonstrated why headline price should be interpreted alongside **property structure and transaction depth**.

---

## 🏘️ Blacktown

* **Regional headline:** By 2025-Q3, Blacktown recorded a median sales price of approximately **$1.25M**, median weekly rent of **$662.50**, a **10.5x Price-to-Income ratio**, **27.8% rent burden**, and around **1,361 sales**.

* **Affordability context:** Blacktown's weekly rent was slightly higher than Parramatta's, yet its measured rent burden was substantially lower. The difference was largely driven by Blacktown's higher **2021 median weekly household-income base**, illustrating the importance of interpreting the denominator behind affordability metrics.

* **Rental-market structure:** Blacktown was strongly oriented toward **family-sized housing**, with houses accounting for approximately **three-quarters of reported new rental-bond activity**.

* **Bedroom insight:** **3-bedroom and 4+ bedroom properties represented a large share of the rental market**, making larger-property affordability especially relevant to the region.

* **Affordability interpretation:** Blacktown therefore combined relatively high nominal housing costs with a stronger household-income base, meaning that absolute rent levels alone overstated its relative rental pressure compared with some other Sydney regions.

* **Standout:** Blacktown demonstrated that **similar or even higher rents do not necessarily imply greater affordability stress** — the income base, housing mix and household structure materially change the interpretation.

---

## 🌆 City & Inner South

* **Regional headline:** By 2025-Q3, City & Inner South recorded a median sales price of approximately **$1.17M**, median weekly rent of **$840**, a **10.0x Price-to-Income ratio**, **37.3% rent burden**, and around **1,783 sales**.

* **Trend:** The strongest finding was a clear **rent-sales divergence**. Rental growth substantially exceeded sales-price growth even across postcode areas where sales prices were stagnant, declining or increasing.

* **Geographic drilldown:** **Zetland** recorded approximately **+1.5% sales-price growth versus +54.4% rent growth**, while **Barangaroo** recorded around **-11.5% sales growth versus +25.2% rent growth**. **Eastlakes** showed that the same divergence could occur even where sales prices were rising, with approximately **+14.0% sales growth versus +55.6% rent growth**.

* **Affordability interpretation:** Median Rent Burden increased from approximately **26% in 2022-Q2 to 37% by 2025-Q3**, while Price-to-Income moved much less dramatically. Rent-v-CPI also showed rents running materially above their inflation-implied path.

* **Segment insight:** **Flats/units were the broadest source of rental pressure**, accounting for approximately **76% of reported new rental bonds** and covering all analysed postcode areas. Their scale made them more important regionally than their burden level alone would suggest.

* **Standout:** Bedroom analysis revealed a clear scale-versus-severity trade-off. **2BR properties combined high market coverage with substantial pressure**, while **3BR properties reached approximately 56% rent burden**, representing much more acute affordability stress despite lower market volume.

---

## 🏘️ Inner South West

* **Regional headline:** By 2025-Q3, Inner South West recorded a median sales price of approximately **$1.21M**, median weekly rent of **$716.50**, a **12.6x Price-to-Income ratio**, **39.8% rent burden**, and around **2,085 sales**.

* **Trend:** The region moved from relatively balanced rent and sales-price movements in 2022 to a clearly **rent-led affordability problem from 2023 onward**. By 2025-Q3, median rent growth across the comparable sample was approximately **+48.1%**, compared with around **+14.2% sales-price growth**.

* **Geographic drilldown:** Highly active postcode areas including those represented by **Arncliffe, Hurstville Grove, Bankstown Aerodrome and Banksia** showed substantial rental pressure despite functioning ownership markets.

* **Combined-pressure areas:** Postcode areas represented by **Lugarno, Punchbowl, Padstow and East Hills** faced stronger pressure across both renting and purchasing, showing that the regional affordability problem was not uniform.

* **Segment insight:** Flats/units and 2BR properties formed the high-volume mainstream rental market and appeared to stabilise at elevated burden levels. In contrast, **houses, townhouses and 3BR+ properties showed substantially greater affordability severity**, including approximately **44.6% burden for 3BR and 63.2% for 4+BR properties**.

* **Standout:** **Mortdale, Belmore and Bardwell Park** showed that strong rental escalation could occur even where sales-price growth was weak or volatile. Recent moderation in some of these markets therefore represented **slower growth from an elevated level rather than a full affordability reversal**.

---

## 🌉 North Sydney & Hornsby

* **Regional headline:** By 2025-Q3, North Sydney & Hornsby recorded a median sales price of approximately **$1.82M**, median weekly rent of **$844**, a **12.9x Price-to-Income ratio**, and **30.9% rent burden**.

* **Trend:** Across a comparable Q3 postcode cohort, median rent growth reached approximately **39%**, compared with only around **6% median sales-price growth**, while sales transactions increased by roughly **22%**. The incremental deterioration since 2022 therefore came increasingly from the rental side.

* **Market activity:** The overall rental-bond base remained broadly stable between comparable Q3 periods, but **new bonds fell by approximately 16.9% and rental turnover declined by 1.69 percentage points**. This was more consistent with lower rental mobility than with a shrinking rental market.

* **Geographic drilldown:** The region contained several distinct affordability regimes: a **rent-led Lower North Shore apartment belt**, a **prestige purchase-led belt**, a **combined-pressure Upper North Shore**, and more affordable transition areas such as **Asquith and Lane Cove North**.

* **Segment insight:** The rental market operated at two speeds. **Flats/units and 1–2BR properties dominated rental activity and showed comparatively lower pressure**, while houses and 3–4+BR properties carried much more severe affordability burdens.

* **Standout:** The region could simultaneously record some of Sydney's highest nominal rents while maintaining a comparatively moderate regional rent-burden ratio. Its relatively high household-income base and large apartment/1–2BR rental market moderated the aggregate measure, while **family-sized housing remained considerably more stressed**.

---

## 📌 Cross-Regional Interpretation

The five detailed regional studies show that Sydney's housing affordability problem is not uniform.

Recurring patterns include:

* rental growth materially diverging from sales-price growth;
* similar housing costs producing different affordability outcomes because of household income;
* postcode-level conditions differing substantially from SA4-level averages;
* rent-led, purchase-led and combined-pressure markets existing within the same region;
* high-volume housing segments differing from the segments experiencing the greatest affordability severity;
* and headline prices becoming more meaningful when considered alongside transaction activity and housing structure.

The analysis therefore treats **price, rent, household income, geography, market activity and housing segment as complementary measures**, rather than relying on any single headline affordability indicator.

> **Methodological note:** Price-to-Income and Rent Burden use fixed 2021 Census household income. They are therefore most useful as comparative affordability benchmarks rather than direct estimates of current household financial stress.

---

# 🛠️ Power BI & DAX Capabilities Demonstrated

The project includes practical implementation of:

* dimensional modelling;
* fact-constellation architecture;
* conformed dimensions;
* time intelligence;
* `CALCULATE`;
* `REMOVEFILTERS`;
* `ALLSELECTED`;
* `ISINSCOPE`;
* virtual tables;
* `TOPN`;
* dynamic field parameters;
* drill-through;
* bookmarks;
* dynamic tooltips;
* conditional formatting;
* reliability scoring;
* statistical volatility measures;
* scenario-based reporting.

---

# 🤖 AI-Assisted Development

AI was used as an **analytical and technical copilot** to accelerate DAX troubleshooting, methodology refinement, Power BI debugging, interpretation and documentation.

AI-generated suggestions were evaluated through:

* implementation in the actual model;
* source-data checks;
* filter-context validation;
* visual testing;
* external evidence review.

Final modelling, methodological and analytical decisions remained under human oversight.

---

# ⚠️ Assumptions & Limitations

* Household-income measures use **2021 Census data as a fixed affordability baseline**.
* Income is not independently forecast within the scenario layer.
* Suppressed observations are excluded rather than treated as zero.
* Median prices and rents describe aggregated market behaviour, not individual properties or households.
* Low-activity markets should be interpreted more cautiously.
* Forecast scenarios are not deterministic predictions.
* Observed relationships do not automatically imply causation.

---

# 📁 Repository Structure

```text
NSW_housingaffordability/
│
├── Dashboard/
│   └── Dashboard screenshots and report views
│
├── Data Model/
│   └── Semantic model architecture and relationship documentation
│
├── Data Preprocessing/
│   └── Cleaning, transformation and validation workflow
│
├── DAX & Forecasting Methodology/
│   └── Selected DAX measures and scenario methodology
│
├── Region Wise Findings/
│   └── Regional analytical case studies
│
├── README.md
└── LICENSE
```

# 💡 Project Value

The project was designed as more than a collection of Power BI visuals.

Its analytical workflow moves from:

> **Sydney-wide conditions → regional pressure → postcode/suburb diagnosis → housing-segment drivers → future affordability scenarios**

while maintaining consistent geography, time, data-quality and analytical logic across the semantic model.

