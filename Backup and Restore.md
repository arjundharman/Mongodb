Understanding the steps to restore and backup MongoDB on Ubuntu

  Restore and Backup are integral components of any database system. They play a pivotal role when you lose your valuable data owing to a server crash, corruption, or any of the other viable reasons. Here are the steps you can follow to Restore and Backup the MongoDB database by leveraging the utilities provided by MongoDB itself.

Step 1: Creating a backup directory
  
  You can begin by creating a backup directory where you can store all the backups for ease of access. It also serves as a more organized repository for your backups. Here’s how you can create a backup directory:

    sudo mkdir /var/backups/mongobackups

Step 2: Creating a backup by leveraging mongodump
  
  mongodump is the go-to utility that you leverage to export data from the database. This utility will retrieve the –db argument that will cite the name of your desired database to create a backup for:

    sudo mongodump --db dbyour --out /var/backups/mongobackups/

  In the above command, you are creating a backup for the database named dbyour. You use the –out argument to set the directory where you wish to create the backup. Once you’ve executed the given command you get the following prompt:
  
    2022-01-04T19:30:24.230+0000    writing dbyour.coach to

    2022-01-04T19:30:24.230+0000    done dumping dbyour.coach (2100 documents)

  In case you forget to add your database name via the –db argument, mongodump will backup and store up all your databases by default.

Step 3: Automating the backups
  
  In the previous step, you created the dump through the manual method. To prevent data loss you need to take backup on a regular basis at a specified interval of time. Say, you want to take a daily backup at morning 2 AM, and to attain this you can follow along with this snippet:

    sudo mongodump --db dbyour --out /var/backups/mongobackups/`date +"%m-%d-%y"`

  To create an automatic backup at Specific on a daily basis, you can set up a cron job as follows:

    sudo crontab -e

  Next, set the mongodump command inside crontab to execute the query at 7:00 PM:

    * 19 * * * mongodump --out /var/backups/mongobackups/`date +"%m-%d-%y"`

  This cron job creates the dump of your database daily, however, if your database size is too large you might soon run out of disk space with too many backups to keep track of. Therefore, to prevent this issue from rearing its head, you need to delete the old backups regularly. For instance, if you want to delete all the backups older than 5 days, you can use this bash command:

    find /var/backups/mongobackups/ -mtime +5 -exec rm -rf {} \;

  Similar to daily backups, set a cron job to delete backups which will run daily before creating new backups at say 7.01 PM. Enter the following command to set the cron job for 7.01 PM daily:

    1 19 * * * find /var/backups/mongobackups/ -mtime +5 -exec rm -rf {} \;

Step 4: Restoring a backup by leveraging mongostore
 
  You can leverage mongostore to restore the database from a backup to extract an exact copy of your database for a given/specific time, along with data types and indexes. Here’s the command for the same:

    sudo mongorestore --db dbyour --drop /var/backups/mongobackups/01-05-22/dbyour/

