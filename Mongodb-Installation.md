# Mongodb
Installation steps for mongodb:

1. Import the public key used by the package management system.
  From a terminal, install gnupg and curl if they are not already available:

       sudo apt-get install gnupg curl

  Issue the following command to import the MongoDB public GPG Key from 
  https://pgp.mongodb.com/server-6.0.asc:

      curl -fsSL https://pgp.mongodb.com/server-6.0.asc | \
       sudo gpg -o /usr/share/keyrings/mongodb-server-6.0.gpg \
       --dearmor

2. Create a list file for MongoDB.
  Create the list file /etc/apt/sources.list.d/mongodb-org-6.0.list for your version of Ubuntu.
  Click on the appropriate tab for your version of Ubuntu. If you are unsure of what Ubuntu version the host is running, open a terminal or shell on the host and execute lsb_release -dc.

  Create the /etc/apt/sources.list.d/mongodb-org-6.0.list file for Ubuntu 22.04 (Jammy):

      echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list

  Create the /etc/apt/sources.list.d/mongodb-org-6.0.list file for Ubuntu 20.04 (Focal):

      echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list

3. Reload local package database.

        sudo apt-get update

4. Install the MongoDB packages.
    You can install either the latest stable version of MongoDB or a specific version of MongoDB
    To install the latest stable version, issue the following

        sudo apt-get install -y mongodb-org

    Optional. Although you can specify any available version of MongoDB, apt-get will upgrade the packages when a newer version becomes available.
     To prevent unintended upgrades, you can pin the package at the currently installed version:

        echo "mongodb-org hold" | sudo dpkg --set-selections
        echo "mongodb-org-database hold" | sudo dpkg --set-selections
        echo "mongodb-org-server hold" | sudo dpkg --set-selections
        echo "mongodb-mongosh hold" | sudo dpkg --set-selections
        echo "mongodb-org-mongos hold" | sudo dpkg --set-selections
        echo "mongodb-org-tools hold" | sudo dpkg --set-selections

6. Start MongoDB.

         sudo systemctl start mongod

    If you receive an error similar to the following when starting mongod:
    Failed to start mongod.service: Unit mongod.service not found.
    Run the following command first:

          sudo systemctl daemon-reload

    Then run the start command above again.

7. Verify that MongoDB has started successfully.

       sudo systemctl status mongod

  You can optionally ensure that MongoDB will start following a system reboot by issuing the following command:

        sudo systemctl enable mongod

7. Stop MongoDB.

       sudo systemctl stop mongod

8. Restart MongoDB

       sudo systemctl restart mongod

9. After installing the mongodb change the bindip in mongodb conf file.

       vim /etc/mongo.conf

   Goto net section
     net
       bindip: 127.0.0.1 # this is the default localhost

   Give your private ip in bind ip section
     net
       bindip: 127.0.0.1,<privateip>

   To access everyone give bindip as 0.0.0.0
     net
       bindip: 0.0.0.0
   
11. Enable mandatory authentication in MongoDB:
    add the security.authorization key to /etc/mongod.conf:

      security:
         authorization: enabled
   After complete the above step restart the mongodb.

13. Begin using MongoDB.
    Start a mongosh session on the same host machine as the mongod. You can run mongosh without any command-line options to connect to a mongod that is running on your localhost with default port 27017.
  
       mongosh

14. Configuring and Connecting MongoDB
      Once you’ve set up MongoDB as a service, you now need to launch your MongoDB installation. To do this, open the Mongo Shell and switch to the database admin mode using the following command:

        > mongosh
        > use admin

    Now, create a root user for your MongoDB installation and exit the Mongo Shell as follows:

        > db.createUser({user:"admin", pwd:"password", roles:[{role:"root", db:"admin"}]})

    You can now connect with your MongoDB, by first restarting MongoDB and then using the following line of code:

        > mongo -u admin -p admin123 --authenticationDatabase admin

    You’ll now be able to see MongoDB set up a connection. You can use the “show dbs” command as follow to open a list of all available databases:

        > show dbs


15. Uninstall Mongodb on ubuntu:
    To uninstall MongoDB on Ubuntu, you first need to stop mongodb and remove the MongoDB packages. To do this, you can stop the MongoDB service and execute the following command to remove the installed packages:

        sudo systemctl stop mongod

        sudo apt-get purge mongodb-org*

  You can remove your created databases, log files and directories using the following command:

        sudo rm -r /var/log/mongodb
        sudo rm -r /var/lib/mongodb

  
  
