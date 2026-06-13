# Part 1: Data Audit, EDA & Business Understanding

---

## Overview

This repository contains the complete data audit, exploratory data analysis, and business understanding work for Part 1 of the D2C Customer Churn Capstone.

**Snapshot Date:** 2025-09-30   
**Goal:** Audit raw data quality, perform EDA, identify churn-risk patterns, and surface business insights before any modeling begins.

---

## Repository Structure

```
├── data/                          
│   ├── customers.csv
│   ├── orders.csv
│   ├── support_tickets.csv
│   ├── web_events_snapshot.csv
│   ├── churn_labels.csv
│   └── intervention_history.csv
├── charts/                        
├── eda_audit.ipynb                
├── data_quality_report.md         
├── business_memo.md               
├── requirements.txt               
└── README.md
```

---

## Setup Instructions

### 1. Clone the repository
```bash
git clone <repo-url>
cd part1-eda
```

### 2. Create a virtual environment 
```bash
python -m venv venv
source venv/bin/activate        # Mac/Linux
venv\Scripts\activate           # Windows
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

### 4. Add the data files
Download the dataset from the provided Google Drive link and place all CSV files in the `data/` folder.

### 5. Create the charts output folder
```bash
mkdir charts
```

### 6. Run the notebook
```bash
jupyter notebook eda_audit.ipynb
```
Run all cells from top to bottom.

---


## Requirements

See `requirements.txt`. Core dependencies:
- pandas
- numpy
- matplotlib
- seaborn
- jupyter
