# Construction Safety Analytics Dashboard

Power BI safety dashboard for construction incident data — star schema, KPI cards, trend charts, and detail drill-down.

## Overview (Dropdown Slicer Style)
![Overview - 2nd Style](https://github.com/AdamLumbley/power-bi-construction-safety-dashboard/blob/main/Safety%20-%20Overview%20-%202nd%20Style.png?raw=true)
KPI cards, monthly trend, and breakdowns by project and contractor.

## Trends (Dropdown Slicer Style)
![Trends - 2nd Style](https://github.com/AdamLumbley/power-bi-construction-safety-dashboard/blob/main/Safety%20-%20Trends%20-%202nd%20Style.png?raw=true)
Multi-line charts showing incidents by severity and type over time, with semantic color coding (green/amber/red for severity).

## Detail (Dropdown Slicer Style)
![Detail - 2nd Style](https://github.com/AdamLumbley/power-bi-construction-safety-dashboard/blob/main/Safety%20-%20Details%20-%202nd%20Style.png?raw=true)
Full incident-level table for drill-down and audit.

## Overview (List Slicer Style)
![Overview](https://github.com/AdamLumbley/power-bi-construction-safety-dashboard/blob/main/Safety%20-%20Overview.png?raw=true)

## Trends (List Slicer Style)
![Trends](https://github.com/AdamLumbley/power-bi-construction-safety-dashboard/blob/main/Safety%20-%20Trends.png?raw=true)

## Detail (List Slicer Style)
![Detail](https://github.com/AdamLumbley/power-bi-construction-safety-dashboard/blob/main/Safety%20-%20Details.png?raw=true)

---

## Design notes

Two slicer styles are included to show a real trade-off: list slicers surface every option at once for faster multi-filter navigation, while dropdown slicers save canvas space for a cleaner layout.

## Data quality & modeling notes

- Found and merged a duplicate contractor entry ("Basalt Concrete LLC" vs "L.L.C.") that was splitting incident counts across two fake entities
- Diagnosed a line chart showing a false downward trend caused by alphabetical month sorting; fixed with a custom numeric sort column (MonthNum) via Sort by Column
- Preserved nulls in Actual Amount / Severity fields rather than defaulting to zero, to avoid misrepresenting incomplete data
- Used Power Query custom columns to truncate long contractor/project names for slicer readability, while leaving semantically meaningful text (e.g. "Phase II") untouched
- Applied consistent semantic color coding across severity-based charts (green = low, amber = medium, red = high)

## Sample DAX

```dax
Total Incidents = COUNTROWS(Fact_Incident)

High Severity = CALCULATE([Total Incidents], Fact_Incident[Severity] = "High")

Dim_Date = 
ADDCOLUMNS(
    CALENDAR(MIN(Fact_Incident[Incident Date]), MAX(Fact_Incident[Incident Date])),
    "Year", YEAR([Date]),
    "Quarter", "Q" & FORMAT([Date], "Q"),
    "Month", FORMAT([Date], "MMMM"),
    "MonthNum", MONTH([Date]),
    "Year-Month", FORMAT([Date], "MMM YYYY")
)
```
