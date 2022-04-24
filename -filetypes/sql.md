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

https://explain.depesz.com/

# Explain query
EXPLAIN (ANALYZE, COSTS, VERBOSE, BUFFERS, FORMAT JSON)
psql -XqAt -f explain.sql > analyze.json
https://explain.dalibo.com/
https://github.com/mgartner/pg_flame

https://aws.amazon.com/blogs/database/optimizing-and-tuning-queries-in-amazon-rds-postgresql-based-on-native-and-external-tools/

https://github.com/ankane/dexter
https://github.com/ankane/blazer
https://github.com/ankane/pghero
