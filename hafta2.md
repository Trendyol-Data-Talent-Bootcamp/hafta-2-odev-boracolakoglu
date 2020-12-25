
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




