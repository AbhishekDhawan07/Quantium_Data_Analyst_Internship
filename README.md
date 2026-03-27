# 🛒 Quantium Data Analytics Virtual Internship

<p align="center">
  <img src="https://img.shields.io/badge/Internship-Quantium%20x%20Forage-blue?style=for-the-badge&logo=databricks&logoColor=white" />
  <img src="https://img.shields.io/badge/Domain-Data%20Analytics-green?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/Language-Python-yellow?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/Status-Completed-success?style=for-the-badge" />
</p>

---

## 📋 Table of Contents

- [About the Internship](#-about-the-internship)
- [Certificate](#-certificate)
- [Repository Structure](#-repository-structure)
- [Tech Stack](#-tech-stack)
- [Task 1 — Data Preparation & Customer Analytics](#-task-1--data-preparation--customer-analytics)
- [Task 2 — Experimentation & Uplift Testing](#-task-2--experimentation--uplift-testing)
- [Key Insights & Findings](#-key-insights--findings)
- [Acknowledgements](#-acknowledgements)

---

## 🏢 About the Internship

This repository contains my work for the **Quantium Data Analytics Virtual Internship**, hosted on [Forage](https://www.theforage.com/). Quantium is one of Australia's leading data analytics firms, and this program simulates real-world analytics challenges faced by their teams.

The internship revolves around the **chips category** within a major retail supermarket chain. As a data analyst, I step into the role of helping the **Category Manager, Julia**, make data-driven decisions ahead of an upcoming category review. The work spans two core tasks — from deep-diving into customer behaviour to rigorously evaluating the impact of a new store trial layout.

---

## 🏆 Certificate

> ✅ Successfully completed the **Quantium Data Analytics Virtual Internship** on Forage.

📄 **[View Certificate →](https://drive.google.com/file/d/1dzjQ8-bBZegXcLPGcKk3fHFwcZHnTsnd/view?usp=drivesdk)**

---

## 📁 Repository Structure

```
Quantium_Data_Analytics_Virtual_Internship/
│
└── Quantium Data Analytics - Forage Virtual Internship/
    ├── Task_1_-_Quantium_-_Forage_data_Analytics_Internship.ipynb
    └── Task_2_-_Quantium_Data_Analytics_-_Forage_Internship.ipynb
```

---

## 🛠 Tech Stack

| Tool / Library | Purpose |
|---|---|
| `Python 3` | Core programming language |
| `Pandas` | Data manipulation & wrangling |
| `NumPy` | Numerical computations |
| `Matplotlib` | Data visualization |
| `Seaborn` | Statistical visualizations |
| `SciPy` | Statistical hypothesis testing (t-tests) |
| `mlxtend` | Apriori algorithm & association rules |
| `Jupyter Notebook` | Interactive development environment |

---

## 📊 Task 1 — Data Preparation & Customer Analytics

### 🎯 Objective

Analyse transaction and customer purchase behaviour datasets to uncover **purchasing trends** and **customer segments** that drive chip sales — and provide actionable commercial recommendations to Julia.

### 📂 Datasets Used

- **`QVI_transaction_data.xlsx`** — Transactional records of chip purchases across all stores
- **`QVI_purchase_behaviour.csv`** — Customer loyalty card data including lifestage and premium tier

### 🔍 Key Steps

**1. Data Loading & Exploration**
- Loaded transaction and purchase behaviour datasets
- Performed `.describe()`, `.info()`, and null-value checks on both datasets
- Merged the two datasets on `LYLTY_CARD_NBR` (loyalty card number) for a unified view

**2. Data Cleaning & Preprocessing**
- Converted the `DATE` column from Excel serial number format to proper `datetime` format
- Identified and removed outlier transactions (a single customer purchasing 200 packs — flagged as non-retail/commercial bulk buy)
- Verified all 365 days of the year were present (discovered Christmas Day had zero sales — expected)
- Extracted and cleaned **packet sizes** from product names (e.g., `175g`, `330G`)
- Standardised **brand names** to handle inconsistent naming (e.g., `Dorito` → `Doritos`, `Smith` → `Smiths`, `GrnWves` → `Grain Waves`)

**3. Customer Segmentation Analysis**

Customers are segmented along two dimensions:
- **LIFESTAGE**: e.g., Young Singles/Couples, Midage Singles/Couples, Older Families, Retirees, etc.
- **PREMIUM_CUSTOMER**: Budget, Mainstream, or Premium

Key analyses performed:
- **Total Sales by Segment** — grouped by lifestage × premium tier
- **Unique Customer Count per Segment** — to understand population size driving sales
- **Purchase Frequency per Customer** — to identify highly engaged segments
- **Average Spending per Transaction** — to spot high-value segments
- **Statistical Significance Testing** (t-test) — confirmed meaningful spending differences between Mainstream Young/Midage vs. Budget/Premium Young/Midage customers

**4. Brand & Pack Size Preferences**

- Used `value_counts()` and bar charts to find top brands per segment
- Applied **Apriori association rule mining** to discover segment-brand co-purchase patterns
- Explored pack size preferences across all 21 customer segments

### 💡 Key Findings

| Rank | Segment | Total Sales |
|---|---|---|
| 🥇 1st | Older Families — Budget | $156,864 |
| 🥈 2nd | Young Singles/Couples — Mainstream | $147,582 |
| 🥉 3rd | Retirees — Mainstream | $145,169 |

- **Older Families (Budget)** drive sales through **high purchase frequency** (not population size)
- **Young Singles/Couples (Mainstream)** are the only segment preferring **Doritos** as second choice (after Kettle)
- **Kettle** is the most purchased brand across **every** customer segment
- Mainstream Young/Midage customers spend **statistically significantly more** per transaction than their Budget/Premium counterparts

---

## 🧪 Task 2 — Experimentation & Uplift Testing

### 🎯 Objective

Evaluate the performance of a **new store layout trial** conducted in **Stores 77, 86, and 88**, and determine whether the trial led to a statistically significant uplift in sales and customer numbers compared to control stores.

### 📂 Dataset Used

- **`QVI_data.csv`** — Cleaned merged dataset from Task 1, enriched with store-level monthly metrics

### 🔍 Key Steps

**1. Monthly Metric Computation**

For every store and every month, the following five metrics were calculated:
- `TOT_SALES` — Total revenue
- `nCustomers` — Unique customer count
- `nTxnPerCust` — Average transactions per customer
- `nChipsPerTxn` — Average chip units per transaction
- `avgPricePerUnit` — Average price per unit

**2. Control Store Selection**

A robust, two-pronged method was used to identify the best control store for each trial store:

- **Pearson Correlation Score** — Measures how similarly two stores' metrics move over time
- **Magnitude Distance Score** — Measures how close the absolute values of each metric are between two stores

Both scores were combined using a **50/50 weighted composite score** to identify the most similar pre-trial control store for each trial store.

**Final Control Store Matches:**

| Trial Store | Control Store |
|---|---|
| Store 77 | Store 233 |
| Store 86 | Store 155 |
| Store 88 | Store 40 |

**3. Statistical Significance Testing (3-Step Framework)**

A rigorous three-step hypothesis testing methodology was applied:

- **Step 1** — Verified control store's own pre-trial vs. trial performance shows no significant difference (null hypothesis holds)
- **Step 2** — Confirmed that trial and control stores perform similarly during the pre-trial period (validates the control store selection)
- **Step 3** — Tested whether the **percentage difference** in performance between trial and control stores during the trial period significantly exceeds the pre-trial baseline (using 95% confidence interval t-score thresholds)

### 💡 Key Findings

**Total Sales Uplift:**
- ✅ **Store 77**: March & April showed statistically significant sales uplift
- ✅ **Store 86**: March showed statistically significant sales uplift
- ❌ **Store 88**: No statistically significant difference detected

**Customer Count Uplift:**
- ✅ **Store 77**: March & April showed statistically significant increase in customers
- ✅ **Store 86**: February, March & April all showed significant increase in customers
- ❌ **Store 88**: No statistically significant difference detected

> ⚠️ **Note on Store 88**: The lack of uplift in Store 88 is flagged for further investigation with the client — there may be store-specific or operational factors that explain the divergence from the other trial stores.

---

## 🔑 Key Insights & Findings

> A synthesis of the most actionable insights across both tasks:

- 📦 **Kettle** dominates chip brand preferences across all customer segments
- 👨‍👩‍👧 **Older Families (Budget)** are power buyers — high visit frequency, not just large population
- 🧑 **Young Singles/Couples (Mainstream)** uniquely prefer Doritos as a secondary brand — a targeted marketing opportunity
- 💰 Mainstream Young/Midage customers are **statistically proven** to spend more per visit
- 🏪 **Trial stores 77 and 86** demonstrated genuine, statistically significant sales and customer uplifts — the new layout works
- 🏪 **Trial store 88** showed no significant change — warrants a deeper client-side investigation

---

## 🙏 Acknowledgements

- [**Quantium**](https://www.quantium.com/) for designing this real-world analytics challenge
- [**Forage**](https://www.theforage.com/) for hosting the virtual internship program

---

<p align="center">
  <i>Made with 💡 data, 🐍 Python, and a lot of ☕</i>
</p>
