#Soru 2) 1980’den itibaren herhangi bir spor grubunda üst üste 3 veya daha fazla madalya almış atletleri bulalım.
```SQL
select distinct(c.athlete)
from
(
select a.athlete as athlete,a.year, b.year as year2
from
(
select  athlete, year from buse_senvardar.summer_medals
where athlete is not null
group by athlete, year
) as a
inner join 
(
select  athlete, year from buse_senvardar.summer_medals
where athlete is not null
group by athlete, year
) as b
on a.athlete=b.athlete and a.year+4=b.year ) as c
inner join
(
select  athlete, year from buse_senvardar.summer_medals
where athlete is not null
group by athlete, year
) as d
on c.athlete=d.athlete and c.year2+4=d.year
order by c.athlete
```
