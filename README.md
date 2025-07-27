# Assignment_UBER_Project
Data_analytics of the uber_fares  data set
## 📊 Data Analysis Process (Expanded Overview)

This analytical report presents findings from the Uber Fares Dataset using Python and Power BI. The objective was to uncover fare dynamics, ride behavior trends, and operational insights that could inform business strategy.

---

### 📘 Introduction

This project investigates Uber ride data to extract meaningful trends and patterns around fare pricing and ride behavior. The key goal was to gain operational insights using Python for data analysis and Power BI for interactive dashboard development. The dataset was obtained from Kaggle and included pickup/dropoff coordinates, timestamps, passenger count, and fare amounts.

---

### 🛠️ Methodology

The methodology followed a structured data pipeline:

* **Data Collection:** The dataset was downloaded from Kaggle and loaded using Pandas.
* **Data Cleaning:** The removal of  missing values and duplicates
* 
###
###
* **Feature Engineering:** We extracted time-based variables (hour, day, month, day\_of\_week) and created new flags (`is_weekend`, `is_peak_hour`).
* **Distance Calculation:** Used the Haversine formula to compute trip distance in kilometers.
* **Exploratory Data Analysis (EDA):** Statistical summaries, correlations, and plots were used to explore data relationships.
* **Export:** Cleaned and enhanced dataset was saved as a CSV for Power BI import.

---

### 📊 Analysis (Code, Findings, and Statistical Insights)

#### 🔍 Descriptive Statistics

Used `.describe()`, `.median()`, `.mode()` to understand fare distribution:

* Mean: \$11.36
* Median: \$8.50
* Mode: \$6.50
* Max: \$499 (outlier)

#### ⚠️ Outlier Detection

Applied IQR method to detect values beyond 1.5×IQR.

```python
Q1 = df['fare_amount'].quantile(0.25)
Q3 = df['fare_amount'].quantile(0.75)
IQR = Q3 - Q1
lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR
```

#### 💡 Feature Engineering

```python
df['hour'] = df['pickup_datetime'].dt.hour
df['day'] = df['pickup_datetime'].dt.day
df['month'] = df['pickup_datetime'].dt.month
df['day_of_week'] = df['pickup_datetime'].dt.dayofweek
df['is_weekend'] = df['day_of_week'].isin([5, 6]).astype(int)
df['is_peak_hour'] = df['hour'].isin([7, 8, 9, 17, 18, 19]).astype(int)
```

#### 🛋️ Distance Calculation (Haversine Formula)

```python
def haversine(lat1, lon1, lat2, lon2):
    r = 6371
    ...
    return 2 * r * np.arcsin(np.sqrt(a))
```

#### 🌍 Correlation Analysis

```python
correlation_matrix = df[['fare_amount', 'distance_km', 'passenger_count', 'hour', 'day_of_week']].corr()
sns.heatmap(correlation_matrix, annot=True)
```

#### 🌈 Visual Analysis

* Histogram for fare distribution
* Scatter plot: Fare vs. Distance
* Boxplot: Fare vs. Hour

---

### 📈 Results: Discoveries and Patterns

* Fare amount is **strongly correlated** with trip distance (r ≈ 0.83).
* **Outliers** were identified and excluded for more reliable visuals.
* **Peak hours (7–9 AM & 5–8 PM)** tend to have higher fares.
* Ride frequency increases during **weekends**, especially Fridays and Saturdays.

---

### 📋 Conclusion

The data showed clear fare trends based on time and trip distance. Most Uber fares fall between \$5 and \$20, with peak demand during commute hours and weekends. The analysis confirmed that fare is directly influenced by ride distance, time of day, and day of the week.

---

### 💡 Recommendations

Based on the analysis:

1. **Increase driver availability** during peak periods to reduce wait time and optimize pricing.
2. **Target promotions during off-peak hours** to balance ride volume.
3. **Flag and investigate outlier rides** to maintain data integrity.
4. **Incorporate external data** (e.g., weather, events) in future analysis to improve context.

---

### 📂 Dataset Export

```python
df.to_csv("uber_enhanced.csv", index=False)
```

This final dataset was imported into Power BI for dashboard development.

---

The structured process of statistical and visual analysis provided actionable insights into ride dynamics and pricing behavior within Uber's service model.
