# Methodology

## 1. Principles
- Raw data is never edited; all changes happen in Power Query so they are repeatable.
- Outliers are **flagged**, not deleted, so the effect can be measured.
- Medians are preferred over means (prices are right-skewed).
- Every removal rule is counted in the cleaning log.

## 2. Cleaning Rules (Power Query)
| # | Rule | Reason |
|---|---|---|
| 1 | Drop contact/phone/agent-name columns | Privacy |
| 2 | Trim, clean, and standardize text (case, Arabic/English variants) | Consistent grouping |
| 3 | Convert price and area to numbers (remove commas, currency text, "sqm" etc.) | Calculations |
| 4 | Remove exact duplicates by listing id; then near-duplicates (same area + price + size + bedrooms) | Avoid double counting |
| 5 | Rent only: keep monthly; convert yearly / 12; drop daily rentals | Comparable rent basis |
| 6 | Remove rows with missing price or area | Cannot compute price/sqm |
| 7 | Sanity filters: price > 0; area between 20 and 2,000 sqm (adjust after profiling); bedrooms 0-10 | Remove data-entry errors |
| 8 | Map location names to standard City / Area using `Map_Location` | Same place spelled differently |
| 9 | Standardize finishing: Fully Finished / Semi Finished / Core & Shell / Unknown | Comparable groups |
| 10 | Standardize delivery: Ready / Under Construction / Unknown | Off-plan prices differ |
| 11 | Create PricePerSqm and SizeBand | Core metrics |
| 12 | Flag outliers with IQR (below) | Robust to skew |

### Cleaning Log (fill during Phase 2)
| Step | Rows before | Rows removed | Rows after |
|---|---|---|---|
| Raw sale + rent | | | |
| Duplicates | | | |
| Missing price/area | | | |
| Sanity filters | | | |
| Rent period filter | | | |
| **Final** | | | |

Check: `raw rows = final rows + all removed rows`.

## 3. Outlier Detection
Group: (City, PropertyType, ListingType). On PricePerSqm:
- Q1, Q3, IQR = Q3 - Q1
- Lower bound = Q1 - 1.5 x IQR, Upper bound = Q3 + 1.5 x IQR
- `IsOutlier = TRUE` if outside bounds. Groups with fewer than 10 rows are not flagged.
- Report key numbers with and without outliers.

## 4. Metric Definitions
- **Price per sqm (sale)** = Price / AreaSqm
- **Monthly rent per sqm** = MonthlyRent / AreaSqm
- **Gross Rental Yield %** = (Median monthly rent per sqm x 12) / Median sale price per sqm
- **Payback years** = 1 / Gross Rental Yield
- **Deviation %** = (listing PricePerSqm - median PricePerSqm of same Area + Type + ListingType) / that median
- **Valuation Label** (default thresholds, adjustable):
  - Deviation > +20% -> Overpriced
  - Deviation < -20% -> Opportunity
  - Between -> Fair
  - Group size < 5 -> Insufficient data

## 5. Statistical Analysis (Excel)
- Descriptive statistics per city and property type
- Pearson correlation between AreaSqm and Price
- Multiple regression on ln(Price): AreaSqm, Bedrooms, Finishing (dummy), Location group (dummy). Report R-squared and interpret coefficients as approximate % effects.

## 6. Assumptions & Limitations (be honest in README)
- Listings show **asking** prices/rents, not final deal prices.
- Yield is **gross** (no maintenance, vacancy, taxes, or fees).
- Yield compares medians of two different listing populations (sale vs rent) in the same area; it is an indicator, not a guarantee.
- Developer listings with installment plans may show higher prices than cash prices.
- EGP value changes quickly; results are valid for the scrape date only.
- Scraped data may not cover the whole market; results describe the sample.
