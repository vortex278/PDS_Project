# PDS Project: FAA Aviation Accident Analysis

This project examines historical Federal Aviation Administration Aircraft Incident Data System (AIDS) records for a Perspectives on Data Science class.

It asks which observable factors appear most frequently in the records and whether those factors are associated with aircraft damage severity, fatalities, or nonfatal injuries.

## Primary research question

**Which observable aviation accident factors are most strongly associated with aircraft damage severity?**

## Secondary outcomes

The project also studies whether a record reported:

- One or more fatalities
- One or more nonfatal injuries

These are descriptive and predictive association tasks. The project does not determine the cause of an individual accident or predict whether a future flight will crash.

## Dataset

The notebook loads `faa_aids_all.csv`, which is expected in the project root.

The executed dataset contained:

- 251,290 raw rows
- 19 original columns
- 4,174 exact duplicate rows
- 247,116 rows after exact duplicate removal
- Event dates from 1945-07-23 through 2026-10-15

Multiple aircraft or pilot records can share one report number. The notebook retains repeated report records and uses `rprt_nbr` as a grouping identifier during splitting, preventing related rows from appearing in both training and test partitions.

## Methods

The analysis:

1. Audits the schema, event dates, duplicates, missing values, and injury quantities
2. Defines aircraft damage classes as `Minor`, `Substantial`, and `Destroyed`
3. Creates binary targets for fatality presence and nonfatal-injury presence
4. Uses event type, flight phase, pilot certification, aircraft make, and aircraft model as base predictors
5. Uses grouped train/test splitting and grouped cross-validation
6. Trains Random Forest, Logistic Regression, and K-Nearest Neighbors classifiers
7. Evaluates models on held-out test data
8. Reports accuracy, balanced accuracy, precision, recall, macro F1, per-class metrics, and confusion matrices

Missing categorical values are represented as separate categories rather than inferred from unrelated variables. Report numbers and high-cardinality identifiers are not used as predictors.

## Expected outcomes

The project is expected to identify recurring descriptive patterns and held-out predictive associations, not inherent aircraft risk or crash causes.

### Descriptive findings

The common-threads analysis is expected to show:

- Frequently occurring flight phases, especially landing and rollout phases
- Frequently occurring aircraft makes and models
- Damage, fatality, and injury outcome rates within each category
- Seasonal patterns in accident-record counts

These frequencies describe the contents of the historical dataset. They do not calculate crash risk because the data lacks total flights, flight hours, aircraft exposure, and aircraft-in-operation totals.

### Modeling findings

The executed modeling is expected to show:

- Event type and flight phase are useful associations for aircraft damage
- Logistic Regression is the strongest model for all three targets by macro F1
- Fatality and nonfatal-injury outcomes are strongly imbalanced
- Accuracy alone can be misleading for rare positive outcomes
- Aircraft make and model have weaker held-out importance than event type or flight phase
- Permutation importance and model coefficients describe associations, not causal effects

Balanced accuracy and macro F1 are emphasized alongside accuracy. Confusion matrices show correct predictions and the classes that models confuse.

## Project notebook

Open [Perspectives_on_Data_Science_Project.ipynb](Perspectives_on_Data_Science_Project.ipynb) to edit and run the analysis.

The notebook expects `faa_aids_all.csv` in the same folder.

The notebook trains Random Forest, Logistic Regression, and K-Nearest Neighbors classifiers for aircraft damage, fatality presence, and nonfatal-injury presence. Model selection uses a compact two-fold grouped cross-validation search, and the final comparison uses held-out test data.

## Current analysis and figures

The exploratory analysis includes:

- Aviation events by year
- Accident versus incident counts
- Aircraft damage distribution
- Crash amount by calendar month
- Injury and fatality totals by year and by aircraft damage category
- Flight phase, aircraft make, and pilot certification relationships
- Missing values by column

The crash-by-month figure counts records marked `Accident`, not every incident.

The common-threads analysis provides one authoritative set of flight-phase and aircraft-make frequency and outcome tables, with matching charts. It does not repeat the same frequency or damage-distribution analysis across multiple sections.

## Important limitations

- The dataset is observational and cannot establish crash causes
- It does not measure future crash risk
- It does not include flight exposure or aircraft usage totals
- Missingness and unknown categories may reflect reporting practices
- Fatality and injury outcomes are strongly imbalanced
- Rare or highly specific categories can be unstable
- A compact grouped cross-validation search was used instead of exhaustive tuning
- KNN tuning and fitting used a 5,000-row training sample

## Setup and execution

Install the Python dependencies:

```powershell
python -m pip install -r requirements.txt
```

Place `faa_aids_all.csv` in the project root. Open `Perspectives_on_Data_Science_Project.ipynb` and run the cells in order.
