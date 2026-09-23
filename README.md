# NSW Housing Affordability & Rental Stress Analytics

**Power BI | Python | DAX | Power Query | Excel | Statistical Scenario Modelling**

An end-to-end housing analytics project examining how **property prices, rents, household income, market activity and housing segments interact across Sydney**.

The project combines historical market analysis, postcode-level investigation and SA4-level scenario forecasting to answer:

> **Where is housing pressure concentrated, what type of pressure is it, what is driving it, how reliable are the observed signals, and how could those pressures evolve under different market scenarios?**

**Historical Period:** 2022-Q1 to 2025-Q3  

**Forecast Horizon:** 2025-Q4 to 2026-Q3  

**Geographic Scope:** Sydney Metro, using NSW housing and Census datasets
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

The Power BI semantic model follows a **fact-constellation architecture built using dimensional-modelling principles**. Two conformed dimensions provide the shared context for historical analysis: `postcode_dim` for geography and `Date_Dim` for reporting quarters. Independent sales and rental fact tables retain their natural grain.

| Table | Grain | Purpose |
| --- | --- | --- |
| `postcode_dim` | Postcode | Shared postcode, suburb, LGA and SA4 geography |
| `Date_Dim` | Historical quarter | Shared time filtering for sales and rental facts |
| `sales_fact` | Postcode × quarter | Sales prices and transaction activity |
| `sales_fact_dwelling` | Postcode × quarter × dwelling type | Sales analysis by dwelling category |
| `rent_fact_summary` | Postcode × quarter | Weekly rents and bond activity |
| `rent_dwelling` | Postcode × quarter × dwelling type | Rental analysis by dwelling category |
| `rent_bedroom` | Postcode × quarter × bedroom count | Rental analysis by bedroom category |
| `rent_detail` | Postcode × quarter × dwelling type × bedroom count | Detailed rental segment analysis |
| `income_expenditure_dim` | Postcode | 2021 Census income and household context for affordability measures |
| `forecast_sa4_dim` | SA4 | Regional dimension linking forecasts with historical geography |
| `forecast_summary` | SA4 × future quarter × metric × scenario | Low, Base and High sales and rent projections |

Each historical fact table connects independently to the shared dimensions. Preserving these grains avoids duplicated values and unnecessary fact-to-fact relationships while allowing sales, rental and segment measures to be compared under consistent filters.

The model is **modular**: the SA4-level forecasting layer was added through `forecast_sa4_dim` and `forecast_summary` without restructuring the historical postcode-level tables or their business logic. Forecast periods remain separate from the historical `Date_Dim`.

**Affordability interpretation:** `income_expenditure_dim` uses a fixed 2021 Census income baseline. Price-to-income ratios and rental burden therefore compare changing housing costs with 2021 income, rather than income measured in each quarter.

**[DATA MODEL]**

<img width="571" height="353" alt="image" src="https://github.com/user-attachments/assets/08defad9-4fe6-45f9-a24f-c8ddb563ee8c" />

➡️ **Detailed documentation:** [Data Model](./Data%20Model/) 


## 🔄 Data Transformation and Integration

Quarterly NSW sales and rental files were combined with 2021 Census data and ABS geography to support analysis from **2022-Q1 to 2025-Q3**. The workflow standardised reporting periods and data types, preserved each dataset’s level of detail, and produced geographic boundaries for Power BI Shape Maps.

### Dataset source files

| Dataset | Provider | Primary use | Source |
| --- | --- | --- | --- |
| Quarterly NSW sales tables | NSW Communities and Justice | Property prices and transaction activity | [Current Rent & Sales Reports](https://dcj.nsw.gov.au/about-us/families-and-communities-statistics/housing-rent-and-sales/rent-and-sales-report.html) |
| Historical NSW sales tables | NSW Communities and Justice | Historical quarterly sales data | [Previous Rent & Sales Reports](https://dcj.nsw.gov.au/about-us/families-and-communities-statistics/housing-rent-and-sales/previous-rent-and-sales-reports.html) |
| Quarterly NSW rental tables | NSW Communities and Justice | Weekly rents, new bonds and total bond activity | [Current Rent & Sales Reports](https://dcj.nsw.gov.au/about-us/families-and-communities-statistics/housing-rent-and-sales/rent-and-sales-report.html) |
| Historical NSW rental tables | NSW Communities and Justice | Historical quarterly rental data | [Previous Rent & Sales Reports](https://dcj.nsw.gov.au/about-us/families-and-communities-statistics/housing-rent-and-sales/previous-rent-and-sales-reports.html) |
| 2021 Census DataPacks | Australian Bureau of Statistics | Household income, mortgage, rent and demographic context | [ABS Census DataPacks](https://www.abs.gov.au/census/find-census-data/datapacks) |
| 2021 SA4 boundaries | Australian Bureau of Statistics | Regional Shape Map boundaries | [ABS Digital Boundary Files](https://www.abs.gov.au/statistics/standards/australian-statistical-geography-standard-asgs/edition-3-july-2021-june-2026/access-and-downloads/digital-boundary-files) |
| 2021 Postal Area boundaries | Australian Bureau of Statistics | Postcode-level Shape Map boundaries | [ABS Digital Boundary Files](https://www.abs.gov.au/statistics/standards/australian-statistical-geography-standard-asgs/edition-3-july-2021-june-2026/access-and-downloads/digital-boundary-files) |

### Transformation workflow

1. **Combine quarterly files:** Reusable import logic appended sales and rental workbooks into master datasets. The year, quarter and `YYYY-Qn` period were derived from each source filename.
2. **Clean and standardise:** Suppressed (`s`) and unavailable (`-`) values became null, rather than zero. Numeric formatting was removed before type conversion; invalid essential price observations were excluded, and relevant small-sample flags were retained.
3. **Preserve analytical grain:** Sales and rental records were split into postcode-period summary tables and separate dwelling, bedroom and detailed segment tables. This prevents summary values from being repeated or double counted across segment rows.
4. **Integrate geography and Census data:** Postcodes were matched to LGA and SA4 attributes, coordinates and broader reporting groups in `postcode_dim`. The 2021 Census data was aligned by postcode in `income_expenditure_dim` to provide income, housing-cost and household context.
5. **Prepare map boundaries:** ABS SA4 and Postal Area shapefiles were filtered, simplified and converted to TopoJSON. The resulting SA4 and postcode identifiers were matched to Power BI model keys for Shape Maps.

### Validation

Checks covered reporting-period completeness, postcode matches and duplicates, coordinate ranges, numeric conversions, market-value distributions, and map-key consistency. Summary and detailed transaction counts were also compared with source files. For example, Sutherland’s **2023-Q4** postcode summary reported **963 sales**, while visible dwelling categories totalled **868**; the **95-sale difference** reflected suppressed categories. Published summary totals were therefore retained rather than reconstructed from visible detail alone.

➡️ **Detailed preprocessing workflow:** [Data Preprocessing](./Data%20Preprocessing/)

---

# 📐 Analytical Methodology

The dashboard uses DAX measures to assess housing costs, market change and the reliability of observed trends.

| Analytical area | Method |
| --- | --- |
| **Purchase affordability** | Median sales price ÷ annual household income |
| **Rental affordability** | Median weekly rent ÷ median weekly household income |
| **Rent versus inflation** | Compares current rent with a CPI-adjusted 2021 Census rent baseline |
| **Market growth** | Tracks quarter-on-quarter change and sales and rent indexes set to **100 in 2022-Q1** |
| **Growth benchmarking** | Compares observed growth with an assumed quarterly growth rate compounded over elapsed quarters |
| **Reliability and volatility** | Assesses sales volume, small samples, extreme prices, rental bond coverage and variation in historical year-on-year sales growth |

Purchase and rental affordability use **2021 Census household income as a fixed baseline**. Changes in these ratios therefore reflect changes in housing costs relative to 2021 income, rather than measured quarterly changes in household income. The expected-growth measures are analytical benchmarks, not forecasts.

For deeper rental analysis, users can switch between **dwelling type and bedroom count** to compare median rent, rental burden and reported new bonds. Segment measures also show how each category’s burden has changed since 2022-Q1 and how it differs from the overall market. Field parameters, dynamic titles and tooltips adapt these views to the selected context.

The forecasting measures translate scenario sales prices and weekly rents into **projected price-to-income ratios and rental burden**, then classify regional affordability pressure. The [forecasting methodology](./DAX%20%26%20Forecasting%20Methodology/) explains how the projected values are generated.

➡️ [Detailed DAX documentation](./DAX%20%26%20Forecasting%20Methodology/)

---

# 📈 Forecasting Methodology

The project uses a **scenario-based framework** to project sales prices and weekly rents across 12 Sydney SA4 regions. Starting from 2025-Q3 actual values, it produces Low, Base and High scenarios for four quarters, from **2025-Q4 to 2026-Q3**. Sales and rent are forecast separately.

### Scenario growth assumptions

For each SA4 and metric, quarterly growth assumptions are derived from historical quarter-on-quarter growth:

| Scenario | Quarterly growth assumption |
| --- | --- |
| **Low** | 10th percentile of historical growth |
| **Base** | 68% × recent four-quarter average growth + 32% × full-period median growth |
| **High** | 90th percentile of historical growth |

The percentiles reflect each region’s observed growth distribution, while the Base scenario balances recent movement with the longer historical pattern.

### Quarterly projection

For each SA4 and metric (sales or rent), the scenario growth rate is blended with recent growth. The result is constrained to that region’s 10th–90th percentile range of historical quarterly growth:

Blended growth = (0.8 × Scenario growth) + (0.2 × Recent four-quarter average growth)

Applied growth = MEDIAN(10th percentile, 90th percentile, Blended growth)

Next-quarter forecast = Current-quarter value × (1 + Applied growth)
Here, \(G_{\text{recent}}\) is the recent four-quarter average growth rate. The forecast for each quarter becomes the starting value for the next, creating a **chained four-quarter projection**. The regional percentile bounds limit unusually large quarterly changes without applying the same fixed cap to every market.

Projected sales prices and rents can then be compared with the fixed **2021 Census household-income baseline** to estimate future price-to-income ratios and rental burden. These are scenario estimates relative to 2021 income, rather than forecasts of household income.

> The Low, Base and High paths show a historically grounded range of possible outcomes. They are not exact predictions or probabilities of future prices and rents.

For the full scenario calculations, projection formula and regional growth bounds, see the [detailed forecasting methodology](./DAX%20%26%20Forecasting%20Methodology/).

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

➡️ **Detailed regional analysis:** [Region Wise Findings](./Region%20Wise%20Findings/)

Detailed regional analysis was conducted for five Sydney SA4 regions:

- Parramatta
- Blacktown
- City & Inner South
- Inner South West
- North Sydney & Hornsby

The analysis moves from the regional position into postcode-level patterns and, where relevant, dwelling-type, bedroom-count and market-activity drivers.

---

## 🏙️ Parramatta

<img width="605" height="234" alt="image" src="https://github.com/user-attachments/assets/9333a99c-5e1c-404c-876c-7fecc53248bf" />

* **Regional headline:** By 2025-Q3, Parramatta recorded a median sales price of approximately **$1.15M**, median weekly rent of **$650**, a **11.8x Price-to-Income ratio**, **36.8% rent burden**, and around **2,682 sales**.

* **Trend:** Rental affordability deteriorated substantially across the analysis period. Across the comparable postcode sample, median rent burden increased from roughly **27.7% in 2022-Q2 to 37.2% by 2025-Q3**, while rental growth remained materially ahead of sales-price growth.

* **Geographic drilldown:** Affordability pressure varied considerably within the region. Postcode areas represented by **Villawood, Bass Hill, Chester Hill and Guildford** showed stronger combined rental and purchase pressure, while areas including **Merrylands, Berala and South Granville** were more strongly rent-led.

* **Segment insight:** The ownership market showed a major split between strata and non-strata property. From 2022-Q1 to 2025-Q3, average postcode median **non-strata prices increased by approximately 27.3%**, compared with only around **1.9% for strata**, despite strata representing roughly **64% of reported sales in 2025-Q3**.

* **Affordability interpretation:** Rental pressure was broad rather than being restricted to the largest properties. **2-bedroom properties represented the core rental market**, while 3BR and particularly 4+BR properties showed substantially greater absolute affordability pressure.

* **Standout:** Postcode 2151, represented as **North Rocks**, recorded a non-strata price of approximately **$2.15M but only 36 non-strata sales**, while **Winston Hills** recorded a similar ~$2.00M price with **179 non-strata sales**. This demonstrated why headline price should be interpreted alongside **property structure and transaction depth**.

---

## 🏘️ Blacktown

<img width="602" height="233" alt="image" src="https://github.com/user-attachments/assets/2eb33658-c15a-4ac0-b6b7-c9dbcd07f3f4" />

* **Regional headline:** By 2025-Q3, Blacktown recorded a median sales price of approximately **$1.25M**, median weekly rent of **$662.50**, a **10.5x Price-to-Income ratio**, **27.8% rent burden**, and around **1,361 sales**.

* **Affordability context:** Blacktown's weekly rent was slightly higher than Parramatta's, yet its measured rent burden was substantially lower. The difference was largely driven by Blacktown's higher **2021 median weekly household-income base**, illustrating the importance of interpreting the denominator behind affordability metrics.

* **Rental-market structure:** Blacktown was strongly oriented toward **family-sized housing**, with houses accounting for approximately **three-quarters of reported new rental-bond activity**.

* **Bedroom insight:** **3-bedroom and 4+ bedroom properties represented a large share of the rental market**, making larger-property affordability especially relevant to the region.

* **Affordability interpretation:** Blacktown therefore combined relatively high nominal housing costs with a stronger household-income base, meaning that absolute rent levels alone overstated its relative rental pressure compared with some other Sydney regions.

* **Standout:** Blacktown demonstrated that **similar or even higher rents do not necessarily imply greater affordability stress** — the income base, housing mix and household structure materially change the interpretation.

---

## 🌆 City & Inner South

<img width="602" height="235" alt="image" src="https://github.com/user-attachments/assets/c8aafd88-d288-41da-abc2-c039fb57eeb6" />

* **Regional headline:** By 2025-Q3, City & Inner South recorded a median sales price of approximately **$1.17M**, median weekly rent of **$840**, a **10.0x Price-to-Income ratio**, **37.3% rent burden**, and around **1,783 sales**.

* **Trend:** The strongest finding was a clear **rent-sales divergence**. Rental growth substantially exceeded sales-price growth even across postcode areas where sales prices were stagnant, declining or increasing.

* **Geographic drilldown:** **Zetland** recorded approximately **+1.5% sales-price growth versus +54.4% rent growth**, while **Barangaroo** recorded around **-11.5% sales growth versus +25.2% rent growth**. **Eastlakes** showed that the same divergence could occur even where sales prices were rising, with approximately **+14.0% sales growth versus +55.6% rent growth**.

* **Affordability interpretation:** Median Rent Burden increased from approximately **26% in 2022-Q2 to 37% by 2025-Q3**, while Price-to-Income moved much less dramatically. Rent-v-CPI also showed rents running materially above their inflation-implied path.

* **Segment insight:** **Flats/units were the broadest source of rental pressure**, accounting for approximately **76% of reported new rental bonds** and covering all analysed postcode areas. Their scale made them more important regionally than their burden level alone would suggest.

* **Standout:** Bedroom analysis revealed a clear scale-versus-severity trade-off. **2BR properties combined high market coverage with substantial pressure**, while **3BR properties reached approximately 56% rent burden**, representing much more acute affordability stress despite lower market volume.

---

## 🏘️ Inner South West

<img width="605" height="233" alt="image" src="https://github.com/user-attachments/assets/7aea1e15-dce3-44bf-b8da-b760b8247d59" />

* **Regional headline:** By 2025-Q3, Inner South West recorded a median sales price of approximately **$1.21M**, median weekly rent of **$716.50**, a **12.6x Price-to-Income ratio**, **39.8% rent burden**, and around **2,085 sales**.

* **Trend:** The region moved from relatively balanced rent and sales-price movements in 2022 to a clearly **rent-led affordability problem from 2023 onward**. By 2025-Q3, median rent growth across the comparable sample was approximately **+48.1%**, compared with around **+14.2% sales-price growth**.

* **Geographic drilldown:** Highly active postcode areas including those represented by **Arncliffe, Hurstville Grove, Bankstown Aerodrome and Banksia** showed substantial rental pressure despite functioning ownership markets.

* **Combined-pressure areas:** Postcode areas represented by **Lugarno, Punchbowl, Padstow and East Hills** faced stronger pressure across both renting and purchasing, showing that the regional affordability problem was not uniform.

* **Segment insight:** Flats/units and 2BR properties formed the high-volume mainstream rental market and appeared to stabilise at elevated burden levels. In contrast, **houses, townhouses and 3BR+ properties showed substantially greater affordability severity**, including approximately **44.6% burden for 3BR and 63.2% for 4+BR properties**.

* **Standout:** **Mortdale, Belmore and Bardwell Park** showed that strong rental escalation could occur even where sales-price growth was weak or volatile. Recent moderation in some of these markets therefore represented **slower growth from an elevated level rather than a full affordability reversal**.

---

## 🌉 North Sydney & Hornsby

<img width="603" height="233" alt="image" src="https://github.com/user-attachments/assets/42e9cca9-40e5-405d-a360-2944c00f070b" />

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

