# dbgov-example

Example repository demonstrating [DBGov](https://github.com/amzar96/dbgov) — Database Access Governance as code.

## How it works

1. Define access policies in `policies/*.yaml`
2. Open a PR — DBGov posts a **plan** comment showing what will change
3. Merge to main — DBGov **applies** the grants to your database

## Setup

### 1. Create database users

Before DBGov can grant permissions, the users must exist in the database:

```sql
CREATE USER analyst_user WITH PASSWORD 'your-password';
CREATE USER etl_user WITH PASSWORD 'your-password';
```

### 2. Add GitHub secrets

Go to **Settings > Secrets and variables > Actions** and add:

| Secret | Example |
|---|---|
| `DBGOV_HOST` | `db.xxxx.supabase.co` |
| `DBGOV_PORT` | `5432` |
| `DBGOV_NAME` | `postgres` |
| `DBGOV_USER` | `postgres` |
| `DBGOV_PASSWORD` | `your-db-password` |

### 3. Test it

1. Create a branch, modify a policy in `policies/`
2. Open a PR — check the plan comment
3. Merge — grants are applied

## Policy format

```yaml
apiVersion: dbgov/v1
kind: AccessPolicy
metadata:
  name: analyst-readonly
spec:
  principal:
    name: analyst_user
    type: user
  grants:
    - level: table          # table | schema | database
      schema: public
      tables:
        - transactions
        - accounts
      privileges:
        - SELECT
```
