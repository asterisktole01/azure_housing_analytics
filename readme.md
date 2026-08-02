# Philippine Foreclosure Affordability Analysis — Azure ETL Pipeline

## Overview
An end-to-end Azure data engineering project analyzing which income brackets in the Philippines can afford to own a home, using foreclosed property listings scraped from major Philippine banks. The project follows a medallion architecture (Bronze → Silver → Gold) and delivers insights through an interactive Power BI dashboard.

## Business Question
Can Filipino families afford to own a foreclosed house — segmented by bank, city, property type, and region?

## Architecture
- **Ingestion:** Azure Data Factory (ADF) for orchestration
- **Storage:** Azure Data Lake Storage Gen2 (ADLS Gen2), organized in a medallion layout
- **Transformation:** Databricks (PySpark) for cleaning, standardization, and modeling
- **Presentation:** Power BI dashboard for interactive analysis

## Data
- ~30,000 foreclosed property listings scraped from 13 Philippine banks
- Custom Python scraper (Jupyter-based) with checkpoint/resume logic and browser-mimicking headers to handle large-scale, resilient scraping
- Supplementary data: Philippine Standard Geographic Code (PSGC) for geography, and family income data by region

## Medallion Layers

**Bronze**
- Raw scraped listings loaded via SQL Server, with data quality issues resolved (embedded newlines, UTF-8 currency symbol encoding, bulk insert failures)

**Silver**
- Cleaned and standardized listings
- Data quality issues addressed: duplicate source columns, null price-per-sqm values, inconsistent floor/lot area fields, uniform low-cardinality fields dropped from modeling

**Gold (Star Schema)**
- `dim_geography` — conformed geography dimension from PSGC
- `housing_fact` — listing-grain fact table (includes days-listed, first-seen date)
- `fact_household` — geography-grain family income data
- `fact_affordability` — geography-grain, comparing Pag-IBIG vs. bank loan affordability side-by-side


## Output
A Power BI dashboard layered on the Gold data, allowing users to explore housing affordability by:
- Bank
- City / Region
- Property type
- Loan term scenario (20 vs. 30 years)

## Skills Demonstrated
Azure Data Factory, ADLS Gen2, Databricks (PySpark), medallion architecture, dimensional/star schema design, data quality troubleshooting, web scraping, Power BI
