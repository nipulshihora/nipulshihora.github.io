---
title: 'Dspace-Auto-Backup'
date: 2025-05-28
layout: post
permalink: 
tags:
  - Dspace
  - Auto Backup
  - Cronjob
---

## How to Setup Automatic Daily Backup of Dspace Database & Files

Create a script for daily automatic DSpace backup of Database & Repository using Dspace Backup Commands in Terminal.

To setup Automatic Daily Dspace Backup, Follow the below Dspace Backup Commands.

First of all, Login to root user and create directory for backup.

```
$ sudo su
```

Enter Login password when prompted.
```
$ mkdir dspace_backup
```
Allow permissions to this newly created folder.
```
$ sudo chmod 777 dspace_backup
```
Next, Exit from root directory.
```
$ exit
```
Change Directory
```
$ cd /usr/local/bin/
```
Create new file backup.sh
```
$ nano backup.sh
```
Copy below text into file and save it.
```
$$
#!/bin/bash

# Setup shell to use for backups
SHELL=/bin/bash

# Setup name of local server to be backed up
SERVER="%[server_IP]%"

# Setup event stamps
DOW=`date +%a`
TIME=`date`

# Setup paths
FOLDER="/home/backup"
FILE="/var/log/backup-$DOW.log"

# Do the backups
{
echo "Backup started: $TIME"

# Make the backup folder if it does not exist
if test ! -d /home/backup
then
  mkdir -p /home/backup
  echo "New backup folder created"
  else
  echo ""
fi

# Make sure we're in / since backups are relative to that
cd /

## PostgreSQL database (Needs a /root/.pgpass file)
which -a psql
if [ $? == 0 ] ; then 
    echo "SQL dump of PostgreSQL databases"
    su - postgres -c "pg_dump --inserts dspace > /tmp/dspace-db.sql"
    cp /tmp/dspace-db.sql $FOLDER/dspace-db-$DOW.sql
    su - postgres -c "vacuumdb --analyze dspace > /dev/null 2>&1"
fi


# Backup '/dspace' folder
echo "Archive '/dspace' folder"
tar czf $FOLDER/dspace-$DOW.tgz dspace/

# View backup folder
echo ""
echo "** Backup folder **"
ls -lhS $FOLDER

# Show disk usage
echo ""
echo "** Disk usage **"
df -h

TIME=`date`
echo "Backup ended: $TIME"

} > $FILE

### EOF ###

$$
```

OR

```
#!/bin/bash
PGPASSWORD="dspace" pg_dump -U dspace -h localhost -Fc dspace | gzip > dspace_backup/dspace-$(date +%Y-%m-%d-%H.%M.%S).sql.gz
now=$(date +"%d_%m_%Y")
zip -r  /home/server/dspace_backup/$now-assetstore.zip /dspace/assetstore
zip -r  /home/server/dspace_backup/$now-log.zip /dspace/log
```

(Change [username] & [password] as per your Dspace Database Credentials).
Make this file executable by following command:
```
$ sudo chmod +x backup.sh
```
Now, Execute this file for testing by following command:
```
$ sh backup.sh
```
Next, Schedule Dspace backup using cron job, Open Crontab using nano editor.
```
$ crontab -e
```
It will ask to select an editor to open crontab, select nano editor.
Copy & Paste following lines at the end of crontab.
```
$$ 55 04 * * * /usr/local/bin/backup.sh
```
OR
```
15 13 * * * /usr/local/bin/backup.sh
00 14 * * * find dspace_backup/* -mtime +180 -exec rm {} ;
```
Save this file using Ctrl+o and ENTER, then press Ctrl+x to exit the cron.
Now, This cron job will take Backup of Dspace Database, files & logs  automatically at 01:15PM daily and save it to dspace_backup folder. It will automatically delete backup files older than 180 days. 
