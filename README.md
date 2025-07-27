 Assignment_UBER_Project
Data_analytics of the uber_fares  data set
## 📊 Data Analysis Process (Expanded Overview)

This analytical report presents findings from the Uber Fares Dataset using Python and Power BI. The objective was to uncover fare dynamics, ride behaviour trends, and operational insights that could inform business strategy.

---

### 📘 Introduction

This project analyses Uber rides' data to identify meaningful trends and patterns related to fare-pricing, distance, time and ride behaviour.
The key goal was to know the correlation of these factors  using Python for data analysis and Power BI for interactive dashboard development.
The dataset was obtained from Kaggle and included pickup/dropoff coordinates, timestamps, passenger count, and fare amounts.

---

### 🛠️ Methodology

The methodology followed a structured data pipeline:

* **Data Collection:** The dataset was downloaded from Kaggle and loaded using Pandas.
* **Data Cleaning:**
* The removal of  missing values, duplicates , exporting data
* <img width="671" height="400" alt="Cleaned6" src="https://github.com/user-attachments/assets/a4b6e09c-e45b-439c-ac82-f022851cf80f" />

* <img width="657" height="595" alt="Cleaned7" src="https://github.com/user-attachments/assets/8aabb6d9-491e-4b25-93a9-c2e1955df298" />


* Through using the cleaned data, applying another data extraction to create a blox-plot
  
   
     
```python

#  Summary for the Box_Plot 
summary = df.groupby('day_name')['fare_amount'].describe()[['min', '25%', '50%', '75%', 'max']].reset_index()

# Renaming the  columns 
summary.columns = ['day_name', 'Min', 'Q1', 'Median', 'Q3', 'Max']

# Sort days 
days_order = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
summary['day_name'] = pd.Categorical(summary['day_name'], categories=days_order, ordered=True)
summary = summary.sort_values('day_name')

# Save as CSV 
summary.to_csv("uber_fare_boxplot_summary.csv",index=False)

```
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

* Mean
* Median
* Mode
* Outliers

#### ⚠️ Outlier Detection

Applied IQR method to detect values beyond 1.5×IQR.

```python
Q1 = df['fare_amount'].quantile(0.25)
Q3 = df['fare_amount'].quantile(0.75)
IQR = Q3 - Q1
lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR
```
<img width="359" height="335" alt="Cleaned8" src="https://github.com/user-attachments/assets/96829524-b597-46f2-bf28-1416ab9af850" />


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


####  Visual Analysis

* Histogram for fare distribution
* <img width="644" height="316" alt="bd69c805-4a17-4305-a6f6-54623e720ca0" src="https://github.com/user-attachments/assets/843fcd40-fb3e-421b-a78c-14b0584cd899" />
* Scatter plot: Fare vs. Distance
* <img width="695" height="393" alt="732cbd46-9018-4112-8683-541f30a76722" src="https://github.com/user-attachments/assets/4cd64890-a90a-4db9-99fa-d108fb4f6a11" />

* Boxplot: Fare vs. Hour
 ```python
import pandas as pd


df = pd.read_csv("uber_enhanced.csv")  

df['pickup_datetime'] = pd.to_datetime(df['pickup_datetime'])

df['hour'] = df['pickup_datetime'].dt.hour
df['day'] = df['pickup_datetime'].dt.day
df['month'] = df['pickup_datetime'].dt.month
df['day_of_week'] = df['pickup_datetime'].dt.dayofweek  
df['day_name'] = df['pickup_datetime'].dt.day_name()

df['is_weekend'] = df['day_of_week'].isin([5, 6])  
df['is_peak_hour'] = df['hour'].isin([7, 8, 9, 17, 18, 19, 20])

df = df[df['fare_amount'] > 0]

df = df[df['fare_amount'] < 100]

#  Summary for the Box_Plot 
summary = df.groupby('day_name')['fare_amount'].describe()[['min', '25%', '50%', '75%', 'max']].reset_index()

# Renaming the  columns 
summary.columns = ['day_name', 'Min', 'Q1', 'Median', 'Q3', 'Max']

# Sort days 
days_order = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
summary['day_name'] = pd.Categorical(summary['day_name'], categories=days_order, ordered=True)
summary = summary.sort_values('day_name')

# Save as CSV 
summary.to_csv("uber_fare_boxplot_summary.csv",index=False)

```

<img width="605" height="394" alt="blox plot" src="https://github.com/user-attachments/assets/61181882-8017-4c46-b744-93f454dabfb2" />

### 📈 Results: Discoveries and Patterns

* Fare amount is **strongly correlated** with trip distance (r ≈ 0.83).
* **Outliers** were identified and excluded for more reliable visuals.
* **Peak hours (7–9 AM & 5–8 PM)** tend to have higher fares.
* Ride frequency increases during **weekends**, especially Fridays and Saturdays.

---


### 📂 Datasets Export used
*Final enhanced (Currently in use)
```python
df.to_csv("uber_enhanced.csv", index=False)
```
*Previous cleaned

```python
df_cleaned.to_csv("uber_cleaned.csv", index=False)
```
*For the box plot
  ```python
summary.to_csv("uber_fare_boxplot_summary.csv",index=False)
```

These were the refinements of the datasets used final dataset was imported into Power BI for dashboard development.

---
#### Link to the THE POWER-BI Dashboard
*Loaded data to the Power-BI report generation

<img width="347" height="579" alt="Loaded_data" src="https://github.com/user-attachments/assets/be24356e-5795-40da-96cd-043a57c4264c" />

* Using Different values to create the interactive dashboard below
   

It contains slicing features to see how the different graphs affect each other
[https://drive.google.com/drive/recent?dmr=1&ec=wgc-drive-globalnav-goto](https://drive.google.com/)


---

### 📋 Conclusion

The data showed clear fare trends based on time and trip distance. 
Most Uber fares fall between \$5 and \$20, with peak demand during commute hours and weekends.
The analysis confirmed that fare is directly influenced by the distance(especially not long distances), time of day, and day of the week.

---
### 💡 Recommendations

Based on the analysis:

1. **Increase driver availability** during peak periods to reduce wait time and optimise pricing.
2. **Target promotions during off-peak hours** to balance ride volume.
3. **Flag and investigate outlier rides** if possible to maintain data integrity.



## 📚 References

This document lists the references used in the Uber Fares Dataset Analysis project.

---

### 📁 Dataset Source

- **Title:** Uber Fares Dataset  
- **Source:** Kaggle  
- **Link:** [https://www.kaggle.com/datasets/fivethirtyeight/uber-pickups-in-new-york-city](https://www.kaggle.com/datasets/fivethirtyeight/uber-pickups-in-new-york-city)  
- **Publisher:** FiveThirtyEight  
- **Access Date:** July 2025  
- **License:** Public Domain (CC0)

---

### 🧪 Tools Used

- **Power BI Desktop** – Microsoft Corporation  
- **Python Libraries:**
  - `pandas` – Data manipulation
  - `numpy` – Numerical computations
  - `seaborn` / `matplotlib` – Data visualization
  - `datetime` – Time formatting and extraction

---

### 📑 External Resources & Methodologies

- **Outlier Detection (IQR Method)**  
  Source: [Towards Data Science – Outlier Removal](https://towardsdatascience.com/why-and-how-to-remove-outliers-from-your-data-using-python-1c2bcea5dcea)

- **Distance Estimation (Haversine Formula)**  
  Source: [Movable Type Scripts](https://www.movable-type.co.uk/scripts/latlong.html)

- **Seaborn Correlation Heatmap**  
  Source: [Seaborn Documentation](https://seaborn.pydata.org/generated/seaborn.heatmap.html)

---

### 📘 Academic Citation (IEEE Format)

[1] FiveThirtyEight, “Uber Pickups in New York City,” *Kaggle*. [Online]. Available: https://www.kaggle.com/datasets/fivethirtyeight/uber-pickups-in-new-york-city. [Accessed: July 2025].

