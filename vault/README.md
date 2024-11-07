# Vault

## Dynamic Secrets

### Overview

1. **Role Creation**: Define a role in the database.
2. **Permission Assignment**: Grant the required permissions to the role.
3. **Role Assignment**: Vault dynamically creates users, assigns them the role, and manages their access.

shell script for role and permission grants.

```
# rw = read write

# Set the name for the role to be created
ROLE_NAME='rw_role'
# Define the name of the database
DB_NAME='myApp'
# Specify the superuser account to perform actions
SUPERUSER='alankar'
# Set the hostname for the database connection
HOST='localhost'

# Set the global variable for the PostgreSQL password to avoid prompt in each command.
# Note: Be cautious with this approach as storing passwords in environment variables has security risks.
export PGPASSWORD="pasword"

# Define a reusable psql command with connection details
PSQL="psql -h $HOST -U $SUPERUSER -d $DB_NAME"

# Create a new role with no inheritance from other roles.
$PSQL -c "CREATE ROLE $ROLE_NAME NOINHERIT;"

# Grant the new role the ability to connect to the specified database.
$PSQL -c "GRANT CONNECT ON DATABASE $DB_NAME TO $ROLE_NAME;"

# Grant usage privileges on the public schema to the role.
# This allows the role to access the schema but not necessarily any tables.
$PSQL -c "GRANT USAGE ON SCHEMA public TO $ROLE_NAME;"

# Grant select, insert, update, and delete privileges on all current tables in the public schema to the role.
# This gives the role read and write access to the tables.
$PSQL -c "GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO $ROLE_NAME;"

# Ensure that any future tables created in the public schema will automatically grant
# select, insert, update, and delete privileges to the role.
$PSQL -c "ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT, INSERT, UPDATE, DELETE ON TABLES TO $ROLE_NAME;"

```

## Database Connection

Each Vault connection serves a single database.

## Roles & User Lifecycle

Vault generates temporary users, assigns them predefined (i.e rw_role we created) roles, and enforces expiration—automatically revoking access and deleting users after expiration.

