---
title: Inflation in Latin America
description: CPI YoY for selected LATAM countries using the Global Macro Database
---

#### Inflation Trends Since 2015
<LastRefreshed prefix="Data last updated"/>

## Key Insights

### Argentina's inflation is significantly higher than that of other Latin American countries.

### Latin American countries experience noticeably higher inflation compared to more 'stable' countries/currencies like the US and UK

### But still similar to more 'middling' economies like India and South Africa, with Turkey as an exception


```sql base_inflation_by_country
with base as (
  select
    countryname as country,
    year,
    cpi,
    infl
  from gmd
  where year between 2014 and 2025
    and countryname in ('Argentina', 'Mexico', 'Colombia', 'Brazil', 'Chile', 'United States', 'United Kingdom', 'Germany', 'Japan', 'Turkey', 'South Africa', 'India')
),
yoy as (
  select
    country,
    year,
    infl,
    cpi,
    100.0 * (cpi / lag(cpi) over (partition by country order by year) - 1) as cpi_yoy
  from base
)

select * from yoy
```

<Dropdown
  name=category_multi_selectAllByDefault
  data={base_inflation_by_country}
  value=country
  title="Select Countries"
  multiple=true
  selectAllByDefault=true
/>

```sql inflation_by_country
select * from ${base_inflation_by_country}
where cpi_yoy is not null
  and country in ${inputs.category_multi_selectAllByDefault.value}
order by country, year
```


## Inflation in last 10 years

<LineChart 
    data={inflation_by_country}
    x=year
    y=infl 
    yAxisTitle="Inflation"
    series=country
/>

## CPI YoY change by Country
#### Since 2015

<Heatmap
  data={inflation_by_country}
  x=year
  y=country
  value=cpi_yoy
/>
