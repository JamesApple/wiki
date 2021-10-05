## Set a variable in PSQL

```sql
\set my_var 'my value'

SELECT :'my_var';
```

## Simplified transaction isolation levels

1. Read Uncommitted
  - Same as Read Committed
1. Read committed
  - Allows the current transaction to see other transactions changes as soon as they are committed. This means that running a query twice during the same query could result in different results from something like `SELECT count(*) FROM stock`
1. Repeatable Read
  - Ensures that all queries in the transaction read from the same snapshot of the database. This is ideal for backups. From `BEGIN` to `COMMIT`
1. Serializable
  - This level guarantees that a one-transaction-at-a-time ordering of what happens on the server exists with the exact same result as what you're obtaining with concurrent activity.

## When to use stored procedures

When the procedure is just SQL, we may be effectively abstracting and preventing round trips. If we end up dipping into procedural code in the DB we lost the advantage and simplicity of SQL

This determines where the business logic goes

SQL stored procedure
```sql
create or replace function get_all_albums
 (
   in  artistid bigint,
   out album    text,
   out duration interval
 )
returns setof record
language sql
as $$
  select album.title as album,
         sum(milliseconds) * interval '1 ms' as duration
    from album
         join artist using(artistid)
         left join track using(albumid)
   where artist.artistid = get_all_albums.artistid
group by album
order by album;
$$;
```

PLPGSQL Procedural stored procedure
```sql
create or replace function get_all_albums
 (
   in  name     text,
   out album    text,
   out duration interval
 )
returns setof record
language plpgsql
as $$
declare
  rec record;
begin
  for rec in select albumid
               from album
                    join artist using(artistid)
              where album.name = get_all_albums.name
  loop
       select title, sum(milliseconds) * interval '1ms'
         into album, duration
         from album
              left join track using(albumid)
        where albumid = record.albumid
     group by title
     order by title;

    return next;
  end loop;
end;
$$;
```

## DDL Transactions

Postgres enables DDL for the schema as well.

# Adding Unique Ids

Notes:
1. Tables without a unique identifier still have a CTID which uniquely
   identifies it within its table. This is the physical location of the row in
   memory so it will change and cannot be used to identify the row outside of a
   single query



Given table t:
```
┌───┐
│ v │
├───┤
│ a │
│ a │
│ a │
│ b │
│ c │
│ c │
│ d │
│ e │
│ a │
└───┘
```

We can first add the column that will accept our ID (created_at for some reason here)
```sql
ALTER TABLE 
  t
ADD COLUMN 
  created_at integer default 0;
```

Row number is unique. So we use this to add a unique identifier. We join on the
CTID and then insert the new ID
```sql
UPDATE t 
  SET created_at = new_id
FROM
  ( 
    SELECT 
      ctid,
      ROW_NUMBER() OVER ( ORDER BY ctid DESC ) AS new_id
    FROM 
      t
  ) as row_numbers
WHERE t.ctid = row_numbers.ctid
;
```
Then you can drop the default and set to non nullable
```sql
ALTER TABLE t
   ALTER COLUMN v 
      DROP DEFAULT,
   ALTER COLUMN v 
      SET NOT NULL
```


# Average Cost By Group

Given:
┌────────────┬────────────────────┬─────────┬──────────┐
│ product_id │    product_name    │  price  │ group_id │
├────────────┼────────────────────┼─────────┼──────────┤
│          1 │ Microsoft Lumia    │  200.00 │        1 │
│          2 │ HTC One            │  400.00 │        1 │
│          3 │ Nexus              │  500.00 │        1 │
│          4 │ iPhone             │  900.00 │        1 │
│          5 │ HP Elite           │ 1200.00 │        2 │
│          6 │ Lenovo Thinkpad    │  700.00 │        2 │
│          7 │ Sony VAIO          │  700.00 │        2 │
│          8 │ Dell Vostro        │  800.00 │        2 │
│          9 │ iPad               │  700.00 │        3 │
│         10 │ Kindle Fire        │  150.00 │        3 │
│         11 │ Samsung Galaxy Tab │  200.00 │        3 │
└────────────┴────────────────────┴─────────┴──────────┘
And a grouping table:
┌──────────┬────────────┐
│ group_id │ group_name │
├──────────┼────────────┤
│        1 │ Smartphone │
│        2 │ Laptop     │
│        3 │ Tablet     │
└──────────┴────────────┘

```sql
SELECT
 product_name,
 price,
 group_name,
 AVG (price) OVER group_name,
 LAG (price) OVER group_name
FROM
 products
INNER JOIN product_groups USING (group_id)
WINDOW group_name AS (PARTITION BY group_name);
```

Returns
┌────────────────────┬─────────┬────────────┬──────────────────────┬─────────┐
│    product_name    │  price  │ group_name │         avg          │   lag   │
├────────────────────┼─────────┼────────────┼──────────────────────┼─────────┤
│ HP Elite           │ 1200.00 │ Laptop     │ 850.0000000000000000 │  [NULL] │
│ Lenovo Thinkpad    │  700.00 │ Laptop     │ 850.0000000000000000 │ 1200.00 │
│ Sony VAIO          │  700.00 │ Laptop     │ 850.0000000000000000 │  700.00 │
│ Dell Vostro        │  800.00 │ Laptop     │ 850.0000000000000000 │  700.00 │
│ Microsoft Lumia    │  200.00 │ Smartphone │ 500.0000000000000000 │  [NULL] │
│ HTC One            │  400.00 │ Smartphone │ 500.0000000000000000 │  200.00 │
│ Nexus              │  500.00 │ Smartphone │ 500.0000000000000000 │  400.00 │
│ iPhone             │  900.00 │ Smartphone │ 500.0000000000000000 │  500.00 │
│ iPad               │  700.00 │ Tablet     │ 350.0000000000000000 │  [NULL] │
│ Kindle Fire        │  150.00 │ Tablet     │ 350.0000000000000000 │  700.00 │
│ Samsung Galaxy Tab │  200.00 │ Tablet     │ 350.0000000000000000 │  150.00 │
└────────────────────┴─────────┴────────────┴──────────────────────┴─────────┘

```sql

SELECT
 product_name,
 price,
 group_name,
 RANK () OVER ( PARTITION BY group_name ORDER BY price)
FROM
 products
INNER JOIN product_groups USING (group_id)
;
```

┌────────────────────┬─────────┬────────────┬──────┐
│    product_name    │  price  │ group_name │ rank │
├────────────────────┼─────────┼────────────┼──────┤
│ Sony VAIO          │  700.00 │ Laptop     │    1 │
│ Lenovo Thinkpad    │  700.00 │ Laptop     │    1 │
│ Dell Vostro        │  800.00 │ Laptop     │    3 │
│ HP Elite           │ 1200.00 │ Laptop     │    4 │
│ Microsoft Lumia    │  200.00 │ Smartphone │    1 │
│ HTC One            │  400.00 │ Smartphone │    2 │
│ Nexus              │  500.00 │ Smartphone │    3 │
│ iPhone             │  900.00 │ Smartphone │    4 │
│ Kindle Fire        │  150.00 │ Tablet     │    1 │
│ Samsung Galaxy Tab │  200.00 │ Tablet     │    2 │
│ iPad               │  700.00 │ Tablet     │    3 │
└────────────────────┴─────────┴────────────┴──────┘
