# HTB

# Tier 0

**Redeemer**

<ins>Summary:<ins>

* This lab focuses on enumerating a Redis server remotely and then dumping its database in order to retrieve the flag.

* Redis - Remote Dictionary Server -  Interacted with Redis server.

<ins>Commands:<ins>

* `ping`
* `nmap -p- -sV` - This command runs an Nmap scan that probes all 65,535 TCP ports (-p-) and attempts to detect the version of services running on open ports (-sV).
* `sudo apt install redis-tools` - Downloading redis CLI

**Explosion**

<ins>Summary:<ins>

* CLI and GUI remote access tools like `xfreerdp`

* xfreedrp output adjustment

<ins>Commands:<ins>

* `sudo nmap -sV [target IP]`
* `xfreerdp /v:[target IP] /cert:ignore /u:Administrator`

**Preignition**

<ins>Summary:<ins>

* WordPress setup and IOC (Indicators of Compromise)
* dir busting is the name used for directory brute forcing
* Using gobuster 

<ins>Commands:<ins>

* `sudo apt install golang-go` - this installs go
* `sudo apt install gobuster` - installs gobuster
* gobuster commands: `dir`, `-w` (specify a wordlist), `-u`(target IP)
  
![1](https://github.com/user-attachments/assets/1f1284e7-cec4-45f0-ab60-1f1c9f92aa8d)

![1](https://github.com/user-attachments/assets/bf2fe490-f86c-49b6-bf90-25f020a113b1)

**Monogod**

<ins>Summary:<ins>

Run a complete TCP port scan using sudo nmap -p- -sV [target IP] to identify open ports and their associated services on the target machine, revealing MongoDB on port 27017.

MongoDB is a NoSQL database, and to access it, use the Mongo shell (mongosh) to connect and list databases with show dbs; use show collections to view collections in a database and db.flag.find().pretty() to display documents in the flag collection in a readable format.

<ins>Taks:<ins>

How many TCP ports are open on the machine?

1. `ping [target IP` - Confirms target is reachable.
2. `sudo nmap -p- -sV [target IP]` - This searches all TCP ports and the version of services running on open ports.

![1](https://github.com/user-attachments/assets/81d2f888-9828-459a-a392-57693ef83da9)

Which service is running on port 27017 of the remote host?

![1](https://github.com/user-attachments/assets/202472d6-a192-4406-a90c-9de7912e9207)

What type of database is MongoDB? (Choose: SQL or NoSQL)

NoSQL

What is the command name for the Mongo shell that is installed with the mongodb-clients package?

mongosh

What is the command used for listing all the databases present on the MongoDB server? (No need to include a trailing.

1. To interact with MongoDB server 1st I install MongoDB shell utillity.

* `curl -O https://downloads.mongodb.com/compass/mongosh-2.3.2-linux-x64.tgz`

2. Extract contents using `tar` utility

* `tar xvf mongosh-2.3.2-linux-x64.tgz`

3. Locate mongosh binary (mongosh-2.3.2-linux-x64/bin)

![1](https://github.com/user-attachments/assets/a2101fcf-5185-49d9-b3ae-458220e7d218)

4. Change directory into that binary.

* `cd mongosh-2.3.2-linux-x64/bin`

5. Connect to the MongoDB sever.

* ./mongosh mongodb://{target_IP}:27017

![1](https://github.com/user-attachments/assets/05069cc8-6a2c-4730-a27e-b73b2a975963)

Answer: show dbs (list databases present)

What is the command used for listing out the collections in a database?

show collections

What is the command used for dumping the content of all the documents within the collection named flag in a format that is easy to read?

db.flag.find().pretty()

This code queries all documents in the flag collection of the db database and formats the output in a readable (pretty-printed) JSON format.

![1](https://github.com/user-attachments/assets/3c3057dc-d120-4302-a999-804473e46e77)

**Synced**

<ins>Summary:<ins>

Learned the basics of rsync, and why its faster for use cases compaired to FTP.

<ins>Taks:<ins>

What is the default port for rsync?

873

How many TCP ports are open on the remote host?

1. `nmap -p- --min-rate=1000 -sV [target IP]` - adding the (--min-rate=1000) this speeds up the scan, higher the number the faster the scan

What is the protocol version used by rsync on the remote machine?

![1](https://github.com/user-attachments/assets/03899578-3914-4666-8023-07c41f38cf87)


















