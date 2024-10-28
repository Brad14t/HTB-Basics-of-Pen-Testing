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

* In this Hack The Box walkthrough, an Nmap scan is used to detect an open rsync service on port 873 of the target machine. By connecting to the rsync server as an anonymous user and syncing files from the "public" directory, the user retrieves a flag.txt file, demonstrating the utility of rsync for file transfer and synchronization between systems.

* This exercise highlights the fundamentals of enumerating services and leveraging file-sharing protocols like rsync for ethical hacking, setting a solid foundation for further cybersecurity learning.

<ins>Taks:<ins>

What is the default port for rsync?

* 873

How many TCP ports are open on the remote host?

* `nmap -p- --min-rate=1000 -sV [target IP]` - adding the (--min-rate=1000) this speeds up the scan, higher the number the faster the scan

What is the protocol version used by rsync on the remote machine?

![1](https://github.com/user-attachments/assets/03899578-3914-4666-8023-07c41f38cf87)

What is the option to only list shares and files on rsync? (No need to include the leading -- characters)

* `list-only` - full code: rsync --list-only [target IP]

![1](https://github.com/user-attachments/assets/89fa8853-8a83-417a-9852-6f00f643fbb5)

* I will take a look inside the public share, in the hopes to find the flag, to do this, use this command:

* `rsync --list-only {target_IP}::public`

* last step is to copy/sync this file to our local machine. To do that, we simply follow the general syntax by specifying the SRC as public/flag.txt and the DEST as flag.txt to transfer the file to our local machine.

* rsync {target_IP}::public/flag.txt flag.txt

Now I have the flag file saved to my machine, to finish just `cat` the flag.txt file.

# Tier 1

**Appointment**

<ins>Summary:<ins>

*  Learn to identify vulnerabilities in web applications by scanning for open ports and exploring HTTP services. They practice exploiting outdated software and uncovering sensitive information in poorly configured applications, emphasizing the importance of version management and secure configurations. This module helps develop skills in basic web exploitation and introduces fundamental concepts in recognizing common web security flaws.

<ins>Taks:<ins>

What does Nmap report as the service and version that are running on port 80 of the target?

* `sudo nmap -sC -sV` - sC is new, this performs a script scan using the default set of scripts. It is equivalent to -script=default. Some of the scripts in this category are considered intrusive and should not be run against a target network without permission.

![1](https://github.com/user-attachments/assets/cad95fcf-ce02-4ef9-95cc-011b5f385147)

* We will be attempting to perform a SQL injection attack, first go to the target IP in the broswer.

![1](https://github.com/user-attachments/assets/d183d630-1ea2-4090-b8ce-dc76fdea5f15)

For the username use: `admin'#` , this command = `SELECT * FROM users WHERE username = 'admin'#' AND password = 'some';`

For password use: anything the password is bypassed.

![1](https://github.com/user-attachments/assets/66849b83-88cb-4af2-9306-46cd1b0c247f)

![2](https://github.com/user-attachments/assets/1c043299-5461-4ca8-8a54-4bf8d85503e2)







