# Dashboard Specification (Power BI)

## Global
- One theme: 1 main color, 1 accent, neutral grays; one font family
- Page size 16:9, consistent grid, top navigation bar (bookmarks)
- Global slicers synced across pages: **City, Property Type, Listing Type (where relevant)**
- Always show data scope: scrape date + number of listings (card or footer)
- Use **median** by default; show average only when it adds value

## Page 1 - Market Overview
- KPI cards: Total listings, Median sale price, Median price per sqm, Median monthly rent per sqm, Gross yield (overall)
- Chart: listings by city (bar)
- Chart: median price per sqm by property type (bar)
- Donut/bar: finishing level mix
- Answers: Q1, Q2

## Page 2 - Location Analysis
- Filled/bubble map by area (size = listings, color = median price per sqm)
- Ranked bar: top 10 / bottom 10 areas by median price per sqm
- Table: area, listings, median PPSQM, premium vs city %
- Drill-through: Area -> listing details
- Answers: Q3, Q4, Q5

## Page 3 - Property Drivers
- Scatter: AreaSqm vs Price (colored by property type)
- Column: median PPSQM by size band
- Column: median PPSQM by bedrooms
- Column: median PPSQM by finishing level
- Ready vs under construction comparison
- Answers: Q6, Q7, Q8, Q11

## Page 4 - Investment & Valuation
- Ranked bar: top areas by gross rental yield (hide areas with low sample)
- Scatter: sale PPSQM vs rent PPSQM per area (bubble = listings)
- Matrix: Valuation Label counts by area
- Table of top "Opportunity" listings (with link/id)
- Disclaimer text box: asking prices, gross yield
- Answers: Q9, Q10

## Quality Checklist
- [ ] Every visual has a clear title (insight, not just "Chart 1")
- [ ] No more than ~6 visuals per page
- [ ] Tooltips show count of listings (sample size)
- [ ] Sorting is meaningful; axes labeled with units (EGP, sqm)
- [ ] Colors do not need a legend when avoidable
- [ ] Tested every slicer combination for blanks/errors
- [ ] Measures validated against Excel pivots
