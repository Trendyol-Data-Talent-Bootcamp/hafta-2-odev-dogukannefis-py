# 1-) 1980’den itibaren Spor grubu bazında en çok madalya alan 1. 3. 5. ülkeyi bulalım.
```SQL
with medal_cnt as(
  select
    country,Event,
    count(*) as medal_count
  from `dsmbootcamp.dogukan_nefis.summer_medals`
  where year >= 1980
  group by Event,country
  order by Event,count(*) DESC
  )
  select country,Event,medal_count,Rank
  from(
  select country,Event,medal_count,
        ROW_NUMBER() OVER(PARTITION BY Event ORDER BY medal_count DESC) AS Row_number,
        RANK() OVER(PARTITION BY Event ORDER BY medal_count DESC) AS Rank
  from medal_cnt
  )
  where Rank in (1,3,5)
```
