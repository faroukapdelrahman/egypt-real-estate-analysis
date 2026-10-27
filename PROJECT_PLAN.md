# Project Plan: Egypt Real Estate - Prices & Rental Yield Analysis

**Owner:** Farouk Abdelrahman  
**Tools:** Power Query, Excel (Pivot Tables + Power Pivot), DAX, Power BI  
**Repo:** `egypt-real-estate-analysis`  
**Estimated duration:** 11 working days (flexible)

---

## 1. Objective
Build an end-to-end analytics project on Egyptian property listings (sale + rent) that answers:
1. Where are prices high or low (price per sqm by city / area / type)?
2. Which listings are overpriced or good opportunities compared with their own area and type?
3. Which areas give the best **gross rental yield** (investment angle)?

The differentiator versus typical real estate dashboards: **sale + rent together -> rental yield**, plus a **price valuation label** per listing.

## 2. Scope
**In scope**
- Sale and rent listings (apartments, villas, duplexes, studios, townhouses, etc.)
- Geographic scope to be confirmed after seeing the scraped data (start with Greater Cairo + Alexandria; add more only if volume is enough)
- Cleaning, modeling, statistics, DAX, dashboard, documentation

**Out of scope**
- Price forecasting / machine learning
- Writing scraping code (a ready-made scraper or dataset is used)
- Collecting personal data (agent phone numbers, emails, names)

## 3. Role of each tool
| Tool | Role |
|---|---|
| Power Query | Import, clean, standardize, split into fact + dimensions |
| Excel (Pivot Tables) | Fast exploration, validation of numbers, statistics |
| Power Pivot + DAX | Star schema, relationships, measures, calculated columns |
| Power BI | Final interactive dashboard (migrated from the Excel model) |
| Git / GitHub | Version control, one push per phase |

**Workflow trick:** build Queries + Data Model + DAX in Excel first, then in Power BI Desktop use  
`File -> Import -> Power Query, Power Pivot, Power View` to migrate everything instead of rebuilding.

## 4. Deliverables (Definition of Done)
- [ ] Raw data (untouched) in `data/raw/`
- [ ] Cleaned data + cleaning log with row counts
- [ ] Power Query scripts exported to `powerquery/`
- [ ] Excel workbook with pivots + statistics (`excel/`)
- [ ] Star schema diagram (`model/data_model.png`) + `model/dax_measures.md`
- [ ] Power BI file (`powerbi/`) with 4 pages
- [ ] Dashboard screenshots in `images/`
- [ ] `docs/` fully filled (questions, dictionary, methodology, dashboard spec, progress log)
- [ ] README with real insights (no placeholders left)
- [ ] LinkedIn post + CV bullet
- [ ] Git tag `v1.0`

---

## 5. Phases

### Phase 0 - Setup (Day 1)
**Purpose:** a clean repo and clear questions before touching data.
- [ ] Create empty public repo `egypt-real-estate-analysis` on GitHub
- [ ] Unzip the starter folder, `git init`, first push
- [ ] Review and adjust `docs/business_questions.md`
- [ ] Fill the "Decisions" table in `docs/progress_log.md`

```bash
cd egypt-real-estate-analysis
git init
git add .
git commit -m "chore: initial project structure and plan"
git branch -M main
git remote add origin https://github.com/faroukapdelrahman/egypt-real-estate-analysis.git
git push -u origin main
```

### Phase 1 - Data Collection (Day 2-3)
**Purpose:** get raw sale and rent data and keep it untouched.
- [ ] Choose source (ready dataset or paid scraper) and record it in `docs/progress_log.md`
- [ ] Scrape/download **sale** and **rent** separately; **exclude contact/phone fields**
- [ ] Save as `data/raw/sale_listings_raw.csv` and `data/raw/rent_listings_raw.csv` (never edit them)
- [ ] Record: scrape date, source, row count, column list
- [ ] Fill the raw section of `docs/data_dictionary.md` from the REAL columns
- [ ] Targets (adjust to reality): 3,000+ sale rows and 1,500+ rent rows before cleaning
- [ ] Check file sizes (GitHub limit 100 MB per file)

**Git:** `data: add raw sale and rent listings` + `docs: data dictionary (raw columns)`

### Phase 2 - Power Query Cleaning (Day 4-5)
**Purpose:** turn messy listings into a trustworthy table.
- [ ] Load both files as **Connection only** staging queries (`stg_Sale`, `stg_Rent`)
- [ ] Apply the rules in `docs/methodology.md` section 2 (types, text cleaning, units, duplicates, sanity filters)
- [ ] Standardize location names (mapping table `Map_Location`, merge to unify spellings)
- [ ] Standardize finishing, property type, delivery status
- [ ] Append sale + rent into `Fact_Listings` with a `ListingType` column
- [ ] Add `PricePerSqm` and `IsOutlier` (IQR rule, flag only, do not delete)
- [ ] Build dimensions: `Dim_Location`, `Dim_PropertyType`, `Dim_Finishing`, `Dim_Date` (if dates exist)
- [ ] Fill the **Cleaning Log** table (rule -> rows removed) and check: raw = clean + removed
- [ ] Export each query's M code to `powerquery/*.pq`
- [ ] Turn on Column Quality / Distribution and confirm no errors in key columns

**Git:** `feat(powerquery): cleaning queries and dimensions` + `data: processed listings`

### Phase 3 - Excel Exploration & Statistics (Day 6)
**Purpose:** understand the data and prove the numbers before DAX.
- [ ] Pivot: median/avg price per sqm by city, area, type, bedrooms (sale and rent separately)
- [ ] Descriptive stats (count, mean, median, SD, Q1, Q3, IQR, skewness) for price, area, price/sqm
- [ ] Box & Whisker charts per city to show outliers
- [ ] `CORREL` between area and price; scatter with trendline
- [ ] Regression (Data Analysis ToolPak): `ln(price)` ~ area + bedrooms + finishing + location group; report R^2 and the main coefficients in plain words
- [ ] Compare Outlier flag results with and without outliers
- [ ] Write key findings (5-8 bullets) in `docs/progress_log.md`
- [ ] Save as `excel/exploration.xlsx`

**Git:** `analysis(excel): pivots, descriptive stats, regression`

### Phase 4 - Data Model & DAX (Day 7-8)
**Purpose:** a clean star schema with reusable measures.
- [ ] Relationships: dimensions (1) -> `Fact_Listings` (many), single direction
- [ ] Hide key columns and the fact table's raw columns from client tools
- [ ] Create measures from `model/dax_measures.md` (organized by display folder)
- [ ] Create calculated columns: `Area Type Median PPSQM`, `Deviation %`, `Valuation Label`
- [ ] **Validate each key measure against the Excel pivot** (write results in the validation table inside `dax_measures.md`)
- [ ] Export the model diagram to `model/data_model.png`

**Git:** `model: star schema and DAX measures`

### Phase 5 - Power BI Dashboard (Day 9-10)
**Purpose:** a polished, readable, interactive dashboard.
- [ ] Import the Excel model into Power BI Desktop
- [ ] Build the 4 pages exactly as in `docs/dashboard_spec.md`
- [ ] Add slicers, tooltips pages, drill-through (Area -> listings), bookmarks navigation
- [ ] Apply a single theme (colors, fonts), align everything to a grid
- [ ] Check performance (Performance Analyzer), check mobile layout if needed
- [ ] Take screenshots of each page -> `images/page1_overview.png` ...
- [ ] Save `.pbix` (and check size < 100 MB)

**Git:** `feat(powerbi): dashboard pages` + `docs: screenshots`

### Phase 6 - Publish (Day 11)
**Purpose:** make the project easy to understand in 2 minutes.
- [ ] Replace every placeholder in `README.md` with real results and screenshots
- [ ] Write the "Key Insights" section from your actual findings (specific, no invented numbers)
- [ ] Write limitations honestly (asking prices, not final deals, etc.)
- [ ] LinkedIn post (problem -> approach -> 3 insights -> link) and CV bullet
- [ ] Final review: broken links, typos, file names

```bash
git add .
git commit -m "docs: final README, insights and screenshots"
git push
git tag v1.0
git push origin v1.0
```

---

## 6. Git Rules
- One push **at the end of every phase** (more often if you wish)
- Commit prefixes: `chore`, `data`, `docs`, `feat`, `analysis`, `model`, `fix`
- Never commit: temp Excel files (`~$*`), personal contact data, files > 100 MB
- Raw data is never modified after commit

## 7. Risks & Mitigations
| Risk | Mitigation |
|---|---|
| Scraper blocked or costs more than expected | Start with a small test run; fall back to a ready dataset |
| Too few **rent** listings per area | Group small areas into zones; show "Insufficient data" when n < 5 |
| Asking prices differ from real deal prices | State clearly in README as a limitation |
| Off-plan / installment prices inflate sale prices | Keep a `DeliveryStatus` column and compare ready vs off-plan separately |
| Rent listings mix daily / monthly / yearly | Keep monthly only, convert yearly/12, drop daily (log the counts) |
| Currency changes quickly (EGP) | Always state the scrape date; avoid mixing scrape dates |
| Personal data in scraped files | Drop contact columns immediately; never commit them |
| Large files | Keep `.pbix` and CSVs under 100 MB each |

## 8. Daily Habit
At the end of each work session update `docs/progress_log.md` (what was done, row counts, decisions, next step), then commit.
