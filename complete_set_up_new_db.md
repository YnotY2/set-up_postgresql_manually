# Setting up DataBase PostgreSQL for any project; 
First make sure the default postgres user is set-up correctly. Follow this guide right here; 
https://github.com/YnotY2/set-up_postgresql_manually

## Adding new user to the DataBase PostgreSQL:
1, First off open a "psql" terminal once more:

```bash
psql -U postgresh
```

2, Create a new user with corresponding password within psql terminal: 

```bas
CREATE USER <user> WITH PASSWORD '<password>';
CREATE USER example_user WITH PASSWORD 'example_password';
```

## Confirm creation of user; 
Make sure you are out of the "psql" terminal

```bash
psql -U <user> -W
psql -U example_user -W
```

The error should indicate "peer auth failed" because we haven not yet added a entry to the "ph_hba.conf" file for the user database and md5 auth method. 


## Edit the "pg_hba.conf" file:
1, Naviaget to the dir containing conf-files for postgreSQL 

```bash
cd etc/postgresql/16/main
```

2, When you are within the pg_hba.conf file add the following line; 

```bash
# TYPE  DATABASE  USER     ADDRESS     METHOD
local	  all	      <user>               md5
local	  all	      example_user         md5
```

Save the file with the changes made. 

## Restart the PostgreSQL service: 

```bash 
systemctl restart postgresql 
```

## Finally properly confirm the creation of the new user! 

1, Open the "psql" terminal window: 

```bash
psql -U postgres
```

2, Check the creation of the user "example_user" 

```bash
SELECT * FROM pg_user WHERE usename = '<user>';
SELECT * FROM pg_user WHERE usename = 'example_user';
```

Output should be as follows; 

```bash
      usename      | usesysid | usecreatedb | usesuper | userepl | usebypassrls |  passwd  | valuntil | useconfig 
-------------------+----------+-------------+----------+---------+--------------+----------+----------+-----------
 example_user      |    16388 | f           | f        | f       | f            | ******** |          | 
(1 row)
```


## Creating specified datbase "example_database" 

1, Now still within the "psql" terminal window run the follow cmd:

```bash
CREATE DATABASE <database_name>;
CREATE DATABASE example_database;
```

2, Confirm creation of specified database: 

```bash
SELECT datname FROM pg_database WHERE datname = '<database_name';
SELECT datname FROM pg_database WHERE datname = 'example_database';
```

Output should be as follows; 

```bash
      datname      
-------------------
 example_database
(1 row)

```

## Create "example_database" database schema:

1, First off we will create the schema for our database we recently created "example_database"/"<database_name>" . We can find the visualisation of the schema within this GitHub repo "database.md" 

Create the scheme by creating a "init-db.psql" file and pasting the following commands within it: 

```bash
sudo touch init-db.psql 
```


(paste this within file) Find examples within current repo file-name "db_visualisation.md"

```bash
-- Create the database if it does not exist
CREATE DATABASE IF NOT EXISTS example_database;

-- Connect to the yahoo_mail_sender database
\c example_database;

-- Create the mail_content_template schema
CREATE SCHEMA IF NOT EXISTS <schema_name>;

-- Create the mail_content_unique_parameters schema
CREATE SCHEMA IF NOT EXISTS <schema_name>;

-- Create the send_ready_mail schema
CREATE SCHEMA IF NOT EXISTS <schema_name>;

-- Create the <schema_name>.<table_name> table
CREATE TABLE IF NOT EXISTS <schema_name>.<table_name> (
    <subject>_id SERIAL PRIMARY KEY,
    <subject>_data JSONB NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Create the mail_content_unique_parameters.parameters_data table
CREATE TABLE IF NOT EXISTS <schema_name>.<table_name> (
    <subject>_id SERIAL PRIMARY KEY,
    <subject>_data JSONB NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Create the send_ready_mail.mail_data table
CREATE TABLE IF NOT EXISTS <schema_name>.<table_name> (
    <subject>_id SERIAL PRIMARY KEY,
    <subject>_data JSONB NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);


GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA <schema_name> TO <user_name_postgresql>;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA <schema_name> TO <user_name_postgresql>;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA <schema_name> TO <user_name_postgresql>;

```

(save the file Ctrl + x)


2, We will run these command from this file using the following "psql" command: 

```bashpsql
psql -U postgres -d <database_name> -a -f init-db.psql
psql -U postgres -d example_database -a -f init-db.psql

```

3, Verify the schema was created succesfully on the database "yahoo_mail_sender" 

verify by running following cmd, we should already be within the "psql" terminal window after running above commnad. 

```bash
\dn
```

This should output the following:

```bash
                  List of schemas
              Name              |       Owner       
--------------------------------+-------------------
 <schema>                       | postgres
 <schema>                       | postgres
 <schema>                       | postgres
(3 rows)

```


## Granting all needed privledges to the user "yahoo_mail_sender" corresponding to database ! 
Final step is checking if permissions where granted successfully the specified user on specified database.

1, Check if table schemas is present and correct :D 

```bash
-- Check if the tables exist
SELECT table_schema, table_name
FROM information_schema.tables
WHERE table_schema IN ('<schema_name>', '<schema_name>', '<schema_name>');

```
Ouput should be:

```bash
          table_schema          |   table_name    
--------------------------------+-----------------
 <schema_name>                  | <table_name>
 <schema_name>                  | <table_name>
 <schema_name>                  | <table_name>
(3 rows)

```

2, Verify permissions have successfully been granted to the user on corresponding datbase : 

Run following cli cmd from within the "psql example_database" terminal window. (example_database=#)

```bash
SELECT grantee, table_schema, table_name, privilege_type
FROM information_schema.table_privileges
WHERE grantee = 'example_database';

```


Ouput:

```bash
      grantee      |          table_schema          |   table_name    | privilege_type 
-------------------+--------------------------------+-----------------+----------------
 example_database | example_schema                  | example_table   | INSERT
 example_database | example_schema                  | example_table   | SELECT
 example_database | example_schema                  | example_table   | UPDATE
 example_database | example_schema                  | example_table   | DELETE
 example_database | example_schema                  | example_table   | TRUNCATE
 example_database | example_schema                  | example_table   | REFERENCES
 example_database | example_schema                  | example_table   | TRIGGER
 example_database | example_schema                  | example_table   | INSERT
 example_database | example_schema                  | example_table   | SELECT
 example_database | example_schema                  | example_table   | UPDATE
 example_database | example_schema                  | example_table   | DELETE
 example_database | example_schema                  | example_table   | TRUNCATE
 example_database | example_schema                  | example_table   | REFERENCES
 example_database | example_schema                  | example_table   | TRIGGER
 example_database | example_schema                  | example_table   | INSERT
 example_database | example_schema                  | example_table   | SELECT
 example_database | example_schema                  | example_table   | UPDATE
 example_database | example_schema                  | example_table   | DELETE
 example_database | example_schema                  | example_table   | TRUNCATE
 example_database | example_schema                  | example_table   | REFERENCES
 example_database | example_schema                  | example_table   | TRIGGER
(21 rows)

```


Su







