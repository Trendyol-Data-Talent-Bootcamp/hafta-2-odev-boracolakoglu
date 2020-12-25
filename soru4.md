# Soru 4

```SQL

create or replace table `dsmbootcamp.bora_colakoglu.content_category`
as 
select * from `dsmbootcamp.sample.content_category`;

create or replace table `dsmbootcamp.bora_colakoglu.content_category_20201222_00_59`
as 
select * from `dsmbootcamp.sample.content_category_20201222_00_59`;

create or replace table `dsmbootcamp.bora_colakoglu.content_category_target`
as 
select * from `dsmbootcamp.sample.content_category_target`; 

/*
select * from  `dsmbootcamp.bora_colakoglu.content_category` limit 50;

select * from `dsmbootcamp.bora_colakoglu.content_category_20201222_00_59` limit 50;

select * from `dsmbootcamp.bora_colakoglu.content_category_target` limit 50;

bunlar da yine veriye bir goz gezdirmek icin
*/


/*
create or replace table `dsmbootcamp.bora_colakoglu.content_category` 
as
(
select 
case
  when b.cdc_date = a.cdc_date then b.cdc_date
  else a.cdc_date
  end as cdc_date,
case
  when b.is_deleted = a.is_deleted then b.is_deleted
  else a.is_deleted
  end as is_deleted,
case
  when b.id = a.id then b.id
  else a.id
  end as id,
case
  when b.category = a.category then b.category
  else a.category
  end as category
from `dsmbootcamp.bora_colakoglu.content_category` b
right join `dsmbootcamp.bora_colakoglu.content_category_20201222_00_59` a
on b.id = a.id
);


BUNUN NEDEN CALISMADIGINA DAIR HIC BIR FIKRIM YOK 
AMA SUREKLI OLARAK 55 TANE NULL DEGER KALIYO
JOINLE YAPMAK ISTEMISTIM AMA MECBUR MERGE ILE YAPICAM
*/

merge dsmbootcamp.bora_colakoglu.content_category as t
using `dsmbootcamp.bora_colakoglu.content_category_20201222_00_59` s
ON t.id = s.id
WHEN MATCHED AND t.cdc_date != s.cdc_date THEN
  UPDATE SET t.cdc_date = s.cdc_date, t.category = s.category 

WHEN not MATCHED by source then
  UPDATE SET t.is_deleted = true

WHEN NOT MATCHED BY target THEN
  insert(cdc_date, is_deleted, id, category)
  values(s.cdc_date, s.is_deleted, s.id, s.category)
;
```
kontrol icin 

```
select count(*)
from
(
select farm_fingerprint(to_json_string(t1)) as hash1,
farm_fingerprint(to_json_string(t2)) as hash2,
t1.category , t1.cdc_date, t1.id, t1.is_deleted,
t2.category , t2.cdc_date, t2.id, t2.is_deleted
from `dsmbootcamp.bora_colakoglu.content_category_target` t1
left join `dsmbootcamp.bora_colakoglu.content_category` t2
on farm_fingerprint(to_json_string(t1)) = farm_fingerprint(to_json_string(t2)))
where hash1 != hash2;

```
