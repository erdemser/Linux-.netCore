Postgresql Backup and restore işlemleri
---------------------------------------------------------------------------------------------------
    1-Backup alınacak dizine izin verilecek
      sudo chown -R user /home/magnus
    
    2- Hata alınıyorsa 
      Peer authentication failed for user “postgres”, when trying to get pgsql working with rails
    	sudo nano /etc/postgresql/11/main/pg_hba.conf
    	local   all             postgres                                peer
    	satırını değiştiriyoruz
    	local   all             postgres                                md5
    	sudo service postgresql restart
    	https://stackoverflow.com/questions/18664074/getting-error-peer-authentication-failed-for-user-postgres-when-trying-to-ge
    
    3-Backup almak için 
      pg_dump -U postgres -d mydb > mydb.pgsql
    
    4-Restore
      psql -U postgres -d mydb < mydb.pgsql
    
    5-Backup and Restore All Databases
      backup  
        pg_dumpall -U postgres > alldbs.pgsql
    	restore  
        psql -U postgres < alldbs.pgsql
    
    6-Backup and Restore Single Table
      backup
        pg_dump -U postgres -d mydb -t mytable > mydb-mytable.pgsql
    	restore
    		psql -U postgres -d mydb < mydb-mytable.pgsql
    
    7-Compressed Backup and Restore Database
    	backup
    		pg_dump -U postgres -d mydb | gzip > mydb.pgsql.gz
    	restore
    		gunzip -c mydb.pgsql.gz | psql -U postgres -d mydb
    
    8-Split Backup in Multiple Files and Restore
    	backup
    		pg_dump -U postgres -d mydb | split -b 100m – mydb.pgsql
    	restore
    		cat mydb.pgsql* | psql -U postgres -d mydb
    	backup compressed 
    		pg_dump -U postgres -d mydb | gzip | split -b 100m – mydb.pgsql.gz
    	restore compressed
    		cat mydb.pgsql.gz* | gunzip | psql -U postgres -d mydb
    		
> Kaynak, [link](https://tecadmin.net/backup-and-restore-database-in-postgresql/)
		
