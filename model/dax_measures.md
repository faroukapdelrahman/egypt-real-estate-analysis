# Data Model & DAX Measures

## Star Schema
```
Dim_Location (AreaKey) --1:*--> Fact_Listings <--*:1-- Dim_PropertyType (PropertyTypeKey)
Dim_Finishing (FinishingKey) --1:*--^        ^--*:1-- Dim_Date (Date)   [only if dates exist]
```
- Single-direction relationships (dimension -> fact)
- `Dim_Location` hierarchy: City > Area (> Compound if available)
- Hide keys and raw fact columns from the report view
- Put measures in a `_Measures` table with display folders

> Adjust table/column names to match your final model. Test every measure against an Excel pivot.

## A. Basic Counts
```dax
Listings Count = COUNTROWS ( Fact_Listings )

Sale Listings =
CALCULATE ( [Listings Count], Fact_Listings[ListingType] = "Sale" )

Rent Listings =
CALCULATE ( [Listings Count], Fact_Listings[ListingType] = "Rent" )
```

## B. Price Measures (exclude outliers optional via slicer on IsOutlier)
```dax
Median Sale Price =
CALCULATE ( MEDIAN ( Fact_Listings[Price] ), Fact_Listings[ListingType] = "Sale" )

Avg Sale Price =
CALCULATE ( AVERAGE ( Fact_Listings[Price] ), Fact_Listings[ListingType] = "Sale" )

Median Sale PPSQM =
CALCULATE ( MEDIAN ( Fact_Listings[PricePerSqm] ), Fact_Listings[ListingType] = "Sale" )

Median Monthly Rent PPSQM =
CALCULATE ( MEDIAN ( Fact_Listings[PricePerSqm] ), Fact_Listings[ListingType] = "Rent" )

Median Monthly Rent =
CALCULATE ( MEDIAN ( Fact_Listings[Price] ), Fact_Listings[ListingType] = "Rent" )
```

## C. Investment
```dax
Gross Rental Yield % =
DIVIDE ( [Median Monthly Rent PPSQM] * 12, [Median Sale PPSQM] )

Payback Years =
DIVIDE ( 1, [Gross Rental Yield %] )

-- Show yield only when both sides have enough data
Gross Yield (Safe) =
IF ( [Sale Listings] >= 5 && [Rent Listings] >= 5, [Gross Rental Yield %] )

Yield Rank =
RANKX ( ALL ( Dim_Location[Area] ), [Gross Yield (Safe)], , DESC, Dense )
```

## D. Comparison to City
```dax
City Median Sale PPSQM =
CALCULATE ( [Median Sale PPSQM], ALLEXCEPT ( Dim_Location, Dim_Location[City] ) )

Premium vs City % =
DIVIDE ( [Median Sale PPSQM] - [City Median Sale PPSQM], [City Median Sale PPSQM] )
```

## E. Valuation (calculated columns on Fact_Listings)
```dax
Area Type Median PPSQM =
CALCULATE (
    MEDIAN ( Fact_Listings[PricePerSqm] ),
    ALLEXCEPT (
        Fact_Listings,
        Fact_Listings[AreaKey],
        Fact_Listings[PropertyTypeKey],
        Fact_Listings[ListingType]
    )
)

Group Size =
CALCULATE (
    COUNTROWS ( Fact_Listings ),
    ALLEXCEPT (
        Fact_Listings,
        Fact_Listings[AreaKey],
        Fact_Listings[PropertyTypeKey],
        Fact_Listings[ListingType]
    )
)

Deviation % =
DIVIDE ( Fact_Listings[PricePerSqm] - Fact_Listings[Area Type Median PPSQM], Fact_Listings[Area Type Median PPSQM] )

Valuation Label =
SWITCH (
    TRUE (),
    Fact_Listings[Group Size] < 5, "Insufficient data",
    Fact_Listings[Deviation %] > 0.2, "Overpriced",
    Fact_Listings[Deviation %] < -0.2, "Opportunity",
    "Fair"
)
```
Tip: make the +/-20% thresholds a What-If parameter in Power BI so the viewer can change them.

## F. Time Intelligence (only if listing dates exist)
```dax
Median Sale PPSQM PM =
CALCULATE ( [Median Sale PPSQM], DATEADD ( Dim_Date[Date], -1, MONTH ) )

Median Sale PPSQM MoM % =
DIVIDE ( [Median Sale PPSQM] - [Median Sale PPSQM PM], [Median Sale PPSQM PM] )
```

## G. Validation Table (fill in Phase 4)
| Measure | Context | DAX result | Excel pivot result | Match? |
|---|---|---|---|---|
| Listings Count | All | | | |
| Median Sale PPSQM | One city | | | |
| Gross Rental Yield % | One area | | | |

## H. Measure Notes
| Measure | Why it exists |
|---|---|
| Median Sale PPSQM | Main price metric, robust to outliers |
| Gross Rental Yield % | Investment angle; project differentiator |
| Premium vs City % | Shows which areas are expensive relative to their city |
| Valuation Label | Flags overpriced listings and opportunities |
