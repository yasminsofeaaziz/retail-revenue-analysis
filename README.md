# Project: Retail Sales & Performance Analysis

## Project Overview

This project provides an end-to-end data integration, modeling, and interactive reporting solution built with Power BI to analyze multi-year global retail transactions. By combining fragmented yearly sales datasets and integrating product and geographic reference data, the project consolidates transactional data across international markets, evaluates category-level revenue drivers (Beverages, Food, Pastries), measures annual target variance, and provides drill-through analysis for key regional markets.

## Dataset

- **Sales Transaction Files:** 4 yearly CSV files (`Sales-2021.csv`, `Sales-2022.csv`, `Sales-2023.csv`, `Sales-2024.csv`) containing 1,220,615 transaction records. Columns include `Order ID`, `Date`, `Product` (with embedded location/city info), `Country Code`, and `Amount`.
- **Products Listing (`Products Listing.txt`):** Tab-delimited text file serving as the product master list, containing `ID`, `Product` name, and product `Category` mapping (Beverages, Food, Pastries).
- **Countries Lookup (`Countries List.xlsx`):** Excel workbook containing ISO country code mappings (`Country Code` to full `Country` name).

## Tools & Methodology:

**Power BI Desktop:** Core tool for data modeling, DAX calculation development, interactive visualization design, and report publishing.

**Power Query (ETL):** Data extraction, text column parsing/extraction, queries merging, and normalization.

**DAX (Data Analysis Expressions):** Calculated tables, time-intelligence functions, dynamic measures, KPI variance logic, and calendar tables.

## Process & Data Pipeline:

1. **Data ETL & Text Parsing (Power Query):**
    - Appended and transformed 4 historical CSV sales records into a unified fact table totaling 1,220,615 orders.
    - Split embedded location data from product descriptions (e.g., extracting city names from string entries like `Chicken Fajita Wrap (Potsdam)`) to isolate clean product names for accurate lookup matching.
    - Imported and cleaned `Countries List.xlsx` and `Products Listing.txt`.

2. **Data Modeling & Query Merging:**
    - Merged queries to associate product category attributes with transactional records.
    - Defined custom single-directional 1-to-many relationships in Model View mode (disabling auto-detect) between dimension tables and fact tables.

3. **Dynamic Calendar Table (DAX):**
    - Generated a dynamic lookup date table covering the full span of transaction dates:
      ```
      Dates = CALENDAR(DATE(YEAR(MIN(Sales[Date])),1,1), DATE(YEAR(MAX(Sales[Date])),12,31))
      ```

4. **Data Hygiene & Measure Engineering:**
    - Hidden unnecessary surrogate keys, raw code fields, and underlying foreign key columns from Report View.
    - Built custom DAX measures including `Total Revenue`, `Count of Orders`, `Total Revenue (Weekday vs. Weekend)`, `Total Revenue Previous Year`, and `% Difference of Revenue`.
  
5. **Dashboard Design & User Experience:**
    - Configured report pages with cross-filtering interactions, dynamic year slicers, card visual KPIs, line graphs for monthly trends, and country drill-through report pages.

## Dashboard Features & Key Insights:

1. **Summary Page**
    - **High-Level Metrics:** Total global revenue reached RM4,272,199 across 1,220,615 total orders.
    - **Category Distribution:** Sales are heavily driven by Beverages (RM1.71M) and Food (RM1.70M), which combined account for ~80% of total revenue, while Pastries generated RM0.86M.
    - **Geographic Revenue:** Global revenue distribution map highlights key high-performing markets across Europe, North America, and Oceania.

2. **Yearly Variance Analysis**
    - **Target Tracking (2024 KPI):** 2024 total revenue reached RM1,396.4K, outperforming the set goal of RM1,335.8K by +4.54% across 399,100 orders.
    - **Monthly Revenue Trajectory:** Identified noticeable seasonal troughs in February (RM108.3K) and June (RM114.2K), with peaks occurring in March (RM118.9K), August (RM118.6K), and October (RM118.3K).

3. **Analysis by Country (e.g. USA analysis)**
    - **Regional Breakdown:** The USA market generated RM802,181 in total revenue across 229,245 orders.
    - **City Performance:** Top city locations include Boston (RM90,280.77), Kirkland (RM90,078.46), and Seattle (RM89,896.24).
    - **Top Product Volume:** Green Tea (RM32,844.67; 9,379 orders), Apple Juice (RM32,706.36; 9,321 orders), and Still Water (RM32,705.27; 9,340 orders) lead product volume in the region.

## Recommendations:

1. **Cross-Selling & Bundling:** Combine low-volume Pastries with top-selling Beverages or Food items (e.g., breakfast coffee and pastry bundles) to lift average order value.
2. **Mitigate Seasonal Dips:** Implement targeted promotional campaigns or seasonal menu offerings during mid-quarter slow months (specifically February and June) to stabilize cash flow.
3. **Regional Inventory Optimization:** Direct marketing budgets and supply chain focus toward top-performing urban hubs (e.g., Boston, Kirkland, Seattle) while expanding local distribution in lower-volume cities.

## Limitations:

- **Cost & Margin Exclusions:** The current sales dataset captures top-line gross revenue only and lacks Cost of Goods Sold (COGS), prevent direct net profit margin analysis.
- **Unstructured Location Strings in Source Data:** Raw sales CSV files stored city names within product description text strings, requiring transformation during ETL preprocessing.
