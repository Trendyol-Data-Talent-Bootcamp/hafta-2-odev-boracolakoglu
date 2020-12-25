# Soru2

``` SQL
/*
http://www.dba-oracle.com/t_advanced_sql_windowing_clause.htm
*/

with athl as
(
  select
    q.Athlete,
    q.Sport,
    q.year,
    count(*) over(partition by q.Athlete order by q.Year rows between unbounded preceding and unbounded following) as smth, --burda niye rows ekledik tam olarak anlamadim
    first_value(q.year) over(partition by q.Athlete  order by year) as first_champ,
    lead(year,1) over(partition by q.Athlete  order by year) as second_champ,
    lead(year,2) over(partition by q.Athlete  order by year) as third_champ
    --ustteki first second third kismini hafta sonu yazdigimiz koddan kopyaladim
  from `dsmbootcamp.bora_colakoglu.summer_medals` as q
  where year > 1979
  group by q.Athlete, q.Year, q.Sport
  order by q.Athlete 
)
select * from athl
where second_champ is not null and third_champ - first_champ = 8

```
