## Download csv from database table command 

```
psql -h localhost -U beshak_backend_user -d beshak_backend -c "\copy (SELECT id, email FROM core_user where email IS NOT NULL) TO '/srv/www/vision_user_email.csv' WITH CSV HEADER"

```
