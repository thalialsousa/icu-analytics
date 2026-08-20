# ICU Analytics — Data-Driven Critical Care Performance Dashboard
This project was developed for educational and self-directed learning purposes.

## Project Overview

This project was developed as a self-directed study in **Healthcare Data Analytics**, combining clinical domain knowledge with data analysis, Business Intelligence, Machine Learning, and Generative AI.

Using the **eICU Collaborative Research Database Demo**, I developed an end-to-end analytical workflow to explore ICU performance, patient severity, length of stay, mortality, and clinical data patterns.

The project demonstrates the complete analytical pipeline:

- SQL and SQLite database exploration
- Python data extraction, cleaning, transformation, and validation
- Exploratory Data Analysis (EDA)
- Power BI dashboard development
- Power Query and DAX
- Machine Learning for prolonged ICU length-of-stay prediction
- Local Generative AI integration using Ollama

---

## Project Objective

ICU data is distributed across multiple clinical tables and can be difficult to translate directly into actionable information.

The objective of this project was to transform raw critical-care data into a structured analytical dataset and develop tools for exploring:

- ICU length of stay
- Patient severity
- ICU mortality
- Differences between ICU unit types
- Observed versus predicted outcomes
- Potential risk of prolonged ICU stay

The project was designed as an **analytical portfolio project and not as a clinical decision-support system**.

---

## Data Source

The project uses the **eICU Collaborative Research Database Demo**, a de-identified critical-care dataset available through PhysioNet.

The source database contains relational tables covering areas such as:

- Patient demographics
- ICU admissions
- Diagnoses
- APACHE severity information
- Laboratory records
- Medication records
- Clinical outcomes

The raw SQLite database is intentionally excluded from this repository.

---


eICU SQLite Database
        ↓
SQL + Python Extraction
        ↓
Data Cleaning & Transformation
        ↓
Analytical Dataset
        ↓
Exploratory Data Analysis
        ↓
Power BI Dashboard
        ↓
Machine Learning
        ↓
Local Generative AI

---

## 1. Database Exploration

The first stage focused on understanding the relational structure of the eICU database.

SQL and Python were used to:

- Inspect available tables
- Identify relevant clinical variables
- Understand primary and foreign-key relationships
- Evaluate record granularity
- Identify missing data and potential duplication risks

---

## 2. Data Engineering

A consolidated analytical dataset was created using Python and SQL.

Key preparation steps included:

- Joining multiple clinical tables
- Aggregating diagnosis records
- Aggregating laboratory and medication information
- Calculating ICU length of stay
- Deriving ICU mortality
- Integrating APACHE severity variables
- Handling missing values
- Standardizing numeric and categorical fields
- Validating duplicate records after joins

A central design decision was maintaining **one analytical row per ICU stay**, preventing one-to-many joins from artificially inflating patient counts and outcome metrics.

---

## 3. Exploratory Data Analysis

Exploratory analysis was performed using **Pandas, NumPy, and Matplotlib**.

The analysis explored:

- ICU length-of-stay distribution
- Mortality across ICU unit types
- APACHE severity patterns
- Patient demographics
- Severity versus length of stay
- Ventilation-data availability
- Relationships between severity and outcomes

Associations identified during EDA were treated as exploratory and were not interpreted as causal relationships.

---

## 4. Power BI Dashboard

An interactive Power BI dashboard was developed to translate the analytical dataset into management-level information.

### Executive Overview

The executive page includes:

- Total ICU stays
- Average ICU length of stay
- ICU mortality rate
- Age distribution
- Gender distribution
- ICU unit distribution
- Admission diagnoses
- Interactive filters

### Severity & Outcomes

The second analytical page explores:

- APACHE severity
- Severity versus length of stay
- ICU mortality by ventilation-data status
- Length of stay by ventilation-data status
- Observed versus APACHE-predicted mortality
- ICU unit comparisons
- Interactive slicers

Dashboard calculations were cross-checked against Python results to improve consistency between the analytical and visualization layers.

---

## 5. Machine Learning

An exploratory supervised Machine Learning pipeline was developed to evaluate whether admission-level information could identify ICU stays at increased risk of prolonged length of stay.

**Target**

Prolonged ICU stay was defined as length of stay above the **75th percentile**.

**Features**

The model used variables available or plausibly available near ICU admission, including:

- Age
- Gender
- ICU type
- APACHE severity score

Restricting the feature set to admission-level information was an intentional decision to reduce **data leakage**.

### Models Evaluated

| Model | Precision | Recall | F1-score | ROC-AUC |
|---|---:|---:|---:|---:|
| Logistic Regression | 0.47 | 0.06 | 0.11 | 0.61 |
| Random Forest | 0.30 | 0.18 | 0.23 | 0.58 |

### Interpretation

Neither model demonstrated strong predictive performance using the available admission-level features alone.

Logistic Regression showed higher precision and ROC-AUC, while Random Forest increased recall and F1-score.

Rather than presenting a weak model as successful, these results highlight an important analytical finding: **the selected admission-level variables alone provide limited discrimination for prolonged ICU stay in this dataset.**

The models are exploratory and are **not intended for clinical use**.

---

## 6. Generative AI

A local Generative AI prototype was implemented using **Ollama** and **Qwen 2.5 3B**.

Instead of allowing the language model to calculate clinical metrics directly, the architecture separates deterministic analytics from natural-language interpretation:

Python Metrics
        ↓
Structured Analytical Context
        ↓
Local LLM
        ↓
Executive Interpretation

The LLM receives previously calculated analytical results and generates a natural-language interpretation.

Prompt-level guardrails were included to:

- Prevent unsupported numerical claims
- Avoid causal conclusions
- Avoid medical advice
- Preserve data-quality limitations
- Avoid interpreting missing ventilation information as absence of mechanical ventilation

This component demonstrates a basic approach to **grounded LLM integration with analytical workflows**.

---

## Key Findings

The exploratory analysis identified several relevant observations:

- Average ICU length of stay was approximately **2.4 days**
- Overall ICU mortality was approximately **5%**
- Severity and outcome patterns varied across ICU unit types
- Admission-level variables alone showed limited predictive performance for prolonged ICU stay
- Missing clinical information can materially affect interpretation and should not automatically be treated as absence of an event

---

## Technologies

**Data Analysis**
- Python
- Pandas
- NumPy
- Matplotlib
- Jupyter Notebook

**Database & Data Preparation**
- SQL
- SQLite
- Data cleaning
- Data transformation
- Relational joins
- Data validation
- Feature engineering

**Business Intelligence**
- Power BI
- Power Query
- DAX
- KPI development
- Interactive dashboards
- Data visualization

**Machine Learning**
- Scikit-learn
- Logistic Regression
- Random Forest
- Classification metrics
- Feature preprocessing
- Train/test evaluation
- Data leakage awareness

**Generative AI**
- Ollama
- Qwen 2.5
- Local LLM inference
- Prompt engineering
- Structured analytical context
- LLM guardrails

**Development**
- VS Code
- Git
- GitHub
- Virtual environments

---

## Structure

data/
>processed/

docs/
>images/
>model_metrics.csv

notebooks/
>01_database_exploration.ipynb
>02_build_dataset.ipynb
>03_exploratory_analysis.ipynb
>04_machine_learning.ipynb
>05_generative_ai.ipynb

powerbi/
sql/
src/

README.md
requirements.txt
.gitignore

The raw eICU database is excluded through `.gitignore`.

---

## How to Run the Project

Clone the repository and install the Python dependencies:

pip install -r requirements.txt


Obtain the eICU Demo dataset separately and place the local SQLite database inside:

data/raw/


Run the notebooks sequentially:


01_database_exploration.ipynb
02_build_dataset.ipynb
03_exploratory_analysis.ipynb
04_machine_learning.ipynb
05_generative_ai.ipynb


For the Generative AI component, Ollama must be installed locally and the corresponding Qwen model must be available.

---

## Limitations

- The project uses the eICU Demo dataset rather than the complete eICU population
- Missing information may reflect unavailable data rather than absence of clinical events
- Data availability can vary across hospitals and interfaces
- Machine Learning performance is limited by the available features and dataset
- Predictive models require additional validation before any real-world application
- Observational associations cannot establish causality
- The Generative AI component is an analytical prototype, not a clinical decision-support system

---

## About This Project

This project was developed as a **self-directed Healthcare Data Analytics study** to practice and demonstrate an end-to-end workflow connecting clinical domain knowledge with data engineering, analytics, visualization, Machine Learning, and Generative AI.

The focus was not only on producing results, but also on understanding data limitations, validating analytical outputs, avoiding data leakage, and communicating findings responsibly.
