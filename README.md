## Download csv from database table command 

```
psql -h localhost -U beshak_backend_user -d beshak_backend -c "\copy (SELECT id, email FROM core_user where email IS NOT NULL) TO '/srv/www/vision_user_email.csv' WITH CSV HEADER"

```


## Download health trumatch data

```
psql -h localhost -U beshak_backend_user -d beshak_backend -c "\copy (SELECT hg.id AS htm_id, hrp.customer_email, cu.name AS user_name, cu.email AS user_email, ph.id, ph.uuid, TO_CHAR(ph.created AT TIME ZONE 'UTC', 'YYYY-MM-DD HH24:MI:SS') AS created, TO_CHAR(ph.modified AT TIME ZONE 'UTC', 'YYYY-MM-DD HH24:MI:SS') AS modified, ph.full_name, ph.type, ph.dob, ph.gender, ph.pincode, ph.is_consumes_alcohol, ph.relationship, ph.fitness, ph.alcohol_frequency, ph.is_consumes_tobacco, ph.tobacco_frequency, ph.is_on_insulin, ph.has_medical_conditions, ARRAY_TO_STRING(ph.medical_conditions, ', ') AS medical_conditions, ph.diabetes_state, ph.diabetes_from_year, ph.hypertension_from_year, ph.hypertension_state, ph.other_medical_conditions, ph.other_diseases, ph.is_hospitalized, ph.reason_for_hospitalization, ph.year_of_hospitalization, ph.is_ongoing_medication, ph.ongoing_medication, ph.health_individual_tm_report_pdf, ph.profile_id AS profile FROM trumatch_htmgeneratedreports hg JOIN trumatch_healthreportprofile hrp ON hrp.id = hg.profile_id JOIN core_user cu ON cu.id = hrp.user_id JOIN trumatch_profilehealth ph ON ph.profile_id = hrp.id WHERE hg.created >= '2026-01-01' ORDER BY hg.created DESC) TO '/srv/www/health_trumatch_profiles.csv' WITH CSV HEADER"
```

