## Global Medical Doctor Workforce Dashboard (WHO data)

An interactive Power BI dashboard that explores how the world's medical doctor workforce is distributed across 194 countries and how it has changed over time.

<img width="737" height="732" alt="image" src="https://github.com/user-attachments/assets/888de2bb-ed58-4985-b4eb-6c39c52e2133" />


Disclaimer: This is an independent portfolio project. It is not endorsed by, or affiliated with, the World Health Organization (WHO).

# Project goal

To practise the full analytics workflow, from a raw flat table to a clean data model and a finished report, using real public health data. The dashboard answers questions like:

How many doctors are there worldwide, and where are they?
Which regions and countries have the highest and lowest doctor density?
How has doctor density changed since 1990?

# Data source

World Health Organization, National Health Workforce Accounts (NHWA) data portal, December 2025 update. Portal: https://apps.who.int/nhwaportal

The dataset covers 194 countries, 5 indicators and years from 1971 to 2024:

Code	Indicator
HWF_0001	Medical doctors (per 10,000 population)
HWF_0002	Medical doctors (number)
HWF_0003	Generalist medical practitioners (number)
HWF_0004	Specialist medical practitioners (number)
HWF_0005	Medical doctors not further defined (number)

Data is reused with attribution, in line with WHO's terms for its datasets. See https://data.who.int for the current terms of use.

# My process step by step

# 1. Cleaned the data (Power Query)

Loaded one flat table with 14,814 rows and 34 columns.
Removed 25 columns that were empty, constant or duplicates of other columns, and kept the 9 that matter.
Created a staging query and built every table from it using Reference.

# 2. Built a star schema

Table	Type	Columns
fact_healthworkforce	Fact	CountryCode, IndicatorCode, Year, Value, IsLatestYear
dim_country	Dimension	CountryCode, Country, RegionCode, Region
dim_indicator	Dimension	IndicatorCode, IndicatorName, MeasureType, Category
Dim_Year	Dimension	Year, Decade

All relationships are one-to-many, with single-direction filtering from each dimension to the fact table. The grain of the fact table is one row per country, indicator and year.

<img width="657" height="631" alt="image" src="https://github.com/user-attachments/assets/df42f0a6-94c1-4189-993d-22fbfac69a6c" />


# 3. Wrote DAX measures

Key measures include:

Doctors (Total) =
CALCULATE(SUM(fact_healthworkforce[Value]), dim_indicator[IndicatorCode] = "HWF_0002")

Doctors (Latest) =
CALCULATE([Doctors (Total)], fact_healthworkforce[IsLatestYear] = TRUE())

Doctors per 10,000 (Latest) =
CALCULATE([Doctors per 10,000], fact_healthworkforce[IsLatestYear] = TRUE())

The "latest" measures use each country's most recent reported year, so values are never added up across years.

# 4. Designed the report

Global Doctors Overview: KPI cards, region and country slicers, a filled map, a regional bar chart, a trend line by region and a Top 10 chart.
Guide and info: where the data comes from, how it was prepared, key insights, how to read the dashboard, and limitations.

<img width="1167" height="722" alt="image" src="https://github.com/user-attachments/assets/66de959c-7615-49c2-9ae9-62055212bb92" />


# Key insights
The workforce is concentrated. China, India, the USA, Russia and Brazil hold about 51% of the 14.8 million doctors. China alone holds about 27%.
Africa has a very small share. It holds about 2.6% of all doctors. Its average density is about 3.6 per 10,000, compared with about 39.5 in Europe.
Many countries have low density. 70 of 194 countries (36%) have fewer than 10 doctors per 10,000 people.
Density is generally rising. For example, Africa's average rose from about 1.3 in 1990 to 3.5 in 2023, but regional gaps remain large.
The specialist share varies. Among countries that report both categories, specialists make up about 77% of generalist-plus-specialist doctors in Europe and about 24% in Africa.
Limitations
The "latest year" differs by country (2020 to 2024), so the figures are not a snapshot of a single year.
Fewer countries reported in 2024 (72) than in 2023 (117), so trend lines stop at 2023.
Trend lines are averages of the countries that reported in each year, and that mix changes over time.
Doctors per 10,000 cannot be summed across countries, so regional values are simple averages and not population-weighted.
Very small countries (such as Monaco and Andorra) can have unusually high rates.
Tools

# Power BI Desktop, Power Query, DAX, star schema data modelling.

# How to open the report
Download WHO_Medical_Workforce_Dashboard.pbix.
Open it in Power BI Desktop (Windows). It is free to download from Microsoft.
If the map shows a "disabled" message, go to File → Options and settings → Options → Security and enable Use Map and Filled Map visuals, then restart Power BI.

A PDF export of the report is also included: WHO_dashboard.pdf.

Repository contents
├── README.md
├── WHO_Medical_Workforce_Dashboard.pbix
├── WHO_dashboard.pdf
└── screenshots/
    ├── 01-dashboard.png
    ├── 02-info-page.png
    └── 03-star-schema.png
## About me

I'm Kristel, based in Tallinn, Estonia. I have around five years of experience in operations management and I'm moving into data analytics.

LinkedIn: <!-- add your link -->
Contact: <!-- add your email or leave out -->
