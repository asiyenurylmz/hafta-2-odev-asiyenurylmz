#Soru 1) 1980’den itibaren spor grubu bazında en çok madalya alan 1. 3. 5. ülkeyi bulalım.

```SQL
with count_medals as
(
  select Sport, Country, Count(1) As Medals_Count 
  from asiyenur_yilmaz.summer_medals
  Where Year >= 1980
  Group By Sport, Country 
)
Select * 
from(
  Select 
    Sport, 
    last_value(country) over(partition by Sport order by Medals_Count Desc) as First_Country, 
    last_value(Medals_Count) over(partition by Sport order by Medals_Count Desc) as First_Countries_Medal_Count,
    last_value(country) over(partition by Sport order by Medals_Count Desc rows between current row and 2 following) as Third_Country, 
    last_value(Medals_Count) over(partition by Sport order by Medals_Count Desc rows between current row and 2 following) as Third_Countries_Medal_Count,
    last_value(country) over(partition by Sport order by Medals_Count Desc rows between current row and 4 following) as Fifth_Country, 
    last_value(Medals_Count) over(partition by Sport order by Medals_Count Desc rows between current row and 4 following) as Fifth_Countries_Medal_Count,
    row_number() over(partition by sport order by sport) as rn
  from count_medals
)
where rn = 1
order by sport ASC
```
