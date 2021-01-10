```SQL
with solution as(

    select name,car, manufacture.year ,array(select as struct p.* replace(timestamp_add(p.date, INTERVAL 3 HOUR) as date) from unnest(purchase) as p) as purchase_modified 
    from `dsmbootcamp.cem_celik.semi_structured_hw`
    cross join unnest(car) car 
    cross join unnest(manufacture) manufacture
   on car.id = manufacture.id --id ler birleştirilerek kartezyen çarpımı yapılmasından doğan fazla kopyalar kaybolur.

),

---- Proof of Solution ----
table2 as (
    Select *,farm_fingerprint(to_json_string(t2)) as _hash2
    from `dsmbootcamp.cem_celik.semi_structured_hw_expected` t2
)

Select *
from 
(select *,farm_fingerprint(to_json_string(solution)) as _hash1
from solution) table1
right outer join table2
on table1._hash1 = table2._hash2
where table1._hash1 <> table2._hash2;

-- There is no difference between expected table and hw_table, when we looked from tables' hashes.
```
