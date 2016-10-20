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

Install MongoDB

Import the public key used by the package management system.

The Ubuntu package management tools (i.e. dpkg and apt) ensure package consistency and authenticity by requiring that distributors sign packages with GPG keys. Issue the following command to import the MongoDB public GPG Key:

Step 1 -

sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10



Create a list file for MongoDB.

Create the /etc/apt/sources.list.d/mongodb.list list file using the following command:

Step 2 -

echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | sudo tee /etc/apt/sources.list.d/mongodb.list



Reload local package database.

Issue the following command to reload the local package database:

Step 4 -
sudo apt-get update



Install the MongoDB packages.

You can install either the latest stable version of MongoDB or a specific version of MongoDB.

Install the latest stable version of MongoDB.

Issue the following command:

Step 5 -

sudo apt-get install -y mongodb-org
Install a specific release of MongoDB.

Specify each component package individually and append the version number to the package name, as in the following example that installs the 2.6.9 release of MongoDB:

Step 6 -

sudo apt-get install -y mongodb-org=2.6.9 mongodb-org-server=2.6.9 mongodb-org-shell=2.6.9 mongodb-org-mongos=2.6.9 mongodb-org-tools=2.6.9

Pin a specific version of MongoDB.

Although you can specify any available version of MongoDB, apt-get will upgrade the packages when a newer version becomes available. To prevent unintended upgrades, pin the package. To pin the version of MongoDB at the currently installed version, issue the following command sequence:

Step 7 -

echo "mongodb-org hold" | sudo dpkg --set-selections
echo "mongodb-org-server hold" | sudo dpkg --set-selections
echo "mongodb-org-shell hold" | sudo dpkg --set-selections
echo "mongodb-org-mongos hold" | sudo dpkg --set-selections
echo "mongodb-org-tools hold" | sudo dpkg --set-selections
Previous versions of MongoDB packages use different naming conventions. See the 2.4 version of documentation for more information.

Run MongoDB

The MongoDB instance stores its data files in /var/lib/mongodb and its log files in /var/log/mongodb by default, and runs using the mongodb user account. You can specify alternate log and data file directories in /etc/mongod.conf. See systemLog.path and storage.dbPath for additional information.

If you change the user that runs the MongoDB process, you must modify the access control rights to the /var/lib/mongodb and /var/log/mongodb directories to give this user access to these directories.


Start MongoDB.


Issue the following command to start mongod:

sudo service mongod start

Verify that MongoDB has started successfully

Verify that the mongod process has started successfully by checking the contents of the log file at /var/log/mongodb/mongod.log for a line reading

[initandlisten] waiting for connections on port <port>
where <port> is the port configured in /etc/mongod.conf, 27017 by default.



Stop MongoDB.

As needed, you can stop the mongod process by issuing the following command:

sudo service mongod stop



Restart MongoDB.

Issue the following command to restart mongod:

sudo service mongod restart



Begin using MongoDB.

To begin using MongoDB, see Getting Started with MongoDB. Also consider the Production Notes document before deploying MongoDB in a production environment.

Later, to stop MongoDB, press Control+C in the terminal where the mongod instance is running.

Uninstall MongoDB

To completely remove MongoDB from a system, you must remove the MongoDB applications themselves, the configuration files, and any directories containing data and logs. The following section guides you through the necessary steps.

WARNING
This process will completely remove MongoDB, its configuration, and all databases. This process is not reversible, so ensure that all of your configuration and data is backed up before proceeding.



Stop MongoDB.

Stop the mongod process by issuing the following command:

sudo service mongod stop



Remove Packages.

Remove any MongoDB packages that you had previously installed.

sudo apt-get purge mongodb-org*



Remove Data Directories.

Remove MongoDB databases and log files.

sudo rm -r /var/log/mongodb
sudo rm -r /var/lib/mongodb


OrientDB Installation
-----------------

How To Install and Configure OrientDB on Ubuntu 16.04

Introduction

OrientDB is a multi-model, NoSQL database with support for graph and document databases. It is a Java application and can run on any operating system. It's also fully ACID-complaint with support for multi-master replication.

In this article, you'll learn how to install and configure the latest Community edition of OrientDB on an Ubuntu 16.04 server.

Prerequisites

To follow this tutorial, you will need the following:

Ubuntu 16.04 Droplet


Installing Oracle Java
OrientDB is a Java application that requires Java version 1.6 or higher. Because it's much faster than Java 6 and 7, Java 8 is highly recommended. And that's the version of Java we'll install in this step.

To install Java JRE, add the following Personal Package Archives (PPA):

Step-1

sudo add-apt-repository ppa:webupd8team/java

Update the package database:

Step-2


sudo apt-get update

Then install Oracle Java. Installing it using this particular package not only installs it, but also makes it the default Java JRE. When prompted, accept the license agreement:

Step-3

sudo apt-get install oracle-java8-set-default

After installing it, verify that it's now the default Java JRE:

Step-4

java -version

The expected output is as follows (the exact version may vary):

output
java version "1.8.0_60"
Java(TM) SE Runtime Environment (build 1.8.0_60-b27)
Java HotSpot(TM) 64-Bit Server VM (build 25.60-b23, mixed mode)



Downloading and Installing OrientDB

In this step, we'll download and install the latest Community edition of OrientDB. At the time of this publication, OrientDB Community 2.1.3 is the latest version. If a newer version has been released, change the version number to match:

Step-5


wget https://orientdb.com/download.php?file=orientdb-community-2.1.3.tar.gz

The downloaded tarball contains pre-compiled binary files that you need to run OrientDB on your system, so all you need to do is untar it to a suitable directory. Since the /opt is the traditional location for third party programs on Linux, let's untar it there:


Step-6

sudo tar -xf download.php?file=orientdb-community-2.1.3.tar.gz -C /opt

The files are extracted into a directory named orientdb-community-2.1.3. To make it easier to work with, let's rename it:

sudo mv /opt/orientdb-community-2.1.3 /opt/orientdb



Starting the Server

Now that the binary is in place, you can start the server and connect to the console. Before that, navigate to the installation directory:

Step-7

cd /opt/orientdb

Then start the server:

Step-8

sudo bin/server.sh


 Connecting to the Console

Now that the server is running, you can connect to it using the console, that is, the command line interface:

sudo /opt/orientdb/bin/console.sh

You will see the following:

Output
OrientDB console v.2.1.3 (build UNKNOWN@r; 2015-10-04 10:56:30+0000) www.orientdb.com
Type 'help' to display all the supported commands.
Installing extensions for GREMLIN language v.2.6.0

orientdb>
Now, connect to the server instance. The password required is the one you specified when you first started the server in the earlier:

connect remote:127.0.0.1 root root-password
If connected, the output should be:

Output
Connecting to remote Server instance [remote:127.0.0.1] with user 'root'...OK
orientdb {server=remote:127.0.0.1/}>
Type exit to quit:

exit
So you've just installed OrientDB, manually started it, and connected to it. That's all good. However, it also means starting it manually anytime you reboot the server. That's not good. In the next steps, we'll configure and set up OrientDB to run just like any other daemon on the server.

Type CTRL-C in the terminal window with OrientDB still running to stop it.

Configuring OrientDB

At this point OrientDB is installed on your system, but it's just a bunch of scripts on the server. In this step, we'll modify the configuration file, and also configure it to run as a daemon on the system. That involves modifying the /opt/orientdb/bin/orientdb.sh script and the /opt/orientdb/config/orientdb-server-config.xml configuration file.

Let's start by modifying the /opt/orientdb/bin/orientdb.sh script to tell OrientDB the user it should be run as, and to point it to the installation directory.

So, first, create the system user that you want OrientDB to run as. The command will also create the orientdb group:

sudo useradd -r orientdb -s /bin/false

Give ownership of the OrientDB directory and files to the newly-created OrientDB user and group:

sudo chown -R orientdb:orientdb /opt/orientdb

Now let's make a few changes to the orientdb.sh script. We start by opening it using:

sudo nano /opt/orientdb/bin/orientdb.sh

First, we need to point it to the proper installation directory, then tell it what user it should be run as. So look for the following two lines at the top of the file:

/opt/orientdb/bin/orientdb.sh

# You have to SET the OrientDB installation directory here

ORIENTDB_DIR="YOUR_ORIENTDB_INSTALLATION_PATH"

ORIENTDB_USER="USER_YOU_WANT_ORIENTDB_RUN_WITH"

And change them to:

/opt/orientdb/bin/orientdb.sh

# You have to SET the OrientDB installation directory here

ORIENTDB_DIR="/opt/orientdb"

ORIENTDB_USER="orientdb"

Now, let's makes it possible for the system user to run the script using sudo.

Further down, under the start function of the script, look for the following line and comment it out by adding the # character in front of it. It must appear as shown:

/opt/orientdb/bin/orientdb.sh

#su -c "cd \"$ORIENTDB_DIR/bin\"; /usr/bin/nohup ./server.sh 1>../log/orientdb.log 2>../log/orientdb.err &" - $ORIENTDB_USER

Copy and paste the following line right after the one you just commented out:

/opt/orientdb/bin/orientdb.sh

sudo -u $ORIENTDB_USER sh -c "cd \"$ORIENTDB_DIR/bin\"; /usr/bin/nohup ./server.sh 1>../log/orientdb.log 2>../log/orientdb.err &"
Under the stop function, look for the following line and comment it out as well. It must appear as shown.

/opt/orientdb/bin/orientdb.sh
#su -c "cd \"$ORIENTDB_DIR/bin\"; /usr/bin/nohup ./shutdown.sh 1>>../log/orientdb.log 2>>../log/orientdb.err &" - $ORIENTDB_USER
Copy and paste the following line right after the one you just commented out:

/opt/orientdb/bin/orientdb.sh
sudo -u $ORIENTDB_USER sh -c "cd \"$ORIENTDB_DIR/bin\"; /usr/bin/nohup ./shutdown.sh 1>>../log/orientdb.log 2>>../log/orientdb.err &"
Save and close the file.

Next, open the configuration file:

sudo nano /opt/orientdb/config/orientdb-server-config.xml

We're going to modify the storages tag and, optionally, add another user to the users tag. So scroll to the storages element and modify it so that it reads like the following. The username and password are your login credentials, that is, those you used to log into your server:

/opt/orientdb/config/orientdb-server-config.xml
<storages>
        <storage path="memory:temp" name="temp" userName="username" userPassword="password" loaded-at-startup="true" />
</storages>
If you scroll to the users tag, you should see the username and password of the root user you specified when you first start the OrientDB server in Step 3. Also listed will be a guest account. You do not have to add any other users, but if you wanted to, you could add the username and password that you used to log into your DigitalOcean server. Below is an example of how to add a user within the users tag:

/opt/orientdb/config/orientdb-server-config.xml
<user name="username" password="password" resources="*"/>
Save and close the file.

Finally, modify the file's permissions to prevent unauthorized users from reading it:

sudo chmod 640 /opt/orientdb/config/orientdb-server-config.xml

Installing the Startup Script

Now that the scripts have been configured, you can now copy them to their respective system directories. For the script responsible for running the console, copy it to the /usr/bin directory:

sudo cp /opt/orientdb/bin/console.sh /usr/bin/orientdb

Then copy the script responsible for starting and stopping the service or daemon to the /etc/init.d directory:

sudo cp /opt/orientdb/bin/orientdb.sh /etc/init.d/orientdb

Change to the /etc/init.d directory:

cd /etc/init.d
Then update the rc.d directory so that the system is aware of the new script and will start it on boot just like the other system daemons.

sudo update-rc.d orientdb defaults

You should get the following output:

Output
update-rc.d: warning: /etc/init.d/orientdb missing LSB information
update-rc.d: see <http://wiki.debian.org/LSBInitScripts>
 Adding system startup for /etc/init.d/orientdb ...
   /etc/rc0.d/K20orientdb -> ../init.d/orientdb
   /etc/rc1.d/K20orientdb -> ../init.d/orientdb
   /etc/rc6.d/K20orientdb -> ../init.d/orientdb
   /etc/rc2.d/S20orientdb -> ../init.d/orientdb
   /etc/rc3.d/S20orientdb -> ../init.d/orientdb
   /etc/rc4.d/S20orientdb -> ../init.d/orientdb
   /etc/rc5.d/S20orientdb -> ../init.d/orientdb


Starting OrientDB

With everything in place, you may now start the service:

sudo service orientdb start

Verify that it really did start:

 sudo service orientdb status

You may also use the netstat commands from Step 3 to verify that the server is listening on the ports. If the server does not start, check for clues in the error log file in the /opt/orientdb/log directory.

 Connecting to OrientDB Studio

OrientDB Studio is the web interface for managing OrientDB. By default, it's listening on port 2480. To connect to it, open your browser and type the following into the address bar:

http://server-ip-address:2480

If the page loads, you should see the login screen. You should be able to login as root and the password you set earlier.

If the page does not load, it's probably because it's being blocked by the firewall. So you'll have to add a rule to the firewall to allow OrientDB traffic on port 2480. To do that, open the IPTables firewall rules file for IPv4 traffic:

sudo /etc/iptables/rules.v4

Within the INPUT chain, add the following rule:

/etc/iptables/rules.v4
-A INPUT -p tcp --dport 2480 -j ACCEPT
Restart iptables:

sudo service iptables-persistent reload

That should do it for connecting to the OrientDB Studio.


Solr Installation
-----------------

How to install and configure Solr 6 on Ubuntu 16.04

What is Apache Solr?
   Apache Solr is an open source enterprise-class search platform written in Java which enables you to create custom search engines that index databases, files, and websites. It has back end support for Apache Lucene. It can e.g. be used to search in multiple websites and can show recommendations for the searched content. Solr uses an XML (Extensible Markup Language) based query and result language. There are APIs (Applications program interfaces) available for Python, Ruby and JSON (Javascript Object Notation).
Some other features that Solr provides are:
Full-Text Search.

Snippet generation and highlighting.

Custom Document ordering/ranking.

Spell Suggestions.

This tutorial will show you how to install the latest Solr version on Ubuntu 16.04 LTS. The steps will most likely work with later Ubuntu versions as well.

Update your System

Use a non-root sudo user to login into your Ubuntu server. Through this user, you will have to perform all the steps and use the Solr later.

To update your system, execute the following command to update your system with latest patches and updates.

Step-1


sudo apt-get update && apt-get upgrade -y

Setting up the Java Runtime Environment

Solr is a Java application, so the Java runtime environment needs to be installed first in order to set up Solr.

We have to install Python Software properties in order to install the latest Java 8. Run the following command to install the software.

Step-2


sudo apt-get install python-software-properties

After executing the command, add the webupd8team Java PPA repository in your system by running:

Step-3


sudo add-apt-repository ppa:webupd8team/java

Press [ENTER] when requested. Now, you can easily install the latest version of Java 8 with apt.

First, update the package lists to fetch the available packages from the new PPA:

Step-4

sudo apt-get update



Then install the latest version of Oracle Java 8 with this command:

Step-5


sudo apt-get install oracle-java8-installer

The package installs a kind of meta-installer which then downloads the binaries directly from Oracle. After installation process, check the version of Java installed by running the following command

Step-6


java -version

Installing the Solr application

Solr can be installed on Ubuntu in different ways, in this article, I will show you how to install the latest package from the source.
We will begin by downloading the Solr distribution. First finding the latest version of the available package from their web page, copy the link and download it using the wget command

For this setup, we will use  http://www.us.apache.org/dist/lucene/solr/6.0.1/

Step-7

cd /tmp

Step-8


wget http://www.us.apache.org/dist/lucene/solr/6.0.1/solr-6.0.1.tgz


Now, run the given below command to extract the service installation file:

Step-9


tar xzf solr-6.0.1.tgz solr-6.0.1/bin/install_solr_service.sh --strip-components=2

And install Solr as a service using the script:

Step-10


sudo ./install_solr_service.sh solr-6.0.1.tgz

Use this command to check the status of the service

Step-11


service solr status

Creating a Solr search collection:

Using Solr, we can create multiple collections. Run the given command, mention the name of the collection (here gettingstarted) and specify its configurations.

Step-12


sudo su - solr -c "/opt/solr/bin/solr create -c gettingstarted -n data_driven_schema_configs"


Use the Solr Web Interface
The Apache Solr is now accessible on the default port, which is 8983. The admin UI should be accessible at http://your_server_ip:8983/solr. The port should be allowed by your firewall to run the links.

For example:

http://192.168.1.100:8983/solr/


PostGreSQL Installation
-----------------------

How To Install and Use PostgreSQL on Ubuntu 16.04

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
