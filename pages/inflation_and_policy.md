---
title: Relationships between Inflation and Price/Labour Market variables
---

## Key Insights

### Brazil raised Central Bank (CB) rates early and Inflation dropped
### Argentina raised CB rates, but inflation kept on rising
### Perhaps this shows that consumer spending is very different between both countries?

```sql base_price_labour_variables_by_year_by_country
with base as (
    select
        countryname as country
        , year
        , cpi
        , 100.0 * ((cpi / lag(cpi) over (partition by countryname order by year)) - 1) as cpi_yoy
        , hpi
        , infl
        , unemp
        , cbrate
    from gmd
    where year between 2000 and 2025
        and countryname in (
        'Argentina'
        , 'Brazil'
        , 'Chile'
        , 'Colombia'
        , 'Mexico'
        , 'United States'
        , 'United Kingdom'
        , 'Japan'
        , 'Germany'
        , 'Turkey'
        , 'India'
        , 'South Africa'
    )
)

select *
from base
```

<Dropdown
  name=category_multi_selectAllByDefault
  data={base_price_labour_variables_by_year_by_country}
  value=country
  title="Select Countries"
  multiple=true
  defaultValue={'Argentina'}
/>

```sql price_labour_variables_by_year_by_country
select *
from ${base_price_labour_variables_by_year_by_country}
where country in ${inputs.category_multi_selectAllByDefault.value}
order by country, year
```

## Variables in last 25 years

<LineChart 
    data={price_labour_variables_by_year_by_country}
    x=year
    y='infl'
    y2='unemp'
    yAxisTitle="Inflation"
    y2AxisTitle="Unemployment"
/>

<LineChart 
    data={price_labour_variables_by_year_by_country}
    x=year
    y='infl'
    y2='cbrate'
    yAxisTitle="Inflation"
    y2AxisTitle="Central Bank Rate"
/>

<LineChart 
    data={price_labour_variables_by_year_by_country}
    x=year
    y='infl'
    y2='HPI'
    yAxisTitle="Inflation"
    y2AxisTitle="HPI"
/>
