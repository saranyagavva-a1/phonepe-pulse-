# 📱 PhonePe Pulse – Data Visualization and Exploration

An interactive, geo-visualization dashboard built in **Power BI** that explores digital-payment trends across Indian states and districts using the PhonePe Pulse dataset.

![Dashboard Overview](screenshots/01_dashboard_overview.png)

| | |
|---|---|
| **Domain** | Fintech / Digital Payments |
| **Tools** | Power BI (Power Query, DAX), Excel / CSV |
| **Data** | PhonePe Pulse (pre-processed CSV files) |
| **Author** | Saranya Gavva |

---

## 📌 Table of Contents
1. [Problem Statement](#-problem-statement)
2. [Objectives](#-objectives)
3. [Dataset](#-dataset)
4. [Approach](#-approach)
5. [Data Model](#-data-model)
6. [DAX Measures & KPIs](#-dax-measures--kpis)
7. [Dashboard Pages](#-dashboard-pages)
8. [Dropdown / Slicer Options](#-dropdown--slicer-options)
9. [Key Insights](#-key-insights)
10. [Project Structure](#-project-structure)
11. [How to Run](#-how-to-run)
12. [Learning Outcomes](#-learning-outcomes)
13. [Future Improvements](#-future-improvements)
14. [Contact](#-contact)

---

## ❓ Problem Statement
The PhonePe Pulse GitHub repository holds a large amount of data on transactions, users and insurance across India. The goal is to process this data and turn it into insights presented in a **user-friendly, interactive geo dashboard**.

Data extraction (cloning the repository) and cleaning/pre-processing were already done, and the transformed data was provided for this project.

## 🎯 Objectives
- Build a **live geo-visualization dashboard** (map-based, interactive, visually appealing).
- Provide **at least 10 dropdown/slicer options** to explore different facts and figures.
- Identify **top users and top transactions** (states, districts, pincodes).
- Calculate **YoY growth and CAGR** to understand trends over time.
- Track **KPIs** for transaction volume, user engagement and revenue.

## 📂 Dataset
- Source: PhonePe Pulse data (pre-processed CSV files provided with the project brief).
- Inspired by: [PhonePe Pulse](https://www.phonepe.com/pulse/)
- Typical tables:

| Table | Description |
|---|---|
| Aggregated Transaction | Transaction count & amount by state, year, quarter, transaction type |
| Aggregated User | Registered users & app opens by state, year, quarter, device brand |
| Aggregated Insurance | Insurance policies count & value by state, year, quarter |
| Map Transaction / User / Insurance | District-level data |
| Top Transaction / User / Insurance | Top states, districts and pincodes |

> Update this table to match the exact files you used.

## 🛠 Approach
1. **Merge** the pre-processed files into consistent tables (state, year, quarter, metrics).
2. **Choose the platform** – Power BI.
3. **Import data** using *Get Data → Folder / Text/CSV*.
4. **Clean in Power Query** – fix state names (e.g. `andaman-&-nicobar-islands` → `Andaman & Nicobar Islands`), set data types, remove duplicates/nulls, create a `Year-Quarter` column.
5. **Create a basic geo map** (Filled Map / Map visual) using the State field.
6. **Add layers** – transaction amount (colour/size), user count, device brand, transaction type.
7. **Design the dashboard** – clean layout, consistent colours (PhonePe purple theme), clear titles.
8. **Add 10+ slicers/dropdowns** for interactivity.
9. **Create growth rate & KPIs** using DAX (YoY, CAGR, average transaction value).
10. **Analyse the visuals** and document insights.
11. **Publish** to Power BI Service for a shareable, live dashboard.

## 🧩 Data Model
Star-schema style model:

- **Fact tables:** Transactions, Users, Insurance (state / district / pincode level)
- **Dimension tables:** `DimState`, `DimDate` (Year, Quarter), `DimTransactionType`, `DimDeviceBrand`

Relationships are one-to-many from dimensions to facts.

![Data Model](screenshots/02_data_model.png)

## 📐 DAX Measures & KPIs

```DAX
Total Transactions = SUM(Transactions[Transaction_Count])

Total Amount = SUM(Transactions[Transaction_Amount])

Avg Transaction Value = DIVIDE([Total Amount], [Total Transactions])

Registered Users = SUM(Users[Registered_Users])

App Opens = SUM(Users[App_Opens])

Engagement Rate = DIVIDE([App Opens], [Registered Users])

Amount Previous Year =
CALCULATE([Total Amount], SAMEPERIODLASTYEAR(DimDate[Date]))

YoY Growth % =
DIVIDE([Total Amount] - [Amount Previous Year], [Amount Previous Year])

CAGR % =
VAR StartVal = CALCULATE([Total Amount], FIRSTDATE(DimDate[Date]))
VAR EndVal   = CALCULATE([Total Amount], LASTDATE(DimDate[Date]))
VAR Years    = DATEDIFF(MIN(DimDate[Date]), MAX(DimDate[Date]), YEAR)
RETURN IF(Years > 0, POWER(DIVIDE(EndVal, StartVal), 1 / Years) - 1)
```

> Adjust table/column names to your model. If you only have Year and Quarter columns, build a `DimDate` table with a real date (e.g. first day of each quarter) so time-intelligence functions work.

**KPI cards:** Total Transactions · Total Amount · Avg Transaction Value · Registered Users · App Opens · YoY Growth % · CAGR %

## 📊 Dashboard Pages

### 1. Overview / Geo Map
Filled map of India showing transaction amount by state, with KPI cards on top.

![Geo Map](screenshots/03_geo_map.png)

### 2. Transaction Analysis
Transaction trend over time by transaction type, with top 10 states / districts / pincodes.

![Transaction Analysis](screenshots/04_transaction_analysis.png)

### 3. User Analysis
Registered users and app opens by state, plus device-brand share.

![User Analysis](screenshots/05_user_analysis.png)

### 4. Insurance Analysis *(if included)*
Insurance policies count and value by state and quarter.

![Insurance Analysis](screenshots/06_insurance_analysis.png)

### 5. Growth & KPIs
YoY growth, CAGR and quarter-over-quarter trends.

![Growth and KPIs](screenshots/07_growth_kpis.png)

## 🎛 Dropdown / Slicer Options
At least 10 selectors let users change what the dashboard shows:

1. Year
2. Quarter
3. State
4. District
5. Transaction Type (Recharge, Peer-to-peer, Merchant payments, etc.)
6. Device Brand
7. Metric selector (Count / Amount) – via field parameter
8. Top N selector (5 / 10 / 15) – via numeric parameter
9. Data category (Transactions / Users / Insurance)
10. Map level (State / District)
11. Pincode (top pincodes view)

![Slicers](screenshots/08_slicers.png)

## 💡 Key Insights
> Replace these with the real findings from your dashboard, with numbers.

- **Top states by transaction value:** _e.g. Telangana, Karnataka, Maharashtra…_
- **Growth trend:** _transaction value grew X% YoY between 20XX and 20XX; CAGR of X%._
- **Most used transaction type:** _e.g. Peer-to-peer payments account for X% of total amount._
- **User engagement:** _states with the highest app opens per registered user._
- **Device brands:** _top brands among registered users._
- **Seasonality:** _quarters with peak activity._

## 🗂 Project Structure
```
PhonePe-Pulse-Dashboard/
│
├── data/                 # Pre-processed CSV files
├── screenshots/          # Dashboard images used in this README
├── PhonePe_Pulse.pbix    # Power BI report file
├── PhonePe_Pulse.pdf     # Exported dashboard (optional)
└── README.md
```

## ▶️ How to Run
1. Clone this repository:
   ```bash
   git clone https://github.com/<your-username>/PhonePe-Pulse-Dashboard.git
   ```
2. Open `PhonePe_Pulse.pbix` in **Power BI Desktop**.
3. If prompted, update the data source path: *Home → Transform data → Data source settings* and point to the `data/` folder.
4. Click **Refresh** and explore the dashboard.
5. Live link (Power BI Service): _add your published link here_

## 🎓 Learning Outcomes
- Building geo-visualization dashboards in Power BI.
- Writing DAX for KPIs, YoY growth and CAGR.
- Data cleaning and modelling with Power Query.
- Understanding digital payments: transactions, users, devices and regional trends.
- Designing interactive, user-friendly dashboards.

## 🚀 Future Improvements
- Automate refresh from the PhonePe Pulse GitHub repository.
- Add forecasting for transaction value.
- Add row-level security and a mobile layout.

## 📬 Contact
**Saranya Gavva**
- GitHub: [@your-username](https://github.com/your-username)
- LinkedIn: _add link_
- Email: _add email_

⭐ If you found this project useful, please star the repo!
