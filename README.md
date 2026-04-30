# TransLogix Shipment Delay Analytics

Big data analytics project on **100,000 logistics shipments** using **PySpark** and **Power BI** to investigate the drivers of shipment delays at TransLogix Solutions and predict delay duration in minutes.

This project covers the full analytics pipeline: loading and cleaning a 100K-record dataset with Spark, engineering time and operational features, exploring the data through aggregations and visualizations, building three regression models to predict delay minutes, and presenting the findings in an interactive Power BI dashboard.

---

## Tech Stack

- **Python** (Pandas, Matplotlib, Seaborn)
- **Apache Spark / PySpark** (Spark SQL, Spark MLlib)
- **Power BI** (interactive dashboard, DAX measures)
- **Jupyter Notebook**

---

## Dataset

- **Size:** 100,000 rows × 29 columns
- **Domain:** Logistics shipments — origin/destination hubs, routes, vehicles, scheduling, congestion indices, and actual vs. planned delivery times
- **Target:** `delay_minutes` (continuous — minutes late vs. scheduled arrival)

A 100-row sample is included in [`data/shipments_sample.csv`](data/shipments_sample.csv) so the notebook is reproducible. The full dataset is not included in the repo for size reasons.

**Key columns used:**
`origin_hub`, `destination_hub`, `route_id`, `distance_km`, `product_weight_kg`, `transport_mode`, `vehicle_reliability`, `planned_transit_hours`, `origin_congestion_idx`, `dest_congestion_idx`, `scheduled_departure`, `actual_arrival`, `delay_minutes`.

---

## What I Did

### 1. Data preparation in PySpark
- Loaded the dataset into a Spark DataFrame for distributed processing
- Cleaned and fixed column types, dropped irrelevant columns
- Engineered new features:
  - **Time features:** `month`, `day_of_week`, `departure_hour`
  - **Delay status:** `late` flag (delay > 0)
  - **Peak hour flag:** rush-hour indicator based on departure time

### 2. Analytical manipulation
- Filtering, grouping, aggregation, and sorting using Spark SQL
- Computed average delay by hub, by route, by day of week, and by hour
- Identified top hubs and routes contributing to delays

### 3. Visualizations
Six exploratory charts to surface delay patterns:
- Average delay by month (line chart)
- On-time vs. late shipments (bar chart)
- Average delay by day of week (bar chart)
- Average delay by departure hour (heatmap)
- Origin congestion vs. delay (scatter plot)
- Top origin hubs by average delay (bar chart)

### 4. Predictive modelling (Spark MLlib)
Built three regression models to predict `delay_minutes`:
- **Linear Regression** — baseline
- **Random Forest Regressor**
- **Gradient-Boosted Trees Regressor**

### 5. Power BI dashboard
Interactive dashboard with KPIs, slicers, and visuals for stakeholders to explore delays by hub, route, and time. Screenshots below.

---

## Results

| Model | RMSE (minutes) | R² |
|---|---|---|
| Linear Regression | 19.948 | 0.092 |
| Random Forest | 18.465 | 0.222 |
| **Gradient-Boosted Trees** | **18.399** | **0.228** |

GBT was the best performer of the three — lowest RMSE and highest R² — and was selected as the preferred model.

### Honest assessment of model performance

R² = 0.228 means the model only explains about 23% of the variance in delay minutes. This is not strong predictive power in absolute terms. The relative ordering (GBT > RF > Linear) is informative — it shows non-linear relationships matter — but the absolute results are limited by:

- **Feature ceiling:** the dataset doesn't include weather, real-time traffic, or driver-specific information, all of which are major drivers of real-world shipment delays
- **Inherent noise:** logistics delays have a large random component (accidents, equipment failures) that no model can predict from scheduled data alone
- **No hyperparameter tuning** was done in the original work — there's room to improve

The model is best treated as a **delay-risk indicator** to flag high-risk shipments for proactive intervention, rather than a precise minute-by-minute predictor.

---

## Exploratory Visualizations (Python/PySpark)

Key visualizations generated during exploratory analysis:

**Average Delay by Departure Hour (Heatmap)**

![Delay Heatmap](screenshots/Average%20Delay%20by%20Departure%20Hour%20(Heatmap).png)

**Average Delay by Month (Line Chart)**

![Monthly Trends](screenshots/Average%20Delay%20by%20Month%20(Line%20Chart).png)

**On-time vs Late Shipments (Bar Chart)**

![On-time vs Late](screenshots/On-time%20vs%20Late%20Shipments%20(Bar%20Chart).png)

**Origin Congestion vs Delay (Scatter Plot)**

![Congestion Scatter](screenshots/Origin%20Congestion%20vs%20Delay%20(Scatter%20Plot).png)

---

## Power BI Dashboard

Interactive dashboard designed for stakeholders to explore delays by hub, route, and time period:

![Power BI Dashboard](screenshots/final%20Power%20BI%20dashboard.png)

---

## Repo Structure

```
translogix-shipment-delay-analytics/
├── README.md                 # This file
├── BDFINL.ipynb              # Main Jupyter notebook (PySpark pipeline + ML)
├── data/
│   └── shipments_sample.csv  # 100-row sample of the dataset
├── screenshots/              # Power BI dashboard screenshots
└── report/
    └── Big_Data_Analytics.docx  # Full project report
```

---

## How to Run

1. Clone the repo
2. Install dependencies:
   ```bash
   pip install pyspark pandas matplotlib seaborn openpyxl
   ```
3. Open `BDFINL.ipynb` in Jupyter Notebook or Google Colab
4. To run on the full dataset, replace the sample CSV path with your own 100K-row file

---

## Key Takeaways

- Built and evaluated an end-to-end big-data pipeline on a 100K-row dataset using **PySpark** — including ingestion, cleaning, feature engineering, aggregation, and machine learning
- Compared three regression models on the same task and selected the best based on **RMSE and R²**
- Translated technical findings into a stakeholder-facing **Power BI dashboard** with KPIs and operational recommendations
- Critically evaluated the model's limitations rather than overstating its performance

---

## Author

**Mohammad Emad Jaber**
Data Science & Artificial Intelligence student at Al-Hussein Technical University (HTU)
[LinkedIn](https://linkedin.com/in/mohammad-jaber-a0729a2aa) · mohammademadjaber2004@gmail.com
