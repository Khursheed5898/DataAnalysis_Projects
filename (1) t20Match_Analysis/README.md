# 🏏 T20 World Cup Cricket Data Analytics Project

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue.svg?logo=python&logoColor=white)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458.svg?logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![Power BI](https://img.shields.io/badge/Power_BI-Dashboard%20%26%20DAX-F2C811.svg?logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626.svg?logo=jupyter&logoColor=white)](https://jupyter.org/)

An end-to-end Data Analytics project designed to analyze T20 Cricket World Cup match performance data, evaluate player metrics across specific playing roles, and assemble the optimal **"Best 11"** team through interactive **Power BI Dashboards** and **DAX Measures**.

---

## 📌 Table of Contents
- [Project Overview](#-project-overview)
- [Key Performance Metrics](#-key-performance-metrics)
- [Tech Stack & Tools](#-tech-stack--tools)
- [Project Pipeline & Architecture](#-project-pipeline--architecture)
- [Data Model & Star Schema](#-data-model--star-schema)
- [DAX Measures & Business Logic](#-dax-measures--business-logic)
- [Repository Structure](#-repository-structure)
- [How to Run & Reproduce](#-how-to-run--reproduce)
- [Author & Acknowledgments](#-author--acknowledgments)

---

## 🎯 Project Overview

The primary objective of this project is to use data-driven decision making to select the ultimate **Best 11 Team** for a T20 World Cup tournament. Players are evaluated not just on aggregate runs or wickets, but against specialized role-based criteria:

1. **Openers / Powerplay Hitters (2 Players):** High strike rate in powerplay overs, solid batting average, and high boundary percentage.
2. **Middle Order / Anchors (2 Players):** High batting average, ability to rotate strike and anchor innings under collapse, good average balls faced per innings.
3. **Finishers (2 Players):** High strike rate in death overs (overs 16-20), ability to finish matches with high boundary percentage.
4. **All-Rounders (2 Players):** High batting strike rate, bowling economy < 7.5, and regular wicket-taking ability.
5. **Specialist Fast Bowlers (3 Players):** Low economy rate, high dot ball percentage, and low bowling strike rate in powerplay and death overs.

---

## 📊 Key Performance Metrics

| Metric | Formula / Definition | Significance |
| :--- | :--- | :--- |
| **Batting Strike Rate** | `(Total Runs / Total Balls Faced) * 100` | Measures scoring velocity |
| **Batting Average** | `Total Runs / Total Innings Dismissed` | Consistency of run-scoring |
| **Boundary %** | `((4s * 4 + 6s * 6) / Total Runs) * 100` | Boundary dependency & aggression |
| **Bowling Economy Rate** | `Total Runs Conceded / (Total Balls Bowled / 6)` | Run containment efficiency |
| **Bowling Strike Rate** | `Total Balls Bowled / Total Wickets Taken` | Wicket frequency (balls per wicket) |
| **Dot Ball %** | `(Total Dot Balls / Total Balls Bowled) * 100` | Pressure creation on batsmen |

---

## 🛠 Tech Stack & Tools

- **Web Scraping:** JavaScript collectors for ESPNCricinfo match pages (via BrightData / DOM scraping)
- **Data Transformation & ETL:** Python (`Pandas`, `JSON`), `Jupyter Notebook`
- **Data Modeling & Power Query:** Power BI Desktop (Star Schema modeling, parameter scoping, conditional columns)
- **Calculated Measures:** DAX (Data Analysis Expressions)
- **Reporting & Visualization:** Power BI Interactive Dashboards

---

## 🔄 Project Pipeline & Architecture

```mermaid
flowchart LR
    A[Web Scraping ESPNCricinfo] -->|Raw JSON| B[t20_Json_Files]
    B -->|Python & Pandas ETL| C[t20_csv_files Fact & Dim]
    C -->|Power Query ETL| D[Data Model Star Schema]
    D -->|DAX Measures| E[Power BI Interactive Dashboard]
```

1. **Data Collection (`Web_Scrapping_Codes/`):**
   - Scraped match results, batting summaries, bowling summaries, and player profiles into raw JSON datasets.
2. **Data Preprocessing (`t20_Data_Preprocessing/`):**
   - Transformed nested JSON structures into clean, standardized tabular format.
   - Handled text formatting, missing values, special characters (e.g. `†`, `(c)`), and normalized player names across datasets.
   - Exported relational tables (`dim_match_summary`, `dim_players`, `fact_bating_summary`, `fact_bowling_summary`).
3. **Data Modeling & Power Query (`t20match_Power_Query.pbix`):**
   - Established one-to-many relationships connecting Fact and Dimension tables using `match_id` and `player_name` foreign keys.
4. **DAX Calculations (`DAX-Measures-and-Calculated-Columns.xlsx`):**
   - Implemented dynamic measures for role filtering, benchmarks, and interactive player comparison.
5. **Interactive Dashboard (`t20Match_Analysis.pbix`):**
   - Multi-page dashboard with role selection views, tooltips, performance KPIs, and dynamic team selection canvas.

---

## 🗄️ Data Model & Star Schema

The project follows a **Star Schema** relational design:
- **Fact Tables:**
  - `fact_bating_summary`: Ball-by-ball batting records, runs, balls, 4s, 6s, strike rate, dismissals.
  - `fact_bowling_summary`: Overs, maidens, runs conceded, wickets, economy, dots.
- **Dimension Tables:**
  - `dim_match_summary`: Match metadata, teams, venue, date, winner, victory margin.
  - `dim_players`: Player metadata, batting style, bowling style, role, country.

---

## 📁 Repository Structure

```
DataAnalysis_Project/
└── t20Match_Analysis/
    ├── t20_csv_files/                    # Cleaned dataset CSVs
    │   ├── dim_match_summary.csv
    │   ├── dim_players.csv
    │   ├── dim_players_no_images.csv
    │   ├── fact_bating_summary.csv
    │   └── fact_bowling_summary.csv
    ├── t20_Data_Preprocessing/           # Data Cleaning & Transformation Notebooks
    │   └── t20_data_preprocessing.ipynb
    ├── t20_Json_Files/                   # Raw JSON Scraped Data
    │   ├── t20_wc_batting_summary.json
    │   ├── t20_wc_bowling_summary.json
    │   ├── t20_wc_match_results.json
    │   └── t20_wc_player_info.json
    ├── Web_Scrapping_Codes/              # Web Scraper Scripts
    │   ├── t20_wc_batting_summary.js
    │   ├── t20_wc_bowling_summary.js
    │   ├── t20_wc_match_results.js
    │   └── t20_wc_player_info.js
    ├── DAX-Measures-and-Calculated-Columns.xlsx  # Complete DAX formula dictionary
    ├── Step - 01.pbix                   # Initial Dashboard Build
    ├── Step - 02.pbix                   # Iterative Dashboard Build
    ├── t20Match_Analysis.ipynb          # Exploratory Data Analysis Notebook
    ├── t20Match_Analysis.pbix           # Final Complete Power BI Dashboard
    ├── t20match_Power_Query.pbix        # Power Query Model
    ├── requirements.txt                 # Python Dependencies
    ├── .gitignore                       # Git ignore configuration
    └── README.md                        # Project documentation
```

---

## 🚀 How to Run & Reproduce

### 1. Clone the Repository
```bash
git clone https://github.com/<your-username>/DataAnalysis_Project.git
cd DataAnalysis_Project/t20Match_Analysis
```

### 2. Set Up Python Environment
```bash
# Create and activate virtual environment
python -m venv venv
# On Windows:
venv\Scripts\activate

# Install required packages
pip install -r requirements.txt
```

### 3. Run Data Preprocessing
Launch Jupyter Notebook to view or run the data cleaning pipeline:
```bash
jupyter notebook t20_Data_Preprocessing/t20_data_preprocessing.ipynb
```

### 4. View Power BI Dashboard
- Open [Power BI Desktop](https://powerbi.microsoft.com/desktop/).
- Open `t20Match_Analysis.pbix` to interact with the complete report and visuals.

---

## 👤 Author & Acknowledgments
- **Developer:**[Khursheed Alam](https://khursheed4k.vercel.app) 🚀
- **Project:** T20 World Cup Data Analysis & Dashboard
- **Data Source:** ESPNCricinfo T20 World Cup Records
