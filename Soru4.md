#Soru 4)

```SQL
create or replace table dsmbootcamp.asiyenur_yilmaz.content_category_target
as
select * from `dsmbootcamp.sample.content_category_target`;
create or replace table dsmbootcamp.asiyenur_yilmaz.content_category
as
select * from `dsmbootcamp.sample.content_category`;
create or replace table dsmbootcamp.asiyenur_yilmaz.content_category_20201222_00_59
as
select * from `dsmbootcamp.sample.content_category_20201222_00_59`;
merge asiyenur_yilmaz.content_category t
using asiyenur_yilmaz.content_category_20201222_00_59 s
on t.id = s.id
when matched and t.is_deleted != true and t.category != s.category then 
  update set category = s.category, cdc_date = s.cdc_date
when matched and t.is_deleted != true and s.is_deleted = true then
  update set is_deleted = true, cdc_date = s.cdc_date
when not matched by target then
  insert(cdc_date, is_deleted, id, category)
  values(s.cdc_date, s.is_deleted,s.id,s.category)
when not matched by source then
  update set is_deleted = true
  
  
Select * from (select *,farm_fingerprint(to_json_string(t)) as _hash1 from asiyenur_yilmaz.content_category_target t) target
Full Outer Join (select *, farm_fingerprint(to_json_string(c)) as _hash1 from asiyenur_yilmaz.content_category c) content_category
On target.id = content_category.id
Where target._hash1 != content_category._hash1 
```
