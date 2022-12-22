# **Cause of Death in the USA**  
---

Rahma Ali, Madina Zhaksylyk

## **Project Description**

Our project will be based on causes of death in the United States from Cancer, 
Chronic Lower Respiratory Disease, Heart Disease, Stroke, and Unintentional Injury 
from the year 2010-2015.

We are interested to discover the following 

1. What causes the most deadly in the US?
2. Do Nonmetropolitan vs Metropolitan areas at the state level have more 
deaths?
3. Which age group, sex, race, and fixed benchmarks have more death 
rate?

# **Extracting data** 
---

The data were extracted from the following sources:

CSV File 

The CSV File was found on data.gov: NCHS - Potentially Excess Deaths from the Five
Leading Causes of Death.csv (Renamed as NCHS_Causes_of_Death_2005-2015.csv 
in our dataset).

Website/Source
https://catalog.data.gov/dataset?
q=Leading+Causes+of+Death+in+the+USA&sort=score+desc%2C+name+asc

API Call 
Used Census.gov to retrieve data on Demographic Characteristics Estimates by Age 
Groups.

Website/Source 
https://www.census.gov/data/developers/data-sets/popest-popproj/
popest.Vintage_2015.html#list-tab-2014455046
api.census.gov/data/2015/pep/charagegroups

# **Transformation**
---

Data collected from the public source were not published in the exact way we wanted. 
Therefore, we had to clean the data to meet our needs.
The data were cleaned using the following

1. Filters to filter out the columns we want for our data.

2. Renamed columns to match with both CSV file and API Call.

3. Checked for missing values.

4. Checked for duplicate values.

5. Joined both CSV File and API calls using inner joint on YEAR.

6. Replaced some of the API data values in the columns from numbers to words.

7. Deleted NAN values

8. dropped rows from Puerto Rico because we are only focusing on 50 States.

Renamed the original CSV file from NCHS - Potentially Excess Deaths from the Five 
Leading Causes of Death.csv to NCHS_Causes_of_Death_2005-2015.csv. The 
original CSV File contained the following columns: Year, Cause of Death, State, State
FIPS Code, HHS Region, Age Range, Benchmark, Locality, Observed Deaths, 
Population, Expected Deaths, Potentially Excess Deaths, Percent Potentially and 
Excess Deaths. Our new CSV File will contain the following columns: Year, Cause of 
Death, Benchmark, Locality (Metropolitan and Nonmetropolitan), Observed Deaths 
and Expected Deaths

We will not be using the following columns: FIPS Code, HHS Region, Potentially 
Excess Deaths, Percent Potentially, and Excess Deaths because those columns did 
not meet the requirement to answer the questions, we were interested in finding 
answers to.  We also not going to use State and Population data from the CSV File 
instead will use State and Population data from API Call Census.
For API call it came from JSON and converted to CSV File and save as 
Census_data.csv. The original columns were POP, HISP, RACE, SEX, DATE_, and state, 
columns were renamed to Population, Hispanic, Race, Sex, Year, and State.  When 
we pulled the data from API, some columns have values of 0, 1,.... etc To clean up, 
we used documentation found in the API to find what the numbers stand for. 
Replaced all the values in the columns to match our data formatting. Please refer to
our notebook for the formatting.

Our data will focus on the year 2010-2015 because we were interested in the last
5 years of the cause of death in the USA. We had options using different Benchmarks and age
ranges. For Benchmark will use Fixed instead of floating because floating changes 
from year to year and the age range will choose 0-84years we’re hoping to see more 
death in this category than any other age range. We read both CSV files and API Calls 
using pandas into Jupiter notebook. 
We merged both the CSV File and API call using Inner joint on Year.

# **Loading Data**
---

After transformation, ERD was created before loading data to the PostgreSQL 
database.

<img width="369" alt="erd_diagram" src="https://user-images.githubusercontent.com/111404552/209063656-bd02f2d4-d818-44fa-8275-91138e44bb23.png">


The cleaned data was then transferred into a Database. We created tables in our 
database to match with our Panda’s DataFrame in the Jupyter notebook. We used 
PG admin in Postgres to store our data.


`CREATE TABLE census_db(
id serial Primary Key,
Population FLOAT,
Hispanic TEXT,
Race TEXT,
Sex TEXT,
Year INTEGER,	
State TEXT
);

CREATE TABLE death_causes(
id serial Primary Key,
Year INTEGER,
Cause_of_Death TEXT,
Benchmark INTEGER,
Locality TEXT,
Age_Range INTEGER,
Observed_Deaths FLOAT,
Expected_Deaths FLOAT
);`
