# DJ-Postgres-cheatsheet
**In this Readme file , you will learn how to integrate a django server to a Postgresql database in the traditional and authentic way. For this we have to set the attributes of the POSTGRESQL database engine, so let's start**

**Installing *psycopg* ( an adapter to connect postgresql with any sql supported backend server )**

**This gives the updated version of Psycopg Driver (For Windows /  For Linux - If You have Python3 (Python3.12 may have the support now)**
```bash
  pip install psycopg[binary,pool]
```
**_Psycopg2 is obsolete now !!_**

Now we have to create a DJANGO PROJECT/APP structure , let's do this by these following commands :
```bash
  django-admin startproject proj_name
```
For benificial purposes , keep the contents of the current project folder outside of it , then
```bash
  python manage.py startapp app_name
```
In PSQL shell------

:exclamation: :exclamation: **Under the postgres (parent database) database**
```bash
  CREATE DATABASE db_name;
```
```bash
  CREATE USER username WITH PASSWORD 'password';
```
Now switch to the newly created database 
```bash
  \c db_name
```
:exclamation: :exclamation: **Under the new database db_name**

*First of all , we have to create a new schema inside the new database*
```bash
  CREATE SCHEMA schema_name AUTHORIZATION username
```
Then, we have to apply some commands to set the client-encoding parameter , timezone etc
https://docs.djangoproject.com/en/4.2/ref/databases/#postgresql-notes 

(In future this docs may be invalid deu to upgraded versions of django)
```bash
  ALTER ROLE username SET client_encoding TO 'utf8';
```
```bash
  ALTER ROLE username SET default_transaction_isolation TO 'read committed';
```
```bash
  ALTER ROLE username SET timezone TO 'UTC';
```
Now  we have to provide the *search_path* for the new database
```bash
  ALTER ROLE username IN DATABASE db_name set search_path=schema_name ;
```
```bash
  \c postgres
```
Now we have to provide all priviledges to our new db (Under **postgres**)
```bash
  GRANT ALL PRIVILEGES ON DATABASE database TO username;
```
:exclamation: :exclamation: **Inside the SETTINGS.PY** of the django PROJECT folder
```bash
    DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'db_name',
        'USER': 'username',
        'PASSWORD': 'password',
        'HOST': '127.0.0.1',
        'PORT': '5432',
    }
  }
```
Now we have to *migrate* all the tables to our postgres database
```bash
  python manage.py makemigrations
```
```bash
  python manage.py migrate
```
To see the changes and the default django tables inside the Postgre database , *RESTART* the Psql shell and
Enter Into the new database from root (Not from parent database 'postgres' ,  just when you start - write db_name, username, password and enter)

**For Linux Based Distros**
```bash
  sudo -i -u postgres

  psql -h <host> -U <user> -d <database_name>
```

```bash
  \dt
```

**For Linux Based Distros**
```bash
  SELECT * FROM pg_catalog.pg_tables; (To see all tables in all databases under root user)

  SELECT table_name FROM information_schema.tables
  WHERE table_name = '<table_name>'; (To see if any table at that name exists)

  SELECT * FROM "table_name" ; (Sometimes without "" , data will not be shown)

```
