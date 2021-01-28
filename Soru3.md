#Soru 3)

```SQL
Declare current_min TIMESTAMP DEFAULT "2020-03-03 23:09:00";
Declare last_min TIMESTAMP DEFAULT "2020-03-03 23:15:00";
Create table if not Exists asiyenur_yilmaz.active_users  (
  `ts` TIMESTAMP,
  `active_users_count` INT64
);
While current_min <  last_min  Do
  Insert into asiyenur_yilmaz.active_users (ts, active_users_count) 
  (Select current_min, approx_count_distinct(deviceid) 
  from asiyenur_yilmaz.pageview Where timestamp_trunc(view_ts,minute) between timestamp_add(current_min,INTERVAL -4 MINUTE) and current_min);
  Set current_min = timestamp_add(current_min, INTERVAL 1 MINUTE);
End While
```
