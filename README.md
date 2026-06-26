# Google Cloud Project Migration Between Two Google Workspace Organizations

This note explains how to move a Google Cloud project from one Google Workspace organization to another.

The examples use dummy domains:

- Source Workspace / Organization: `example.com`
- Destination Workspace / Organization: `example.co.in`
- Source admin account: `admin@example.com`
- Destination admin account: `agent@example.co.in`
- Project creator / old owner: `meetings@example.com`
- Project ID: `exampledevmeetings`
- Source Organization ID: `111111111111`
- Destination Organization ID: `222222222222`

> Replace all example values with your actual domain, account, project ID, and organization IDs.

---

## 1. What this migration means

This is not a domain migration.

This is a **Google Cloud project organization migration**.

You are moving a project from:

```text
example.com organization
```

to:

```text
example.co.in organization
```

The project ID normally remains the same.

Example:

```text
Before:
Project ID: exampledevmeetings
Parent org: example.com

After:
Project ID: exampledevmeetings
Parent org: example.co.in
```

Most project-level resources remain the same, such as:

```text
Enabled APIs
Service accounts
OAuth clients
Pub/Sub topics
Cloud resources
Project-level IAM
```

But these things may change or need checking:

```text
Inherited IAM
Organization policies
Billing restrictions
OAuth/domain restrictions
Quotas
Firewall policies
Access visibility in Console
```

---

## 2. Important condition

The destination domain must be a **separate Google Workspace account**.

Correct case:

```text
example.co.in has its own Admin Console,
its own users,
its own billing,
and its own Google Cloud Organization.
```

If `example.co.in` is only a secondary domain inside `example.com`, then it usually does not have a separate Google Cloud Organization.

---

## 3. Confirm both organizations exist

Login with the source admin:

```bash
gcloud auth login admin@example.com
gcloud organizations list
```

You should see something like:

```text
DISPLAY_NAME     ID
example.com      111111111111
```

Then login with the destination admin:

```bash
gcloud auth login agent@example.co.in
gcloud organizations list
```

You should see:

```text
DISPLAY_NAME       ID
example.co.in      222222222222
```

Save these IDs:

```text
SOURCE_ORG_ID = 111111111111
DEST_ORG_ID   = 222222222222
PROJECT_ID    = exampledevmeetings
```

### Why this step is needed

Google Cloud migration uses numeric organization IDs, not domain names.

---

## 4. Confirm source admin can access the project

Login with source admin:

```bash
gcloud auth login admin@example.com
gcloud config set project exampledevmeetings
gcloud projects describe exampledevmeetings
```

Expected output should include:

```yaml
projectId: exampledevmeetings
lifecycleState: ACTIVE
parent:
  id: '111111111111'
  type: organization
```

### Why this step is needed

Google Workspace Super Admin access is not always the same as Google Cloud project access.

If this fails, the current project owner must add the source admin to the project IAM.

---

## 5. If source admin cannot access project, add them first

Login with the current project owner, for example:

```bash
gcloud auth login meetings@example.com
```

Then add the source admin as temporary Owner:

```bash
gcloud projects add-iam-policy-binding exampledevmeetings \
  --member="user:admin@example.com" \
  --role="roles/owner"
```

Verify:

```bash
gcloud projects get-iam-policy exampledevmeetings \
  --flatten="bindings[].members" \
  --filter="bindings.members:user:admin@example.com" \
  --format="table(bindings.role)"
```

Expected:

```text
ROLE
roles/owner
```

### Why this step is needed

The source admin needs project access before setting up migration.

---

## 6. Add destination admin to the project

Try adding destination admin as Owner:

```bash
gcloud projects add-iam-policy-binding exampledevmeetings \
  --member="user:agent@example.co.in" \
  --role="roles/owner"
```

### Possible error

You may get:

```text
ORG_MUST_INVITE_EXTERNAL_OWNERS
```

This means Google Cloud is blocking an external user from being added as Owner.

In that case, add non-Owner roles instead:

```bash
gcloud projects add-iam-policy-binding exampledevmeetings \
  --member="user:agent@example.co.in" \
  --role="roles/browser"
```

```bash
gcloud projects add-iam-policy-binding exampledevmeetings \
  --member="user:agent@example.co.in" \
  --role="roles/viewer"
```

```bash
gcloud projects add-iam-policy-binding exampledevmeetings \
  --member="user:agent@example.co.in" \
  --role="roles/resourcemanager.projectIamAdmin"
```

Also add:

```bash
gcloud projects add-iam-policy-binding exampledevmeetings \
  --member="user:agent@example.co.in" \
  --role="roles/serviceusage.serviceUsageConsumer"
```

Verify:

```bash
gcloud projects get-iam-policy exampledevmeetings \
  --flatten="bindings[].members" \
  --filter="bindings.members:user:agent@example.co.in" \
  --format="table(bindings.role)"
```

Expected:

```text
ROLE
roles/browser
roles/resourcemanager.projectIamAdmin
roles/serviceusage.serviceUsageConsumer
roles/viewer
```

### Why this step is needed

The destination admin should have access before the move. This avoids losing visibility/control after migration.

---

## 7. Check the current parent of the project

Run:

```bash
gcloud projects describe exampledevmeetings
```

Check:

```yaml
parent:
  id: '111111111111'
  type: organization
```

If the type is:

```text
organization
```

the project is directly under the organization.

If the type is:

```text
folder
```

then the project is inside a folder and folder-level permissions may also matter.

### Why this step is needed

Migration permissions depend on the project’s current parent.

---

## 8. Allow export from source organization

Create a file:

```bash
notepad d:/source-export-policy.yaml
```

Paste:

```yaml
name: organizations/111111111111/policies/resourcemanager.allowedExportDestinations
spec:
  rules:
  - values:
      allowedValues:
      - under:organizations/222222222222
```

Apply it with the source admin:

```bash
gcloud config set account admin@example.com
gcloud org-policies set-policy d:/source-export-policy.yaml
```

### If API is not enabled

You may see:

```text
API [orgpolicy.googleapis.com] not enabled...
Would you like to enable and retry? (y/N)?
```

Type:

```text
y
```

### If permission denied

You may see:

```text
Permission 'orgpolicy.constraints.list' denied
```

Give the source admin this role on the source organization:

```bash
gcloud organizations add-iam-policy-binding 111111111111 \
  --member="user:admin@example.com" \
  --role="roles/orgpolicy.policyAdmin"
```

Then retry:

```bash
gcloud org-policies set-policy d:/source-export-policy.yaml
```

### Why this step is needed

This tells the source organization:

```text
Projects are allowed to be exported to the destination organization.
```

---

## 9. Allow import into destination organization

Create a file:

```bash
notepad d:/destination-import-policy.yaml
```

Paste:

```yaml
name: organizations/222222222222/policies/resourcemanager.allowedImportSources
spec:
  rules:
  - values:
      allowedValues:
      - under:organizations/111111111111
```

Apply it with the destination admin:

```bash
gcloud config set account agent@example.co.in
gcloud org-policies set-policy d:/destination-import-policy.yaml
```

### If permission denied

Give the destination admin this role on the destination organization:

```bash
gcloud organizations add-iam-policy-binding 222222222222 \
  --member="user:agent@example.co.in" \
  --role="roles/orgpolicy.policyAdmin"
```

Then retry:

```bash
gcloud org-policies set-policy d:/destination-import-policy.yaml
```

### Why this step is needed

This tells the destination organization:

```text
Projects are allowed to be imported from the source organization.
```

---

## 10. Give Project Mover role

The account that performs the final move should have Project Mover role on both source and destination organizations.

In this example, the final migration runner is:

```text
agent@example.co.in
```

### Give Project Mover on source org

Run using the source admin:

```bash
gcloud config set account admin@example.com
gcloud organizations add-iam-policy-binding 111111111111 \
  --member="user:agent@example.co.in" \
  --role="roles/resourcemanager.projectMover"
```

### Give Project Mover on destination org

Run using the destination admin:

```bash
gcloud config set account agent@example.co.in
gcloud organizations add-iam-policy-binding 222222222222 \
  --member="user:agent@example.co.in" \
  --role="roles/resourcemanager.projectMover"
```

### Common mistake

If you run the destination command while still logged in as the source admin, you may see:

```text
The caller does not have permission.
This command is authenticated as admin@example.com.
```

Fix:

```bash
gcloud config set account agent@example.co.in
```

Then retry the destination command.

### Why this step is needed

Import/export policies allow the move at organization level, but the user running the move also needs permission to move the project.

---

## 11. Enable Cloud Asset API if needed

The `analyze-move` command uses Cloud Asset API.

Enable it using the source admin if the destination admin does not have permission:

```bash
gcloud config set account admin@example.com
gcloud services enable cloudasset.googleapis.com --project=exampledevmeetings
```

### Why this step is needed

Without Cloud Asset API, the analysis command may fail.

---

## 12. Run analyze-move before actual migration

Switch to destination admin:

```bash
gcloud config set account agent@example.co.in
```

Run:

```bash
gcloud asset analyze-move \
  --project=exampledevmeetings \
  --destination-organization=222222222222
```

### Possible warning

You may see warnings about:

```text
IAM policies
Organization policies
Quotas
Hierarchical firewall policies
Privileged Access Manager
```

Example:

```text
The current effective policies may no longer be effective after migration.
```

These are warnings, not always blockers.

### Firewall policy warning

You may see:

```text
Failed to retrieve inherited firewall policies.
Required compute.organizations.listAssociations permission.
```

This means Google could not fully analyze firewall policies because the account lacks read permission on source org firewall policies.

It does not always mean the move will fail.

### Why this step is needed

It checks for blockers before the actual migration.

---

## 13. Optional Compute/VPC check

If the project uses Compute Engine, VPC, GKE, Cloud Run VPC connector, VPN, or firewall rules, check:

```bash
gcloud config set account admin@example.com
gcloud compute instances list --project=exampledevmeetings
gcloud compute networks list --project=exampledevmeetings
```

If billing is not enabled, this may not work.

### Why this step is useful

Firewall policy warnings mainly matter if the project uses Compute/VPC networking.

---

## 14. Move the project

Switch to destination admin:

```bash
gcloud config set account agent@example.co.in
```

Run:

```bash
gcloud beta projects move exampledevmeetings \
  --organization=222222222222
```

It may ask:

```text
Do you want to continue (Y/n)?
```

Type:

```text
y
```

Expected success output:

```text
Updated [https://cloudresourcemanager.googleapis.com/v1/projects/exampledevmeetings].

PROJECT_ID           NAME                 PROJECT_NUMBER
exampledevmeetings   ExampleDevMeetings   123456789012
```

### Why this step is needed

This is the actual migration command. It changes the project parent from the source organization to the destination organization.

---

## 15. Verify the project parent changed

Run:

```bash
gcloud projects describe exampledevmeetings
```

Expected:

```yaml
parent:
  id: '222222222222'
  type: organization
```

### Why this step is needed

The move command says updated, but this confirms the project is now under the destination organization.

---

## 16. How to see the project in Google Cloud Console

Login with:

```text
agent@example.co.in
```

Open:

```text
https://console.cloud.google.com/
```

Click the project dropdown.

Select organization:

```text
example.co.in
```

Search by project ID:

```text
exampledevmeetings
```

Direct URL format:

```text
https://console.cloud.google.com/home/dashboard?project=exampledevmeetings
```

IAM page direct URL:

```text
https://console.cloud.google.com/iam-admin/iam?project=exampledevmeetings
```

### If project is not visible immediately

Try:

```text
Hard refresh
Incognito window
Login only with destination admin
Search by project ID
Wait a few minutes for IAM/Console propagation
```

---

## 17. Why old source projects may show under "No organization"

After migration, the destination admin may still see some old source projects under:

```text
No organization
```

This usually does not mean those projects moved.

It means the destination admin has project-level IAM access but may not have permission to view the real parent organization.

To verify the real parent of any project:

```bash
gcloud projects describe PROJECT_ID
```

Check:

```yaml
parent:
  id: '111111111111'
  type: organization
```

or:

```yaml
parent:
  id: '222222222222'
  type: organization
```

---

## 18. Post-migration checks

After migration, check IAM:

```bash
gcloud projects get-iam-policy exampledevmeetings \
  --format="table(bindings.role,bindings.members)"
```

Check billing:

```bash
gcloud beta billing projects describe exampledevmeetings
```

Check APIs:

```bash
gcloud services list --enabled --project=exampledevmeetings
```

Check OAuth:

```text
Google Cloud Console → APIs & Services → OAuth consent screen
```

Check:

```text
Authorized domains
Support email
OAuth client IDs
Redirect URIs
Publishing status
Test users
```

### Why this step is needed

Project-level settings usually remain, but inherited organization policies and access can change.

---

## 19. Do not remove old users immediately

Keep these users temporarily:

```text
meetings@example.com
admin@example.com
agent@example.co.in
```

After testing, remove old source users if no longer needed.

Example:

```bash
gcloud projects remove-iam-policy-binding exampledevmeetings \
  --member="user:meetings@example.com" \
  --role="roles/owner"
```

Also remove source admin if no longer required:

```bash
gcloud projects remove-iam-policy-binding exampledevmeetings \
  --member="user:admin@example.com" \
  --role="roles/owner"
```

### Why this step is needed

Do not remove old access until you confirm the project works fully from the destination organization.

---

## 20. Summary flow

```text
1. Confirm both organizations exist.
2. Confirm source admin can access project.
3. Add destination admin to project.
4. Check current project parent.
5. Allow export from source org.
6. Allow import into destination org.
7. Give Project Mover role on both orgs.
8. Enable Cloud Asset API if needed.
9. Run analyze-move.
10. Review warnings.
11. Move project.
12. Verify parent changed.
13. Check Console visibility.
14. Check IAM, billing, APIs, OAuth.
15. Keep old users temporarily.
16. Remove old source access only after testing.
```

---

## 21. Useful command placeholders

```text
SOURCE_DOMAIN=example.com
DEST_DOMAIN=example.co.in
SOURCE_ADMIN=admin@example.com
DEST_ADMIN=agent@example.co.in
OLD_OWNER=meetings@example.com
PROJECT_ID=exampledevmeetings
SOURCE_ORG_ID=111111111111
DEST_ORG_ID=222222222222
```

---

## 22. Important notes

- Google Workspace Super Admin is not always Google Cloud Organization Admin.
- Organization Policy Admin may be needed separately.
- Project Mover role is required for the migration runner.
- External users may not be allowed to become Owner before migration.
- Non-Owner roles can be used temporarily.
- Console project picker can show accessible projects under "No organization" if the user cannot read their parent org.
- Always verify with `gcloud projects describe PROJECT_ID`.
