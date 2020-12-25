# Soru3

```SQL
select * from `dsmbootcamp.bora_colakoglu.pageview`  limit 150;
-- ustteki kodu yine ufak bir goz gezdirmek icin yazdim


with page_view
as
(
select
timestamp_trunc(view_ts , minute) as view_time, --dakika seklinde ayarladim
approx_count_distinct(deviceid) as approx_user_count -- dakikadaki uniqe kullanici sayisi
from `dsmbootcamp.bora_colakoglu.pageview` 
group by view_time 
order by view_time 
)

select view_time,
approx_user_count,
sum(approx_user_count) over(order by view_time rows between 4 preceding and 0 following) as approx_real_user_count, -- her dakikave onceki 4 dakikanin toplami
from page_view 
order by approx_real_user_count desc;
```
