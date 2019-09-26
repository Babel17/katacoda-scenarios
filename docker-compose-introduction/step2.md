### kubernetes community dojo for docker presentation
---  
# before we begin:

## Quick docker recap

Using all utilities which you have learnt during previous docker dojo run 2 containers django + database (mysql) and
connect one to another.

# Database
Lets create database container (remembering about volumes as we want to keep data created in our database)

`docker run -v $(pwd)/mysql_data_dir:/var/lib/mysql --name database -e MYSQL_DATABASE=db -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_USER=django -e MYSQL_PASSWORD=django -d mysql:5.7`{{execute}}

# Web
For this training purpose I have created django Image in dockerhub:
`docker run --name app1 -p 80:8000 greg4fun/django:2.2.5`{{execute}}

Access web and check stdout in our console (It will by default say you need to add host to allowed hosts but will actually display django server error message)

stop ctrl+c and remove the container

`docker rm app`{{execute}}

Run it in detached mode (note this django image will use mysql by default):

`docker run -d --rm --name app -p 80:8000 greg4fun/django:katacoda`{{execute}}
 

To check database connection run migrate - command responsible for creating/updating database tables.

`docker exec app python3 manage.py migrate`{{execute}}

the command will fail as there is no connection to the database the containers are not even withing same network.

We now have 2 containers running:
1. The database mysql contaner 
2. The main web app django container


We need our application to be able to connect to the databse.

---
