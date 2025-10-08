# Urban Pluvial Flood Risk Analysis

## Project Overview
This project involves a comprehensive data analysis of urban pluvial (surface water) flood risk. Using a dataset of nearly 3,000 urban segments from cities worldwide, the analysis aims to identify risk factors, clean and prepare the data, engineer meaningful features, and derive insights into what makes certain areas more susceptible to flooding.

The project demonstrates a complete data science workflow: from data ingestion and cleaning to feature engineering and exploratory analysis, culminating in the creation of a target variable for potential predictive modeling.

## Dataset
The dataset `urban_pluvial_flood_risk_dataset.csv` contains 2,963 records with 17 initial features, including:

*   **Location Data:** `city_name`, `admin_ward`, `latitude`, `longitude`
*   **Topographical & Environmental:** `elevation_m`, `land_use`, `soil_group`, `drainage_density_km_per_km2`
*   **Infrastructure:** `storm_drain_proximity_m`, `storm_drain_type`
*   **Meteorological:** `rainfall_source`, `historical_rainfall_intensity_mm_hr`, `return_period_years`
*   **Target:** `risk_labels` (A complex field containing multiple risk flags and event dates)

## Workflow & Technical Steps

### 1. Data Loading & Initial Inspection
*   Loaded the dataset using Pandas.
*   Performed initial inspection using `.head()`, `.info()`, and `.isnull().sum()` to understand the data structure and identify missing values.

### 2. Data Cleaning & Imputation
Handled missing values to ensure data quality:
*   **Numerical Features:** Filled nulls with the median value.
    *   `elevation_m` → 25.13
    *   `drainage_density_km_per_km2` → 6.25
    *   `storm_drain_proximity_m` → 91.7
*   **Categorical Features:** Filled nulls with the mode (most frequent value).
    *   `soil_group` → 'B'
    *   `storm_drain_type` → 'CurbInlet'
    *   `rainfall_source` → 'ERA5'

### 3. Feature Engineering
This was a critical phase to transform the raw data into a more analysis-friendly format.

*   **Decomposed `risk_labels`:**
    *   Extracted `event_date` from strings like `event_2025-05-02`.
    *   Cleaned the remaining labels and performed **one-hot encoding** to create binary columns for each risk type:
        *   `extreme_rain_history`
        *   `low_lying`
        *   `ponding_hotspot`
        *   `sparse_drainage`
        *   `monitor` (indicates low-risk segments for monitoring)
*   **Split `city_name`:**
    *   Split into separate `city` and `country` columns for more granular geographical analysis.
*   **Created Target Variable `is_risky`:**
    *   A binary column where `1` indicates a segment has at least one of the active risk labels (`ponding_hotspot`, `low_lying`, `sparse_drainage`, `extreme_rain_history`), and `0` indicates it is only labeled for `monitor`.

### 4. Preliminary Analysis
*   Calculated the total number of segments and risk categories.
*   Analyzed the distribution of segments across the four main risk labels, providing a high-level view of the most common flood risk factors in the dataset.

## Key Insights & Results
The initial analysis reveals the prevalence of different risk factors among the 2,963 urban segments:
*   **Low-lying areas** are the most common risk factor (`low_lying`: 666 segments).
*   A significant number of segments have a history of extreme rain (`extreme_rain_history`: 254 segments).
*   Ponding hotspots (`ponding_hotspot`: 222 segments) and areas with sparse drainage (`sparse_drainage`: 181 segments) are also notable risks.
*   The engineered `is_risky` column successfully categorizes segments, providing a clear target for any subsequent classification model.

## Future Work
This project lays the groundwork for several advanced analyses:
*   **Predictive Modeling:** Build a classification model (e.g., Logistic Regression, Random Forest) to predict the `is_risky` flag based on the other features.
*   **Geospatial Visualization:** Plot the segments on a world map (e.g., using Folium or Plotly) to visualize the global distribution of flood risk.
*   **Correlation Analysis:** Investigate relationships between features (e.g., between `elevation_m` and `low_lying`, or `drainage_density` and `ponding_hotspot`).
*   **Risk Score Calculation:** Develop a composite risk score for each segment based on the weighted importance of its features.

## Technologies Used
*   **Python**
*   **Pandas** (Data Manipulation)
*   **NumPy** (Numerical Operations)
*   **Matplotlib** / **Seaborn** (Visualization - implied by import)
*   **Jupyter Notebook** (Environment)

### Author
**Chethan Kumar S**  
Python | Pandas | NumPy | Matplotlib | Jupyter Notebook
