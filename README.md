# ✈️ Airline Passenger Satisfaction Analytics & Prediction

**AICTE | IBM SkillsBuild Data Analytics with AI Internship Project**

---

## Project Overview

This is a complete, end-to-end **Data Analytics and Predictive Analytics** project built using real airline passenger satisfaction survey data. The project demonstrates all core concepts covered during the AICTE | IBM SkillsBuild Data Analytics with AI internship, including data cleaning, exploratory data analysis (EDA), business-question-driven storytelling, customer segmentation, logistic regression modelling, data leakage prevention, and model evaluation.

The project delivers:
- A reproducible **Jupyter Notebook** with the full analysis
- An interactive **Streamlit Dashboard** for business exploration and prediction
- A professional **Project Report** (DOCX)

---

## Business Problem

Airlines face a critical challenge: passenger satisfaction directly influences **loyalty, repeat bookings, and brand reputation**. Understanding which factors drive satisfaction — and being able to predict whether a passenger will be satisfied — gives airlines a competitive edge in a highly contested market.

This project answers the question:  
> **What distinguishes a satisfied airline passenger from a neutral or dissatisfied one — and can we predict satisfaction from passenger and flight attributes?**

---

## Objectives

| # | Objective |
|---|-----------|
| 1 | Understand the profile of satisfied vs. neutral/dissatisfied passengers |
| 2 | Identify which service quality dimensions most strongly associate with satisfaction |
| 3 | Analyse how passenger type, travel purpose, class, and age affect satisfaction |
| 4 | Investigate how operational factors (delays, distance) relate to satisfaction |
| 5 | Build a Logistic Regression model to predict passenger satisfaction |
| 6 | Provide actionable, data-validated business recommendations |

---

## Dataset Description

| Attribute | Value |
|-----------|-------|
| File      | `airline_passenger_satisfaction.csv` |
| Rows      | 129,880 |
| Columns   | 24 |
| Target    | `Satisfaction` (Satisfied / Neutral or Dissatisfied) |
| Satisfied Rate | ~43.5% |
| Missing Values | 393 in `Arrival Delay` only |
| Duplicates | 0 |

### Variable Types

- **Identifier**: `ID` (excluded from analysis)
- **Demographic**: `Gender`, `Age`
- **Segment**: `Customer Type`, `Type of Travel`, `Class`
- **Operational**: `Flight Distance`, `Departure Delay`, `Arrival Delay`
- **Service Quality (14 variables)**: Rated 1–5; 0 = Not Applicable
- **Target**: `Satisfaction`

### Dataset Source

The dataset used in this project was obtained from Kaggle:

[Airline Passenger Satisfaction Dataset](https://www.kaggle.com/datasets/mysarahmadbhat/airline-passenger-satisfaction)

---

## Data Dictionary Summary

| Field | Description | Type | Notes |
|-------|-------------|------|-------|
| ID | Unique passenger identifier | int | Excluded from modelling |
| Gender | Female / Male | categorical | — |
| Age | Passenger age in years | int | Range: 7–85 |
| Customer Type | First-time / Returning | categorical | — |
| Type of Travel | Business / Personal | categorical | — |
| Class | Business / Economy / Economy Plus | categorical | — |
| Flight Distance | Miles | int | Range: 31–4,983 |
| Departure Delay | Minutes | int | Min: 0, Max: 1,592 |
| Arrival Delay | Minutes | float | 393 missing values |
| 14 service ratings | 0=N/A, 1–5 scale | int | 0 retained (not converted to NaN) |
| Satisfaction | Satisfied / Neutral or Dissatisfied | categorical | Binary target |

Full data dictionary: `data_dictionary.csv`

---

## Data Cleaning Methodology

| Issue | Decision | Justification |
|-------|----------|---------------|
| `ID` column | Removed | Identifier with no predictive value |
| 393 missing `Arrival Delay` | Imputed using each row's `Departure Delay` | Arrival and departure delays are highly correlated; conservative median fallback applied |
| Service rating = 0 | **Retained as-is** | Per data dictionary, 0 = "Not Applicable" — a meaningful category |
| Duplicate rows | None found; step included in pipeline | Good practice |
| Negative delays | None found | Min value = 0 minutes |
| Categorical inconsistencies | None found | All values match data dictionary |

The cleaning pipeline is fully reproducible — run the notebook or the app to regenerate `data/processed_airline_passenger_data.csv`.

---

## EDA Methodology

EDA was structured around 10 business questions:

1. What proportion of passengers are satisfied vs. neutral/dissatisfied?
2. How does satisfaction vary by customer type?
3. How does satisfaction vary by type of travel?
4. How does satisfaction vary across travel classes?
5. How does satisfaction vary across age groups?
6. Which service dimensions are most strongly associated with satisfaction?
7. How do delays relate to satisfaction?
8. Does flight distance show meaningful differences in satisfaction?
9. Which passenger characteristics are associated with satisfaction?
10. Which variables are most useful for predicting satisfaction?

Visualisations include: bar charts, stacked bar charts, histograms, box plots, correlation heatmap, and coefficient charts.

---

## Business Questions & Key Findings

| Business Question | Finding |
|-------------------|---------|
| Overall satisfaction rate | ~43.5% Satisfied, ~56.5% Neutral/Dissatisfied |
| By Customer Type | Returning customers are more satisfied than First-time customers |
| By Type of Travel | Business-travel passengers are substantially more satisfied |
| By Class | Business class shows highest satisfaction; Economy lowest |
| By Age | Middle-aged passengers (30–59) show higher satisfaction |
| Service dimensions | Online Boarding, Seat Comfort, and In-flight Entertainment show the largest gaps |
| Delays | Neutral/Dissatisfied passengers experience higher average delays |
| Flight Distance | Satisfied passengers tend to take longer flights (more Business-class routes) |

---

## Predictive Modelling Methodology

**Primary Model**: Logistic Regression (sklearn)  
**Secondary Comparison**: Random Forest (for benchmarking only)

### Features Used (22 variables):
All variables except `ID`, `Satisfaction` (target), and `Satisfaction_Binary`.

### Pipeline:
1. `ColumnTransformer`: `StandardScaler` for numerical, `OneHotEncoder` for categorical features
2. `LogisticRegression` (max_iter=1000, solver='lbfgs')

### Train/Test Split:
- 80% training / 20% test
- Stratified by target class
- Random state = 42

---

## Data Leakage Prevention

| Leakage Risk | Action |
|-------------|--------|
| `Satisfaction` used as feature | **Never used** — only as target |
| `ID` used as feature | Removed before modelling |
| `Satisfaction_Binary` used as feature | Excluded from feature set |
| Preprocessing fit on test data | Prevented by sklearn Pipeline — fit only on X_train |
| Test set seen during training | Prevented by strict train/test split before any fitting |

---

## Model Evaluation

Results are computed on the **held-out test set (20%)** — data never seen during training.

| Metric | Logistic Regression |
|--------|-------------------|
| Accuracy | ~87% |
| Precision | ~0.87 |
| Recall | ~0.84 |
| F1-Score | ~0.85 |
| ROC-AUC | ~0.94 |

*Exact values are calculated dynamically from the data — run the notebook to see current results.*

**Confusion Matrix**: Available in the notebook and Streamlit dashboard.

**Class Imbalance**: The dataset has a mild imbalance (~43.5% Satisfied vs. ~56.5% Neutral/Dissatisfied). The model is evaluated using Precision, Recall, F1, and AUC — not accuracy alone.

---

## Business Recommendations

All recommendations are derived from validated analytical results.

1. **Invest in Online Boarding**: This dimension shows one of the largest gaps between satisfied and dissatisfied passengers.
2. **Improve In-flight Wi-Fi and Entertainment**: Low ratings here are a consistent pain point for dissatisfied passengers.
3. **Target Economy Class Improvements**: Economy passengers show the lowest satisfaction; improvements here reach the largest passenger segment.
4. **Serve Personal-Travel Passengers Better**: This segment is significantly less satisfied than Business-travel passengers.
5. **Manage Operational Delays**: Passengers who experience delays are more likely to be dissatisfied.
6. **Protect Returning Customer Loyalty**: Returning customers have higher satisfaction; loyalty programmes help retain this advantage.

> Note: These reflect observed **associations**, not proven causal effects. Controlled experiments (A/B tests) are recommended before large-scale investment.

---

## Dashboard Description

The Streamlit dashboard (`app.py`) contains 6 interactive sections:

| Section | Content |
|---------|---------|
| 1 · Executive Overview | KPI cards, overall satisfaction distribution |
| 2 · Passenger Analysis | Interactive filters, satisfaction by segment |
| 3 · Service Experience | Service dimension rating comparison |
| 4 · Operational Analysis | Delay and distance analysis |
| 5 · Satisfaction Prediction | Live Logistic Regression predictor |
| 6 · Business Insights | Model metrics, findings, recommendations |

---

## Project Structure

```
Airline_Passenger_Analytics/
│
├── airline_passenger_satisfaction.csv   ← Raw dataset (unchanged)
├── data_dictionary.csv                  ← Variable definitions
├── airline_passenger_analysis.ipynb     ← Full analysis notebook
├── app.py                               ← Streamlit dashboard
├── requirements.txt                     ← Python dependencies
├── README.md                            ← This file
├── project_report.docx                  ← Formal project report
│
├── data/
│   └── processed_airline_passenger_data.csv  ← Cleaned dataset (generated)
│
└── assets/
    └── charts/                          ← Saved chart images (generated)
```

---

## Installation

### Prerequisites
- Python 3.9 or later
- pip

### Install dependencies

```bash
pip install -r requirements.txt
```

---

## How to Run

### Run the Streamlit Dashboard

```bash
streamlit run app.py
```

Then open your browser at `http://localhost:8501`

### Run the Jupyter Notebook

```bash
jupyter notebook airline_passenger_analysis.ipynb
```

Or use VS Code / JupyterLab to open the `.ipynb` file.

> The notebook generates `data/processed_airline_passenger_data.csv` and all chart images on first run.

---

## Limitations

1. **Observational data**: Causal conclusions cannot be drawn — only associations.
2. **Survey bias**: Satisfaction ratings are self-reported and may not fully represent the passenger experience.
3. **No temporal information**: The dataset has no date/time stamps, preventing time-series analysis.
4. **Model simplicity**: Logistic Regression is intentionally chosen for interpretability (internship alignment); more complex models could improve predictive accuracy.
5. **Rating scale ambiguity**: The 0 rating ("Not Applicable") is retained per the data dictionary but introduces mixed semantics in service rating variables.

---

## Future Improvements

1. Add temporal analysis if date/flight data becomes available
2. Explore ensemble methods (Gradient Boosting, XGBoost) for improved predictive performance
3. Apply SHAP (SHapley Additive exPlanations) for deeper model interpretability
4. Conduct A/B test design recommendations based on the model findings
5. Build a cluster-based segmentation (K-Means) to identify latent passenger personas
6. Deploy the Streamlit app to Streamlit Cloud

---

## Conclusion

This project delivers a complete, reproducible, portfolio-quality Data Analytics and Predictive Analytics solution. It answers 10 measurable business questions, builds a validated Logistic Regression classifier with ~87% test accuracy, and translates results into 6 actionable business recommendations. The interactive Streamlit dashboard makes the findings accessible to business stakeholders without requiring technical knowledge.
