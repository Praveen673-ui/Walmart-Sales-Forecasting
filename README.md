# Walmart Sales Analysis & Forecasting

An end-to-end data analysis and time-series forecasting project using Walmart weekly sales data. The project explores how sales vary across stores and over time, examines relationships between weekly sales and external factors, and uses Auto ARIMA to forecast sales for the next 12 weeks.

## Project Objectives

- Understand weekly sales patterns and distributions.
- Explore the relationship between sales and factors such as unemployment, temperature, and the Consumer Price Index (CPI).
- Examine seasonal and holiday-related sales patterns.
- Compare store performance.
- Build short-term weekly sales forecasts for individual stores and across all stores.

## Dataset

The notebook loads a local CSV file named `Walmart DataSet.csv`.

The columns used in the analysis are:

| Column | Description |
|---|---|
| `Store` | Store identifier |
| `Date` | Week/date of the sales record |
| `Weekly_Sales` | Weekly sales for the store |
| `Holiday_Flag` | Indicates whether the week is a holiday week |
| `Temperature` | Temperature associated with the record |
| `Fuel_Price` | Fuel price |
| `CPI` | Consumer Price Index |
| `Unemployment` | Unemployment rate |

**Note:** The dataset is not included in this repository/notebook export. Place `Walmart DataSet.csv` in the same directory as the notebook before running it. The notebook expects that exact filename.

## Project Workflow

### 1. Data loading and preprocessing

- Imported the analysis and time-series libraries.
- Loaded the Walmart CSV dataset with Pandas.
- Converted `Date` to datetime format.
- Extracted `Month` and `Year` from the date.
- Checked for missing values. The notebook output shows zero missing values in the listed columns.
- Applied an IQR-based filter to remove records with `Weekly_Sales` outside 1.5 times the interquartile range.

### 2. Exploratory Data Analysis

The notebook uses visualizations and grouped summaries to investigate:

- Distribution of weekly sales.
- Potential outliers in weekly sales before and after IQR filtering.
- Monthly average sales trends.
- Differences in weekly sales between holiday and non-holiday weeks.
- Relationships between weekly sales and unemployment, temperature, and CPI.
- Total sales by store, including the top-performing and lowest-performing stores.

### 3. Store and economic-factor analysis

The notebook calculates the overall correlation between `Weekly_Sales` and selected economic variables, and calculates the unemployment-sales correlation separately for each store.

Selected results from the notebook:

- Overall correlation between weekly sales and unemployment: **-0.104** (weak negative relationship).
- Overall correlation between weekly sales and temperature: **-0.044** (very weak negative relationship).
- Overall correlation between weekly sales and CPI: **-0.070** (very weak negative relationship).
- Store **4** has the highest total weekly sales in the filtered dataset.
- Store **33** has the lowest total weekly sales in the filtered dataset.
- The difference between those stores' total sales is approximately **243.88 million** in the dataset's sales units.

These are descriptive results from the notebook, not evidence that the economic variables cause sales to change.

### 4. Time-series forecasting with Auto ARIMA

The forecasting section:

1. Filters the data for Store 1 and sorts it by date.
2. Uses the Augmented Dickey-Fuller (ADF) test to check stationarity.
3. Runs `auto_arima` with seasonal modeling enabled and a seasonal period of 52 weeks.
4. Generates a 12-week forecast for Store 1.
5. Repeats the Auto ARIMA process for each store and stores the forecasts in a dictionary.
6. Plots actual and forecast sales for the first five stores.

For Store 1, the notebook's ADF test reports a p-value of approximately **0.0000139**. The selected Auto ARIMA model shown in the notebook output is **ARIMA(1,1,1)(1,0,0)[52]**. The notebook generates 12 future weekly forecast values.

## Key Findings

- Weekly sales show variation across months and stores.
- The notebook's monthly and holiday visualizations are used to explore seasonal sales patterns and possible holiday effects.
- The overall relationships between weekly sales and unemployment, temperature, and CPI are weak in the calculated correlations.
- Store 4 is the highest-selling store and Store 33 is the lowest-selling store by total sales in the filtered data.
- Auto ARIMA is used to generate short-term, 12-week forecasts for Store 1 and all stores.

## Tools and Libraries

- **Python** — analysis and forecasting
- **Pandas** — data loading, cleaning, grouping, and manipulation
- **NumPy** — numerical operations
- **Matplotlib** and **Seaborn** — data visualization
- **Statsmodels** — Augmented Dickey-Fuller stationarity test and ARIMA tools
- **pmdarima** — automatic ARIMA model selection (`auto_arima`)
- **Jupyter Notebook** — development environment

## Repository Structure

```text
Walmart-Sales-Forecasting/
├── Walmart Sales-Forcast(Project).ipynb
├── Walmart DataSet.csv          # Add the dataset locally
├── README.md
└── requirements.txt             # Optional dependency file
```

## How to Run the Project

### 1. Clone the repository

```bash
git clone <your-repository-url>
cd Walmart-Sales-Forecasting
```

### 2. Create and activate a virtual environment (recommended)

**Windows**

```bash
python -m venv .venv
.venv\Scripts\activate
```

**macOS / Linux**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install pandas numpy matplotlib seaborn statsmodels pmdarima jupyter
```

Alternatively, if you maintain a `requirements.txt` file:

```bash
pip install -r requirements.txt
```

### 4. Add the dataset

Place `Walmart DataSet.csv` in the project directory, alongside the notebook.

### 5. Launch Jupyter Notebook

```bash
jupyter notebook
```

Open `Walmart Sales-Forcast(Project).ipynb` and run the cells in order.

## Business Relevance

Weekly sales analysis and short-term forecasts can support retail planning activities such as inventory replenishment, staffing, and preparation for seasonal demand. Store-level comparisons can also help identify locations that may need further investigation. The forecasts in this notebook are analytical estimates and should be validated against later actual sales before being used for operational decisions.

## Scope and Limitations

- The notebook forecasts weekly sales; it does not build a complete inventory optimization or sales-planning system.
- Forecasting uses Auto ARIMA with a 52-week seasonal period. Forecast quality is not established through a held-out test period or reported error metrics in the notebook.
- The IQR filter is applied to `Weekly_Sales` across the dataset. This may remove legitimate high-sales weeks, including unusual seasonal or holiday peaks.
- Correlation describes association, not causation.
- The notebook's Store 1 forecast uses generated weekly dates; date alignment should be checked when comparing forecasts with the original data.
- The notebook's all-store plotting section should be reviewed to ensure forecast dates are used on the x-axis.

## Future Improvements

- Evaluate forecasts using a chronological train/test split.
- Report forecasting metrics such as MAE, RMSE, and MAPE.
- Compare Auto ARIMA with seasonal-naive and other forecasting baselines.
- Review outlier handling by store and season rather than relying only on a single dataset-wide IQR filter.
- Improve forecast visualizations by plotting the actual forecast dates.
- Save forecast outputs to CSV for further analysis.

---

**Project focus:** Retail sales analysis, store performance comparison, and 12-week time-series forecasting.
