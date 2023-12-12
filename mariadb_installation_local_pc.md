# MariaDB installation to local PC

## 1. Download MariaDB server 

I prefer the .zip version with no processes nor-services running without starting them explicitly manually.

- Download the .zip version of latest stable (not beta, not RC=Release Candidate) MariaDB server from https://mariadb.org/download/ 
- Extract possibly first to the Downloads folder
- then cut and paste so that the bin, lib, share etc. folders will be inside e.g. c:\users\your_username\mariadb folder 
- then delete the extra files like the .zip and the extracted sceleton folder

## 2. Create the database instance (datafiles with metadata about the server, root user, configs ... )
- Information here https://mariadb.com/kb/en/mysql_install_dbexe/
- Information here https://mariadb.com/kb/en/installing-system-tables-mariadb-install-db/
- go to the \bin folder and start e.g. GitBash console there
```
> ./mariadb-install-db --port=3306
```
- created by default the data files to the \data subfolder
- ran the bootstrap script(s)
- created the my.ini file in \data subfolder
- set the mariadbd/mysqld server to later listen to port 3306 or what you specify
- removed the default user
- did NOT set the root password like this:     --password=secret_root_pw
- As it would have went to the console history that way, root password still empty

**Write all secrets to safe excel sheet / secrets document (in school in Teams)!**


## 3. Start the database server (DBMS)
- Information here https://mariadb.com/kb/en/starting-and-stopping-mariadb/
- ... and here https://mariadb.com/kb/en/mariadbd-options/
- 
```./mariadbd --console```

You can leave that console open and kill the DBMS process there with Ctrl+c.

Or start it with the & at the end and then kill the process different way. Using Windows services would be an option too, but I like "cold file" installation.

Is MariaDB server running? (Or is any process listening to port 3306)
```
 ps -a | grep 'mariadbd'
 netstat -aof | grep ':3306'
 sudo lsof -i :3306
```

```
kill -9 12345    //to kill the process with the PID. Some tools call the Windows process id also PID, but you want to use the 'Linux'/bash one.
```

## 4. Take admin connection from console

Now something strange. Connecting from GitBash console failed, but connecting from Powershell worked. Security mechanism related, or path definition related? (= is there a broken mysql/mariadb cmd line tool somewhere in the defined path? On the other hand ./ was used to refer to the current folder)...

In the \bin folder, tryin to connect to the DBMS as root user, asking for password to be prompted. 

```./mariadb -h localhost -u root -p```

Immediately changing root password to some secure password that looks like a hash and is long enough.

```
ALTER USER 'root'@'localhost' IDENTIFIED BY 'secret_root_pw_HERE';
```

## 5. Creating a SCHEMA (='database', namespace for database objects like tables) called generically   *casedb*

DB stuff as db root:
```
CREATE SCHEMA casedb;
```

## 6. What DB users we want to create?

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
CREATE USER 'jyser3'@'localhost' IDENTIFIED BY 'xyzxyzxyz';

GRANT ALL ON casedb.* TO 'jyser3'@'localhost' WITH GRANT OPTION;    /* granting jyser3 needed privileges on casedb */

QUIT;
```  

## 7. Continuing to create needed database (=tables) using a less powerful DB user

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
Or run SQL script files inside the database console tool like this:

```
SOURCE ~/my_repo/Database/SQLScripts/00_create_tables.sql;
```

Or run SQL script files from outside of the database like this:
(You'll need to say "USE casedb;" in the beginning of the file )

```
$ mysql -h localhost -u jyser3 < ~/my_repo/Database/SQLScripts/00_create_tables.sql;
```


## 8.a Do we want to open port 3306 from Firewall?

That would make unnecessary to use the SSH tunnel (that uses port 22, no need for 3306)

## 8.b.1 from PC, take connection with e.g. DBeaver, HeidiSQL, using jyser3

Just to see you are in control of the connection making and the opened port 3306 is available

---

OR

--- 


## 8.b.2 from PC, create SSH tunnel PC :3308 to PC :3306, using your own Windows user. Thus you can make your local :3306 DBMS to appear at port :3308 and thus not change e.g. backend server database settings if you have :3308 there.

Tunnel created like this from GitBash on your own computer:

```
ssh -f localhost -L 3308:localhost:3306 -N
```

## 9. from PC, take connection "to PC=localhost:3308" with e.g. DBeaver, HeidiSQL, using database user jyser3

Note! Do not use the ip address of the database server anymore, but just localhost:
 127.0.0.1

The tunnel takes you from localhost to the remote. Although in e.g. DBeaver this might be confusing, if you create the tunnel INSIDE DBeaver. Then you DO use the remote IP address, but on the separate SSH tab tell that you use a tunnel there.

But, all in all, when all details are correct. We have got things working. So you just have to be patient. Draw a picture. Try to find out where that one mistake or two mistakes are.



