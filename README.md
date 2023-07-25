### Output sql data from table to csv file

```sql
SELECT * FROM product INTO OUTFILE "C:\\ProgramData\\MySQL\\MySQL Server 8.0\\Uploads\\outputs.csv" FIELDS TERMINATED BY '|' ENCLOSED BY '"'  LINES TERMINATED BY '\n';
Query OK, 1 row affected (0.02 sec)
```
