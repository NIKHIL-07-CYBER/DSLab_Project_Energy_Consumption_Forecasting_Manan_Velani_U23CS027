# 🔌 UCI Household Power Consumption — Data Preprocessing & Forecasting Pipeline

## 📋 Project Introduction

This project implements a comprehensive **data preprocessing, feature engineering, and machine learning pipeline** for the [UCI Household Power Consumption](https://archive.ics.uci.edu/ml/datasets/Individual+household+electric+power+consumption) dataset. The objective is to prepare raw, high-frequency time-series energy consumption data for **short-term power load forecasting** — a critical task in modern smart grid and energy management systems.

The pipeline covers the full data science workflow:
1. **Data Ingestion** — Loading and parsing raw sensor data with datetime indexing.
2. **Physical Validation** — Detecting and filtering physically implausible readings.
3. **Hierarchical Imputation** — Filling missing values using seasonal (hour × weekday) mean imputation for long gaps and cubic interpolation for short gaps.
4. **Cyclical Time Encoding** — Encoding temporal features (hour, day-of-week, month) as sine/cosine pairs to preserve periodicity.
5. **Lag & Rolling Feature Engineering** — Generating lagged values and rolling window statistics (mean, std) for key power metrics.
6. **Scaling** — Standardizing features for downstream model compatibility.
7. **Model Training & Evaluation** — Training and comparing Linear Regression, Decision Tree, and Random Forest models.

---

## 📊 Dataset Brief

| Property              | Details                                                                 |
|------------------------|-------------------------------------------------------------------------|
| **Source**             | [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/Individual+household+electric+power+consumption) |
| **Description**        | Measurements of electric power consumption in one household, sampled at **1-minute intervals** over nearly 4 years (Dec 2006 – Nov 2010). |
| **Total Records**      | ~2,075,259 raw observations                                            |
| **Features (Raw)**     | `Global_active_power`, `Global_reactive_power`, `Voltage`, `Global_intensity`, `Sub_metering_1`, `Sub_metering_2`, `Sub_metering_3` |
| **Target Variable**    | `Global_active_power` (household global minute-averaged active power, in kilowatts) |
| **Missing Values**     | ~1.25% of total records contain missing values (`?` markers)            |
| **File Format**        | Semicolon-separated `.txt` file                                         |

### Key Preprocessing Steps Applied
- Conversion of `Date` and `Time` string columns into a unified `datetime` index.
- Reindexing to a strict 1-minute frequency to handle gaps.
- Physical outlier detection and removal using domain-specific thresholds (e.g., voltage ∈ [100V, 500V]).
- Hierarchical imputation: seasonal mean fill for long gaps, cubic interpolation for short gaps.
- Feature expansion from 7 raw features to **51 engineered features** (lag, rolling mean/std, cyclical encodings).

---

## 🚀 Steps to Execute the Project

### 1. Prerequisites

Ensure you have **Python 3.8+** installed on your system. The following libraries are required:

```bash
pip install pandas numpy scikit-learn joblib scipy matplotlib seaborn plotly statsmodels
```

> **Note:** If using Google Colab, most of these libraries are pre-installed. You can run the notebook directly without additional setup.

### 2. Data Preparation

Ensure the raw dataset file `household_power_consumption.txt` is placed in the **same directory** as the notebook (`main.ipynb`).

> The dataset can be downloaded from the [UCI Repository](https://archive.ics.uci.edu/ml/datasets/Individual+household+electric+power+consumption) or extracted from the provided zip file `individual+household+electric+power+consumption (1).zip`.

### 3. Running the Notebook

1. Open `main.ipynb` in **Jupyter Notebook**, **JupyterLab**, or **Google Colab**.
2. **Run all cells sequentially** (Cell → Run All, or use `Shift+Enter` cell by cell).
3. The notebook will execute the following stages automatically:
   - **📥 Load** the dataset (~50,000 sampled rows for efficient processing).
   - **🧹 Clean** the data (handle missing values, remove physical anomalies).
   - **🔧 Engineer features** (lag features, rolling statistics, cyclical time encodings).
   - **✂️ Split** the data into 80% training / 20% testing sets.
   - **🤖 Train** three regression models: Linear Regression, Decision Tree, and Random Forest.
   - **📊 Evaluate** and compare model performance (MAE, RMSE).
   - **📈 Generate** visualization plots and save them as `energy_forecasting_results.png`.

### 4. Output Artifacts

After successful execution, the following files will be generated:
- `uci_power_pipeline.joblib` — Serialized preprocessing pipeline object (reusable for new data).
- `energy_forecasting_results.png` — Visualization of model predictions and performance comparison.

---

## 📈 Results

### Data Processing Summary

| Stage                  | Details                                                  |
|------------------------|----------------------------------------------------------|
| **Rows Loaded**        | 50,000                                                   |
| **After Cleaning**     | 49,995 rows                                              |
| **Features Engineered**| 51 features (from original 7)                            |
| **Train Set Size**     | 39,988 samples                                           |
| **Test Set Size**      | 9,997 samples                                            |
| **Missing Values**     | 0 (after imputation)                                     |

### Model Performance Comparison

| Model                | MAE     | RMSE    |
|----------------------|---------|---------|
| Linear Regression    | 0.0392  | 0.0640  |
| Decision Tree        | 0.0392  | 0.0676  |
| **Random Forest** 🏆 | **0.0267** | **0.0473** |

> **🏆 Best Model: Random Forest** — Achieved the lowest MAE (0.0267) and RMSE (0.0473), demonstrating superior generalization for power load forecasting.

### Visualizations Generated

The notebook produces a comprehensive 3-panel figure (`energy_forecasting_results.png`):
1. **Actual vs. Predicted** — Time-series comparison of the best model's predictions against ground truth (first 500 test samples).
2. **Model Comparison Bar Chart** — Side-by-side MAE and RMSE comparison across all three models.
3. **Hourly Consumption Pattern** — Average hourly energy consumption trend across the dataset.

---

## 👥 Team Members

| Name                  | Roll Number   |
|-----------------------|---------------|
| **Nikhil Chhasiya**   | U23CS022      |
| **Manan Velani**      | U23CS027      |
| **Dharav Bodat**      | U23CS049      |
| **Om Amin**           | U23CS076      |

---

## 📂 Repository Structure

```
ds/
├── main.ipynb                          # Main project notebook (run this)
├── household_power_consumption.txt            # Raw dataset (UCI)
├── uci_power_pipeline.joblib                  # Saved preprocessing pipeline
├── Copy_of_DS_Project.ipynb                   # Alternate project notebook
├── DS_Project.ipynb                           # Additional project notebook
├── EDA_Code_Explanation_and_Model_Training_Guide.txt  # Documentation
├── uci_pipeline_documentation.pdf             # Pipeline documentation
├── README.md                                  # This file
└── outputs/                                   # Generated output files
```

---

## 📚 References

- Hebrail, G. & Berard, A. (2012). *Individual Household Electric Power Consumption Dataset*. UCI Machine Learning Repository.
- Scikit-learn Documentation: [https://scikit-learn.org/](https://scikit-learn.org/)
- Pandas Documentation: [https://pandas.pydata.org/](https://pandas.pydata.org/)

---

## 📝 License

This project is developed as part of an academic coursework assignment (Semester 6, Data Science Lab). It is intended for educational purposes only.
