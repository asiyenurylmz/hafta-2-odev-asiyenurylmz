#Soru 2) 1980’den itibaren herhangi bir spor grubunda üst üste 3 veya daha fazla madalya almış atletleri bulalım.

```SQL
Declare year_ INT64 Default 1980;
Create table if not Exists asiyenur_yilmaz.temp  (
  `athlete` String
);
Create temporary table if not Exists athletes as (select athlete, year from asiyenur_yilmaz.summer_medals where year >= 1980 group by athlete, year)
;
While year_ <= 2020 Do
  Insert into asiyenur_yilmaz.temp 
  (Select athlete from athletes where year = (year_ + 8) and athlete in 
  (Select athlete from athletes where year = (year_ + 4) and athlete in 
  (Select athlete from athletes where year = year_)));
Set year_ = year_ + 4;
End While;
```
