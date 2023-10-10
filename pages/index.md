---
title: Life Expectancy Survey
---

_Presented by Rachael Garozzo_

This report is based on data from [ourworldindata.org](https://ourworldindata.org/life-expectancy)


Of special interest to me in the data for this study is the non-country data which includes groupings by economics, industrialization, and large land masses. Let's take a look at the most recent year's data for these categories.

## Large Land Masses and/or Continent data for 2021

<BarChart
  data={life_expect_continent}
  x=entity
  y=life_expectancy  
  title = "Life Expectancy By Year: Continents">
</BarChart>

The results of this query find that Oceania has the highest life expectancy. My personal thought had been that Europe would have the highest value, so this was a bit of a surprise.

## Industrialization data for 2021

<BarChart
  data={life_expect_industrialization}
  x=entity
  y=life_expectancy  
  title = "Life Expectancy By Year: Industrialization">
</BarChart>

The result that more developed regions have the highest life expectancy average is not surprising, but to me seeing that small island developing states are not far behind was interesting. It raises questions as to what contributes to that. 

## Economic data for 2021

<BarChart
  data={life_expect_economic}
  x=entity
  y=life_expectancy  
  title = "Life Expectancy By Year: Economic">
</BarChart>

As expected, the life expectancy based on economic levels goes from high to low based on economics.


## Zambia

The dive into the country data to find the country with the longest continuous period of life expectancy decline found Zambia to be a country with a 21 year period of decline that ended at the end of the 1990s. The length and dates of that decline surprised me and made me wonder what contributed to it.

### The Cause

A quick internet search pointed out that millions of Zambians succumb to premature deaths due to many factors: HIV/AIDS, malaria, tuberculosis, and poverty.


The life expectancy for Zambia in 2021 was <Value data = {life_expect_Zambia_2021} column = life_expectancy /> years, a <Value data = {life_expect_Zambia_Chg} column=Chg fmt='0.00%' /> decrease from 2020.

### 2021 Life Expectancy

For reference, below is a table that shows life expectancy values in many countries.

<DataTable data={life_expect_country} rows=10/>


## SQL data sources using Code Fences

```sql life_expect_Zambia_2021
select  entity, year, life_expectancy
from    life_expectancy
where   code = 'ZMB' and year = 2021
```

```sql life_expect_Zambia_Chg
with CTE_2021 as
(
select  life_expectancy Curr
from    life_expectancy
where   code = 'ZMB' and year = 2021
),
CTE_2020 as
(
select  life_expectancy Prev 
from    life_expectancy
where   code = 'ZMB' and year = 2020
)

select (C.Curr-P.Prev)/P.Prev Chg from CTE_2020 P, CTE_2021 C;
```

```sql life_expect_country
select  entity, year, life_expectancy
from    life_expectancy
where   code is not null and year = 2021
order by entity, year
```

<!--- contents of continent table:
Africa
Americas
Asia
Europe
Northern America
Oceania
Latin America and the Caribbean -->
```sql life_expect_continent
select  le.entity, le.year, le.life_expectancy
from    life_expectancy le join continent cn on le.entity = cn.entity
where   le.year = 2021
order by le.life_expectancy desc;
```

<!--- contents of economic table:
High-income countries
Low-income countries
Lower-middle-income countries
Upper-middle-income countries -->
```sql life_expect_economic
select  le.entity, le.year, le.life_expectancy
from    life_expectancy le join economic ec on le.entity = ec.entity
where   le.year = 2021
order by le.life_expectancy desc;
```

<!--- contents of industrialization table:
Land-locked developing countries (LLDC)
Least developed countries
Less developed regions
More developed regions
Small island developing states (SIDS)
Less developed regions, excluding China
Less developed regions, excluding least developed countries -->
```sql life_expect_industrialization
select  le.entity, le.year, le.life_expectancy
from    life_expectancy le join industrialization ind on le.entity = ind.entity
where   le.year = 2021
order by le.life_expectancy desc;
```


