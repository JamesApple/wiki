| IMPORTANT
| window functions only happens after the where clause, so you only get to see
| rows from the available result set of the query.

```
function_name ([expression [, expression ... ]]) [ FILTER ( WHERE filter_clause ) ] OVER window_name
function_name ([expression [, expression ... ]]) [ FILTER ( WHERE filter_clause ) ] OVER ( window_definition )
function_name ( * ) [ FILTER ( WHERE filter_clause ) ] OVER window_name
function_name ( * ) [ FILTER ( WHERE filter_clause ) ] OVER ( window_definition ). In this case it is the current row and all following rows.
where window_definition has the syntax

[ existing_window_name ]
[ PARTITION BY expression [, ...] ]
[ ORDER BY expression [ ASC | DESC | USING operator ] [ NULLS { FIRST | LAST } ] [, ...] ]
[ frame_clause ]
The optional frame_clause can be one of

{ RANGE | ROWS | GROUPS } frame_start [ frame_exclusion ]
{ RANGE | ROWS | GROUPS } BETWEEN frame_start AND frame_end [ frame_exclusion ]
where frame_start and frame_end can be one of

UNBOUNDED PRECEDING
offset PRECEDING
CURRENT ROW
offset FOLLOWING
UNBOUNDED FOLLOWING
and frame_exclusion can be one of

EXCLUDE CURRENT ROW
EXCLUDE GROUP
EXCLUDE TIES
EXCLUDE NO OTHERS
```

# Basic windowing

A window gives each row the ability to view the data of other rows in a sort of perspective.
An empty window gives each row access to all other rows, as seen below.
```sql
SELECT 
  x, 
  ARRAY_AGG(x) OVER ()
FROM 
  GENERATE_SERIES(1, 5) AS t(x)
;
┌───┬─────────────┐
│ x │  array_agg  │
├───┼─────────────┤
│ 1 │ {1,2,3,4,5} │
│ 2 │ {1,2,3,4,5} │
│ 3 │ {1,2,3,4,5} │
│ 4 │ {1,2,3,4,5} │
│ 5 │ {1,2,3,4,5} │
└───┴─────────────┘
```

## Implied window

We can define the set of rows that are viewed by the window using other parameters. This uses an order by which has a default window of `order by x rows between unbounded preceding and current row`
```sql
SELECT
  x,
  ARRAY_AGG(x) OVER (ORDER BY x)
FROM
  GENERATE_SERIES(1, 5) AS t(x)
;
┌───┬─────────────┐
│ x │  array_agg  │
├───┼─────────────┤
│ 1 │ {1}         │
│ 2 │ {1,2}       │
│ 3 │ {1,2,3}     │
│ 4 │ {1,2,3,4}   │
│ 5 │ {1,2,3,4,5} │
└───┴─────────────┘
```

We can invert this default behaviour by defining the window with a specific range. In this case it is the current row and all following rows.

```
{ RANGE | ROWS | GROUPS } frame_start [ frame_exclusion ]
{ RANGE | ROWS | GROUPS } BETWEEN frame_start AND frame_end [ frame_exclusion ]
where frame_start and frame_end can be one of

UNBOUNDED PRECEDING
offset PRECEDING
CURRENT ROW
offset FOLLOWING
UNBOUNDED FOLLOWING
and frame_exclusion can be one of

EXCLUDE CURRENT ROW
EXCLUDE GROUP
EXCLUDE TIES
EXCLUDE NO OTHERS
```

```SQL
SELECT 
  x, 
  ARRAY_AGG(x) OVER (ORDER BY x ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING)
FROM 
  GENERATE_SERIES(1, 5) AS t(x)
;
┌───┬─────────────┐
│ x │  array_agg  │
├───┼─────────────┤
│ 1 │ {1,2,3,4,5} │
│ 2 │ {2,3,4,5}   │
│ 3 │ {3,4,5}     │
│ 4 │ {4,5}       │
│ 5 │ {5}         │
└───┴─────────────┘
```
