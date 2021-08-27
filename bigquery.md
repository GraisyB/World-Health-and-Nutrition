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
  4.  Average data is given against country_name 'World'<br><br>
**_To ensure correctness in analysis, we will only focus on South Asian countries here.<br>
The countries will be 'Afghanistan', 'Bangladesh', 'Bhutan', 'India', 'Nepal', 'Pakistan', 'Sri Lanka' and 'Maldives'_**

* **Various indicators we can obtain from this dataset**
```sql
SELECT count(distinct indicator_name) FROM `bigquery-public-data.world_bank_health_population.health_nutrition_population`
```
There are 425 distinct indicators.


* **Time (Year) range**
```sql
SELECT country_name, min(year) as MinYear,max(year) as MaxYear FROM `bigquery-public-data.world_bank_health_population.health_nutrition_population` group by country_name
```
![image](https://user-images.githubusercontent.com/87647811/131004880-c76abc4b-38c3-4b0d-a8b3-750182b749a9.png)<br>
The data for each country ranges from year 1960 to 2020

# Healthcare, Population and Literacyin Women in South Asia!

## Healthcare 
#### Q1. What is the minimum, maximum and average Domestic general government health expenditure (% of general government expenditure) per country since year 2000?
```sql
SELECT country_name, min(value) as MinExp,max(value) as MaxExp,avg(value) as AvgExp
FROM `bigquery-public-data.world_bank_health_population.health_nutrition_population` 
where indicator_name='Domestic general government health expenditure (% of general government expenditure)' and year >= 2000
and country_name in ('Afghanistan', 'Bangladesh', 'Bhutan', 'India', 'Nepal', 'Pakistan', 'Sri Lanka', 'Maldives')
group by country_name
order by avg(value) desc
```
The government health expenditure in South Asia is the highest in Maldives. 
![image](https://user-images.githubusercontent.com/87647811/131183099-c1209654-5272-4af2-acc3-3dacfc782c24.png)

#### Q2. In which year(s) does India have the highest of the following?
* Community health workers (per 1,000 people)
* Hospital beds (per 1,000 people)
* Physicians (per 1,000 people)
* Specialist surgical workforce (per 100,000 population)
```sql
WITH MaxIndia as (SELECT indicator_name, max(value) as value
from `bigquery-public-data.world_bank_health_population.health_nutrition_population` 
where 1=1
    and indicator_name in ('Community health workers (per 1,000 people)','Hospital beds (per 1,000 people)','Physicians (per 1,000 people)','Specialist surgical workforce (per 100,000 population)')
    and country_name = 'India'
group by indicator_name)

select b.indicator_name, b.country_name,b.value, b.year
from MaxIndia m
join `bigquery-public-data.world_bank_health_population.health_nutrition_population` b
on b.value = m.value 
    and country_name = 'India'
    and b.indicator_name = m.indicator_name
```
![image](https://user-images.githubusercontent.com/87647811/131174978-9d435d62-f9fe-4ae3-ba42-63d0dda158f7.png)

## Population
#### Q3. In which year did the South Asian countries have the highest Population growth (annual %)?
```sql
With population as (SELECT distinct country_name, max(value) as MaxPopulationGrowth
from `bigquery-public-data.world_bank_health_population.health_nutrition_population` 
where 1=1
    and indicator_name = 'Population growth (annual %)'
    and country_name in ('Afghanistan', 'Bangladesh', 'Bhutan', 'India', 'Nepal', 'Pakistan', 'Sri Lanka','Maldives')
group by country_name)

select distinct b.country_name,p.MaxPopulationGrowth, b.year
from population p
join `bigquery-public-data.world_bank_health_population.health_nutrition_population` b
on b.value = p.MaxPopulationGrowth 
order by p.MaxPopulationGrowth desc
```
![image](https://user-images.githubusercontent.com/87647811/131176800-1e8158f8-f051-4230-b54e-14b2156d4b63.png)

#### Q4. List the 5 countries with highest female population
```sql
SELECT country_name, avg(value) as AvgFemalePopulation
from `bigquery-public-data.world_bank_health_population.health_nutrition_population` 
where 1=1
    and indicator_name = 'Population, female (% of total population)'
group by country_name
order by AvgFemalePopulation desc
limit 5
```
![image](https://user-images.githubusercontent.com/87647811/131177910-a2f176c0-478b-4f58-be9e-8b668fcbd7c7.png)<br>
Similarly, female population in South Asian countries is as follows.<br>
![image](https://user-images.githubusercontent.com/87647811/131183441-25d988f2-0d7e-4389-92a1-d0c96f6a9b59.png)<br>
This indicates, that South Asia has a nearly balanced population.
  
#### Q5. List the 5 countries with highest Male population
```sql
SELECT country_name, avg(value) as AvgMalePopulation
from `bigquery-public-data.world_bank_health_population.health_nutrition_population` 
where 1=1
    and indicator_name = 'Population, male (% of total population)'
group by country_name
order by AvgMalePopulation desc
limit 5
```
![image](https://user-images.githubusercontent.com/87647811/131178146-7d932f8c-e6f9-459f-8ebd-8f6affc04b7e.png)

## Literacy
#### Q6. How much has been the average Public spending on education as a % of GDP in South Asia from year 2000 onwards.
```sql
SELECT country_name, avg(value) as AvgEducationSpending
from `bigquery-public-data.world_bank_health_population.health_nutrition_population` 
where 1=1
    and indicator_name = 'Public spending on education, total (% of GDP)'
    and country_name in ('Afghanistan', 'Bangladesh', 'Bhutan', 'India', 'Nepal', 'Pakistan', 'Sri Lanka','Maldives')
    and year >= 2000
group by country_name
order by AvgEducationSpending desc
```
![image](https://user-images.githubusercontent.com/87647811/131180698-354932b5-ea9c-454d-88f9-febd07e425ed.png)

#### Q7. How does Literacy rate in adult female in South asia compare to total literacy rate
```sql
SELECT country_name, avg(value) as FemaleLiteracy
from `bigquery-public-data.world_bank_health_population.health_nutrition_population` 
where 1=1
    and indicator_name = 'Literacy rate, adult female (% of females ages 15 and above)'
    and country_name in ('Afghanistan', 'Bangladesh', 'Bhutan', 'India', 'Nepal', 'Pakistan', 'Sri Lanka','Maldives')
    and year >= 2000
group by country_name
order by FemaleLiteracy desc
```
```sql
SELECT country_name, avg(value) as TotalLiteracy
from `bigquery-public-data.world_bank_health_population.health_nutrition_population` 
where 1=1
    and indicator_name = 'Literacy rate, adult total (% of people ages 15 and above)'
    and country_name in ('Afghanistan', 'Bangladesh', 'Bhutan', 'India', 'Nepal', 'Pakistan', 'Sri Lanka','Maldives')
    and year >= 2000
group by country_name
order by TotalLiteracy desc
```
![image](https://user-images.githubusercontent.com/87647811/131181303-56044aed-13a5-42db-989d-86e192b113a8.png)
![image](https://user-images.githubusercontent.com/87647811/131181463-6c99403d-39d9-4238-9641-f2fb0b20b322.png)

#### Q8. Female headed households (% of households with a female head)
```sql
SELECT country_name, avg(value) as FemaleHeadedFamilyPercentage
from `bigquery-public-data.world_bank_health_population.health_nutrition_population` 
where 1=1
    and indicator_name = 'Female headed households (% of households with a female head)'
    and country_name in ('Afghanistan', 'Bangladesh', 'Bhutan', 'India', 'Nepal', 'Pakistan', 'Sri Lanka','Maldives')
    and year >= 2000
group by country_name
order by FemaleHeadedFamilyPercentage desc
```
![image](https://user-images.githubusercontent.com/87647811/131182121-d5db20ec-2adf-4bdc-babb-72a676f01a2e.png)

### ------ The End -------

