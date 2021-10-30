# Postgres

## Tips

Force index usage in query plans

```sql
SET enable_seqscan = OFF;
```


## Order of Operations

1. FROM, JOIN
1. WHERE
1. GROUP BY
1. aggregate functions
1. HAVING
1. window functions
1. SELECT
1. DISTINCT
1. UNION/INTERSECT/EXCEPT
1. ORDER BY
1. OFFSET
1. LIMIT/FETCH/TOP
