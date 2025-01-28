# What is the difference between the following 2 scripts?
1. 
```
select *
from a left join b on a.id=b.id and b.name='Eden'
```
2. 
```
select *
from a left join b on a.id=b.id
where b.name = 'Eden'
```
The first one with more null may have more records than the seconds one.
