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

## Confirm all needed files are present.h

1, Navigate to postgres-dir with configuration files.

```
cd /etc/postgresql/16/main
```

The number (16) is the version, could be different for you. 

2, The following files should be within the directory; 

```bash
conf.d  environment  pg_ctl.conf  pg_hba.conf  pg_ident.conf  postgresql.conf  start.conf
```

## Edit the "pg_hba.conf" file.

1, This file is responsible for authentication method to the specified databases:

```bash
sudo nano pg_hba.conf 
```

2, Within the file you should scroll down all the way till you see:

```bash
# Database administrative login by Unix domain socket

# TYPE  DATABASE        USER            ADDRESS                 METHOD
```

3, Here we will add a new entry with method trust for our default postgres user, so we can set a password for the user in next few commands. 
Trust simply means no password is needed to acces the "psql" terminal window for the specified user. :

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

4, After adding the single entry we will save the file. Press: 

```bash
Ctr (+) x
```

## Restart the PostgreSQL service.
1, For the changes in the "pg_hba.conf" file to take effect we need to restart the PostgreSQL service:

```bash
systemctl restart postgresql 
```

2, Check if PostgreSQL service has restarted successfully: 

```bash
systemctl status postgresql 
```

## Set password for user 'postgres' .

1, login to "psql" terminal window:

```bash
psql -U postgresql 
```

2, set the actuall password for the specified user: 

```bash
ALTER USER postgres WITH PASSWORD 'postgres';

```

In our use case we simply set the password for the user "postgres" to "postgres" . You can set it to whatever you like. 

3, quit the psql terminal:

```bash
\q
```

## Confirm password for user "postgres" is set successfull! 
1, Enter the password promted for after running the following command, this is the password you just set corresponding to the user postgres. 

```bash
psql -U postgres -W 
```

2, If you get logged into the "psql" terminal window... You successfully set the password :D 

3, Quit the "psql" terminal once more: 

```bash
\q
```

## Remove the old entry and add new to "pg_hba.conf" 
Because we have successfully set-up a password for the user "postgres" we won't need to use "trust" authentication method for the "postgres" user anymore.

1, Edit the "pg_hba.conf" file again:

```bash
nano pg_hba.conf 
```

2, Deactivate the line you just added within the config-file. We do this by simply placing a "#" infornt of the line: 

```bash
# TYPE  DATABASE  USER     ADDRESS     METHOD
#local   all       postgres             trust
```

3, Add new entry with password authentication for the user "postgresql" . We will use "md5" as it will store our password encrypted. 

```bash
# TYPE  DATABASE  USER     ADDRESS     METHOD
#local   all       postgres             trust
local   all       postgres             md5

```

4, Save the file we are currently editing once more! 

```bash
Ctrl (+) x
```


## Restart the PostgreSQL service one final time! 

```bash
systemctl restart postgreSQL 
```

Now all our changes to the "ph_hba.conf" file will have taken effect again!


## Confirm our "set-up_postgresql_manually" guide has worked! 

1, Run the following command to login to the "psql" terminal with user "postgres":

```bash
psql -U postgres
```

2, Enter the password when promted.

3, Login succeeded!! :D :D 

* If you get a Authentication error please follow the guide once more from the start. 

