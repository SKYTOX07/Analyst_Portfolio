# 💼 Portfolio – Data Analyst (SQL • Power BI • Excel)

A curated set of **real-world data projects** demonstrating end-to-end skills in **data cleaning, transformation, exploration, and reporting**.  
This repository includes:

- **SQL Data Cleaning (Nashville Housing)**
- **SQL Data Exploration (COVID-19 deaths & vaccinations)**
- **Power BI** assets for a data-professional survey
- **Excel** workbook used in BI reporting

---

## 📦 Repository Contents



PortfolioProject-main/
├── CovidDeaths.csv # COVID deaths time series (country-date level)
├── CovidVaccinations.csv # COVID vaccinations time series (country-date level)
├── Data_Professional_Survey_PowerBI.pbit # Power BI Template (Data-Professional Survey)
├── Data_Professional_survey_project.pdf # PDF slide/report artifact
├── Nashville Housing Data for Data Cleaning .csv # Nashville housing dataset (raw)
├── Power BI - Final Project.xlsx # Excel model used with BI
├── SQL_Data_Cleaning_Project.sql # T-SQL: Nashville Housing data cleaning pipeline
└── SQL_Project_Data_Exploration.sql # T-SQL: COVID exploration queries (joins, windows)


> ⚠️ Note: The Nashville CSV filename includes a space before `.csv` in the original content. Keep it as is or rename consistently and update your import steps.

---

## 🚀 What This Portfolio Demonstrates

- **Data Cleaning & Transformation (SQL Server / T-SQL):**
  - Type conversions, null handling, **self-joins** for data imputation
  - Text parsing with `SUBSTRING`, `CHARINDEX`, `PARSENAME`
  - Categorical normalization with `CASE`
  - **Deduplication** using `CTE + ROW_NUMBER()`
  - Schema hygiene (`ALTER TABLE`, `DROP COLUMN`)
- **Exploratory SQL Analysis:**
  - Joins across subject tables, **rolling calculations** with window functions
  - Rate/ratio derivations (e.g., cases vs deaths, vaccinations tracking)
  - Country/period level slicing for insights
- **Business Intelligence:**
  - **Power BI** template and **Excel** model for dashboarding/reporting

---

## 🧭 Quick Start

### 1) Set up a SQL Server database
Create (or use) a database named `portfolioproject` (or update script references accordingly).

```sql
CREATE DATABASE portfolioproject;

2) Import the datasets (SQL Server Import Wizard)

Nashville Housing → NashvilleHousing

CovidDeaths.csv → CovidDeaths

CovidVaccinations.csv → CovidVaccinations

SSMS → Right-click database → Tasks → Import Flat File…
Validate date/time columns as DATE/DATETIME and numeric columns as INT/FLOAT where appropriate.

3) Run the SQL projects

A) Data Cleaning – Nashville Housing
File: SQL_Data_Cleaning_Project.sql
This script will:

Standardize SaleDate into a proper DATE column (SaleDateConverted)

Impute missing property addresses via self-join on ParcelID

Split address into PropertySplitAddress and PropertySplitCity

Split owner address into OwnerSplitAddress, OwnerSplitCity, OwnerSplitState

Normalize SoldAsVacant to Yes/No

Remove duplicates with a CTE (ROW_NUMBER())

Drop unused columns

After execution you’ll have an analysis-ready NashvilleHousing table with tidy address fields and consistent categories.

B) Data Exploration – COVID
File: SQL_Project_Data_Exploration.sql
This script includes:

Case fatality explorations (Total Cases vs Total Deaths)

Infection rates (Total Cases vs Population)

Rolling vaccination calculations using window functions

Country/date-level trend queries ready for visualization

🧪 Highlights & Sample Patterns
Normalize categorical fields
UPDATE NashvilleHousing
SET SoldAsVacant =
  CASE
    WHEN SoldAsVacant IN ('1','Y') THEN 'Yes'
    WHEN SoldAsVacant IN ('0','N') THEN 'No'
    ELSE SoldAsVacant
  END;

Deduplicate with window functions
WITH RowNumCTE AS (
  SELECT *,
         ROW_NUMBER() OVER (
           PARTITION BY ParcelID, PropertyAddress, SalePrice, SaleDate, LegalReference
           ORDER BY UniqueID
         ) AS row_num
  FROM portfolioproject..NashvilleHousing
)
DELETE FROM RowNumCTE
WHERE row_num > 1;

Rolling vaccinations (exploration)
SELECT
  dea.location,
  dea.date,
  vac.new_vaccinations,
  SUM(CAST(vac.new_vaccinations AS INT)) OVER
    (PARTITION BY dea.location ORDER BY dea.date) AS RollingPeopleVaccinated
FROM portfolioproject..CovidDeaths AS dea
JOIN portfolioproject..CovidVaccinations AS vac
  ON dea.location = vac.location
 AND dea.date = vac.date
WHERE dea.continent IS NOT NULL;

📊 BI & Excel Assets

Power BI Template: Data_Professional_Survey_PowerBI.pbit
Open the file in Power BI Desktop → provide data source path(s) if prompted.

Excel Model: Power BI - Final Project.xlsx
Used for measures/lookup sheets or direct visuals; keep paths consistent.

Report PDF: Data_Professional_survey_project.pdf
A static artifact showcasing the project in slide/report form.

📈 Suggested Analytical Queries (Post-Cleaning)

After running the cleaning pipeline, try these to “talk about the data”:

-- Average sale price by city
SELECT PropertySplitCity, AVG(SalePrice) AS AvgSalePrice
FROM portfolioproject..NashvilleHousing
GROUP BY PropertySplitCity
ORDER BY AvgSalePrice DESC;

-- Sales trend by year
SELECT YEAR(SaleDateConverted) AS SaleYear, COUNT(*) AS TotalSales
FROM portfolioproject..NashvilleHousing
GROUP BY YEAR(SaleDateConverted)
ORDER BY SaleYear;

-- Vacant vs Non-Vacant price differences
SELECT SoldAsVacant, AVG(SalePrice) AS AvgPrice
FROM portfolioproject..NashvilleHousing
GROUP BY SoldAsVacant;

📚 Data Provenance

Nashville Housing – public educational dataset commonly used for SQL cleaning practice; included here as a CSV snapshot.

COVID datasets – deaths and vaccinations time series (country/date level) included as CSV snapshots for exploration.

If you use refreshed sources, document their URLs and download dates in this section.

🛠️ Tech Stack

SQL Server / T-SQL

Power BI Desktop

Excel

Window functions, CTEs, string parsing, data type conversions, categorical normalization

🧩 Folder Roadmap / How to Extend

Add a Power BI report for the Nashville dataset (city-level KPIs, price distribution, YoY sales).

Parameterize COVID exploration queries to generate country dashboards.

Add a /reports folder with screenshots and narrative findings for recruiters.

✅ Outcomes

Clean, analysis-ready tables

Reusable SQL cleaning patterns for addresses, dates, and categories

Exploration queries illustrating vaccination and fatality trends

BI/Excel assets suitable for stakeholder reporting

🙋 About the Author

Aman Kumar

📧 [amanyadav880495@gmail.com](mailto:amanyadav880495@gmail.com)  
🔗 [LinkedIn – Aman Kumar](https://www.linkedin.com/in/aman-kumar-ba444225a/)  
💻 [GitHub – SKYTOX07](https://github.com/SKYTOX07)
