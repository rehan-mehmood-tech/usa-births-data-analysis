# USA Births Data Analysis | 1969–2008

An exploratory analysis of historical United States birth counts using Python and Jupyter Notebook. The project examines annual trends, male and female birth totals, and seasonal patterns through aggregation, descriptive statistics, and visual comparisons.

These records offer a way to explore how birth volumes vary over time. The dataset contains **birth counts**, not population-adjusted birth rates.

## Project Overview

The analysis uses Pandas pivot tables to summarize births by year, month, and recorded gender, then visualizes the results with line and grouped bar charts. It also calculates annual male–female differences and the standard deviation across the two gender totals within each year.

The notebook includes an attempted weekday analysis. Its date mapping needs correction before those charts can support weekday conclusions; see [Data Quality and Limitations](#data-quality-and-limitations).

## Dataset Information

| Attribute | Details |
| --- | --- |
| Source | Kaggle, as identified in the project brief; no verified dataset URL is supplied |
| Local file | [`births.csv`](births.csv) |
| Coverage | 1969–2008 inclusive, covering 40 years |
| Records | 15,547 rows and five original columns |
| Total represented births | 151,774,378, calculated by summing `births` |
| Granularity | Daily records in 1969–1988; monthly records in 1989–2008, where `day` is missing |

Each row represents an aggregated birth count, rather than an individual birth.

| Column | Loaded type | Description |
| --- | --- | --- |
| `year` | Integer | Calendar year, 1969–2008 |
| `month` | Integer | Calendar month, 1–12 |
| `day` | Float | Day of the month; includes values 1–31, the non-calendar value `99`, and missing values |
| `gender` | String | Recorded category: `F` or `M` |
| `births` | Integer | Number of births represented by the row |

`day` loads as a floating-point column because it contains missing values. The notebook additionally derives `weekday` and `month_name`; these are not original CSV columns.

## Tools and Technologies

| Tool | Role |
| --- | --- |
| Python | Analysis programming language |
| Pandas | CSV loading, exploration, pivot tables, aggregation, and statistics |
| Matplotlib | Line charts, labels, legends, and plot styling |
| Seaborn | Installed and imported; no Seaborn plotting calls are currently used |
| Jupyter Notebook | Interactive analysis, narrative, and inline chart outputs |

## Data Analysis Workflow

**Data Loading → Data Exploration → Data Aggregation → Statistical Analysis → Visualization → Key Insights**

1. Load `births.csv` with `pd.read_csv()`.
2. Inspect sample records with `head()` and numeric summaries with `describe()`.
3. Sum birth counts in pivot tables grouped by year, month, and recorded gender.
4. Calculate annual totals, male–female differences, and within-year standard deviations.
5. Compare the grouped counts using line charts and grouped bar charts.
6. Interpret patterns against the source data and its limitations.

The notebook does not currently implement date cleaning or a valid calendar-to-weekday conversion.

## Exploratory Data Analysis

### Yearly Birth Trends

A year-by-gender pivot table sums birth counts and derives `total_births`. A two-series line chart compares annual female and male births. The series includes both increases and declines, so it does not show uninterrupted annual growth.

### Male vs. Female Birth Comparisons

The annual table calculates `different_in_births` as male births minus female births. Its `std_dev` column uses Pandas' sample standard deviation across the female and male totals **within each year** (`axis=1`, default `ddof=1`). This measures the spread between those two totals, rather than year-to-year variability or uncertainty in an estimate.

### Monthly Birth Distribution

Numeric months are mapped to names and reordered from January through December. A month-by-gender pivot table sums births across all 40 years, supporting line and grouped bar comparisons. These totals are not adjusted for different month lengths.

### Weekday Birth Distribution — Requires Correction

The notebook maps `day` values 1–7 directly to Monday–Sunday. Because `day` represents a day of the month, this labels calendar dates incorrectly and excludes every other day and missing value from the pivot table. The resulting charts do **not** describe actual weekday birth distributions.

A valid weekday analysis would require constructing dates from `year`, `month`, and `day`, excluding invalid dates, and deriving weekdays from those dates. Records with missing days cannot support that analysis, so its coverage would differ from the full annual and monthly analyses. This correction is not implemented in the existing notebook.

### Descriptive Statistics

`describe()` summarizes numeric source columns and the derived annual table with counts, means, standard deviations, quartiles, and extrema. Because source rows mix daily and monthly aggregation, row-level birth statistics should not be interpreted as statistics for individual days across the entire period.

## Data Visualizations

The five charts are embedded in [`script.ipynb`](script.ipynb). No standalone chart image files are included.

| Chart | Format | Interpretation |
| --- | --- | --- |
| Yearly Male and Female Births | Line chart | Annual birth totals by gender |
| Births by Day of the Week | Line chart | Existing output uses an invalid weekday mapping |
| Births by Day of the Week | Grouped bar chart | Existing output uses an invalid weekday mapping |
| Births by Month | Line chart | Cumulative monthly totals by gender |
| Month-wise Male and Female Births | Grouped bar chart | Cumulative monthly totals by gender |

## Key Findings

The following results were recalculated directly from the supplied CSV using sums of `births`:

| Measure | Verified result |
| --- | --- |
| Dataset size | 15,547 aggregated records |
| Total births represented | 151,774,378 |
| Highest annual total | **2007: 4,324,008 births** |
| Lowest annual total | 1973: 3,146,125 births |
| Highest cumulative monthly total | **August: 13,528,007 births** |
| Male births | 77,738,555 |
| Female births | 74,035,823 |
| Annual gender comparison | Male births exceed female births in every year in the file |

The notebook's executive summary names 2006 as the peak year; the CSV instead identifies 2007. Its claim that Wednesday has the most births is unsupported because the weekday calculation is invalid. The August finding is confirmed for cumulative counts, without adjustment for month length.

## Data Quality and Limitations

- `day` contains 480 missing values, covering the monthly records for 1989–2008. It also includes `99`, which is not a valid calendar day.
- Calendar-date parsing rejects 937 rows: 480 with missing days, 350 with `day=99`, and 107 with other invalid year–month–day combinations. These require explicit handling before any daily analysis.
- Annual and monthly summaries above retain all supplied birth counts, including rows without a valid daily date.
- The weekday charts require correction before interpretation. They cannot establish a weekday peak.
- Population denominators are absent, so the project cannot calculate birth rates or compare population-adjusted fertility trends.
- Aggregated comparisons describe patterns; the notebook does not test statistical significance or establish causes.
- The supplied project does not include a dataset source URL, dependency lockfile, or license. Dataset provenance and reuse terms require confirmation from the original source.

## Run the Analysis

From the project directory, install the required packages and launch Jupyter:

```bash
python -m pip install pandas matplotlib seaborn notebook
python -m notebook
```

Open `script.ipynb` and run its cells in order. Keep `births.csv` in the notebook's working directory because the notebook loads it using a relative path. The notebook also includes a package-installation cell.

The existing code and saved outputs are preserved. Running the notebook reproduces its current calculations, including the weekday mapping issue described above.

## Repository Structure

```text
.
├── README.md       # Project documentation and verified findings
├── births.csv      # Aggregated birth counts
└── script.ipynb    # Analysis, statistical summaries, and inline charts
```
