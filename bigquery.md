# Exploring the data

* **Checking the columns in the data set**
```sql
SELECT * FROM `bigquery-public-data.world_bank_health_population.health_nutrition_population` LIMIT 100
```
![image](https://user-images.githubusercontent.com/87647811/130986752-cd4a6d2b-87b1-4689-ae1b-13cb665ee147.png)


* **No. of countries in the data set and what are the countries**
```sql
SELECT count(distinct country_name) FROM `bigquery-public-data.world_bank_health_population.health_nutrition_population`
```
260 is the result. (But there are only 195 countries at present)
Few reasons are,
  1.  This is because large countries have been divided by regions. Like 'Africa eastern and southern' is considered as one region, north and west are another.
  2.  Combined areas like 'Arab World' and 'Central Europe and the Baltics'
  3.  Segregation based on income is also done like 'Europe and central asia (excluding high income)'
  4.  Average data is given against country_name 'World'


* **Various indicators we can obtain from this dataset**
```sql
SELECT count(distinct indicator_name) FROM `bigquery-public-data.world_bank_health_population.health_nutrition_population`
```
There are 425 distinct indicators.


* **Time (Year) range**
```sql
SELECT country_name, min(year) as MinYear,max(year) as MaxYear FROM `bigquery-public-data.world_bank_health_population.health_nutrition_population` group by country_name
```
![image](https://user-images.githubusercontent.com/87647811/131004880-c76abc4b-38c3-4b0d-a8b3-750182b749a9.png)
The data for each country ranges from year 1960 to 2020

# How is India doing in terms of Healthcare, Population, Literacy and Sanitation

## Healthcare
* Which country has the highest of the following in the year 2000
Community health workers (per 1,000 people)
Hospital beds (per 1,000 people)
Physicians (per 1,000 people)
Specialist surgical workforce (per 100,000 population)
Domestic general government health expenditure (% of general government expenditure)
Adolescent fertility rate (births per 1,000 women ages 15-19)

## Population
Population growth (annual %)
Net migration
Population, female (% of total population)
Population, male (% of total population)

## Literacy
Public spending on education, total (% of GDP)
Literacy rate, adult female (% of females ages 15 and above)
Literacy rate, adult male (% of males ages 15 and above)
Literacy rate, adult total (% of people ages 15 and above)
Female headed households (% of households with a female head)
Labor force, female (% of total labor force)
Labor force, total

* Sanitation
People practicing open defecation (% of population)
People practicing open defecation, rural (% of rural population)
People practicing open defecation, urban (% of urban population)
Mortality rate attributed to unsafe water, unsafe sanitation and lack of hygiene (per 100,000 population)
Mortality rate attributed to unsafe water, unsafe sanitation and lack of hygiene, female (per 100,000 female population)
Mortality rate attributed to unsafe water, unsafe sanitation and lack of hygiene, male (per 100,000 male population)
People using at least basic drinking water services (% of population)
People using at least basic drinking water services, rural (% of rural population)
People using at least basic drinking water services, urban (% of urban population)

