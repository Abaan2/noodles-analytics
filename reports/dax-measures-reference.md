# Executive Dashboard — DAX Measures Documentation

## 1. Overview

This document records the DAX measures used in the Executive Dashboard, including their purpose, source tables or views, formulas, dependencies, slicer behaviour, example outputs and report usage.

The measures are grouped into the following categories:

- **KPI:** Executive-level engagement indicators.
- **Time Intelligence:** Date-based trends, daily growth and rolling averages.
- **Ranking:** Ranking currencies or tokens by engagement.
- **Helper:** Supporting calculations for platform analytics, drill-through, tooltips and engagement classification.

### Data Sources

| Data Source | Purpose |
|---|---|
| `vw_ExecutiveDashboard` | Aggregated engagement metrics by currency or token. |
| `vw_TimeSeries` | Engagement totals used for time-series analysis. |
| `vw_SocialAnalytics` | Platform-level engagement metrics, including likes, comments and engagement scores. |
| `DimDate` | Date dimension used for filtering and time intelligence. |
| `Measures` | Dedicated measure table, if used to organise measures. The `ROW("Placeholder", 1)` expression creates a placeholder table rather than an analytical measure. |

**Documentation note:** Example outputs below are illustrative, not measured results from the actual report. Report pages and visual names must be confirmed against the final `.pbix` file.

---

## 2. Executive KPI Measures

### 2.1 Total Engagements

- **Name:** `Total Engagements`
- **Category:** KPI
- **Purpose:** Calculates the total number of engagements across the selected currencies or tokens. Used as a primary indicator of overall audience interaction.
- **Data Source:** `vw_ExecutiveDashboard`
- **DAX Formula:**

```dax
Total Engagements =
SUM(vw_ExecutiveDashboard[TotalEngagements])
```

- **Example Output:** `125,430` engagements.
- **Dependencies:** None; base measure.
- **DimDate:** No direct reference. Date filtering depends on the data model and relationships.
- **Slicer affected:** Date — conditional on model relationships; Currency — yes, if the currency field filters the source; Platform — conditional on relationships.
- **Used In:**
  - **Report page:** Executive Dashboard (confirm final page name).
  - **Visual name:** Total Engagements KPI card; engagement breakdown or related charts, if configured.

### 2.2 Total Likes

- **Name:** `Total Likes`
- **Category:** KPI
- **Purpose:** Aggregates the total number of likes recorded in the executive dashboard dataset.
- **Data Source:** `vw_ExecutiveDashboard`
- **DAX Formula:**

```dax
Total Likes =
SUM(vw_ExecutiveDashboard[TotalLikes])
```

- **Example Output:** `82,600` likes.
- **Dependencies:** None; base measure.
- **DimDate:** No direct reference.
- **Slicer affected:** Date — conditional; Currency — conditional; Platform — conditional on model relationships.
- **Used In:**
  - **Report page:** Executive Dashboard (confirm).
  - **Visual name:** Total Likes KPI card, if configured.

### 2.3 Total Comments

- **Name:** `Total Comments`
- **Category:** KPI
- **Purpose:** Calculates the total number of comments in the executive dashboard dataset.
- **Data Source:** `vw_ExecutiveDashboard`
- **DAX Formula:**

```dax
Total Comments =
SUM(vw_ExecutiveDashboard[TotalComments])
```

- **Example Output:** `14,250` comments.
- **Dependencies:** None; base measure.
- **DimDate:** No direct reference.
- **Slicer affected:** Date — conditional; Currency — conditional; Platform — conditional on model relationships.
- **Used In:**
  - **Report page:** Executive Dashboard (confirm).
  - **Visual name:** Total Comments KPI card, if configured.

### 2.4 Total Retweets

- **Name:** `Total Retweets`
- **Category:** KPI
- **Purpose:** Aggregates the total number of retweets recorded in the executive dashboard dataset.
- **Data Source:** `vw_ExecutiveDashboard`
- **DAX Formula:**

```dax
Total Retweets =
SUM(vw_ExecutiveDashboard[TotalRetweets])
```

- **Example Output:** `9,840` retweets.
- **Dependencies:** None; base measure.
- **DimDate:** No direct reference.
- **Slicer affected:** Date — conditional; Currency — conditional; Platform — conditional on model relationships.
- **Used In:**
  - **Report page:** Executive Dashboard (confirm).
  - **Visual name:** Total Retweets KPI card, if configured.

### 2.5 Average Engagement Score

- **Name:** `Average Engagement Score`
- **Category:** KPI
- **Purpose:** Calculates the average engagement score across the rows included in the current filter context.
- **Data Source:** `vw_ExecutiveDashboard`
- **DAX Formula:**

```dax
Average Engagement Score =
AVERAGE(vw_ExecutiveDashboard[AvgEngagementScore])
```

- **Example Output:** `0.72`, assuming the source score is stored on a 0–1 scale.
- **Dependencies:** None; base measure.
- **DimDate:** No direct reference.
- **Slicer affected:** Date — conditional; Currency — conditional; Platform — conditional on model relationships.
- **Used In:**
  - **Report page:** Executive Dashboard (confirm).
  - **Visual name:** Average Engagement Score KPI card and Engagement Quality classification, if configured.

**Interpretation note:** `AVERAGE` calculates the mean of the source column's row values. It is not necessarily a weighted average across all underlying social posts.

### 2.6 Active Tokens

- **Name:** `Active Tokens`
- **Category:** KPI
- **Purpose:** Counts the number of distinct currency symbols represented in the current filter context.
- **Data Source:** `vw_ExecutiveDashboard`
- **DAX Formula:**

```dax
Active Tokens =
DISTINCTCOUNT(vw_ExecutiveDashboard[CurrencySymbol])
```

- **Example Output:** `12` distinct currency symbols.
- **Dependencies:** None; base measure.
- **DimDate:** No direct reference.
- **Slicer affected:** Date — conditional; Currency — yes, if the currency slicer filters this column; Platform — conditional on model relationships.
- **Used In:**
  - **Report page:** Executive Dashboard (confirm).
  - **Visual name:** Active Tokens KPI card, if configured.

**Interpretation note:** This measure counts distinct symbols in the filtered data. It does not independently verify whether each token is currently active in the market.

---

## 3. Time Intelligence Measures

### 3.1 Total Engagements (Time)

- **Name:** `Total Engagements (Time)`
- **Category:** Time Intelligence
- **Purpose:** Calculates total engagements from the time-series view. Serves as the base measure for daily comparisons and rolling engagement analysis.
- **Data Source:** `vw_TimeSeries`
- **DAX Formula:**

```dax
Total Engagements (Time) =
SUM(vw_TimeSeries[TotalEngagements])
```

- **Example Output:** `18,500` engagements for the selected date or date range.
- **Dependencies:** None; base time-series measure.
- **DimDate:** No direct reference; required for the dependent time-intelligence calculations.
- **Slicer affected:** Date — conditional on the relationship between `DimDate` and `vw_TimeSeries`; Currency and Platform — conditional on the model.
- **Used In:**
  - **Report page:** Trends / Time Series page (confirm).
  - **Visual name:** Engagement trend line chart or daily engagement chart, if configured.

### 3.2 Engagements Previous Day

- **Name:** `Engagements Previous Day`
- **Category:** Time Intelligence
- **Purpose:** Returns the total engagements for the previous day relative to the date context in the report.
- **Data Source:** `vw_TimeSeries`; date context from `DimDate`.
- **DAX Formula:**

```dax
Engagements Previous Day =
CALCULATE(
    [Total Engagements (Time)],
    DATEADD(DimDate[FullDate], -1, DAY)
)
```

- **Example Output:** `17,200` engagements for the previous day.
- **Dependencies:** `[Total Engagements (Time)]`, `DimDate[FullDate]`.
- **DimDate:** Yes.
- **Slicer affected:** Date — yes, subject to the date selection and date-table relationship; Currency and Platform — conditional on the model.
- **Used In:**
  - **Report page:** Trends / Time Series page (confirm).
  - **Visual name:** Previous-day comparison card, tooltip or growth calculation, if configured.

### 3.3 Engagement Growth %

- **Name:** `Engagement Growth %`
- **Category:** Time Intelligence
- **Purpose:** Measures the percentage change in engagement compared with the previous day.
- **Data Source:** `vw_TimeSeries`; date context from `DimDate`.
- **DAX Formula:**

```dax
Engagement Growth % =
DIVIDE(
    [Total Engagements (Time)] - [Engagements Previous Day],
    [Engagements Previous Day],
    0
) * 100
```

- **Example Output:** `7.56`, representing approximately `7.56%` growth when the previous-day total is `17,200` and the current total is `18,500`.
- **Dependencies:** `[Total Engagements (Time)]`, `[Engagements Previous Day]`.
- **DimDate:** Yes, through the previous-day calculation.
- **Slicer affected:** Date — yes, subject to date context; Currency and Platform — conditional on the model.
- **Used In:**
  - **Report page:** Trends / Time Series page (confirm).
  - **Visual name:** Engagement Growth KPI, trend tooltip or comparison visual, if configured.

**Formatting note:** The formula multiplies the ratio by 100. Format the result as a regular number with a `%` suffix, or remove `* 100` and use Power BI percentage formatting. Do not do both, as that would display an incorrectly scaled percentage.

### 3.4 Engagements 7D Avg

- **Name:** `Engagements 7D Avg`
- **Category:** Time Intelligence
- **Purpose:** Calculates engagement over a seven-day period ending on the maximum date in the current date context, then divides the result by seven.
- **Data Source:** `vw_TimeSeries`; date context from `DimDate`.
- **DAX Formula:**

```dax
Engagements 7D Avg =
CALCULATE(
    [Total Engagements (Time)],
    DATESINPERIOD(
        DimDate[FullDate],
        MAX(DimDate[FullDate]),
        -7,
        DAY
    )
) / 7
```

- **Example Output:** `16,400` engagements per day, assuming the seven-day total is `114,800`.
- **Dependencies:** `[Total Engagements (Time)]`, `DimDate[FullDate]`.
- **DimDate:** Yes.
- **Slicer affected:** Date — yes, subject to the selected date context; Currency and Platform — conditional on the model.
- **Used In:**
  - **Report page:** Trends / Time Series page (confirm).
  - **Visual name:** Seven-day moving-average line, trend comparison or tooltip, if configured.

**Interpretation note:** This measure divides by seven even when fewer than seven days contain data. It therefore represents a seven-calendar-day average, with missing dates effectively contributing zero to the numerator. Confirm this is the intended business definition.

---

## 4. Platform Analytics Measures

These measures use `vw_SocialAnalytics`. They should be kept separate from the executive measures because the source view and aggregation context differ.

### 4.1 Platform Total Engagements

- **Name:** `Platform Total Engagements`
- **Category:** Helper
- **Purpose:** Calculates total engagements from the social analytics view. Serves as the base measure for platform-level metrics and engagement-share calculations.
- **Data Source:** `vw_SocialAnalytics`
- **DAX Formula:**

```dax
Platform Total Engagements =
SUM(vw_SocialAnalytics[TotalEngagements])
```

- **Example Output:** `42,600` engagements across the selected platform data.
- **Dependencies:** None; base platform measure.
- **DimDate:** No direct reference.
- **Slicer affected:** Date — conditional; Currency — conditional; Platform — yes, if the platform slicer filters `vw_SocialAnalytics`.
- **Used In:**
  - **Report page:** Platform Analytics page (confirm).
  - **Visual name:** Platform engagement KPI, likes-share calculation, comments-share calculation or tooltip, if configured.

### 4.2 Platform Total Comments

- **Name:** `Platform Total Comments`
- **Category:** Helper
- **Purpose:** Calculates the total number of comments from the social analytics view.
- **Data Source:** `vw_SocialAnalytics`
- **DAX Formula:**

```dax
Platform Total Comments =
SUM(vw_SocialAnalytics[TotalComments])
```

- **Example Output:** `6,850` comments.
- **Dependencies:** None; base platform measure.
- **DimDate:** No direct reference.
- **Slicer affected:** Date — conditional; Currency — conditional; Platform — conditional on the model.
- **Used In:**
  - **Report page:** Platform Analytics page (confirm).
  - **Visual name:** Platform comments KPI or supporting tooltip, if configured.

### 4.3 Platform Avg Engagement Score

- **Name:** `Platform Avg Engagement Score`
- **Category:** Helper
- **Purpose:** Calculates the average engagement score for the social analytics data in the current filter context.
- **Data Source:** `vw_SocialAnalytics`
- **DAX Formula:**

```dax
Platform Avg Engagement Score =
AVERAGE(vw_SocialAnalytics[AvgEngagementScore])
```

- **Example Output:** `0.68`, assuming a 0–1 source scale.
- **Dependencies:** None; base platform measure.
- **DimDate:** No direct reference.
- **Slicer affected:** Date — conditional; Currency — conditional; Platform — conditional on the model.
- **Used In:**
  - **Report page:** Platform Analytics page (confirm).
  - **Visual name:** Platform engagement score KPI, comparison chart or tooltip, if configured.

### 4.4 Last Engagement Date

- **Name:** `Last Engagement Date`
- **Category:** Helper
- **Purpose:** Returns the latest engagement timestamp or date present in the filtered social analytics data.
- **Data Source:** `vw_SocialAnalytics`
- **DAX Formula:**

```dax
Last Engagement Date =
MAX(vw_SocialAnalytics[LastEngagement])
```

- **Example Output:** `18 October 2026`, if that is the maximum source date.
- **Dependencies:** None; base platform measure.
- **DimDate:** No direct reference.
- **Slicer affected:** Date — conditional; Currency — conditional; Platform — conditional on the model.
- **Used In:**
  - **Report page:** Platform Analytics page (confirm).
  - **Visual name:** Latest Engagement Date card or platform freshness indicator, if configured.

**Interpretation note:** The measure returns the maximum timestamp in the current filter context, not necessarily the latest engagement date across the entire dataset regardless of slicers.

---

## 5. Ranking and Top N Measures

### 5.1 Token Engagement Rank

- **Name:** `Token Engagement Rank`
- **Category:** Ranking
- **Purpose:** Ranks currency symbols according to total engagements in descending order. Dense ranking assigns consecutive ranks without gaps after ties.
- **Data Source:** `vw_ExecutiveDashboard`
- **DAX Formula:**

```dax
Token Engagement Rank =
RANKX(
    ALL(vw_ExecutiveDashboard[CurrencySymbol]),
    [Total Engagements],
    ,
    DESC,
    Dense
)
```

- **Example Output:** `1` for the highest-ranked currency symbol.
- **Dependencies:** `[Total Engagements]`, `vw_ExecutiveDashboard[CurrencySymbol]`.
- **DimDate:** No direct reference; ranking can still respond to date filters that affect `[Total Engagements]`.
- **Slicer affected:** Date — conditional; Currency — the currency-symbol filter is explicitly removed for ranking; Platform — conditional on the model.

**Ranking behaviour:** `ALL` removes filters on `CurrencySymbol` for the ranking comparison. Other filters may remain active. The rank is therefore intended to compare currencies within the remaining filter context, rather than necessarily ranking all currencies across every possible filter.

- **Used In:**
  - **Report page:** Executive Dashboard or Token Analysis page (confirm).
  - **Visual name:** Currency ranking table, ranked bar chart or Top N visual, if configured.

---

## 6. Drill-through Support Measures

### 6.1 Engagement Share %

- **Name:** `Engagement Share %`
- **Category:** Helper
- **Purpose:** Calculates the selected currency or token's percentage contribution to total engagements after removing filters from the entire executive dashboard view.
- **Data Source:** `vw_ExecutiveDashboard`
- **DAX Formula:**

```dax
Engagement Share % =
DIVIDE(
    [Total Engagements],
    CALCULATE(
        [Total Engagements],
        ALL(vw_ExecutiveDashboard)
    ),
    0
) * 100
```

- **Example Output:** `15.0`, representing a 15% contribution to the denominator's total.
- **Dependencies:** `[Total Engagements]`, `vw_ExecutiveDashboard`.
- **DimDate:** No direct reference. The denominator removes filters from the executive view, but filters originating from other tables may still affect the result depending on the model.
- **Slicer affected:** Date — may be removed or retained depending on how the date filter reaches the view; Currency — removes filters from the executive view; Platform — depends on the model and filter propagation.
- **Used In:**
  - **Report page:** Executive Dashboard or drill-through detail page (confirm).
  - **Visual name:** Engagement Share KPI, contribution tooltip or currency detail table, if configured.

**Interpretation note:** `ALL(vw_ExecutiveDashboard)` removes filters on that table, not necessarily all filters in the entire model. Validate the denominator against the intended business definition of total engagement share.

---

## 7. Tooltip-Only Measures

### 7.1 Likes Share %

- **Name:** `Likes Share %`
- **Category:** Helper
- **Purpose:** Calculates total likes as a percentage of platform total engagements. Intended to provide context in platform-level tooltips.
- **Data Source:** `vw_SocialAnalytics`
- **DAX Formula:**

```dax
Likes Share % =
DIVIDE(
    SUM(vw_SocialAnalytics[TotalLikes]),
    [Platform Total Engagements],
    0
) * 100
```

- **Example Output:** `35.0`, representing a 35% likes-to-engagements ratio.
- **Dependencies:** `[Platform Total Engagements]`, `vw_SocialAnalytics[TotalLikes]`.
- **DimDate:** No direct reference.
- **Slicer affected:** Date — conditional; Currency — conditional; Platform — conditional on the model.
- **Used In:**
  - **Report page:** Platform Analytics page (confirm).
  - **Visual name:** Platform tooltip or engagement breakdown visual, if configured.

### 7.2 Comments Share %

- **Name:** `Comments Share %`
- **Category:** Helper
- **Purpose:** Calculates total comments as a percentage of platform total engagements. Intended to provide context in platform-level tooltips.
- **Data Source:** `vw_SocialAnalytics`
- **DAX Formula:**

```dax
Comments Share % =
DIVIDE(
    SUM(vw_SocialAnalytics[TotalComments]),
    [Platform Total Engagements],
    0
) * 100
```

- **Example Output:** `12.0`, representing a 12% comments-to-engagements ratio.
- **Dependencies:** `[Platform Total Engagements]`, `vw_SocialAnalytics[TotalComments]`.
- **DimDate:** No direct reference.
- **Slicer affected:** Date — conditional; Currency — conditional; Platform — conditional on the model.
- **Used In:**
  - **Report page:** Platform Analytics page (confirm).
  - **Visual name:** Platform tooltip or engagement breakdown visual, if configured.

**Interpretation note:** Likes and comments are component metrics, and the engagement total may also include retweets, shares or other interactions. These ratios should not automatically be interpreted as mutually exclusive percentages that sum to 100%.

---

## 8. Engagement Classification Measure

### 8.1 Engagement Quality

- **Name:** `Engagement Quality`
- **Category:** Helper
- **Purpose:** Converts the average engagement score into a descriptive classification using four thresholds: Excellent, Good, Average and Low.
- **Data Source:** `vw_ExecutiveDashboard`
- **DAX Formula:**

```dax
Engagement Quality =
SWITCH(
    TRUE(),
    [Average Engagement Score] >= 0.8, "Excellent",
    [Average Engagement Score] >= 0.6, "Good",
    [Average Engagement Score] >= 0.4, "Average",
    "Low"
)
```

- **Example Output:** `"Good"` when `[Average Engagement Score]` is `0.72`.
- **Dependencies:** `[Average Engagement Score]`.
- **DimDate:** No direct reference; may respond to date filters through the dependent measure and model relationships.
- **Slicer affected:** Date — conditional; Currency — conditional; Platform — conditional on the model.
- **Used In:**
  - **Report page:** Executive Dashboard or Token Analysis page (confirm).
  - **Visual name:** Engagement Quality card, classification column or conditional-formatting visual, if configured.

**Thresholds:**

| Average Engagement Score | Classification |
|---|---|
| 0.80 and above | Excellent |
| 0.60 to below 0.80 | Good |
| 0.40 to below 0.60 | Average |
| Below 0.40 | Low |

**Implementation note:** The formula above corrects the extra closing parenthesis in the supplied expression. Confirm that the source engagement score uses a 0–1 scale before applying these thresholds.

---

## 9. Dependency Reference

The following table summarises dependencies between measures.

| Measure | Depends On |
|---|---|
| Total Engagements | None |
| Total Likes | None |
| Total Comments | None |
| Total Retweets | None |
| Average Engagement Score | None |
| Active Tokens | None |
| Total Engagements (Time) | None |
| Engagements Previous Day | Total Engagements (Time), `DimDate[FullDate]` |
| Engagement Growth % | Total Engagements (Time), Engagements Previous Day |
| Engagements 7D Avg | Total Engagements (Time), `DimDate[FullDate]` |
| Platform Total Engagements | None |
| Platform Total Comments | None |
| Platform Avg Engagement Score | None |
| Last Engagement Date | None |
| Token Engagement Rank | Total Engagements |
| Engagement Share % | Total Engagements |
| Likes Share % | Platform Total Engagements |
| Comments Share % | Platform Total Engagements |
| Engagement Quality | Average Engagement Score |

### Source and Filter Considerations

1. `Total Engagements` and `Total Engagements (Time)` use different views. They should not be substituted for one another without verifying their aggregation levels and intended uses.
2. `Platform Total Engagements` belongs to `vw_SocialAnalytics`. Platform-level share measures should use this measure rather than the executive-level `Total Engagements`.
3. Time-intelligence measures require a correctly configured `DimDate` table and a valid relationship to the time-series data. Mark `DimDate` as the date table where appropriate.
4. Slicer behaviour depends on the model's relationships, visual interactions and filter direction. The absence of a direct `DimDate` reference does not mean a measure cannot respond to a date slicer.
5. Example values in this document are illustrative. Replace them with actual report outputs when validating the finished dashboard.

---

## 10. Report Usage Register

The following register should be completed against the final Power BI report. A measure may be used in more than one visual or page.

| Measure | Report Page | Visual Name |
|---|---|---|
| Total Engagements | Executive Dashboard — confirm | KPI card / engagement chart — confirm |
| Total Likes | Executive Dashboard — confirm | KPI card — confirm |
| Total Comments | Executive Dashboard — confirm | KPI card — confirm |
| Total Retweets | Executive Dashboard — confirm | KPI card — confirm |
| Average Engagement Score | Executive Dashboard — confirm | KPI card / classification — confirm |
| Active Tokens | Executive Dashboard — confirm | KPI card — confirm |
| Total Engagements (Time) | Trends — confirm | Engagement trend chart — confirm |
| Engagements Previous Day | Trends — confirm | Comparison / tooltip — confirm |
| Engagement Growth % | Trends — confirm | Growth KPI / tooltip — confirm |
| Engagements 7D Avg | Trends — confirm | Rolling-average line — confirm |
| Platform Total Engagements | Platform Analytics — confirm | KPI card / tooltip — confirm |
| Platform Total Comments | Platform Analytics — confirm | KPI card / tooltip — confirm |
| Platform Avg Engagement Score | Platform Analytics — confirm | KPI / comparison chart — confirm |
| Last Engagement Date | Platform Analytics — confirm | Latest-date card — confirm |
| Token Engagement Rank | Token Analysis — confirm | Ranking table / bar chart — confirm |
| Engagement Share % | Drill-through / Executive Dashboard — confirm | Share KPI / tooltip — confirm |
| Likes Share % | Platform Analytics — confirm | Tooltip — confirm |
| Comments Share % | Platform Analytics — confirm | Tooltip — confirm |
| Engagement Quality | Executive Dashboard — confirm | Classification visual — confirm |
