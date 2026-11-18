# Ontario-distribution-reliability-scorecard
Power BI scorecard of Ontario electricity distributor reliability (SAIDI/SAIFI) using OEB open data. Star-schema model, Power Query, DAX. Personal project on public data.

A one-page Power BI scorecard comparing the reliability of Ontario electricity distributors, built on public Ontario Energy Board (OEB) data.

> **Personal project.** Built on a personal computer with free Power BI Desktop, using public data only. Not affiliated with any employer or utility.

## What it shows
- SAIDI (average hours of interruption per customer) and SAIFI (average interruptions per customer) for a selected distributor and year
- Year-over-year change
- Comparison with a calculated 5-year average benchmark
- Rank among Ontario distributors
- Interruptions and customer-hours by cause

Example (Hydro Ottawa, 2025): SAIDI 0.976, SAIFI 0.709; SAIDI up 11.6% on 2024; 0.06 above its prior 5-year average; ranked 21 of 50 distributors on SAIDI.

## Data sources
1. OEB, Reporting and Record Keeping Requirements, System Reliability Indicators (2015-2025): https://oeb.ca/open-data/electricity-reporting-record-keeping-requirements-rrr-section-2142-system-reliability
2. OEB, Electricity Distributor Performance Scorecard (2025), used to verify the calculations: https://oeb.ca/open-data/electricity-distributor-performance-scorecard

## Data model
Star schema, two fact tables and two dimensions:
- `fact_reliability`: one row per distributor, year and cause of interruption
- `fact_customers`: average customers served, one row per distributor and year
- `dim_distributor`, `dim_year`

Power Query steps: parse the OEB XML, trim cause text, add a Loss of Supply flag, move customer counts into their own table (so they are not repeated per cause and summed in error), set data types.

## KPI definitions
- **SAIFI (scorecard basis):** customer interruptions from non-major events, excluding Loss of Supply, divided by average customers served.
- **SAIDI (scorecard basis):** customer-hours of interruption from non-major events, excluding Loss of Supply, divided by average customers served.
- **Benchmark:** average of the previous five years of the same measure. Calculated for this project. It is not an OEB target.
- **Rank:** 1 is the lowest (best) value among distributors with data for the selected year.

DAX measures: SAIDI/SAIFI, prior year, YoY %, 5-year average, benchmark, variance to benchmark, rank, rank label.

## Verification
The SAIDI/SAIFI definition above was checked against the figures published in the OEB 2025 scorecard for Hydro Ottawa and Alectra Utilities, and against Hydro Ottawa for 2020. The calculated values matched.

## Limitations
- The OEB scorecard file contains no target field, so the benchmark is a calculated 5-year average, not an official target.
- Rank compares all distributors together. Utilities differ widely in size and territory, so rank is not a like-for-like measure of performance.
- The 5-year average and benchmark are simple averages of annual ratios, not weighted by customers.
- The OEB notes that SAIDI/SAIFI reporting practices differ between distributors.
- Distributor names use the OEB's current company name. Mergers and renames can change a series.
- The benchmark is only meaningful from 2020, when five prior years exist.

## Files
- `report/[file name].pdf`: PDF export of the scorecard page

Data source: Ontario Energy Board open data. See https://oeb.ca/ontarios-energy-sector/open-data for terms.
