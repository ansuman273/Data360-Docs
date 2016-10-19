Project setup in Digital Ocean
==============================

These are the software requirements and their set up process in the server(Digital Ocean)

MySQL Installation
------------------

Describe MySQL Installation


MongoDB Installation (Mongodb Version-2.6,Ubuntu Version-16.04)
--------------------
How to  Install MongoDB on Ubuntu 16.04?

Introduction:

MongoDB is a free and open-source NoSQL document database used commonly in modern web applications. This tutorial will help you set up MongoDB on your server for a production application environment.

Prerequisites

To follow this tutorial, you will need:

One Ubuntu 16.04 server set up by following this initial server setup tutorial, including a sudo non-root user
Step 1 — Adding the MongoDB Repository

MongoDB is already included in Ubuntu package repositories, but the official MongoDB repository provides most up-to-date version and is the recommended way of installing the software. In this step, we will add this official repository to our server.

Ubuntu ensures the authenticity of software packages by verifying that they are signed with GPG keys, so we first have to import they key for the official MongoDB repository.

sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
After successfully importing the key, you will see:

Output
gpg: Total number processed: 1
gpg:               imported: 1  (RSA: 1)
Next, we have to add the MongoDB repository details so apt will know where to download the packages from.

Issue the following command to create a list file for MongoDB.

echo "deb http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
After adding the repository details, we need to update the packages list.

sudo apt-get update

Step 2 — Installing and Verifying MongoDB
Now we can install the MongoDB package itself.

sudo apt-get install -y mongodb-org
This command will install several packages containing latest stable version of MongoDB along with helpful management tools for the MongoDB server.

In order to properly launch MongoDB as a service on Ubuntu 16.04, we additionally need to create a unit file describing the service. A unit file tells systemd how to manage a resource. The most common unit type is a service, which determines how to start or stop the service, when should it be automatically started at boot, and whether it is dependent on other software to run.

We'll create a unit file to manage the MongoDB service. Create a configuration file named mongodb.service in the /etc/systemd/system directory using nano or your favorite text editor.

sudo nano /etc/systemd/system/mongodb.service
Paste in the following contents, then save and close the file.

/etc/systemd/system/mongodb.service
[Unit]
Description=High-performance, schema-free document-oriented database
After=network.target

[Service]
User=mongodb
ExecStart=/usr/bin/mongod --quiet --config /etc/mongod.conf

[Install]
WantedBy=multi-user.target
This file has a simple structure:

The Unit section contains the overview (e.g. a human-readable description for MongoDB service) as well as dependencies that must be satisfied before the service is started. In our case, MongoDB depends on networking already being available, hence network.target here.

The Service section how the service should be started. The User directive specifies that the server will be run under the mongodb user, and the ExecStart directive defines the startup command for MongoDB server.

The last section, Install, tells systemd when the service should be automatically started. The multi-user.target is a standard system startup sequence, which means the server will be automatically started during boot.

Next, start the newly created service with systemctl.

sudo systemctl start mongodb
While there is no output to this command, you can also use systemctl to check that the service has started properly.

sudo systemctl status mongodb
Output
● mongodb.service - High-performance, schema-free document-oriented database
   Loaded: loaded (/etc/systemd/system/mongodb.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2016-04-25 14:57:20 EDT; 1min 30s ago
 Main PID: 4093 (mongod)
    Tasks: 16 (limit: 512)
   Memory: 47.1M
      CPU: 1.224s
   CGroup: /system.slice/mongodb.service
           └─4093 /usr/bin/mongod --quiet --config /etc/mongod.conf
The last step is to enable automatically starting MongoDB when the system starts.

sudo systemctl enable mongodb
The MongoDB server now configured and running, and you can manage the MongoDB service using the systemctl command (e.g. sudo systemctl mongodb stop, sudo systemctl mongodb start).

Step 3 — Adjusting the Firewall (Optional)
Assuming you have followed the initial server setup tutorial instructions to enable the firewall on your server, MongoDB server will be inaccessible from the internet.

If you intend to use the MongoDB server only locally with applications running on the same server, it is a recommended and secure setting. However, if you would like to be able to connect to your MongoDB server from the internet, we have to allow the incoming connections in ufw.

To allow access to MongoDB on its default port 27017 from everywhere, you could use sudo ufw allow 27017. However, enabling internet access to MongoDB server on a default installation gives unrestricted access to the whole database server.

in most cases, MongoDB should be accessed only from certain trusted locations, such as another server hosting an application. To accomplish this task, you can allow access on MongoDB's default port while specifying the IP address of another server that will be explicitly allowed to connect.

sudo ufw allow from your_other_server_ip/32 to any port 27017
You can verify the change in firewall settings with ufw.

sudo ufw status
You should see traffic to 27017 port allowed in the output.If you have decided to allow only a certain IP address to connect to MongoDB server, the IP address of the allowed location will be listed instead of Anywhere in the output.

Output
Status: active

To                         Action      From
--                         ------      ----
27017                      ALLOW       Anywhere
OpenSSH                    ALLOW       Anywhere
27017 (v6)                 ALLOW       Anywhere (v6)
OpenSSH (v6)               ALLOW       Anywhere (v6)
More advanced firewall settings for restricting access to services are described in UFW Essentials: Common Firewall Rules and Commands.

Conclusion
You can find more in-depth instructions regarding MongoDB installation and configuration in these DigitalOcean community articles.

Describe MongoDB Installation

OrientDB Installation
-----------------

How To Install and Configure OrientDB on Ubuntu 16.04

Introduction

Install MongoDB

1
Import the public key used by the package management system.

The Ubuntu package management tools (i.e. dpkg and apt) ensure package consistency and authenticity by requiring that distributors sign packages with GPG keys. Issue the following command to import the MongoDB public GPG Key:

sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
2
Create a list file for MongoDB.

Create the /etc/apt/sources.list.d/mongodb.list list file using the following command:

echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | sudo tee /etc/apt/sources.list.d/mongodb.list
3
Reload local package database.

Issue the following command to reload the local package database:

sudo apt-get update
4
Install the MongoDB packages.

You can install either the latest stable version of MongoDB or a specific version of MongoDB.

Install the latest stable version of MongoDB.

Issue the following command:

sudo apt-get install -y mongodb-org
Install a specific release of MongoDB.

Specify each component package individually and append the version number to the package name, as in the following example that installs the 2.6.9 release of MongoDB:

sudo apt-get install -y mongodb-org=2.6.9 mongodb-org-server=2.6.9 mongodb-org-shell=2.6.9 mongodb-org-mongos=2.6.9 mongodb-org-tools=2.6.9
Pin a specific version of MongoDB.

Although you can specify any available version of MongoDB, apt-get will upgrade the packages when a newer version becomes available. To prevent unintended upgrades, pin the package. To pin the version of MongoDB at the currently installed version, issue the following command sequence:

echo "mongodb-org hold" | sudo dpkg --set-selections
echo "mongodb-org-server hold" | sudo dpkg --set-selections
echo "mongodb-org-shell hold" | sudo dpkg --set-selections
echo "mongodb-org-mongos hold" | sudo dpkg --set-selections
echo "mongodb-org-tools hold" | sudo dpkg --set-selections
Previous versions of MongoDB packages use different naming conventions. See the 2.4 version of documentation for more information.

Run MongoDB

The MongoDB instance stores its data files in /var/lib/mongodb and its log files in /var/log/mongodb by default, and runs using the mongodb user account. You can specify alternate log and data file directories in /etc/mongod.conf. See systemLog.path and storage.dbPath for additional information.

If you change the user that runs the MongoDB process, you must modify the access control rights to the /var/lib/mongodb and /var/log/mongodb directories to give this user access to these directories.

1
Start MongoDB.

Issue the following command to start mongod:

sudo service mongod start
2
Verify that MongoDB has started successfully

Verify that the mongod process has started successfully by checking the contents of the log file at /var/log/mongodb/mongod.log for a line reading

[initandlisten] waiting for connections on port <port>
where <port> is the port configured in /etc/mongod.conf, 27017 by default.

3
Stop MongoDB.

As needed, you can stop the mongod process by issuing the following command:

sudo service mongod stop
4
Restart MongoDB.

Issue the following command to restart mongod:

sudo service mongod restart
5
Begin using MongoDB.

To begin using MongoDB, see Getting Started with MongoDB. Also consider the Production Notes document before deploying MongoDB in a production environment.

Later, to stop MongoDB, press Control+C in the terminal where the mongod instance is running.

Uninstall MongoDB

To completely remove MongoDB from a system, you must remove the MongoDB applications themselves, the configuration files, and any directories containing data and logs. The following section guides you through the necessary steps.

WARNING
This process will completely remove MongoDB, its configuration, and all databases. This process is not reversible, so ensure that all of your configuration and data is backed up before proceeding.
1
Stop MongoDB.

Stop the mongod process by issuing the following command:

sudo service mongod stop
2
Remove Packages.

Remove any MongoDB packages that you had previously installed.

sudo apt-get purge mongodb-org*
3
Remove Data Directories.

Remove MongoDB databases and log files.

sudo rm -r /var/log/mongodb
sudo rm -r /var/lib/mongodb
Solr Installation
-----------------


How to install and configure Solr 6 on Ubuntu 16.04

What is Apache Solr? Apache Solr is an open source enterprise-class search platform written in Java which enables you to create custom search engines that index databases, files, and websites. It has back end support for Apache Lucene. It can e.g. be used to search in multiple websites and can show recommendations for the searched content. Solr uses an XML (Extensible Markup Language) based query and result language. There are APIs (Applications program interfaces) available for Python, Ruby and JSON (Javascript Object Notation).
Some other features that Solr provides are:
Full-Text Search.
Snippet generation and highlighting.
Custom Document ordering/ranking.
Spell Suggestions.
This tutorial will show you how to install the latest Solr version on Ubuntu 16.04 LTS. The steps will most likely work with later Ubuntu versions as well.
Update your System
Use a non-root sudo user to login into your Ubuntu server. Through this user, you will have to perform all the steps and use the Solr later.

To update your system, execute the following command to update your system with latest patches and updates.
sudo apt-get update && apt-get upgrade -y

Install Ubuntu System updates.
Setting up the Java Runtime Environment
Solr is a Java application, so the Java runtime environment needs to be installed first in order to set up Solr.
We have to install Python Software properties in order to install the latest Java 8. Run the following command to install the software.
root@server1:~# sudo apt-get install python-software-properties
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
libpython-stdlib libpython2.7-minimal libpython2.7-stdlib python python-apt
python-minimal python-pycurl python2.7 python2.7-minimal
Suggested packages:
python-doc python-tk python-apt-dbg python-apt-doc libcurl4-gnutls-dev
python-pycurl-dbg python-pycurl-doc python2.7-doc binutils binfmt-support
The following NEW packages will be installed:
libpython-stdlib libpython2.7-minimal libpython2.7-stdlib python python-apt
python-minimal python-pycurl python-software-properties python2.7
python2.7-minimal
0 upgraded, 10 newly installed, 0 to remove and 3 not upgraded.
Need to get 4,070 kB of archives.
After this operation, 17.3 MB of additional disk space will be used.
Do you want to continue? [Y/n]

Press Y to continue.
Install Python.
After executing the command, add the webupd8team Java PPA repository in your system by running:
sudo add-apt-repository ppa:webupd8team/java

Press [ENTER] when requested. Now, you can easily install the latest version of Java 8 with apt.
First, update the package lists to fetch the available packages from the new PPA:
sudo apt-get update

Update Ubuntu 16.04
Then install the latest version of Oracle Java 8 with this command:
sudo apt-get install oracle-java8-installer

root@server1:~# sudo apt-get install oracle-java8-installer
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
 binutils gsfonts gsfonts-x11 java-common libfontenc1 libxfont1 x11-common xfonts-encodings xfonts-utils
Suggested packages:
 binutils-doc binfmt-support visualvm ttf-baekmuk | ttf-unfonts | ttf-unfonts-core ttf-kochi-gothic | ttf-sazanami-gothic ttf-kochi-mincho | ttf-sazanami-mincho ttf-arphic-uming firefox
 | firefox-2 | iceweasel | mozilla-firefox | iceape-browser | mozilla-browser | epiphany-gecko | epiphany-webkit | epiphany-browser | galeon | midbrowser | moblin-web-browser | xulrunner
 | xulrunner-1.9 | konqueror | chromium-browser | midori | google-chrome
The following NEW packages will be installed:
 binutils gsfonts gsfonts-x11 java-common libfontenc1 libxfont1 oracle-java8-installer x11-common xfonts-encodings xfonts-utils
0 upgraded, 10 newly installed, 0 to remove and 3 not upgraded.
Need to get 6,498 kB of archives.
After this operation, 20.5 MB of additional disk space will be used.
Do you want to continue? [Y/n]
Press Y to continue.
You MUST agree to the license available in http://java.com/license if you want to use Oracle JDK, clicking on the OK button.
Accept Java License
Downloading Java
The package installs a kind of meta-installer which then downloads the binaries directly from Oracle. After installation process, check the version of Java installed by running the following command
java -version

java version "1.8.0_91"
Java(TM) SE Runtime Environment (build 1.8.0_91-b14)
Java HotSpot(TM) 64-Bit Server VM (build 25.91-b14, mixed mode)
Now you have installed Java 8 and we will move to the next step.
Installing the Solr application
Solr can be installed on Ubuntu in different ways, in this article, I will show you how to install the latest package from the source.
We will begin by downloading the Solr distribution. First finding the latest version of the available package from their web page, copy the link and download it using the wget command
For this setup, we will use  http://www.us.apache.org/dist/lucene/solr/6.0.1/
cd /tmp
wget http://www.us.apache.org/dist/lucene/solr/6.0.1/solr-6.0.1.tgz

root@server1:/tmp# wget http://www.us.apache.org/dist/lucene/solr/6.0.1/solr-6.0.1.tgz
--2016-06-03 11:31:54-- http://www.us.apache.org/dist/lucene/solr/6.0.1/solr-6.0.1.tgz
Resolving www.us.apache.org (www.us.apache.org)... 140.211.11.105
Connecting to www.us.apache.org (www.us.apache.org)|140.211.11.105|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 137924507 (132M) [application/x-gzip]
Saving to: ‘solr-6.0.1.tgz’
Now, run the given below command to extract the service installation file:
tar xzf solr-6.0.1.tgz solr-6.0.1/bin/install_solr_service.sh --strip-components=2

And install Solr as a service using the script:
sudo ./install_solr_service.sh solr-6.0.1.tgz

The output will be similar to this:
 root@server1:/tmp# sudo ./install_solr_service.sh solr-6.0.1.tgz
id: ‘solr’: no such user
Creating new user: solr
Adding system user `solr' (UID 111) ...
Adding new group `solr' (GID 117) ...
Adding new user `solr' (UID 111) with group `solr' ...
Creating home directory `/var/solr' ...

Extracting solr-6.0.1.tgz to /opt


Installing symlink /opt/solr -> /opt/solr-6.0.1 ...


Installing /etc/init.d/solr script ...


Installing /etc/default/solr.in.sh ...

? solr.service - LSB: Controls Apache Solr as a Service
 Loaded: loaded (/etc/init.d/solr; bad; vendor preset: enabled)
 Active: active (exited) since Fri 2016-06-03 11:37:05 CEST; 5s ago
 Docs: man:systemd-sysv-generator(8)
 Process: 20929 ExecStart=/etc/init.d/solr start (code=exited, status=0/SUCCESS)

Jun 03 11:36:43 server1 systemd[1]: Starting LSB: Controls Apache Solr as a Service...
Jun 03 11:36:44 server1 su[20934]: Successful su for solr by root
Jun 03 11:36:44 server1 su[20934]: + ??? root:solr
Jun 03 11:36:44 server1 su[20934]: pam_unix(su:session): session opened for user solr by (uid=0)
Jun 03 11:37:05 server1 solr[20929]: [313B blob data]
Jun 03 11:37:05 server1 solr[20929]: Started Solr server on port 8983 (pid=20989). Happy searching!
Jun 03 11:37:05 server1 solr[20929]: [14B blob data]
Jun 03 11:37:05 server1 systemd[1]: Started LSB: Controls Apache Solr as a Service.
Service solr installed.
Use this command to check the status of the service
service solr status

You should see an output that begins with this:
root@server1:/tmp# service solr status
? solr.service - LSB: Controls Apache Solr as a Service
 Loaded: loaded (/etc/init.d/solr; bad; vendor preset: enabled)
 Active: active (exited) since Fri 2016-06-03 11:37:05 CEST; 39s ago
 Docs: man:systemd-sysv-generator(8)
 Process: 20929 ExecStart=/etc/init.d/solr start (code=exited, status=0/SUCCESS)

Jun 03 11:36:43 server1 systemd[1]: Starting LSB: Controls Apache Solr as a Service...
Jun 03 11:36:44 server1 su[20934]: Successful su for solr by root
Jun 03 11:36:44 server1 su[20934]: + ??? root:solr
Jun 03 11:36:44 server1 su[20934]: pam_unix(su:session): session opened for user solr by (uid=0)
Jun 03 11:37:05 server1 solr[20929]: [313B blob data]
Jun 03 11:37:05 server1 solr[20929]: Started Solr server on port 8983 (pid=20989). Happy searching!
Jun 03 11:37:05 server1 solr[20929]: [14B blob data]
Jun 03 11:37:05 server1 systemd[1]: Started LSB: Controls Apache Solr as a Service.


Creating a Solr search collection:
Using Solr, we can create multiple collections. Run the given command, mention the name of the collection (here gettingstarted) and specify its configurations.
sudo su - solr -c "/opt/solr/bin/solr create -c gettingstarted -n data_driven_schema_configs"

root@server1:/tmp# sudo su - solr -c "/opt/solr/bin/solr create -c gettingstarted -n data_driven_schema_configs"

Copying configuration to new core instance directory:
/var/solr/data/gettingstarted

Creating new core 'gettingstarted' using command:
http://localhost:8983/solr/admin/cores?action=CREATE&name=gettingstarted&instanceDir=gettingstarted

{
 "responseHeader":{
 "status":0,
 "QTime":4427},
 "core":"gettingstarted"}
The new core directory for our first collection has been created. To view the default schema file, got to:
/opt/solr/server/solr/configsets/data_driven_schema_configs/conf

Use the Solr Web Interface
The Apache Solr is now accessible on the default port, which is 8983. The admin UI should be accessible at http://your_server_ip:8983/solr. The port should be allowed by your firewall to run the links.
For example:
http://192.168.1.100:8983/solr/
The Solr web interface.
To see the details of the first collection that we created earlier, select the "gettingstarted" collection in the left menu.
Details of our data collection.
After you selected the "gettingstarted" collection, select Documents in the left menu. There you can enter real data in JSON format that will be searchable by Solr. To add more data, copy and paste the following example JSON onto Document field:
{
 "id": 1,
 "book_title": "My First Book",
 "published": 1985,
 "description": "All about Linux"
}
Click on the submit document button after adding the data.
 Submit a document to Solr.
Status: success
Response:

{
 "responseHeader": {
 "status": 0,
 "QTime": 189
 }
}
Now we can click on Query on the left side then click on Execute Query,
Execute a query in Solr.
We will see something like this:
{
  "responseHeader":{
    "status":0,
    "QTime":24,
    "params":{
      "q":"*:*",
      "indent":"on",
      "wt":"json",
      "_":"1464947017056"}},
  "response":{"numFound":1,"start":0,"docs":[
      {
        "id":"1",
        "book_title":["My First Book"],
        "published":[1985],
        "description":["All about Linux"],
        "_version_":1536108205792296960}]
  }}


PostGreSQL Installation
-----------------------

How To Install and Use PostgreSQL on Ubuntu 16.04
Posted May 4, 2016 59.8k views PostgreSQL Ubuntu Ubuntu 16.04
Introduction

Relational database management systems are a key component of many web sites and applications. They provide a structured way to store, organize, and access information.

PostgreSQL, or Postgres, is a relational database management system that provides an implementation of the SQL querying language. It is a popular choice for many small and large projects and has the advantage of being standards-compliant and having many advanced features like reliable transactions and concurrency without read locks.

In this guide, we will demonstrate how to install Postgres on an Ubuntu 16.04 VPS instance and go over some basic ways to use it.

Installation
Ubuntu's default repositories contain Postgres packages, so we can install these easily using the apt packaging system.

Since this is our first time using apt in this session, we need to refresh our local package index. We can then install the Postgres package and a -contrib package that adds some additional utilities and functionality:

sudo apt-get update
sudo apt-get install postgresql postgresql-contrib
Now that our software is installed, we can go over how it works and how it may be different from similar database management systems you may have used.

Using PostgreSQL Roles and Databases
By default, Postgres uses a concept called "roles" to handle in authentication and authorization. These are, in some ways, similar to regular Unix-style accounts, but Postgres does not distinguish between users and groups and instead prefers the more flexible term "role".

Upon installation Postgres is set up to use ident authentication, which means that it associates Postgres roles with a matching Unix/Linux system account. If a role exists within Postgres, a Unix/Linux username with the same name will be able to sign in as that role.

There are a few ways to utilize this account to access Postgres.

Switching Over to the postgres Account

The installation procedure created a user account called postgres that is associated with the default Postgres role. In order to use Postgres, we can log into that account.

Switch over to the postgres account on your server by typing:

sudo -i -u postgres
You can now access a Postgres prompt immediately by typing:

psql
You will be logged in and able to interact with the database management system right away.

Exit out of the PostgreSQL prompt by typing:

\q
You should now be back in the postgres Linux command prompt.

Accessing a Postgres Prompt Without Switching Accounts

You can also run the command you'd like with the postgres account directly with sudo.

For instance, in the last example, we just wanted to get to a Postgres prompt. We could do this in one step by running the single command psql as the postgres user with sudo like this:

sudo -u postgres psql
This will log you directly into Postgres without the intermediary bash shell in between.

Again, you can exit the interactive Postgres session by typing:

\q
Create a New Role
Currently, we just have the postgres role configured within the database. We can create new roles from the command line with the createrole command. The --interactive flag will prompt you for the necessary values.

If you are logged in as the postgres account, you can create a new user by typing:

createuser --interactive
If, instead, you prefer to use sudo for each command without switching from your normal account, you can type:

sudo -u postgres createuser --interactive
The script will prompt you with some choices and, based on your responses, execute the correct Postgres commands to create a user to your specifications.

Output
Enter name of role to add: sammy
Shall the new role be a superuser? (y/n) y
You can get more control by passing some additional flags. Check out the options by looking at the man page:

man createuser
Create a New Database
By default, another assumption that the Postgres authentication system makes is that there will be an database with the same name as the role being used to login, which the role has access to.

So if in the last section, we created a user called sammy, that role will attempt to connect to a database which is also called sammy by default. You can create the appropriate database with the createdb command.

If you are logged in as the postgres account, you would type something like:

createdb sammy
If, instead, you prefer to use sudo for each command without switching from your normal account, you would type:

sudo -u postgres createdb sammy
Open a Postgres Prompt with the New Role
To log in with ident based authentication, you'll need a Linux user with the same name as your Postgres role and database.

If you don't have a matching Linux user available, you can create one with the adduser command. You will have to do this from an account with sudo privileges (not logged in as the postgres user):

sudo adduser sammy
Once you have the appropriate account available, you can either switch over and connect to the database by typing:

sudo -i -u sammy
psql
Or, you can do this inline:

sudo -u sammy psql
You will be logged in automatically assuming that all of the components have been properly configured.

If you want your user to connect to a different database, you can do so by specifying the database like this:

psql -d postgres
Once logged in, you can get check your current connection information by typing:

\conninfo
Output
You are connected to database "sammy" as user "sammy" via socket in "/var/run/postgresql" at port "5432".
This can be useful if you are connecting to non-default databases or with non-default users.

Create and Delete Tables
Now that you know how to connect to the PostgreSQL database system, we can to go over how to complete some basic tasks.

First, we can create a table to store some data. Let's create a table that describes playground equipment.

The basic syntax for this command is something like this:

CREATE TABLE table_name (
    column_name1 col_type (field_length) column_constraints,
    column_name2 col_type (field_length),
    column_name3 col_type (field_length)
);
As you can see, we give the table a name, and then define the columns that we want, as well as the column type and the max length of the field data. We can also optionally add table constraints for each column.

You can learn more about how to create and manage tables in Postgres here.

For our purposes, we're going to create a simple table like this:

CREATE TABLE playground (
    equip_id serial PRIMARY KEY,
    type varchar (50) NOT NULL,
    color varchar (25) NOT NULL,
    location varchar(25) check (location in ('north', 'south', 'west', 'east', 'northeast', 'southeast', 'southwest', 'northwest')),
    install_date date
);
We have made a playground table that inventories the equipment that we have. This starts with an equipment ID, which is of the serial type. This data type is an auto-incrementing integer. We have given this column the constraint of primary key which means that the values must be unique and not null.

For two of our columns (equip_id and install_date), we have not given a field length. This is because some column types don't require a set length because the length is implied by the type.

We then give columns for the equipment type and color, each of which cannot be empty. We create a location column and create a constraint that requires the value to be one of eight possible values. The last column is a date column that records the date that we installed the equipment.

We can see our new table by typing:

\d
Output
                  List of relations
 Schema |          Name           |   Type   | Owner
--------+-------------------------+----------+-------
 public | playground              | table    | sammy
 public | playground_equip_id_seq | sequence | sammy
(2 rows)
Our playground table is here, but we also have something called playground_equip_id_seq that is of the type sequence. This is a representation of the serial type we gave our equip_id column. This keeps track of the next number in the sequence and is created automatically for columns of this type.

If you want to see just the table without the sequence, you can type:

\dt
Output
          List of relations
 Schema |    Name    | Type  | Owner
--------+------------+-------+-------
 public | playground | table | sammy
(1 row)
Add, Query, and Delete Data in a Table
Now that we have a table, we can insert some data into it.

Let's add a slide and a swing. We do this by calling the table we're wanting to add to, naming the columns and then providing data for each column. Our slide and swing could be added like this:

INSERT INTO playground (type, color, location, install_date) VALUES ('slide', 'blue', 'south', '2014-04-28');
INSERT INTO playground (type, color, location, install_date) VALUES ('swing', 'yellow', 'northwest', '2010-08-16');
You should take care when entering the data to avoid a few common hangups. First, keep in mind that the column names should not be quoted, but the column values that you're entering do need quotes.

Another thing to keep in mind is that we do not enter a value for the equip_id column. This is because this is auto-generated whenever a new row in the table is created.

We can then get back the information we've added by typing:

SELECT * FROM playground;
Output
 equip_id | type  | color  | location  | install_date
----------+-------+--------+-----------+--------------
        1 | slide | blue   | south     | 2014-04-28
        2 | swing | yellow | northwest | 2010-08-16
(2 rows)
Here, you can see that our equip_id has been filled in successfully and that all of our other data has been organized correctly.

If the slide on the playground breaks and we have to remove it, we can also remove the row from our table by typing:

DELETE FROM playground WHERE type = 'slide';
If we query our table again, we will see our slide is no longer a part of the table:

SELECT * FROM playground;
Output
 equip_id | type  | color  | location  | install_date
----------+-------+--------+-----------+--------------
        2 | swing | yellow | northwest | 2010-08-16
(1 row)
How To Add and Delete Columns from a Table
If we want to modify a table after it has been created to add an additional column, we can do that easily.

We can add a column to show the last maintenance visit for each piece of equipment by typing:

ALTER TABLE playground ADD last_maint date;
If you view your table information again, you will see the new column has been added (but no data has been entered):

SELECT * FROM playground;
Output
 equip_id | type  | color  | location  | install_date | last_maint
----------+-------+--------+-----------+--------------+------------
        2 | swing | yellow | northwest | 2010-08-16   |
(1 row)
We can delete a column just as easily. If we find that our work crew uses a separate tool to keep track of maintenance history, we can get rid of the column here by typing:

ALTER TABLE playground DROP last_maint;
How To Update Data in a Table
We know how to add records to a table and how to delete them, but we haven't covered how to modify existing entries yet.

You can update the values of an existing entry by querying for the record you want and setting the column to the value you wish to use. We can query for the "swing" record (this will match every swing in our table) and change its color to "red". This could be useful if we gave the swing set a paint job:

UPDATE playground SET color = 'red' WHERE type = 'swing';
We can verify that the operation was successful by querying our data again:

SELECT * FROM playground;
Output
 equip_id | type  | color | location  | install_date
----------+-------+-------+-----------+--------------
        2 | swing | red   | northwest | 2010-08-16
(1 row)
As you can see, our slide is now registered as being red.

MirthConnect Installation
-------------------------
How To Install and Use Mirth on Ubuntu 16.04

Follow the below Link you can install mirth in ubuntu server.

https://www.youtube.com/watch?v=omZyAO2naqs

Manually You can install by below process.

* Download the mirth version what ever you want to install .And keep it in a separate folder.i kept it in download folder

Then follow below process.

*sudo apt-get install tasksel
*sudo apt-get install lamp-server^
*sudo apt-get purge openjdk-\*
*sudo apt-get install python-software-properties
*add-apt-repository ppa:webupd8team/java
*sudo apt-get update
*sudo apt-get install oracle-java7-installer
*Downloaded  your version Exa-mirthconnect-2.2.1.
*sudo chmod a+x ~/Downloads/mirthconnect-2.2.1.5861.b1248-unix.sh
*sudo ~/Downloads/mirthconnect-2.2.1.5861.b1248-unix.sh

Then You can install mirth in your Server.

Python Installation
-------------------

Describe Python Installation
