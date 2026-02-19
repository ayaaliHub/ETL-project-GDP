# 📊 Data Engineering ETL Projects

> Two end-to-end ETL (Extract, Transform, Load) pipelines built as part of a Data Engineering curriculum — one focused on global bank market capitalizations, and the other on country GDP rankings.

---

## 📁 Repository Structure

```
data-engineering-etl/
│
├── banks_project/
│   ├── banks_project.py          # Main ETL script
│   ├── exchange_rate.csv         # Currency exchange rates (USD → GBP, EUR, INR)
│   ├── Banks.db                  # SQLite database output
│   ├── Largest_banks_data.csv    # Final processed CSV output
│   └── code_log.txt              # Execution log
│
├── gdp_project/
│   ├── etl_gdp.py                # Main ETL script
│   ├── Countries_by_GDP.json     # JSON output
│   ├── World_Economies.db        # SQLite database output
│   └── etl_project_log.txt       # Execution log
│
└── README.md
```

---

## 🏦 Project 1 — Top 10 Largest Banks by Market Capitalization

### Overview

This project scrapes, transforms, and stores data about the **top 10 largest banks in the world** ranked by market capitalization in USD. The data is enriched with multi-currency conversions and persisted both as a CSV file and an SQLite database table.

### Objectives

- Extract the list of the top 10 largest banks by market cap (in billion USD) from a web source
- Transform the market cap values into **GBP**, **EUR**, and **INR** using a provided exchange rate CSV
- Load the processed data into:
  - A **CSV file** (`Largest_banks_data.csv`)
  - A **SQLite database table** (`Banks.db` → table: `Largest_banks`)
- Log all ETL steps to `code_log.txt`

### Data Source

- Web-scraped from a publicly available Wikipedia-style table listing the world's largest banks

### Exchange Rate File (`exchange_rate.csv`)

| Currency | Rate (relative to USD) |
|----------|----------------------|
| GBP      | 0.8 (example)        |
| EUR      | 0.93 (example)       |
| INR      | 82.95 (example)      |

### Output Schema

| Column                  | Description                        |
|-------------------------|------------------------------------|
| `Name`                  | Bank name                          |
| `MC_USD_Billion`        | Market cap in USD (billion)        |
| `MC_GBP_Billion`        | Market cap in GBP (billion)        |
| `MC_EUR_Billion`        | Market cap in EUR (billion)        |
| `MC_INR_Billion`        | Market cap in INR (billion)        |

### Sample Query

```sql
-- Get all banks with their market cap in EUR
SELECT Name, MC_EUR_Billion FROM Largest_banks;

-- Get the bank with the highest market cap in INR
SELECT Name, MC_INR_Billion FROM Largest_banks ORDER BY MC_INR_Billion DESC LIMIT 1;
```

---

## 🌍 Project 2 — Countries Ranked by GDP (IMF Data)

### Overview

This project builds an **automated ETL pipeline** that extracts GDP data for all countries from the IMF (as published on Wikipedia), transforms the values into rounded billion USD figures, and makes the data accessible as both a JSON file and a queryable SQLite database.

Since the IMF updates this data **twice a year**, the script is designed to be re-run periodically to keep the data current.

### Objectives

- Extract GDP data for all countries from the [IMF GDP Wikipedia page](https://en.wikipedia.org/wiki/List_of_countries_by_GDP_%28nominal%29)
- Transform GDP values to **billion USD**, rounded to **2 decimal places**
- Load the processed data into:
  - A **JSON file** (`Countries_by_GDP.json`)
  - A **SQLite database table** (`World_Economies.db` → table: `Countries_by_GDP`)
- Run a query to display only countries with **GDP > 100 billion USD**
- Log all ETL steps to `etl_project_log.txt`

### Output Schema

| Column            | Description                        |
|-------------------|------------------------------------|
| `Country`         | Country name                       |
| `GDP_USD_billion` | GDP in billion USD (rounded to 2dp)|

### Sample Query

```sql
-- Display only countries with GDP greater than 100 billion USD
SELECT * FROM Countries_by_GDP WHERE GDP_USD_billion >= 100 ORDER BY GDP_USD_billion DESC;
```

### Sample Output (`Countries_by_GDP.json`)

```json
[
  { "Country": "United States", "GDP_USD_billion": 26854.60 },
  { "Country": "China",         "GDP_USD_billion": 19373.59 },
  { "Country": "Germany",       "GDP_USD_billion": 4308.85  },
  ...
]
```

---

## ⚙️ Technologies Used

| Tool / Library  | Purpose                                      |
|-----------------|----------------------------------------------|
| Python 3.x      | Core programming language                    |
| `requests`      | HTTP requests for web scraping               |
| `BeautifulSoup` | HTML parsing                                 |
| `pandas`        | Data manipulation and transformation         |
| `sqlite3`       | Local database storage                       |
| `json`          | JSON file I/O                                |
| `datetime`      | Timestamping log entries                     |

---

## 🚀 Getting Started

### Prerequisites

Make sure you have Python 3.7+ installed. Then install the required libraries:

```bash
pip install requests beautifulsoup4 pandas
```

### Running Project 1 (Banks)

```bash
cd banks_project
python banks_project.py
```

**Expected outputs:**
- `Largest_banks_data.csv` — processed data with multi-currency columns
- `Banks.db` — SQLite database with `Largest_banks` table
- `code_log.txt` — timestamped ETL execution log

### Running Project 2 (GDP)

```bash
cd gdp_project
python etl_gdp.py
```

**Expected outputs:**
- `Countries_by_GDP.json` — GDP data for all countries
- `World_Economies.db` — SQLite database with `Countries_by_GDP` table
- `etl_project_log.txt` — timestamped ETL execution log

---

## 📋 ETL Log Format

Both projects maintain a log file with timestamped entries for every major step:

```
2024-01-15 10:23:01 : Preliminaries complete. Initiating ETL process
2024-01-15 10:23:02 : Data extraction complete. Initiating Transformation process
2024-01-15 10:23:03 : Data transformation complete. Initiating Loading process
2024-01-15 10:23:04 : Data saved to CSV file
2024-01-15 10:23:05 : SQL Connection initiated
2024-01-15 10:23:06 : Data loaded to Database as a table. Running queries
2024-01-15 10:23:07 : Process Complete
```

---

## 📌 Notes

- Exchange rates in Project 1 are read dynamically from `exchange_rate.csv` — update this file to reflect current rates before re-running.
- Project 2 scrapes live Wikipedia data, so results may vary slightly depending on when the script is run relative to IMF update cycles.
- Both SQLite databases are local files and do not require any database server setup.

---

## 👤 Author

**[Aya Ali]**
Junior Data Engineer
(https://github.com/ayaaliHub)

---

## 📄 License

This project is for educational purposes. Data sourced from publicly available Wikipedia pages citing IMF and other financial publications.# ETL-project-GDP
