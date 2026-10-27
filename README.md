# Egypt Real Estate Analysis: Prices & Rental Yield

End-to-end analytics project on Egyptian property listings using **Power Query, Excel (Pivot Tables + Power Pivot), DAX, and Power BI**.

> Status: in progress. Replace placeholders (marked `TODO`) with real results before publishing.

## Business Problem
Where are Egyptian property prices high or low, which listings are priced above or below their market, and where does renting pay best compared with buying?

## Key Questions
See [`docs/business_questions.md`](docs/business_questions.md).

## Data
- Source: TODO (website / scraper / dataset)
- Scrape date: TODO
- Size: TODO sale rows, TODO rent rows (after cleaning)
- Personal contact data was **not** collected or stored.
- Details: [`docs/data_dictionary.md`](docs/data_dictionary.md)

## Approach
1. **Power Query**: clean and standardize raw listings, build fact + dimension tables
2. **Excel**: explore with Pivot Tables, descriptive statistics, correlation, and regression
3. **Power Pivot + DAX**: star schema and measures (median price per sqm, gross rental yield, valuation label)
4. **Power BI**: 4-page interactive dashboard

Methodology: [`docs/methodology.md`](docs/methodology.md)

## Data Model
TODO: insert `model/data_model.png`

## Key Measures
Median Sale PPSQM, Median Monthly Rent PPSQM, Gross Rental Yield %, Payback Years, Premium vs City %, Valuation Label.  
Full list: [`model/dax_measures.md`](model/dax_measures.md)

## Dashboard
| Page | Preview |
|---|---|
| Market Overview | TODO `images/page1_overview.png` |
| Location Analysis | TODO `images/page2_location.png` |
| Property Drivers | TODO `images/page3_drivers.png` |
| Investment & Valuation | TODO `images/page4_investment.png` |

## Key Insights
TODO: 4-6 specific findings from the real data (no invented numbers).

## Limitations
- Asking prices and rents, not final deal prices
- Gross yield only (no costs, vacancy, taxes)
- Results reflect the scrape date and the sample collected

## Repository Structure
```
data/raw          untouched source files
data/processed    cleaned data
powerquery        exported M scripts + query map
excel             exploration workbook
model             data model diagram + DAX measures
powerbi           Power BI file
docs              questions, dictionary, methodology, dashboard spec, progress log
images            dashboard screenshots
PROJECT_PLAN.md   full phase-by-phase plan
```

## How to Reproduce
1. Put the raw files in `data/raw/`
2. Open the Excel workbook and refresh queries (update the folder path parameter)
3. Open the Power BI file and refresh

## Author
**Farouk Abdelrahman**: Data & Business Analyst  
[LinkedIn](https://www.linkedin.com/in/farouk-apd-elrahman-260448327) | [GitHub](https://github.com/faroukapdelrahman)
