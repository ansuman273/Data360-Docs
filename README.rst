Project setup in Digital Ocean
==============================

These are the software requirements and their set up process in the server(Digital Ocean)

MySQL Installation
------------------

Describe MySQL Installation


MongoDB Installation
--------------------
How to Install MongoDB on Ubuntu 16.04
Posted May 5, 2016 91.1k views MongoDB Ubuntu Ubuntu 16.04
Introduction

MongoDB is a free and open-source NoSQL document database used commonly in modern web applications. This tutorial will help you set up MongoDB on your server for a production application environment.

As of publication time, the official Ubuntu 16.04 MongoDB packages have not yet been updated to use the new systemd init system which is enabled by default on Ubuntu 16.04. Running MongoDB using those packages on a clean Ubuntu 16.04 server involves following an additional step to configure MongoDB as a systemd service that will automatically start on boot.

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

OrientDB is a multi-model, NoSQL database with support for graph and document databases. It is a Java application and can run on any operating system. It's also fully ACID-complaint with support for multi-master replication.

In this article, you'll learn how to install and configure the latest Community edition of OrientDB on an Ubuntu 14.04 server.

Prerequisites
To follow this tutorial, you will need the following:

Ubuntu 14.04 Droplet
Non-root user with sudo privileges (Initial Server Setup with Ubuntu 14.04 explains how to set this up.)
Step 1 — Installing Oracle Java
OrientDB is a Java application that requires Java version 1.6 or higher. Because it's much faster than Java 6 and 7, Java 8 is highly recommended. And that's the version of Java we'll install in this step.

To install Java JRE, add the following Personal Package Archives (PPA):

sudo add-apt-repository ppa:webupd8team/java
Update the package database:

sudo apt-get update
Then install Oracle Java. Installing it using this particular package not only installs it, but also makes it the default Java JRE. When prompted, accept the license agreement:

sudo apt-get install oracle-java8-set-default
After installing it, verify that it's now the default Java JRE:

java -version
The expected output is as follows (the exact version may vary):

output
java version "1.8.0_60"
Java(TM) SE Runtime Environment (build 1.8.0_60-b27)
Java HotSpot(TM) 64-Bit Server VM (build 25.60-b23, mixed mode)

Step 2 — Downloading and Installing OrientDB
In this step, we'll download and install the latest Community edition of OrientDB. At the time of this publication, OrientDB Community 2.1.3 is the latest version. If a newer version has been released, change the version number to match:

wget https://orientdb.com/download.php?file=orientdb-community-2.1.3.tar.gz
The downloaded tarball contains pre-compiled binary files that you need to run OrientDB on your system, so all you need to do is untar it to a suitable directory. Since the /opt is the traditional location for third party programs on Linux, let's untar it there:

sudo tar -xf download.php?file=orientdb-community-2.1.3.tar.gz -C /opt
The files are extracted into a directory named orientdb-community-2.1.3. To make it easier to work with, let's rename it:

sudo mv /opt/orientdb-community-2.1.3 /opt/orientdb


Step 3 — Starting the Server
Now that the binary is in place, you can start the server and connect to the console. Before that, navigate to the installation directory:

cd /opt/orientdb
Then start the server:

sudo bin/server.sh
Aside from generating a bunch of output, by starting the server for the first time, you'll be prompted to specify a password for the root user account. This is an internal OrientDB account that will be used to access the server. For example, it's the username and password combination that will be used to access OrientDB Studio, the web-based interface for managing OrientDB. If you don't specify a password, one will be generated automatically. However, it's best to specify one yourself, do so when prompted.

Part of the output generated from starting the server tells you what ports the server and OrientDB Studio are listening on:

Output
2015-10-12 11:27:45:095 INFO  Databases directory: /opt/orientdb/databases [OServer]
2015-10-12 11:27:45:263 INFO  Listening binary connections on 0.0.0.0:2424 (protocol v.32, socket=default) [OServerNetworkListener]
2015-10-12 11:27:45:285 INFO  Listening http connections on 0.0.0.0:2480 (protocol v.10, socket=default) [OServerNetworkListener]

...

2015-10-12 11:27:45:954 INFO  OrientDB Server v2.1.3 (build UNKNOWN@r; 2015-10-04 10:56:30+0000) is active. [OServer]
Since OrientDB is now running in your terminal window, in a second terminal window to the same Droplet, confirm that the server is listening on ports 2424 (for binary connections) and 2480 (for HTTP connections). To confirm that it's listening for binary connections, execute:

sudo netstat -plunt | grep 2424
The output should look similar to

Output
tcp6       0      0 :::2424                 :::*                    LISTEN      1617/java
To confirm that it's listening for HTTP connections, execute:

sudo netstat -plunt | grep 2480
The expected output is as follows:

Output
tcp6       0      0 :::2480                 :::*                    LISTEN      1617/java
Step 4 — Connecting to the Console
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

Step 5 — Configuring OrientDB
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
Step 6 — Installing the Startup Script
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
Step 7 — Starting OrientDB
With everything in place, you may now start the service:

sudo service orientdb start
Verify that it really did start:

sudo service orientdb status
You may also use the netstat commands from Step 3 to verify that the server is listening on the ports. If the server does not start, check for clues in the error log file in the /opt/orientdb/log directory.

Step 8 — Connecting to OrientDB Studio
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

Conclusion
Congratulations! You've just installed the Community edition of OrientDB on your server. To learn more, check out the How To Back Up Your OrientDB Databases on Ubuntu 14.04 and How To Import and Export an OrientDB Database on Ubuntu 14.04 articles.

More information and official OrientDB documentation links can be found on orientdb.com.

Solr Installation
-----------------

Describe Solr Installation

PostGreSQL Installation
-----------------------

Describe PostGreSQL Installation

MirthConnect Installation
-------------------------

Describe MirthConnect Installation

Python Installation
-------------------

Describe Python Installation
