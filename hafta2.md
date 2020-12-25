
# SORU1


```SQL
--veri setini onden bir tanimak icin bunu yazdim
--ayrica with ... as komutu nasil calisiyo onu da anlamama yardimci oluyo basit seyler
--sql'i anlamakta bir miktar zorluk cekiyorum
-- bir de 1 3 5 konumlarinda ayni sayida madalya almis ulke var mi buna bakabiliyoruz
-- bu sayede row_number ya da dense_rank fonksiyonlari sikinti ciikarcak mi bunu da gorebiliyoruz
with medals as
(
  select
    year,
    country,
    event,
    medal,
    athlete
  from `dsmbootcamp.sample.summer_medals`
)

select country, event, count(*) from  medals
group by country, event

```
Sorunu cevabi icin burasi
```SQL
with medals as
(
  select
    q.country,
    q.Sport,
    count(*) as cnt,
    row_number() over(partition by q.Sport order by q.Sport , count(*) desc) as siralama 
  from `dsmbootcamp.bora_colakoglu.summer_medals` as q
  where year > 1979
  group by q.Country , q.Sport 
)

select Country, Sport, siralama, cnt from medals 
where siralama in (1,3,5)
order by Sport ,siralama
```

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


