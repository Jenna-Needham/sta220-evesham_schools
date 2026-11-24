# STA 220 Dashboard: Evesham Township Schools

## What is this?

This project is an interactive dashboard examining educational outcomes across the six elementary schools in Evesham Township, New Jersey (Marlton). 

The dashboard compares schools using data from the New Jersey Department of Education, including mathematics and ELA proficiency, and student growth. It also aims to incorporate neighborhood and housing information to provide additional context in order to understand the differences across schools. 

The intended audience is Evesham Township parents/families interested in understanding local elementary schools and which areas would be best to live in in order to foster growth among their young children. The dashboard is intended to support comparisons and inform discussions about educational outcomes. 

## Where did the data come from? 

### Source 1: New Jersey Department of Education

Agency: New Jersey Department of Education (NJDOE) 

Dataset: 2024-2025 School Performance Reports Database

Data year: 2024-2025 

Files downloaded: 
- School-level database (Excel)
- District/State-level database (Excel)

Downloaded: 11/2/2026

Source: [NJDOE School Performance Reports - Downloadable Data] (https://www.nj.gov/education/spr/download/). 

The School database provides school-level information used to compare the six elementary schools. The District/State database provides additional data for comparison with broader benchmarks for education. 

### Source 2: U.S. Census Bureau (if used) 

Agency: U.S. Census Bureau 

Dataset: 2024 American Community Survey (ACS) 5-Year Estimates (2020-2024)

Data year: 2024

Data Collection: Census API accessed through the 'tidycensus' package in R

Downloaded: 11/2/2026

Sources: 
- [U.S. Census Bureau - data.census.gov] (https://data.census.gov/). 
- [U.S. Census Bureau - 2024 ACS 5-Year Data] (https://www.census.gov/data/developers/data-sets/acs-5year/2024.html).
- [Census API Key Registration] (https://api.census.gov/data/key_signup.html).

The U.S. Census Bureau is the source of the original data. Any analysis, interpretations, or conclusions drawn from these data are the sole responsibility of the project author and do not necessarily represent the views of the U.S. Census Bureau.

## Accessing the Data 

The original NJDOE Excel files are too large to be stored directly in this GitHub repository. 

To use the data for the project, download the 2024-2025 Performance Reports Database in Excel format from the NJDOE Downloadable Data page. Download both the School and District/State files. 

After downloading, place each file in its corresponding folder in the project directory: 
- 'data/njdoe_2024-2025_school/'
- 'data/njdoe_2024-2025_district_state/'

The original Excel files are storied locally and not uploaded to GitHub. When using the repository on another computer, these files must be downloaded again and placed in the correct folders before running the import script. 

The tables used were B19013, B25

## How do I run the project? 

1. Download the required sources and place them in the 'data/' folders
2. Open the '.Rproj' file in RStudio
3. Run the 'scripts/01_import.R' to import and prepare the datasets
4. Open 'app.R' and run the Shiny application to view the dashboard.

### Census API Key Setup

The ACS download portion of the import script requires a Census API key 

Before running the ACS data retrieval code: 
1. Obtain a Census API key from the [U.S. Census Bureau API Key Registration Page] (https://api.census.gov/data/key_signup.html).
2. Register the key in R using the following commands in the CONSOLE:

library(tidycensus) 
census_api_key("YOUR_API_KEY_HERE") 

3. Restart RStudio before running the import script 

## Manually Preparing the Data

The following changes were made manually to prepare the NJDOE data for use in R. 

### ELA Data
- Used the 'ELAPerformanceTest' worksheet
- Removed the first three rows containing introductory/header information
- Removed the final row which indicated the end of the worksheet
- Changed all columns to lower snakecase (i.e. School Code to school_code)
- Filtered the data to include only the 2024-2025 school year
- Filtered the data to include only grades 3, 4, and 5
- Filtered the data to include only Evesham Township School District and its six elementary schools
- Removed percentage signs and converted them to decimals (i.e. 75% to 0.75)

### Math Data
- Used the 'MathPerformanceTest' worksheet
- Removed the first three rows containing introductory/header information
- Removed the final row which indicated the end of the worksheet
- Changed all columns to lower snakecase (i.e. School Code to school_code)
- Filtered the data to include only the 2024-2025 school year
- Filtered the data to include only grades 3, 4, and 5
- Filtered the data to include only Evesham Township School District and its six elementary schools
- Removed percentage signs and converted them to decimals (i.e. 75% to 0.75)

### Student Growth Trends Data
- Used the 'MathPerformanceTest' worksheet
- Removed the first three rows containing introductory/header information
- Removed the final row which indicated the end of the worksheet
- Changed all columns to lower snakecase (i.e. School Code to school_code)
- Filtered the data to include only the 2024-2025 school year
- Filtered the data to include only grades 3, 4, and 5
- Filtered the data to include only Evesham Township School District and its six elementary schools

## ACS Data

The ACS data was retrieved and prepared in R using the 'tidycensus' package. The ACS did not need to be edited in Excel beforehand. It was cleaned using R code

### Selecting Variables

Three variables were used for the analysis 
- 'B19013_001': Median household income
- 'B25077_001': Median value of owner-occupied housing units
- 'B25064_001': Median gross rent

These variables were stored in a vector named 'acs_variables' and passed into each 'get_acs()' function. The 'output = "wide"' argument was used to return said variables into wide format 

### Geographic Data

Four separate datasets were retrieved using 'get_acs()'. Each dataset used the 2024 ACS five-year estimates ('year = 2024', 'survey = 'acs5"'). 

**1. New Jersey** 
- Geography: State
- State Identifier: NJ
- Retrieved the selected ACS variables for NJ
- Added a 'Geography' column identifying the dataset as "New Jersey"

**2. Burlington County** 
- Geography: County
- State: NJ
- GEOID: 34005
- Filtered the county dataset to keep only Burlington County
- Added a 'Geography' column to identify the dataset as "Burlington County".

**3. Evesham Township** 
- Geography: County subdivision
- State: NJ
- County: Burlington
- GEOID: 3400522110
- Filtered the county subdivision dataset to keep only Evesham Township
- Added a 'Geography' column to identify the dataset as "Evesham Township"

**4. Marlton CDP** 
- Geography: Place
- State: NJ
- GEOID: 3444100
- Filtered the place dataset to keep only Marlton Census-Designated Place (CDP)
- Added a 'Geography' column to identify the dataset as "Marlton CDP". 
