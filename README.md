# DJ-Postgre-cheatsheet
## In this Readme file , you will learn how to integrate a django server to a Postgresql server. For this we have to set the attributes of the POSTGRESQL database engine, so let's start

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
Now we have to make some changes in the *Settings.py* file of the project folder , change the default *DATABASES* structure , by this
```bash
  DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'mydatabase(change it if required)',
        'USER': 'mydatabaseuser(change it if required)',
        'PASSWORD': 'mypassword(change it if required)',
        'HOST': '127.0.0.1',
        'PORT': '5432',
    }
}
```
