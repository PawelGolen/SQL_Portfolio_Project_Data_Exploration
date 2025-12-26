# COVID-19 Data Exploration (SQL Portfolio Project)

This repository contains a set of **SQL queries** used to explore and analyze COVID‑19 data from two tables:

- `PortfolioProject..CovidDeaths`
- `PortfolioProject..CovidVaccinations`

The script focuses on exploratory analysis (EDA) such as infection rates, death percentages, global rollups, and vaccination progress using **joins**, **window functions**, **CTEs**, **temp tables**, and a **view** for downstream visualization.

---

## 🎯 Goals

- Validate and preview the raw datasets
- Analyze infections and deaths over time by country
- Compare infection rate vs population
- Identify countries/continents with the highest death counts
- Produce global aggregates (daily and total)
- Track vaccination progress using rolling sums
- Persist a reusable dataset via a SQL view for visualization

---

## 🧾 Data Source & Tables

### `CovidDeaths`
Typical fields used:
- `location`
- `date`
- `continent`
- `population`
- `total_cases`, `new_cases`
- `total_deaths`, `new_deaths`

### `CovidVaccinations`
Typical fields used:
- `location`
- `date`
- `continent`
- `new_vaccinations`

> Note: The script filters out summary rows by requiring `continent IS NOT NULL`.

---

## 🔍 What’s in the SQL Script?

### 1) Quick dataset checks
- `SELECT *` previews for both tables
- filters out non-country rollups (`continent IS NOT NULL`)

### 2) Core metrics
- **Death percentage** by location (e.g., Poland):  
  `total_deaths / total_cases * 100`
- **Infection rate** vs population:  
  `total_cases / population * 100`

### 3) Rankings
- Countries with the highest infection rate vs population
- Countries with the highest total death counts
- Continents with the highest total death counts

### 4) Global numbers
- Daily global totals: `SUM(new_cases)`, `SUM(new_deaths)`
- Global overall totals and death percentage

### 5) Vaccinations analysis (Population vs Vaccinations)
- Join deaths + vaccinations on `location` and `date`
- Rolling vaccinated people using a window function:
  ```sql
  SUM(CAST(vac.new_vaccinations as bigint))
    OVER (PARTITION BY dea.location ORDER BY dea.location, dea.date)
  ```
- Same logic implemented in:
  - a **CTE**
  - a **temp table**
  - a **view**: `PercentPopulationVaccinated`

---

## 🧠 Key SQL Concepts Used

- `JOIN` on multi-column keys (`location`, `date`)
- `GROUP BY` + aggregates (`SUM`, `MAX`)
- Type casting (`CAST(...)`) for correct numeric calculations
- Window functions (`SUM(...) OVER (PARTITION BY ...)`)
- `CTE` (Common Table Expression) for readability
- Temporary tables for intermediate storage
- `VIEW` creation for reusable visualization datasets

---

## ▶️ How to Run

1. Import/load the two datasets into your SQL Server environment (or adapt table names for your DB).
2. Open the `.sql` file in SSMS (SQL Server Management Studio) or your SQL client.
3. Run sections top-to-bottom, or execute individual blocks depending on your analysis goal.

---

## ⚠️ Notes / Improvements

- Consider protecting division by zero using `NULLIF(total_cases, 0)`
- Some fields may require explicit casting to avoid integer division
- Temp table column names in the original script contain typos (e.g., `Continetn`, `Data`, `Nev_vaccinations`) — consider fixing for clarity

---

## 📄 License

MIT License
