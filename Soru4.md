# 4) Merge tables

```SQL
CREATE TABLE dogukan_nefis.content_category_target
as
SELECT * FROM `dsmbootcamp.sample.content_category_target`;
-----
-----

merge `dsmbootcamp.dogukan_nefis.content_category` T
using `dsmbootcamp.dogukan_nefis.content_category_20201222_00_59` S
on T.id=S.id
when matched then
  UPDATE SET cdc_date=S.cdc_date
when not matched by target then
  insert(cdc_date,is_deleted,id,category)
  values(S.cdc_date,S.is_deleted,S.id,S.category)
when not matched by source then
  UPDATE SET is_deleted=True
------
------


select *
from (select farm_fingerprint(to_json_string(t1)) as _hash1,
        farm_fingerprint(to_json_string(t2)) as _hash2,*
from `dsmbootcamp.dogukan_nefis.content_category` t1  
  inner join `dsmbootcamp.dogukan_nefis.content_category_target` t2
    on t1.id=t2.id)
where _hash1!=_hash2;
```
