# 🪙 MetalMind — Predicting Gold Recovery in Industrial Processing

## 📌 Overview
In modern mining operations, efficient recovery of valuable metals like **gold, silver, and lead** is essential to maximizing profitability and reducing waste.  
This project simulates a **real-world production pipeline** where we model and evaluate gold recovery at different stages of mineral processing using **machine learning**, while navigating real-world challenges like **missing data, feature leakage, and skewed distributions**.

un prototipo de un modelo de machine learning para Zyfra. La empresa desarrolla soluciones de eficiencia para la industria pesada.

El modelo debe predecir la cantidad de oro extraído del mineral de oro. Dispones de los datos de extracción y purificación.

El modelo ayudará a optimizar la producción y a eliminar los parámetros no rentables.

Tendrás que:

Preparar los datos.
Realizar el análisis de datos.
Desarrollar un modelo y entrenarlo.
Para completar el proyecto, puedes utilizar la documentación de pandas, matplotlib y sklearn.

La siguiente lección trata sobre el proceso de depuración del mineral. Te tocará seleccionar la información importante para el desarrollo del modelo. 

---

## 🎯 Business Objective
- **Goal:** Predict recovery efficiency to improve decision-making in ore processing.
- **Why it matters:** Accurate predictions reduce loss of precious metals and help optimize industrial operations.
- **Target metric:** **Symmetric Mean Absolute Percentage Error (sMAPE)** across two key targets:
  - `rougher.output.recovery`
  - `final.output.recovery`

---

## 📂 Data Description

### 🧾 Datasets
- `gold_recovery_train.csv` — training set
- `gold_recovery_test.csv` — test set (features only)
- `gold_recovery_full.csv` — complete set with all features and targets

### ⛏️ Schema Highlights
- `date` — timestamp (index)
- `feature.*` — input parameters (e.g. chemical concentrations, particle sizes)
- `rougher.output.*` — intermediate process outputs
- `final.output.*` — final product outputs

### ⚠️ Notes
- The **test set lacks some features** due to real-world measurement delays.
- **No target values** in the test set; all evaluation must be done via internal validation.
- Recovery is a **derived value**: we independently verified its calculation using physical formulas.

---

## 🔬 Methodology

### 1. Data Preprocessing
- Loaded & inspected datasets for nulls, dtypes, and timestamp alignment.
- Verified rougher recovery with formula:

\[
\text{recovery} = \frac{C \cdot (F - T)}{F \cdot (C - T)} \cdot 100
\]

- Cleaned anomalies where total concentrations exceeded physical limits (i.e. >100%).

### 2. Feature Handling
- Identified missing-in-test features and excluded them from training.
- Scaled relevant features (optional for tree-based models).
- Ensured **no data leakage** from future stages or targets.

### 3. Exploratory Analysis
- Tracked concentration changes of **Au, Ag, Pb** across processing stages.
- Compared particle size distributions in train vs test — confirmed similar distributions.
- Detected and removed invalid rows with negative or >100% concentrations.

### 4. Modeling
- Developed and compared:
  - **RandomForestRegressor**
  - **GradientBoostingRegressor**
  - **LinearRegression (baseline)**
- Applied **cross-validation** using custom sMAPE scorer.

### 5. Evaluation
- Final metric combines two sMAPE values:

\[
\text{Total sMAPE} = 0.25 \cdot \text{sMAPE}_{\text{rougher}} + 0.75 \cdot \text{sMAPE}_{\text{final}}
\]

- Best model selected via lowest total sMAPE on validation data.

---

## ✅ Results

| Model                 | Rougher sMAPE | Final sMAPE | Total sMAPE |
|----------------------|---------------|-------------|-------------|
| Linear Regression     | 13.1%         | 11.8%       | 12.1%       |
| Random Forest         | 7.4%          | 6.3%        | **6.6%**    |
| Gradient Boosting     | 7.6%          | 6.5%        | 6.8%        |

- 📌 **Best model:** `RandomForestRegressor` with **6.6% total sMAPE**
- 📊 **Test data inference** performed using pre-selected features and saved model

---

## 📊 Key Visuals

- 📉 Metal concentration evolution (Au, Ag, Pb)
- 🧪 Particle size distribution comparison
- 🚫 Anomaly detection heatmaps
- ✅ Model performance comparison (bar chart)

---

## 🧰 Tech Stack

- **Language:** Python 3.10+
- **Libraries:** pandas, numpy, scikit-learn, matplotlib, seaborn, SciPy
- **ML:** RandomForest, GradientBoosting, custom metrics
- **Environment:** Jupyter Notebook
