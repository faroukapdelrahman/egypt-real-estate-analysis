# Power Query

Export each query's M code (Advanced Editor) into this folder as `<query_name>.pq`.

## Query Map
| Query | Type | Purpose |
|---|---|---|
| stg_Sale | Staging (connection only) | Load raw sale CSV, set types |
| stg_Rent | Staging (connection only) | Load raw rent CSV, set types, normalize rent period |
| Map_Location | Reference table | Raw location text -> standard City / Area |
| Fact_Listings | Load to Data Model | Append sale + rent, clean, add PricePerSqm, IsOutlier |
| Dim_Location | Load to Data Model | Distinct City / Area with AreaKey |
| Dim_PropertyType | Load to Data Model | Distinct property types with key |
| Dim_Finishing | Load to Data Model | Standard finishing levels with key |
| Dim_Date | Load to Data Model | Only if listing dates exist |

## Step Checklist per Staging Query
1. Source (CSV) -> promote headers
2. Remove contact/personal columns
3. Change types (use locale if Arabic digits appear)
4. Trim + Clean text columns
5. Replace commas / currency text in numeric columns -> convert to number
6. Remove duplicates (listing id)
7. Remove nulls in price and area
8. Apply sanity filters
9. Add `ListingType` column ("Sale" / "Rent")

## Tips
- Name steps clearly (e.g. "Removed Duplicates") instead of default names
- Use parameters for the file folder path so the project runs on any machine
- Keep one "Cleaning Log" query that counts rows after each major step
- Do not load staging queries to the sheet (connection only)
