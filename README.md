# set-up_postgresql_manually
This is a guide to setting up postgresql service correctly on Kali Linux.
Make sure you are in root user when following this guide. ("sudo su")

## Download PostgreSQL .

```bash
sudo apt-get update
sudo apt-get install postgresql
```

## Confirm successfull download.

```bash
psql --version
```

## Confirm all needed files are present.

```
cd /etc/postgresql/16/main
```
The number (16) is the version, could be different for you. 
The following files should be within the directory; 

```bash
conf.d  environment  pg_ctl.conf  pg_hba.conf  pg_ident.conf  postgresql.conf  start.conf
```

# Edit the "pg_hba.conf" file.
This file is responsible for authentication method to the specified databases. 

```bash
sudo nano pg_hba.conf 
```

Within the file you should scroll down all the way till you see:

```bash
# Database administrative login by Unix domain socket

# TYPE  DATABASE        USER            ADDRESS                 METHOD
```

Here we will add a new entry with method trust for our default postgres user, so we set a password for the user in next few commands. h
Trust simply means no password is needed to acces the "psql" terminal window for the specified user. 
:

```bash
# TYPE  DATABASE  USER     ADDRESS     METHOD
local   all       postgres             trust

```

* NOTE that only ONE METHOD for combination of database and user CAN HAVE EFFECT! 
if a multiple entrys are present like so; it will fail. 

```bash
# TYPE  DATABASE  USER     ADDRESS     METHOD
local   all       postgres             trust
local   all       postgres             md5
```

After adding the single entry we will save the file. Press: 

```bash
Ctr (+) x
```


## Set password for user 'postgres' 

1, login to "psql" terminal window:

```bash
psql -U postgresql 
```

2, set the actuall password for the specified user: 

```bash
ALTER USER postgres WITH PASSWORD 'postgres';

```

In our use case we simply set the password for the user "postgres" to "postgres" .

3, quit the psql terminal:

```bash
\q
```


