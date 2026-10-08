# Sheffield NHS Talking Therapies: SPC Performance Dashboard

A Power BI performance dashboard analysing NHS Talking Therapies data for Sheffield, using statistical process control (SPC) charts to separate normal monthly variation from meaningful change.

![Dashboard](images/dashboard.png)

## Overview

Performance teams in the NHS increasingly use SPC charts, following NHS England's *Making Data Count* approach, rather than simple red/amber/green ratings. This project applies that approach to public data for Sheffield's NHS Talking Therapies service (provider: Sheffield Health Partnership University NHS Foundation Trust), with England as a benchmark.

**Period:** July 2024 to July 2026 (25 months)

**Key measures:**

| Measure | National standard |
|---|---|
| Recovery rate | 50% |
| % starting treatment within 6 weeks | 75% |
| Referrals received | (demand measure) |

## Key insights

- **Recovery is stable.** Only common cause variation is present, at an average of 52%, above the 50% standard. Because the standard sits inside the process limits, it will sometimes be missed unless the underlying process changes.
- **6-week access shows an early warning.** Performance consistently exceeds the 75% standard, but May to July 2026 fall below the lower process limit: a statistically significant deterioration that a RAG rating would not show.
- **Referrals are seasonal.** Demand dips in August and December. High referrals in early 2026 may have contributed to the later dip in 6-week access, although capacity data would be needed to confirm this.

![Insights](images/insights.png)

## Recommendations

- Investigate the drop in 6-week access since May 2026 by reviewing therapist capacity, sessions delivered and waiting-list size alongside referrals.
- Agree a recovery action with the service and track it on the SPC chart until performance returns within the previous limits.
- Continue monitoring recovery: recent months above average are not yet a sustained improvement.

## Method

**Data preparation (Power Query)**

- Source: NHS England, *NHS Talking Therapies Monthly Statistics Including Employment Advisors*, Monthly Time Series for Key Measures (two releases: July 2024 to July 2025, and July 2025 to July 2026).
- Profiled both files before transforming them, checking columns, measures, date ranges and suppressed values.
- Appended the two releases and removed the duplicate overlap month, keeping the most recently published figure, as NHS data can be revised after first publication.
- Filtered to provider-level rows for Sheffield (code TAH) and the England total, then pivoted the long-format measures into one row per month per area.
- Converted suppressed values (`*`) to null rather than zero.

**SPC calculations (DAX)**

XmR (individuals and moving range) charts, built as calculated columns:

- Moving range = absolute difference between each month and the previous month
- Mean and average moving range calculated per area
- Process limits = mean ± 2.66 × average moving range (capped at 0% and 100%)
- Points outside the limits flagged and highlighted as special cause

```DAX
Recovery_UPL = TalkingTherapies_KPIs[Recovery_Mean] + 2.66 * TalkingTherapies_KPIs[Recovery_AvgMR]
```

## Data quality notes

- **Organisation name change.** The Trust changed its name in October 2025, partway through the period. Filtering on the provider code (TAH) rather than the name kept the time series continuous.
- **Suppression.** Suppressed values appeared only in sub-area breakdowns. Provider-level figures for the key measures were complete.
- **Rounding.** Provider-level percentages are published as whole numbers, which limits precision, particularly for points close to the mean.
- **Baseline length.** Limits are based on 25 months of data and should be recalculated as more data becomes available.

## Tools

Power BI, Power Query, DAX

## Data source and licence

NHS England, NHS Talking Therapies Monthly Statistics Including Employment Advisors.

Contains public sector information licensed under the Open Government Licence v3.0.

This is an independent portfolio project using public, aggregated data. It contains no patient-level information.

## Author

Vivian Francis · [GitHub](https://github.com/VivFrancis)
