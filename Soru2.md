# 2-) 1980’den itibaren herhangi bir spor grubunda üst üste 3 veya daha fazla madalya almış atletleri bulalım.

```SQL
with athlet as(select 
  Athlete,year
from `dsmbootcamp.dogukan_nefis.summer_medals`
where year >= 1980
group by Athlete,year
order by Athlete,year ASC)

select *
from(
select Athlete,year,
      lead(year,1) over(partition by Athlete order by Athlete) as next_year,
      lead(year,2) over(partition by Athlete order by Athlete) as next_year2,
      lead(year,3) over(partition by Athlete order by Athlete) as next_year3,
      lead(year,4) over(partition by Athlete order by Athlete) as next_year4,
      lead(year,5) over(partition by Athlete order by Athlete) as next_year5,
from athlet)
where next_year-year=4 AND next_year2-next_year=4
```
