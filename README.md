# sample-django-postgres
A sample PIKU project using Django and Postgres.

This is a sample Django project that uses the https://pypi.org/project/dj-database-url/ library to connect to any number of production Databases

## Installation

### Clone repo
```
git clone https://github.com/jfmatth/sample-django-postgres.git
```
### Virtual Environment
Build the virtual environment and install dependencies

```
pipenv shell
pipenv install
```

## Usage
Once you have the environment installed, you can check that it's working (without DATABASE_URL) and create the local SqLite DB

```
python manage.py migrate
```

This should create a local SQLITE database called db.sqlite3 in the folder

If you now define your ```DATABASE_URL``` pointing to your Postgres, MySQL, SQLServer based on the dj_database_url specs, for example:

```
export DATABASE_URL=postgres://USER:PASSWORD@HOST:PORT/NAME
```

Run ```python manage.py migrate``` again and it should create the database on (postgres in this case)


## Running on PIKU

Publish your app to Piku  
```
git remote add piku piku@your_server:pypostgres
git push piku master
```

Setup your DATABASE_URL on your PIKU instance
```
piku config:set DATABASE_URL=postgres://USER:PASSWORD@HOST:PORT/NAME
piku config:set NGINX_SERVER_NAME=<your FQDN here>
```
Migrate the DB (when needed)
```
piku run -- ./manage.py migrate --no-input
```

At this point you should ```piku shell``` into the instance and create the superuser

Once you've done all that, you should be able to just push changes from then on out

Enjoy

# Notes about setup

## Procfile
The procfile has a WSGI entry for Django and a CRON for clearning expired sessions

```
wsgi: sample.wsgi:application
cron: 0 0 * * * python manage.py clearsessions -v 3
```

