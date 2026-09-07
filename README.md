# Seasonal Agriculture Performance Analysis

A Python-based data analytics project that analyzes how agricultural performance—including yield, production, profitability, water efficiency, and disease/pest risk—varies across the Kharif, Rabi, and Zaid seasons. Using a 4,000-record farm-level dataset, the project applies exploratory analysis, statistical testing, visualization, and predictive modeling to generate evidence-based insights and recommendations.

[![Python](https://img.shields.io/badge/Python-3.11-blue.svg)](https://www.python.org/)
[![Jupyter Notebook](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458.svg)](https://pandas.pydata.org/)
[![NumPy](https://img.shields.io/badge/NumPy-Numerical%20Computing-013243.svg)](https://numpy.org/)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-Data%20Visualization-11557C.svg)](https://matplotlib.org/)
[![Seaborn](https://img.shields.io/badge/Seaborn-Statistical%20Visualization-4C72B0.svg)](https://seaborn.pydata.org/)
[![SciPy](https://img.shields.io/badge/SciPy-Scientific%20Computing-8CAAE6.svg)](https://scipy.org/)
[![Scikit--learn](https://img.shields.io/badge/Scikit--learn-Machine%20Learning-F7931E.svg)](https://scikit-learn.org/)

---

## 📌 Project Overview

Agricultural performance is shaped by seasonal variation in environmental conditions, resource use, and market conditions — yet raw farm data rarely makes those seasonal patterns explicit. This project analyzes a **4,000-record farm-level dataset** spanning **8 states, 10 districts, 8 crops, 3 seasons, and 4 irrigation methods** to identify meaningful, statistically supported seasonal patterns in agricultural performance.

The analysis covers the full data-analytics lifecycle — data understanding, quality assessment, cleaning, feature engineering, exploratory data analysis, group-wise comparison, correlation and hypothesis testing, outlier review, and an optional predictive model — before closing with evidence-based insights and recommendations. Every statistic, chart, and conclusion in the notebook is calculated dynamically from the dataset; nothing is hard-coded or invented.

---

## 🎯 Objectives

- Understand and clean the seasonal agricultural dataset
- Compare yield, production, and profitability across seasons
- Examine seasonal differences in environmental conditions (rainfall, temperature, humidity, sunlight, soil)
- Study resource usage (fertilizer, pesticide, nutrients, water) across seasons
- Compare performance across crops, states, districts, and irrigation methods
- Analyze water-use efficiency and disease/pest risk by season
- Apply appropriate statistical tests (correlation, ANOVA/Kruskal-Wallis, t-test/Mann-Whitney) to validate seasonal differences
- Build an optional predictive model for yield, with explicit data-leakage checks
- Generate data-driven insights and practical recommendations for farmers, planners, and farm managers

---

## 📂 Dataset

**File:** `seasonal_agriculture_performance_dataset.csv`
**Size:** 4,000 rows × 28 columns

| Category | Columns |
|---|---|
| Farm & Geography | `Farm_ID`, `State`, `District` |
| Crop & Season | `Crop`, `Season` |
| Environmental Conditions | `Farm_Area_Hectares`, `Rainfall_mm`, `Avg_Temperature_C`, `Humidity_pct`, `Sunlight_Hours_Day` |
| Soil Conditions | `Soil_pH`, `Soil_Moisture_pct` |
| Nutrient & Input Usage | `Nitrogen_kg_ha`, `Phosphorus_kg_ha`, `Potassium_kg_ha`, `Fertilizer_kg_ha`, `Pesticide_Litre_ha`, `Seed_Quality_Score` |
| Farming & Irrigation | `Irrigation_Method` |
| Agricultural Performance | `Yield_Tonnes_Ha`, `Production_Tonnes` |
| Economic Performance | `Market_Price_INR_Tonne`, `Total_Cost_INR`, `Revenue_INR`, `Profit_INR` |
| Water Performance | `Water_Used_m3`, `Water_Efficiency_t_per_1000m3` |
| Risk | `Disease_Pest_Risk_pct` |

**Coverage:** 8 states · 10 districts · 8 crops · 3 seasons (Kharif, Rabi, Zaid) · 4 irrigation methods

---

## 🛠️ Technologies Used

- **Python 3.11**
- **Jupyter Notebook**
- **Pandas** — data manipulation and aggregation
- **NumPy** — numerical operations
- **Matplotlib** — visualization
- **Seaborn** — statistical visualization
- **SciPy** — hypothesis testing (Pearson/Spearman correlation, ANOVA, Kruskal-Wallis, t-test, Mann-Whitney U, Levene's test)
- **Scikit-learn** — predictive modeling (Linear Regression, Random Forest), preprocessing, and evaluation metrics

---

## 🔄 Project Workflow

1. Dataset loading and structural verification
2. Data understanding (numerical/categorical breakdown, unique categories)
3. Data quality assessment (missing values, duplicates, invalid values, economic consistency checks)
4. Data cleaning (season-wise median imputation for selected environmental variables)
5. Feature engineering (profit margin, per-hectare metrics, risk categories)
6. Exploratory data analysis (univariate, bivariate, multivariate)
7. Seasonal, environmental, crop, state, district, and irrigation analysis
8. Resource usage, soil/nutrient, yield, production, and economic analysis
9. Water efficiency and disease/pest risk analysis
10. Cross-dimensional analysis (Season × Crop, Season × Irrigation, Season × State)
11. Correlation analysis and formal statistical testing
12. Outlier analysis
13. Optional predictive modeling
14. Key insights, recommendations, limitations, and conclusion

---

## 🧹 Data Cleaning

- Verified dataset shape, data types, and category labels dynamically rather than assuming them
- Checked for logically invalid values (negative area, out-of-range percentages, negative costs, etc.) — none found
- Missing values found in **3 columns** (`Rainfall_mm`, `Soil_Moisture_pct`, `Yield_Tonnes_Ha`), each under 1.5% of records
- `Rainfall_mm` and `Soil_Moisture_pct` imputed using **season-wise medians** for descriptive analysis
- **`Yield_Tonnes_Ha` missing values (32 records, 0.8%) were deliberately *not* imputed.** A separate observed-only dataframe (`df_yield_observed`, 3,968 records) is used for every correlation, hypothesis test, and the predictive model target, so no guessed value can influence a statistical result
- Verified `Profit = Revenue − Cost` and `Revenue ≈ Production × Market Price` hold almost exactly across the dataset, confirming the economic columns are internally consistent

---

## ⚙️ Feature Engineering

| Feature | Definition | Purpose |
|---|---|---|
| `Profit_Margin_pct` | `Profit / Revenue × 100` | Profitability independent of farm size |
| `Cost_per_Hectare` | `Total_Cost / Farm_Area` | Size-normalized cost comparison |
| `Revenue_per_Hectare` | `Revenue / Farm_Area` | Size-normalized revenue comparison |
| `Profit_per_Hectare` | `Profit / Farm_Area` | Size-normalized profit comparison |
| `Water_Used_per_Hectare` | `Water_Used / Farm_Area` | Size-normalized water usage |
| `Revenue_per_Tonne` | `Revenue / Production` | Effective realized price per tonne |
| `Risk_Category` | Binned `Disease_Pest_Risk_pct` | Low / Medium / High risk grouping |

All divisions use safe-division handling to avoid division-by-zero errors.

---

## 📊 Exploratory Data Analysis

- **Univariate:** distribution analysis of yield, production, profit, and disease/pest risk (all right-skewed except risk)
- **Bivariate:** performance metrics compared against season, crop, state, and irrigation method using bar charts, box plots, and violin plots
- **Multivariate:** full correlation matrix and a multi-feature predictive model

Skewed distributions in yield, production, and profit justified reporting **both mean and median** throughout, rather than relying on the mean alone.

---

## 🔥 Correlation Analysis

A full correlation matrix was built across 22 numerical variables to identify the strongest relationships involving yield, profit, and water efficiency.

| Relationship | Pearson r |
|---|---|
| Water Used vs. Yield | 0.389 |
| Production vs. Profit | 0.554 |
| Market Price vs. Profit | 0.225 |
| Market Price vs. Revenue | 0.182 |
| Rainfall vs. Yield | 0.028 (not significant, p = 0.074) |
| Soil Moisture vs. Yield | 0.010 |

Correlations involving `Yield_Tonnes_Ha` use only the 3,968 observed-yield records. **Correlation is consistently framed as association, not causation**, throughout the analysis.

---

## 📊 Statistical Analysis

Formal hypothesis testing was applied at **α = 0.05**, with assumptions checked rather than assumed:

- **Kruskal-Wallis test** (primary, given yield's skew) — yield differs significantly across seasons (**p < 0.00001**)
- **One-way ANOVA** (reference) — consistent with the Kruskal-Wallis result
- **Levene's test** — checked variance homogeneity across season groups before relying on ANOVA
- **Kruskal-Wallis test** — profit also differs significantly across seasons (**p < 0.00001**)
- **Independent t-test & Mann-Whitney U** — yield compared between the two largest seasons by record count
- **Pearson/Spearman correlation tests** — rainfall, temperature, soil moisture, fertilizer, water use, and disease/pest risk tested against yield, with exact test statistics and p-values reported
- A multiple-comparisons caveat is stated explicitly given the number of tests run

---

## 🤖 Predictive Modeling

An optional yield-prediction model was built to complement the descriptive and correlational analysis:

| Model | MAE | RMSE | R² |
|---|---|---|---|
| Linear Regression | 1.923 | 4.931 | 0.842 |
| **Random Forest** | **0.735** | **2.589** | **0.957** |

- **Target:** `Yield_Tonnes_Ha` (observed values only — missing-target rows excluded, never imputed)
- **Features:** environmental, soil, nutrient, input, and categorical variables (`Season`, `Crop`, `Irrigation_Method`)
- **Leakage prevention:** `Production_Tonnes`, `Revenue_INR`, `Total_Cost_INR`, `Profit_INR`, and all profit/revenue-derived engineered features were deliberately excluded, since they are mathematically downstream of yield
- Train/test split performed before preprocessing; all metrics computed fresh on held-out data

---

## 💡 Key Insights

1. **Kharif** has the highest average yield (5.64 t/ha); **Zaid** has the lowest (4.67 t/ha)
2. **Kharif** generates the highest average profit (₹178,915 per farm on average)
3. **Kharif** has the highest total production (82,388 tonnes)
4. **Zaid** uses the most water on average (6,420 m³)
5. **Kharif** has the best seasonal water efficiency (5.89 t/1000m³)
6. **Rainfed** irrigation has the highest average observed water efficiency (7.56 t/1000m³)
7. **Sugarcane** is the most profitable crop on average; **Wheat** the least
8. **Kharif** has the highest average disease/pest risk (54.5%)
9. **Punjab** leads both average profit and average yield among the 8 states
10. Market price correlates only modestly with profit (r = 0.225) — price alone does not determine profitability
11. Water usage correlates moderately with yield (r = 0.389) — more water is not proportionally better
12. **49.1% of farms** in this dataset (1,966 of 4,000 records) operated at a loss — a substantial, dataset-confirmed finding, not an outlier to be dismissed

---

## 📋 Project Results

- Yield and profit differences across seasons are **statistically significant** (Kruskal-Wallis p < 0.00001 for both)
- The season, crop, or state that leads on one metric does **not** consistently lead on all others — reinforcing the need for multi-metric evaluation over single-indicator decisions
- Random Forest achieved an R² of 0.957 on the held-out test set, using only pre-harvest features (with all yield-derived and profit-derived variables excluded to prevent leakage)
- Nearly half of all farms in the dataset are currently loss-making, identified and analyzed rather than excluded as outliers

---

## 📸 Visualizations

The notebook includes multiple purpose-built analytical visualizations covering seasonal, crop, state, irrigation, economic, water-efficiency, risk, correlation, and outlier analysis, including:

- Seasonal bar charts, box plots, and violin plots for yield, profit, and water efficiency
- Environmental condition comparisons across seasons
- Crop × Season and State × Season heatmaps
- A full 22-variable correlation heatmap
- Feature-importance chart from the Random Forest model
- Outlier screening box plots across all key numerical variables

*(See the notebook  for all rendered charts with full titles, axis labels, and legends.)*

---

## 📌 Limitations

- **Missing yield observations:** 32 records (0.8%) with missing `Yield_Tonnes_Ha` were excluded — not imputed — from every yield-related test and model
- **Cross-sectional, not time-series:** each record is a single farm-season snapshot, not a historical time series
- **No causal design:** irrigation method, crop choice, and farming practices are not randomly assigned, so all relationships are associations, not proven causes
- **District naming:** the same district names recur across multiple states in this dataset
- **Missing contextual variables:** no farm-management history, planting/harvest dates, or granular market/demand data
- **Predictive model scope:** the optional model is an exploratory tool, not a production forecasting system

---

## 🚀 Future Improvements

- Real-time weather and seasonal data integration
- Predictive crop-yield modeling on larger, multi-year datasets
- Satellite / remote-sensing data integration
- Broader multi-region and multi-year historical coverage
- Real-time crop-risk monitoring
- Improved resource and water-management analytics

---

## 🚀 How to Run

1. **Clone the repository**
   ```bash
   git clone https://github.com/Hariomdubey01/Seasonal-Agriculture-Performance-Analysis.git
   cd Seasonal-Agriculture-Performance-Analysis
   ```

2. **Install dependencies**
   ```bash
   pip install pandas numpy matplotlib seaborn scipy scikit-learn jupyter
   ```

3. **Launch the notebook**
   ```bash
   jupyter notebook Seasonal_Agriculture_Performance_Analysis.ipynb
   ```

4. **Run all cells** — the notebook executes top-to-bottom with no manual steps required, using `seasonal_agriculture_performance_dataset.csv` as the data source.

---

## 📁 Project Structure

```
Seasonal-Agriculture-Performance-Analysis/
│
├── Seasonal_Agriculture_Performance_Analysis.ipynb   # Main analysis notebook
├── seasonal_agriculture_performance_dataset.csv      # Source dataset (4,000 records)
├── Project_Presentation.pptx                         # Project presentation slides
├── Project_Problem_Statement.pdf                     # Official project requirements
└── README.md                                         # Project documentation
```

---

## 👨‍💻 About Me

**Hariom Dubey**

Aspiring **Data Analyst** passionate about transforming data into meaningful business insights.

### Areas of Interest

- Data Analytics
- Business Intelligence
- Data Visualization
- SQL
- Python
- Power BI
- Machine Learning

---

## 📬 Contact

| Platform | Link |
|----------|------|
| 📧 Email | <mailto:hariomkumard8@gmail.com> |
| 💼 LinkedIn | [linkedin.com/in/itzhariomdubey](https://www.linkedin.com/in/itzhariomdubey) |
| 💻 GitHub | [github.com/Hariomdubey01](https://github.com/Hariomdubey01) |
---


## 🙏 Acknowledgements

This project was developed as part of the VOIS / AICTE Data Analytics internship program. Special thanks to the organizations and mentors involved in supporting this learning and project experience, including Edunet Foundation, Vodafone Idea Foundation, VOIS, and AICTE.

---

## 📄 License

This project is intended for educational and academic purposes.

---

## ⭐ Support

If you found this project useful or interesting, please consider giving it a ⭐ on GitHub — it helps others discover it and supports continued development.
