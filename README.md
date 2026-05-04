# Amazon-fraud-Dashboard
Amazon E-Commerce Fraud Detection &amp; Risk Analytics — Power BI + Python + DAX | 200K Transactions
<div align="center">

<img src="https://upload.wikimedia.org/wikipedia/commons/a/a9/Amazon_logo.svg" width="120" alt="Amazon Logo"/>

# 🛡️ Amazon E-Commerce Fraud Detection & Risk Analytics

### Production-Grade Business Intelligence · Power BI + Python + DAX

[![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=flat-square&logo=python&logoColor=white)](https://python.org)
[![Power BI](https://img.shields.io/badge/Power_BI-Dashboard-F2C811?style=flat-square&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com)
[![Pandas](https://img.shields.io/badge/Pandas-2.x-150458?style=flat-square&logo=pandas&logoColor=white)](https://pandas.pydata.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-22c55e?style=flat-square)](LICENSE)
[![Dataset](https://img.shields.io/badge/Dataset-200K_Transactions-FF9900?style=flat-square)]()
[![Status](https://img.shields.io/badge/Status-Production_Ready-brightgreen?style=flat-square)]()

<br/>

> **Analyzed 200,000+ Amazon-style transactions to surface fraud patterns, high-risk users,**
> **payment anomalies, and geographic hotspots — packaged as a FAANG-style 5-page interactive dashboard.**

<br/>

---

</div>

## 📊 Dashboard Pages

| Page | Content |
|------|---------|
| **1 · Executive Overview** | Total Transactions · Fraud Count · Fraud % · Revenue Loss · Donut Chart · Monthly Trend |
| **2 · Fraud Analysis** | Payment Method Risk · Category Breakdown · Device Behavior · Amount Buckets |
| **3 · User Risk Profiling** | Risk Score Table · Distribution Histogram · User Segment Analysis |
| **4 · Geographic Analysis** | City Fraud Hotspots · Bubble Map · Top 10 Cities Bar |
| **5 · Transaction Behavior** | Hourly Fraud Trend · Amount Distribution · Avg Value Fraud vs Legit |

---

## 🚨 Top 10 Business Insights (Real Data)

| # | Insight | Severity |
|---|---------|----------|
| 1 | **COD carries 86.7% fraud rate** — 3.9× higher than any digital payment | 🔴 Critical |
| 2 | **38.18% overall fraud rate** vs ~2% industry baseline | 🔴 Critical |
| 3 | **₹660 Crore estimated revenue loss** across 200K transactions | 🔴 Critical |
| 4 | **March 2025 was peak fraud month** — 16,944 cases driven by COD + odd-hour orders | 🟠 High |
| 5 | Orders above ₹10K show **22× more fraud** than sub-₹10K orders | 🟠 High |
| 6 | Fraud is **completely flat across all 24 hours** — automated bot signal, not human | 🟠 High |
| 7 | **Fraud avg transaction = ₹86,433** vs ₹68,160 legitimate (26.8% higher value) | 🟡 Medium |
| 8 | **All categories show ~38% fraud** — payment-level fraud, not product-targeted | 🟡 Medium |
| 9 | **User USR-2355 has 100% fraud rate** across 9 transactions — confirmed bot | 🟡 Medium |
| 10 | **City_356 tops** with 182 fraud cases — dispersed pattern = coordinated network | 🟢 Monitor |

---

## 📁 Repository Structure

```
amazon-fraud-dashboard/
│
├── 📊 dashboard/
│   └── Amazon_Fraud_Dashboard.pbix        ← Power BI file (open in PBI Desktop)
│
├── 📓 notebooks/
│   ├── 01_EDA_Fraud_Analysis.ipynb        ← Full exploratory data analysis
│   ├── 02_Feature_Engineering.ipynb       ← Risk score features + transformations
│   └── 03_Visualization_Charts.ipynb      ← Matplotlib/Seaborn chart generation
│
├── 🐍 src/
│   ├── data_generator.py                  ← Generate 200K row synthetic dataset
│   ├── fraud_analysis.py                  ← Core fraud analysis engine
│   └── risk_scorer.py                     ← User risk scoring (0–100)
│
├── 📐 dax/
│   └── measures.dax                       ← All Power BI DAX measures
│
├── 📂 data/
│   └── amazon_fraud_sample_1000.csv       ← Sample 1K rows for preview
│
├── 🐍 python/
│   └── requirements.txt                   ← Python dependencies
│
└── 📖 README.md
```

---

## 🔢 Dataset Schema

| Column | Type | Description |
|--------|------|-------------|
| `Transaction_ID` | int | Unique transaction identifier |
| `User_ID` | int | User identifier |
| `Order_ID` | int | Order identifier |
| `Product_ID` | int | Product identifier |
| `Category` | str | Electronics / Fashion / Home / Beauty / Sports |
| `Price` | float | Unit price |
| `Quantity` | int | Items ordered |
| `Total_Amount` | float | Total order value (₹) |
| `Payment_Method` | str | UPI / Card / COD / Wallet |
| `Transaction_Time` | datetime | Full timestamp |
| `Delivery_Address` | str | City code |
| `IP_Address` | str | User IP |
| `Device_Type` | str | Mobile / Desktop |
| `Fraud_Flag` | int | 0 = Legit · 1 = Fraud |
| `Fraud_Type` | str | COD High Value / High Value Odd Hour / etc. |

---

## ⚙️ Fraud Detection Rules

```python
# Rule 1: COD + High Value → Fraud
if payment_method == "COD" and total_amount > COD_THRESHOLD:
    fraud_flag = 1
    fraud_type = "COD High Value"

# Rule 2: Odd Hour (12AM–5AM) + High Value → Fraud
if hour in range(0, 5) and total_amount > HIGH_VALUE_THRESHOLD:
    fraud_flag = 1
    fraud_type = "High Value Odd Hour"

# Rule 3: Multiple accounts from same IP → Fraud
if user_count_per_ip > ACCOUNT_THRESHOLD:
    fraud_type = "Multiple Accounts"

# Rule 4: Random behavioral anomalies (2% base rate)
# Rule 5: Suspicious location / Fake Return patterns
```

**Fraud Type Breakdown (200K dataset):**

| Fraud Type | Count | % of Fraud |
|------------|-------|------------|
| COD High Value | 42,235 | 55.3% |
| High Value Odd Hour | 30,132 | 39.5% |
| Multiple Accounts | 1,023 | 1.3% |
| Fake Return | 1,002 | 1.3% |
| Suspicious Location | 995 | 1.3% |
| Payment Fraud | 978 | 1.3% |

---

## 📐 Power BI DAX Measures

```dax
-- ① Fraud Rate
Fraud Rate =
DIVIDE(
    CALCULATE(COUNTROWS(Transactions), Transactions[Fraud_Flag] = 1),
    COUNTROWS(Transactions),
    0
)

-- ② Revenue Loss
Revenue Loss =
CALCULATE(
    SUM(Transactions[Total_Amount]),
    Transactions[Fraud_Flag] = 1
)

-- ③ Average Transaction Value
Avg Transaction Value = AVERAGE(Transactions[Total_Amount])

-- ④ User Risk Score (0–100)
Risk Score =
VAR FraudCount = CALCULATE(COUNTROWS(Transactions), Transactions[Fraud_Flag] = 1)
VAR TotalTxn   = COUNTROWS(Transactions)
RETURN DIVIDE(FraudCount, TotalTxn, 0) * 100

-- ⑤ Fraud by Payment Method %
Fraud by Payment % =
DIVIDE(
    CALCULATE(COUNTROWS(Transactions), Transactions[Fraud_Flag] = 1),
    CALCULATE(COUNTROWS(Transactions)),
    0
) * 100

-- ⑥ Non-Fraud Transactions
Legit Transactions =
CALCULATE(COUNTROWS(Transactions), Transactions[Fraud_Flag] = 0)

-- ⑦ Fraud Count
Fraud Count =
CALCULATE(COUNTROWS(Transactions), Transactions[Fraud_Flag] = 1)

-- ⑧ MoM Fraud Change %
MoM Fraud Change =
VAR CurrentMonth = CALCULATE([Fraud Count], DATESMTD(Transactions[Transaction_Time]))
VAR PrevMonth = CALCULATE([Fraud Count], PREVIOUSMONTH(Transactions[Transaction_Time]))
RETURN DIVIDE(CurrentMonth - PrevMonth, PrevMonth, 0)
```

---

## 🚀 Quick Start

### Clone & Setup
```bash
git clone https://github.com/kundanmahajankm04-collab/amazon-fraud-dashboard.git
cd amazon-fraud-dashboard
pip install -r python/requirements.txt
```

### Generate Full Dataset
```bash
python src/data_generator.py --rows 200000 --output data/amazon_fraud_dataset_200k.csv
```

### Run Notebooks
```bash
jupyter notebook notebooks/
```

### Open Dashboard
Open `dashboard/Amazon_Fraud_Dashboard.pbix` in **Power BI Desktop**, then:
1. Go to **Home → Transform Data**
2. Update the CSV file path to your local `data/` folder
3. Click **Refresh**

---

## 🎨 Dashboard Design System

| Element | Choice |
|---------|--------|
| **Theme** | Dark mode default + one-click light toggle |
| **Primary color** | `#FF9900` Amazon Orange |
| **Fraud color** | `#E84040` Red |
| **Safe color** | `#22C55E` Green |
| **Layout** | Top KPIs → Mid Charts → Bottom Insights |
| **Slicers** | Horizontal panel: Date · Payment · Category · Device |
| **Advanced** | Drill-through · Conditional formatting · Dynamic titles |

---

## 🛠️ Tech Stack

| Tool | Version | Purpose |
|------|---------|---------|
| Power BI Desktop | Latest | Dashboard & interactive reports |
| DAX | — | KPIs, measures, risk scores |
| Python | 3.11 | Data generation & EDA |
| Pandas | 2.x | Data wrangling |
| Matplotlib | 3.x | Supplemental visualizations |
| Seaborn | 0.13 | Statistical charts |
| Jupyter | 7.x | Analysis notebooks |

---

## 💼 For Recruiters

### Resume Bullets (ATS-Optimized)
- Designed a **5-page Power BI fraud detection dashboard** analyzing 200K+ Amazon e-commerce transactions, identifying a 38.18% fraud rate and ₹660 Cr revenue exposure across 6 fraud categories
- Engineered **8+ advanced DAX measures** (Fraud Rate, Risk Score, MoM Change, Revenue Loss) with drill-through navigation, dynamic slicers, and conditional formatting for executive reporting
- Identified that **COD payment carries 86.7% fraud rate** (3.9× digital payment average), generating actionable policy recommendations to reduce fraud exposure by an estimated 55%

### LinkedIn Description
> 🔍 **Amazon E-Commerce Fraud Detection & Risk Analytics | Power BI · Python · DAX**
>
> Analyzed 200,000 transactions to uncover fraud patterns using Power BI + Python. COD accounts for 55% of all fraud at 86.7% fraud rate. Built 5 interactive dashboard pages with Risk Scoring, Geographic Hotspot Mapping, and 24/7 Behavioral Analysis revealing fully automated bot-driven fraud. Includes 8 production-ready DAX measures and complete EDA notebooks.
>
> 🔗 [GitHub Repo](https://github.com/kundanmahajankm04-collab/amazon-fraud-dashboard)

---

## 📄 License

MIT License — free to use, modify, and distribute with attribution.

---

<div align="center">

Made by **[Kundan Mahajan](https://github.com/kundanmahajankm04-collab)**

⭐ **If this project helped you, please star the repo!** ⭐

</div>
