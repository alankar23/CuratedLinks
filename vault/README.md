# Vault

## Dynamic Secrets

### Glossry

1 . Create a role in database.
2 . Grant permissions to the role.
3 . Vault will assign the role to created user.


shell script for role and permission grants.

```
# rw = read write

ROLE_NAME='rw_role'
DB_NAME='myApp'
SUPERUSER='alankar'
HOST='localhost'

## setting global variable for password, so there is no need to put in every command and avoid promt
export PGPASSWORD="pasword"

PSQL="psql -h $HOST -U $SUPERUSER -d $DB_NAME"

# Create role
$PSQL -c "CREATE ROLE $ROLE_NAME NOINHERIT;"
# Grant connect
$PSQL -c "GRANT CONNECT ON DATABASE $DB_NAME TO $ROLE_NAME;"
# Grant usage
$PSQL -c "GRANT USAGE ON SCHEMA public TO $ROLE_NAME;"

#$PSQL -c "GRANT SELECT ON ALL TABLES IN SCHEMA public TO $ROLE_NAME;" -->

# Grant actions
$PSQL -c "GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO $ROLE_NAME;"

# For future tables

$PSQL -c "ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT, INSERT, UPDATE, DELETE ON TABLES TO $ROLE_NAME;"

```