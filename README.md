
```
SELECT
  l.id, l.uuid, l.fullname AS name, l.email, l.mobile,
  cu.name AS name2, cu.email AS email2, cu.mobile AS mobile2,
  l.region, l.country_code,
  l.advisor_id, ap.name AS advisor_name, au.email AS advisor_email, au.slug AS advisor_slug,
  bu.name AS bpo_user_name, bu.email AS bpo_user_email,
  l.referral_user_id, l.language, l.product_type, l.status, l.age, l.pincode, l.income,
  l.financial_plan, l.profession_type, l.family_members, l.time_to_buy,
  l.qualification_status, l.call_type, l.lead_tagging_status, l.lead_sub_tagging_status,
  (SELECT string_agg(i.illness_name, ', ')
     FROM marketplace_lead_ped_diseases lp
     JOIN trumatch_listofillnesses i ON i.id = lp.listofillnesses_id
    WHERE lp.lead_id = l.id) AS ped_diseases,
  l.lead_dropped_reason, l.postponement_reason, l.postponed_date,
  l.follow_up_marked_on_date, l.follow_up_date, l.lp_lead_tagging_status, l.preferred_slots,
  to_char(l.lp_call_back_date AT TIME ZONE 'Asia/Kolkata', 'YYYY-MM-DD HH24:MI:SS') AS lp_call_back_date,
  l.buying_reason, l.tm_generated_date, l.quote_upload_date, l.payment_done_date, l.is_lp_lead,
  l.notes, l.insurance_company, l.plan_name, l.num_of_policies, l.premium, l.premium_updated,
  l.mode, l.added_payment_details, l.active, l.whatsapp_available, l.health_tm_flow,
  l.trumatch_report_generated, l.is_tm_deviated, l.scheduled_date,
  to_char(l.advisor_assignment_date AT TIME ZONE 'Asia/Kolkata', 'YYYY-MM-DD HH24:MI:SS') AS advisor_assignment_date,
  l.assignment_type, l.lead_source, l.product, l.is_protect_me_well_lead,
  l.is_instant_connect_lead, l.customer_declined_trumatch, l.platform,
  us.name AS utm_source, l.utm_medium, l.utm_campaign, l.utm_content, l.utm_term,
  l.utm_ad_set_name, l.utm_site_source_name, l.utm_placement_name, l.origin, l.gclid,
  l.tracking_id, l.enable_whatsapp_notification, l.enable_email_notification,
  l.advisor_reassignment,
  to_char(l.created  AT TIME ZONE 'Asia/Kolkata', 'YYYY-MM-DD HH24:MI:SS') AS created,
  to_char(l.modified AT TIME ZONE 'Asia/Kolkata', 'YYYY-MM-DD HH24:MI:SS') AS modified,
  (SELECT count(*) FROM marketplace_quotedocument q WHERE q.lead_id = l.id) AS quote_docs_count,
  (SELECT count(*) FROM marketplace_policydetails p WHERE p.lead_id = l.id) AS policies_count
FROM marketplace_lead l
LEFT JOIN core_user                        cu   ON cu.id   = l.user_id
LEFT JOIN marketplace_advisorpublicprofile apub ON apub.id = l.advisor_id
LEFT JOIN advisor_advisorprofile           ap   ON ap.id   = apub.profile_id
LEFT JOIN core_user                        au   ON au.id   = ap.user_id
LEFT JOIN marketplace_bpouser              b    ON b.id    = l.bpo_user_id
LEFT JOIN core_user                        bu   ON bu.id   = b.user_id
LEFT JOIN marketplace_utmsource            us   ON us.id   = l.utm_source_id
WHERE l.created >= '2025-01-01 00:00:00+05:30'
ORDER BY l.id DESC;
```
