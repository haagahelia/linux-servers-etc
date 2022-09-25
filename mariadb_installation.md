
# MariaDB installation

## 1. Installing MariaDB server to a VM
```
sudo apt update
sudo apt install mariadb-server
sudo systemctl start mariadb.service
sudo mysql_secure_installation  
   // (runs a script, select all the safest options)
   // includes setting mariadb root password, 
   // write all secrets to safe excel sheet / secrets document (in school in Teams)

// => Possibly take now a snaphot of that in CSC cloud dashboard
```

Is MariaDB server running? (Or more exactly is any process listening to port 3306)
```
sudo lsof -i :3306
```

## 2. Creating a SCHEMA (='database', namespace for database objects like tables) called generically   *casedb*

DB stuff as db root:
```
$ sudo mysql -h localhost -u root -p      
(plus paste password after that command)
CREATE SCHEMA casedb;
```

## 3. What DB users we want to create?

| user | explanation |
| :--: | :-----: |
| root | all rights to the database |
| jyser3 | normal database developer, all rights to needed schema. Could be used in clients like HeidiSQL, or our app |

### What DB users we could create too?

| user | explanation |
| :--: | :-----: |
| dba | A bit less powerful admin user |
| webapp_admin | db user a web application backend would use to run admin features |
| webapp_normal | db user a web application backend would use to run normal user features |


creating user jyser3 with passwd - Again as the db root user
```
CREATE USER 'jyser3' IDENTIFIED BY 'xyzxyzxyz';

GRANT ALL ON casedb.* TO jyser3 WITH GRANT OPTION; // granting jyser3 needed privileges on casedb

QUIT;
```  

## 4. Continuing to create needed database (=tables) using a less powerful DB user

DB creation and testing stuff as normal db user:
```
$ mysql -h localhost -u jyser3 -p

USE casedb;

CREATE TABLE Testi 
(	id INTEGER AUTO_INCREMENT, 
	firstName VARCHAR(20),
	lastName VARCHAR(20),

	CONSTRAINT pk_testi PRIMARY KEY (id) 
);

DESCRIBE Testi;

INSERT INTO Testi 
	(firstName, lastName) 
		VALUES
	("Mike","Smith");

SELECT * FROM Testi;

QUIT;
```


## 4.a Do we want to open port 3306 from Firewall?

That would make unnecessary to use the SSH tunnel (that uses port 22, no need for 3306)

## 5.b from PC, take connection with e.g. HeidiSQL, using jyser3

Just to see you are in control of the connection making and the opened port 3306 is available

---

OR

--- 


## 4b. from PC, create SSH tunnel PC :3306 via CSC :22 to CSC :3306, using jyser1 or 2
(No need to open port 3306 from Firewall if create the tunnel!)

Tunnel created either in Putty connection settings, ssh > auth > tunnel,
Or from e.g. HeidiSQL connection settings,

Or like this from GitBash on your own computer:

```
ssh -f jyser1@here_the_ip_address_of_the_database_server -L 3306:localhost:3306 -N
```

## 5b. from PC, take connection "to PC=localhost:3306" with e.g. HeidiSQL, jyser3

Note! Do not use the ip address of the database server anymore, but just localhost:
 127.0.0.1

The tunnel takes you from localhost to the remote.



