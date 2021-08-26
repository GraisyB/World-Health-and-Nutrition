## Exploring the data

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

## Business Related Questions

* 
* 
