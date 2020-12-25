# Soru 3) "sample.pageview": tablosunda 1 gün içerisinde [trendyol.com](http://trendyol.com/) a gelen tüm ziyaretlerin logu var.
```SQL
select timestamp_trunc(view_ts,minute) view_ts_slice
      ,count(distinct deviceid) cnt_deviceid
from `dsmbootcamp.dogukan_nefis.pageview`
group by 1
order by cnt_deviceid desc;
--- 44.5 sec elapsed time
--- 19 min 11.8sec slot time
--- 9.73 GB Shuffled


select timestamp_trunc(view_ts,minute) view_ts_slice
      ,approx_count_distinct(deviceid) active_user_cnt
from `dsmbootcamp.dogukan_nefis.pageview`
group by 1
order by active_user_cnt desc;
-- 13.1 sec elapsed time
-- 8 min 25.77 slot time consumed
--- 200 MB Shuffled


select view_minute,hll_count.extract(approx_dist_user) as active_user_cnt
from (
select timestamp_trunc(pv.view_ts,minute) view_minute
      ,hll_count.init(deviceid,14) approx_dist_user
from `dsmbootcamp.dogukan_nefis.pageview` pv
group by 1)
order by active_user_cnt DESC;
-- 12.6 sec
-- 4 min 27 sec
-- 122 MB Bytes Shuffled
```
