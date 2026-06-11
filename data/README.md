# Data

| File                   | What it is                                               |
| ---------------------- | -------------------------------------------------------- |
| `affordability-*.xlsx` | Your Shimberg Center download (main dataset)             |
| `fl_county_fips.csv`   | Florida county name → Census FIPS code (for Plotly maps) |

## Shimberg Excel (`affordability-*.xlsx`)

Download from the [Florida Housing Data Clearinghouse](https://flhousingdata.shimberg.ufl.edu/):

1. Click **Affordability**
2. Under **Geographic Areas: Affordability**, select all Florida counties
3. Click **Next** → **Download data as Excel**
4. Save the `.xlsx` to this `data/` folder

## County FIPS lookup (`fl_county_fips.csv`)

This file is **not** downloaded from Shimberg. The Shimberg export only has county names, but Plotly choropleth maps need 5-digit [U.S. Census Bureau](https://www.census.gov/library/reference/code-lists/ansi.html) county FIPS codes. This lookup pairs all 67 Florida county names with their Census codes so the notebook can merge county stats to [Plotly's county GeoJSON](https://raw.githubusercontent.com/plotly/datasets/master/geojson-counties-fips.json) in Section 6.

**How this file was built:**

1. Open the [Census ANSI county code list](https://www.census.gov/library/reference/code-lists/ansi.html) (or the [county FIPS reference table](https://www2.census.gov/geo/docs/reference/codes/files/national_county.txt))
2. Filter to Florida (state FIPS code `12`)
3. For each county, combine state + county code into one 5-digit FIPS (e.g. Manatee = `12081`)
4. Save two columns, `county` and `fips`, matching the county names in the Shimberg export (no ` County` suffix)

The file is already in the repo and rarely changes. You do not need to rebuild it to run the notebook unless you want to verify the codes from source.

## Sheets used

The workbook has many tabs, but this project reads seven:

| Sheet      | Content                                                                                                                                                           |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Sheet 1`  | All Households, Cost Burden by Income, 2023 estimates (statewide analysis, maps, income-band heatmap)                                                             |
| `Sheet 2`  | Renter Households, Cost Burden by Income (renter vs owner comparison)                                                                                             |
| `Sheet 3`  | Owner-Occupied Households, Cost Burden by Income (renter vs owner comparison)                                                                                     |
| `Sheet 4`  | Homeownership Rate (%), 1990 through 2024 (time-trend line plot in Section 7)                                                                                     |
| `Sheet 5`  | Median Income by Tenure (2024 ACS median household income by county: all households, owners, renters; used to translate AMI bands into real dollars in Section 5) |
| `Sheet 18` | Wage and Rent Comparison by Occupation, 2024 (median wages vs 2025 HUD 2-bedroom Fair Market Rent, by metro area; Section 8)                                      |
| `Sheet 22` | Households by Tenure, Household Type, and Cost Burden, 2020-2024 ACS (household-type heatmap in Section 9)                                                        |

**A note on blank cells:** in the cost-burden sheets, a few small counties have blank cells where an estimate was too small to report. The notebook fills those with 0 - leaving them as `NaN` would make whole rows of households vanish from county totals and inflate some rural counties' burden rates by 15-30 percentage points.
