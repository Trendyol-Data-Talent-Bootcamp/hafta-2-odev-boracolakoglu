
#SORU1


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









