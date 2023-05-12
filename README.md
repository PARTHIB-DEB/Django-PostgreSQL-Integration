# DJ-Postgre-cheatsheet
**In this Readme file , you will learn how to integrate a django server to a Postgresql server. For this we have to set the attributes of the POSTGRESQL database engine, so let's start**

Installing *psycopg2* ( an adapter to connect to postgresql with any sql supported backend server )
```bash
  pip install psycopg2
```
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
Then, we have to apply some commands to set the slient-encoding parameter , timezone etc
https://docs.djangoproject.com/en/4.2/ref/databases/#postgresql-notes
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
```
  \dt
```
