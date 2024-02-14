# Manual installation of a Full-stack application to a cloud Ubuntu Linux server

2024-02-14

Goals: 

1. To have the application running in public internet for customer to test it.
1. To show the manual commands so that people who need to dockerize it cand read the commands, and use/adapt them for their docker or docker-compose files

Installed components:

* Ubuntu Linux with Linux users
* Database (MariaDB + root user, adding database based on given SQL to create schema/database, backend server db user, tables, test data)
* Backend (Node.js, Express, Knex, mariadb driver)
* Frontend (React app, but after the npm build, so not the development-time server)

All as well based on latest versions from GitHub repos' main branches as possible.

## 1. Installing the Ubuntu Linux VM and its Linux users

[Followed the instructions from here](CSC_virtual_machine_and_user_creation.md)

## 1. Installing MariaDB server to the Ubuntu Linux VM

	Plus the schema 'casedb' and user 'jyser3' using that schema.

	[Followed the instructions from here](CSC_virtual_machine_and_user_creation.md)

## 1. Creating the needed database tables, test data and stored procedures

Inside mysql command line tool:
```
SOURCE ~/my_repo/Database/SQLScripts/000_createAllDb.sql;
```

Or run SQL script files from outside of the database like this:
(You'll need to say "USE casedb;" in the beginning of the file )

```
$ mysql -h localhost -u jyser3 -p < ~/my_repo/Database/SQLScripts/000_createAllDb.sql;
```

## 1. from console take connection using jyser3 and test that you see the tables and data

Just to see you are in control of the connection making and the opened port 3306 is available

```
$ mysql -h localhost -u jyser3 -p

mysql> USE casedb;
...
mysql> SELECT * FROM SubjectEquipment;
```