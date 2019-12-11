#MySQL Database Backup to FTP Server – Shell Script
-----------------------------------------------------------------------------------
    ######################################################
    #  Script Written by : Rahul Kumar
    #  Date: Feb 21, 2013
    ######################################################
    
    #!/bin/bash
     
    DATE=`date +%d%b%y`
    LOCAL_BACKUP_DIR="/backup/"
    DB_NAME="test"
    DB_USER="root"
    DB_PASSWORD="your password"
    FTP_SERVER="ftp.tecadmin.net"
    FTP_USERNAME="ftp user name"
    FTP_PASSWORD="ftp user password"
    FTP_UPLOAD_DIR="/backup/"
    LOG_FILE=/backup/backup-DATE.log
     
    ############### Local Backup  ########################
 
    mysqldump -u $DB_USER  -p$DB_PASSWORD $DB_NAME | gzip  > $LOCAL_BACKUP_DIR/$DB_NAME-$DATE.sql.gz
     
    ############### UPLOAD to FTP Server  ################
     
    ftp -n $FTP_SERVER << EndFTP
    user "$FTP_USERNAME" "$FTP_PASSWORD"
    binary
    hash
    cd $FTP_UPLOAD_DIR
    #pwd
    lcd $LOCAL_BACKUP_DIR
    put "$DB_NAME-$DATE.sql.gz"
    bye
    EndFTP
     
    if test $? = 0
    then
        echo "Database Successfully Uploaded to Ftp Server
            File Name $DB_NAME-$DATE.sql.gz " > $LOG_FILE
    else
        echo "Error in database Upload to Ftp Server" > $LOG_FILE
    fi
    
    
> Kaynak, [Link](https://tecadmin.net/mysql-database-backup-to-ftp-server-shell-script/)。  
    
------------------------------------------------------------------------------------------
    
    #!/bin/bash
     
    ################################################################
    ##
    ##   MySQL Database Backup Script 
    ##   Written By: Rahul Kumar
    ##   URL: https://tecadmin.net/bash-script-mysql-database-backup/
    ##   Last Update: Jan 05, 2019
    ##
    ################################################################
     
    export PATH=/bin:/usr/bin:/usr/local/bin
    TODAY=`date +"%d%b%Y"`
     
    ################################################################
    ################## Update below values  ########################
     
    DB_BACKUP_PATH='/backup/dbbackup'
    MYSQL_HOST='localhost'
    MYSQL_PORT='3306'
    MYSQL_USER='root'
    MYSQL_PASSWORD='mysecret'
    DATABASE_NAME='mydb'
    BACKUP_RETAIN_DAYS=30   ## Number of days to keep local backup copy
     
    #################################################################
     
    mkdir -p ${DB_BACKUP_PATH}/${TODAY}
    echo "Backup started for database - ${DATABASE_NAME}"
     
     
    mysqldump -h ${MYSQL_HOST} \
       -P ${MYSQL_PORT} \
       -u ${MYSQL_USER} \
       -p${MYSQL_PASSWORD} \
       ${DATABASE_NAME} | gzip > ${DB_BACKUP_PATH}/${TODAY}/${DATABASE_NAME}-${TODAY}.sql.gz
     
    if [ $? -eq 0 ]; then
      echo "Database backup successfully completed"
    else
      echo "Error found during backup"
      exit 1
    fi
     
     
    # Remove backups older than {BACKUP_RETAIN_DAYS} days
    ------------------------------------------------------------------------------------------
        DBDELDATE=`date +"%d%b%Y" --date="${BACKUP_RETAIN_DAYS} days ago"`
         
        if [ ! -z ${DB_BACKUP_PATH} ]; then
              cd ${DB_BACKUP_PATH}
              if [ ! -z ${DBDELDATE} ] && [ -d ${DBDELDATE} ]; then
                    rm -rf ${DBDELDATE}
              fi
        fi
> Kaynak, [Link](https://tecadmin.net/bash-script-mysql-database-backup/)。   
    
    
### İzin 
------------------------------------------------------------------------------------------
    chmod +x /backup/mysql-backup.sh

### Schedule Script in Crontab
------------------------------------------------------------------------------------------
    crontab -e
    0 2 * * * root /backup/mysql-backup.sh


    
