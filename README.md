# Django-X-Docker-

Steps Running Django App On Visual Studio Code.

Create a project directory and used the Git to activate the environment.

 In the folder created, i activated the environment using gitbash with this command(python -m venv env) by right clicking and using the the GitBash GUI.


Then run the following command

python -m django --version
django-admin startproject projectname
python manage.py runserver 

Creating the Django App

python manage.py startapp mydjangoApp

 In the cretaed App file looks likes this 
mydjangoApp/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py

On the views.py kindly insert the following 

from django.http import HttpResponse


def index(request):
    return HttpResponse("Hello, world. You're at the mydjangoApp index.")

In the mydjangoApp file, create a new urls.py file  and insert the following (mydjangoApp/urls.py)

from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
]

Adding Docker files to the project

-- Create a dockerfile in the same directory as manage.py
Inserted the following, then save and close

FROM python:3
ENV PYTHONUNBUFFERED=1
WORKDIR /code
COPY requirements.txt /code/
RUN pip install -r requirements.txt
COPY . /code/


-- Create a requirements.txt in the project directory
Inserted the following, then save and close.

Django>=3.0,<4.0
psycopg2-binary>=2.8


-- Create a file called docker-compose.yml 

Inserted the following, then save and close.

version: "3.9"
   
services:
  db:
    image: postgres
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
  web:
    build: .
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/code
    ports:
      - "8000:8000"
    depends_on:
      - db

To connect to the database, In the project directory,edit the kennydjangoapp/settings.py file.
Replace the DATABASES = ... with the following:

# settings.py
   
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'postgres',
        'USER': 'postgres',
        'PASSWORD': 'postgres',
        'HOST': 'db',
        'PORT': 5432,
    }
}

On the terminal and cd into the project directory where we have manage.py and run the following command:

docker compose up

docker compose up --build

docker ps

docker container ls















