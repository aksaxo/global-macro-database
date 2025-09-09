---
title: Global Inflation Hotspots
---

## Where else in the world is experiencing high inflation?

## Key Insights

Ignoring outliers like Venezuela and Sudan, we see that Middle Eastern countries like Turkey and Iran experience the most inflation behind Argentina.
It is then African countries like Nigeria, Ghana, and Ethiopia that experience the next highest amount of inflation

```sql base_inflation_by_country
with base as (
  select
    countryname as country,
    avg(infl) as avg_yearly_inflation
  from gmd
  where year between 2019 and 2025
    and pop > 20 and countryname not in ('Venezuela', 'Sudan')
  group by 1
),
yoy as (
  select
    country,
    avg_yearly_inflation
  from base
)

select * from yoy
order by 2 desc limit 20
```

## Countries by Avg Yearly Inflation past 5 years
##### With population greater than 20 million

<BarChart 
    data={base_inflation_by_country}
    x=country
    y=avg_yearly_inflation
    swapXY=true
/>