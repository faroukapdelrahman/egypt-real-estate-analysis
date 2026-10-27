# Data Dictionary

> Fill the **Raw** table from the REAL scraper output (column names differ per source). Keep the **Derived** table updated as you build.

## Source Information
| Item | Value |
|---|---|
| Source | (fill: website / scraper / dataset) |
| Scrape date | (fill) |
| Sale rows (raw) | (fill) |
| Rent rows (raw) | (fill) |
| Contact fields dropped | Yes (privacy) |

## Raw Columns
| Column (raw name) | Meaning | Type | Notes |
|---|---|---|---|
| (listing id / url) | Unique listing identifier | Text | Used for duplicate removal |
| (title) | Listing title | Text | Optional |
| (price) | Asking price (sale) or rent amount | Text/Number | May contain commas, currency text |
| (rent period) | Monthly / yearly / daily | Text | Rent listings only |
| (area_sqm) | Size in square meters | Text/Number | May contain units |
| (bedrooms) | Number of bedrooms | Number | Studio may be 0 |
| (bathrooms) | Number of bathrooms | Number | |
| (property type) | Apartment, villa, duplex... | Text | |
| (finishing) | Finishing level | Text | Arabic/English variants |
| (delivery status) | Ready / under construction | Text | If available |
| (location text) | City / area / compound | Text | Needs standardization |
| (listing date) | Date posted | Date | If available |

## Derived Columns (created by us)
| Column | Table | Definition |
|---|---|---|
| ListingType | Fact_Listings | "Sale" or "Rent" |
| Price | Fact_Listings | Clean numeric price in EGP (rent normalized to **monthly**) |
| AreaSqm | Fact_Listings | Clean numeric size in sqm |
| PricePerSqm | Fact_Listings | Price / AreaSqm (sale = EGP per sqm, rent = EGP per sqm per month) |
| SizeBand | Fact_Listings / Dim | e.g. <80, 80-120, 120-180, 180-250, 250+ sqm |
| IsOutlier | Fact_Listings | TRUE if PricePerSqm outside IQR bounds within (city, type, listing type) |
| AreaKey / PropertyTypeKey / FinishingKey | Fact_Listings | Foreign keys to dimensions |
| Area Type Median PPSQM | Fact_Listings (calc. column) | Median PricePerSqm of same area + type + listing type |
| Deviation % | Fact_Listings (calc. column) | (PricePerSqm - median) / median |
| Valuation Label | Fact_Listings (calc. column) | Overpriced / Fair / Opportunity / Insufficient data |
