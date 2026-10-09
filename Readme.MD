# Exoplanet Data Analysis Through Visualization

This document provides a comprehensive overview, structural breakdown, and execution guide for the **Exoplanet Data Integration and Logarithmic Plotting Pipeline**. The application interfaces directly with external CSV data stores, cleans and stages parameters inside a **MySQL relational database**, and delivers dynamic, multi-cycle **logarithmic scatter plots** using Matplotlib.

---

## 📋 System Architecture Overview

The pipeline executes through a linear data ingestion and processing flow split into four primary operational layers:

```
[ Data Ingestion ] ➔ [ Data Conditioning ] ➔ [ Relational Staging ] ➔ [ Dynamic Visualisation Loop ]
  - CSV via TAP API     - Missing values filled    - MySQL DB Engine         - Multi-run column filtering
  - Windows User I/O     - Dataframe isolation    - Append-only schema      - Log-log scale plotting
```

---

## 🛠️ Detailed Code Breakdown

### 1. Ingestion Layer
The codebase begins by providing the exact **NASA Exoplanet Archive TAP API endpoint** to fetch composite planetary records. It utilizes dynamic standard input queries (`input()`) to map onto the user's specific Windows local profile path structure.

```python
# Fetches data columns including names, host stars, orbital periods, masses, radii, and stellar metrics
print("https://exoplanetarchive.ipac.caltech.edu/TAP/sync?query=select+pl_name,hostname...")

import pandas as pd
A = input('What is username?')
df = pd.read_csv(f"C:/Users/{A}/Downloads/data.csv")
```

### 2. Conditioning & Data Quality Layer
To ensure data robustness before relational database entry, missing numerical and structural fields are handled globally:
* **Null Substitution:** `.fillna('Unknown')` replaces missing entries (`NaN`) with explicit string constants.
* **Integrity Validation:** Displays non-null frequency summaries via `print(df.notnull().sum())` to give you an overview of data density across parameters.

### 3. Relational Storage Engine (MySQL Staging)
The application leverages **SQLAlchemy** combined with the **PyMySQL** driver to map the data stream onto an on-premise relational network.

```python
import pymysql
import sqlalchemy
from sqlalchemy import create_engine

U = input("Username,root:")
L = input("localhost:3306:")
P = input("SQL password:")

# Establishes a persistent SQLAlchemy link to the 'analysis' instance
engine = create_engine(f"mysql+pymysql://{U}:{P}@{L}/analysis")

# Migrates the Dataframe into the 'PlanetData' table
df.to_sql('PlanetData', engine, if_exists='append', index=False)
```

### 4. Continuous Iterative Visualization Framework
The visual core of the script runs a consecutive series of **12 identical execution blocks**. Each block acts as an independent querying cycle allowing you to perform cross-parameter data mining:
* **Interactive Parameters:** Prompts dynamically for explicit $X$ and $Y$ parameters (e.g., `pl_bmasse`, `pl_orbper`).
* **SQL Constraint Filtering:** Queries the active engine using `read_sql_query`, systematically dropping unquantified string placeholders (`'Unknown'`) to prevent casting issues.
* **Logarithmic Transmutation:** Converts data views to floating-point metrics (`.astype(float)`) and applies log-log scaling configurations via `plt.xscale('log')` and `plt.yscale('log')` to clearly reveal power-law relationships between astronomical units.

---

## 📈 Database Parameter Dictionary
When prompted for `x-parameter` or `y-parameter`, you can input any of the following standard schema fields present in your database:

| Column Name | Parameter Target Description | Type Profile |
| :--- | :--- | :--- |
| `pl_name` | Full Name of the Exoplanet | Categorical |
| `hostname` | Parent Star / Host Star Name | Categorical |
| `discoverymethod` | Detection Mechanism (e.g., Transit, Radial Velocity) | Categorical |
| `disc_year` | Calendar Year of Confirmed Discovery | Integer |
| `pl_orbper` | Orbital Period duration (Days) | Floating-point (Log-ready) |
| `pl_orbsmax` | Semi-major Axis orbit length (AU) | Floating-point (Log-ready) |
| `pl_rade` | Planetary Radius scaled against Earth Radii | Floating-point (Log-ready) |
| `pl_bmassj` | Total Mass scaled against Jupiter Masses | Floating-point (Log-ready) |
| `st_teff` | Effective Stellar Temperature (Kelvin) | Floating-point |
| `sy_dist` | Distance from Solar System (Parsecs) | Floating-point |

---

## 🚀 Execution Pre-requisites

To run this pipeline without execution faults, ensure the following steps are met locally:
1. **Directory Integrity:** The target CSV file must be renamed explicitly to `data.csv` and situated cleanly inside your Windows profile's local `Downloads` directory.
2. **Database Existence:** Prior to script invocation, launch your local MySQL command line interface or workbench UI and manually build the parent container using:
   ```sql
   CREATE DATABASE analysis;
   ```
3. **Environment Packages:** Install the core software prerequisites within your Python environment terminal:
   ```bash
   pip install pandas pymysql sqlalchemy matplotlib
   ```
