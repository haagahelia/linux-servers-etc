# Juhani's collection of steps for mainly teachers (admins) and sometimes students (developers, users) to utilize Ubuntu Linux servers (e.g. in CSC cPouta cloud)

This first document is CSC cloud dependent.

## CSC - Setting up the course 'project' in my.csc.fi

1. teacher sets up a project and asks cPouta resources for it and possibly for additional resources
1. write down the project number

## CSC - Creating virtual machine(s) in the https://pouta.csc.fi/dashboard/auth/login/

1. create a PPK public private key pair on your PC. Save the private key to your PC's safe folder. Copy the public key hash text. Remember also the keyphrase.

1. go to pouta, select correct project and Compute > Key Pairs + Import Public key + paste to the Public key box and name the key

1. go to pouta, Network > Security Groups > Create Security Group, set e.g. 22 allowed ingress (inbound), maybe not 3306 to protect the MariaDB/MySQL database

1. then go to Compute > Instances > Launch instance. Instance name something, Flavor e.g. standard.tiny to standard.large (depending of available resources and needs). 1 instance, Instance boot source: Boot from image. Image name e.g.: Ubuntu-22.04. Access & Security: add the the PPK key pair and the Security group.

1. Launch

1. go to pouta, Intances, select desired instance, then from right Associate floating IP. (Give the machine a real internet IP. They have their CSC internal IP too)

1. (You could create snapshot here or, soon)


## CSC - connecting as admin to create the real users

1. Use e.g. Putty to connect to the machine (using the associated floating IP, port 22, user ubuntu, ssh authentication with the private key file)

1. Accept a new computer fingerprint/hash. Give the passphrase from the key creation time.

1. adduser juser1, write password e.g. to Teams protected folder excel (secrets excel, put there also the server IP, usernames, passwords etc.) and then copy paste the password to the command line when needed.
1. Add that juser1 to sudo group by editing nano /etc/group
1. adduser juser2, write password to excel, don't add this user to sudo group
1. Allow password authentication by editing nano /etc/ssh/sshd_config
1. At least now might be good idea to take a snapshot (for restarting/resetting this server, or for multiplying one model server to be many)

Thus, Once in, run 
	
```
sudo apt update 
sudo apt upgrade

sudo adduser jyser1
sudo nano /etc/group    // add jyser1 to line   sudo:ubuntu,jyser1
sudo adduser jyser2
```

allow password authentication (less secure, easier in big student group)

```
sudo nano /etc/ssh/sshd_config      // put password auth to yes
sudo systemctl restart sshd
```

=> Make a snaphot now so that we can redo this easily, or duplicate other VM instances based on above

## CSC - then connecting as sudo user juser1 to install server software like e.g MariaDB, Node, nginx, ...next documents
