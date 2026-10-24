# NoodlesCrypto Top Performers — Power BI Report Guide

## 1. Overview

The NoodlesCrypto Top Performers Power BI report provides an interactive overview of social media engagement performance. It enables users to explore engagement metrics, analyse changes over time, compare social media platforms, and investigate engagement across different currencies.

The report connects to data from the MySQL `noodles_dw` data warehouse and uses Power BI visualisations and DAX measures to summarise and present the data.

### Objectives

* Monitor overall social media engagement performance.
* Analyse engagement trends over time.
* Compare engagement across different social media platforms.
* Investigate engagement data by currency.
* Provide an interactive way to filter and explore engagement information.

## 2. Report Pages

The report contains four main pages:

1. Executive Dashboard
2. Time Series Analysis
3. Platform Analysis
4. Currency Deep Dive

The Task 6 test page is excluded from this guide.

### 2.1 Executive Dashboard

**Purpose:** Provide a high-level summary of engagement performance.

This page presents key performance indicators (KPIs) and visualisations to give users an overview of the engagement data.

The page includes:

* Summary cards displaying key engagement metrics.
* A currency-based table displaying currency symbols and names.
* A bar chart for comparing engagement across currencies.
* A date slicer for filtering the displayed data by date.

**How to use:**

1. Review the KPI cards to understand the overall engagement performance.
2. Use the currency table and bar chart to explore differences between currencies.
3. Adjust the date slicer to focus on a particular date or reporting period.
4. Observe how the displayed metrics and visualisations respond to the selected filters.

This page is intended to serve as the starting point for understanding overall performance before exploring the more detailed analysis pages.

### 2.2 Time Series Analysis

**Purpose:** Examine how social media engagement changes over time.

This page focuses on the temporal patterns within the engagement data. It uses date-based visualisations to help identify changes, trends, and fluctuations in performance.

The page includes:

* A line chart for analysing engagement trends over time.
* An area chart for visualising changes in engagement across the reporting period.
* A 100% stacked column chart for comparing the relative composition of engagement over time.
* A date slicer for narrowing the analysis to a selected period.

**How to use:**

1. Review the line chart to identify increases, decreases, and fluctuations in engagement.
2. Use the area chart to examine the overall pattern of engagement across dates.
3. Use the 100% stacked column chart to compare the relative proportions represented by its categories over time.
4. Adjust the date slicer to investigate a specific reporting period.
5. Compare the visualisations to identify periods that may warrant further investigation.

This page helps users understand the development of engagement over time rather than relying only on overall totals.

### 2.3 Platform Analysis

**Purpose:** Compare social media engagement performance across platforms.

This page focuses on platform-level analysis, allowing users to compare engagement between the available social media platforms.

The page includes:

* A table containing platform names.
* A bar chart for comparing engagement across platforms.
* A 100% stacked column chart for comparing the relative composition of engagement across platforms.
* A date slicer for filtering the platform analysis by date.

**How to use:**

1. Review the platform table to identify the platforms included in the analysis.
2. Use the bar chart to compare engagement between platforms.
3. Examine the 100% stacked column chart to understand the relative distribution of the categories represented.
4. Apply a date filter to compare platform performance over a selected period.
5. Use the visualisations together to identify platforms that contribute more or less to the measured engagement.

This page supports comparisons between platforms and helps users investigate differences in engagement performance.

### 2.4 Currency Deep Dive

**Purpose:** Provide a more detailed investigation of engagement associated with individual currencies.

This page is designed as a drill-through destination, allowing users to move from a broader report view into a more focused currency-level analysis.

The page includes:

* A currency slicer using currency symbols.
* A line chart for examining engagement trends over time.
* A donut chart for viewing the distribution of engagement across social media platforms.
* A date slicer for filtering the analysis to a selected period.
* A back-navigation button to return to the previous report view.

**How to use:**

1. Navigate to this page from a relevant currency data point when drill-through is available.
2. Review the selected currency context.
3. Use the line chart to investigate engagement trends over time.
4. Examine the donut chart to compare the relative platform distribution.
5. Adjust the currency or date filters when appropriate to investigate a different selection.
6. Use the back button to return to the previous report page.

The drill-through functionality allows users to investigate a particular currency in greater detail without relying solely on the summary visualisations.

## 3. Key Measures

The report uses DAX measures to calculate and display engagement metrics.

| Measure                | Description                                                                                     |
| ---------------------- | ----------------------------------------------------------------------------------------------- |
| Total Engagements      | Calculates the sum of total engagements in the executive dashboard data.                        |
| Total Likes            | Calculates the sum of likes.                                                                    |
| Total Comments         | Calculates the sum of comments.                                                                 |
| Total Retweets         | Calculates the sum of retweets.                                                                 |
| Avg Engagement Score   | Calculates the average engagement score across the rows included in the current filter context. |
| Engagement by Platform | Calculates the sum of total engagements from the social analytics data.                         |

These measures allow engagement information to be summarised in cards and charts. Where applicable, their results respond to report filters and slicers.

## 4. Filters and Interactivity

The report provides interactive features to support data exploration.

### Date Filtering

Date slicers are available on the main analysis pages. Selecting a date or date range filters the relevant visualisations according to the report's data model and filter relationships.

### Visual Interactions

Users can select data points in charts or select categories in tables to explore the data. Depending on the configured visual interactions, these selections can filter or highlight information in other visuals on the page.

### Drill-Through Navigation

The Currency Deep Dive page supports drill-through based on currency-related fields. This allows users to investigate a currency from a broader report view.

### Back Navigation

The back button on the Currency Deep Dive page allows users to return to the previous report view after completing their investigation.

## 5. Data Source and Model

The report uses a MySQL data warehouse named `noodles_dw`.

The data is accessed through the MySQL Connector/ODBC driver and a configured ODBC Data Source Name (DSN). Power BI connects to the data through its ODBC connector.

The report references data from views and tables including:

* `vw_ExecutiveDashboard` — provides data for executive-level metrics and currency analysis.
* `vw_TimeSeries` — provides date-based data for trend analysis.
* `vw_SocialAnalytics` — provides platform-level engagement data and related analysis.
* `dimDate` — provides date information used in filtering and time-based analysis.
* `dimCurrency` — provides currency information used in currency filtering and drill-through.

The report uses an Import-mode approach, in which data is loaded into the Power BI model for reporting.

## 6. How to Navigate the Report

For the recommended workflow:

1. Start with the Executive Dashboard to review the overall engagement metrics.
2. Open Time Series Analysis to examine engagement patterns over time.
3. Open Platform Analysis to compare the performance of social media platforms.
4. Use the Currency Deep Dive page to investigate a particular currency in greater detail.
5. Apply date filters and interact with the visualisations to explore the information relevant to your analysis.

## 7. Notes and Limitations

* The displayed results depend on the data available in the MySQL data warehouse and the last successful data refresh.
* Date slicers and chart selections affect results according to the report's configured relationships and visual interactions.
* The average engagement score is an average of the underlying row-level values, not necessarily an engagement-weighted average.
* Currency and platform comparisons should be interpreted in the context of the available data and the definitions used by the underlying views.
* If the report does not display updated data, verify the MySQL server connection, ODBC configuration, and Power BI refresh status.

## 8. Conclusion

The NoodlesCrypto Top Performers Power BI report brings together executive-level KPIs, time-series analysis, platform comparisons, and currency-level drill-through analysis in one interactive reporting solution.

By combining data from the MySQL data warehouse with Power BI visualisations, DAX measures, slicers, and drill-through navigation, the report enables users to explore engagement performance at different levels of detail and make more informed observations about social media engagement patterns.
