Postgresql Kurulumu
---------------------------------------------------------------------------------------------------------------------------
    sudo apt update && sudo apt -y upgrade
    sudo reboot
    
    wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
    
    RELEASE=$(lsb_release -cs)
    --echo "deb http://apt.postgresql.org/pub/repos/apt/ ${RELEASE}"-pgdg main | sudo tee  /etc/apt/sources.list.d/pgdg.list
    
    cat /etc/apt/sources.list.d/pgdg.list
    --deb http://apt.postgresql.org/pub/repos/apt/ bionic-pgdg main
    
    sudo apt update
    sudo apt -y install postgresql-11
    
    sudo ss -tunelp | grep 5432
    tcp   LISTEN  0  128  127.0.0.1:5432         0.0.0.0:*      users:(("postgres",pid=15785,fd=3)) uid:111 ino:42331 sk:6 <->
    
    sudo nano /etc/postgresql/11/main/postgresql.conf
    
    ---Dışarıdan erişim için ip dinlemesi
    listen_addresses = '*'
    
    sudo systemctl restart postgresql
    
    sudo ss -tunelp | grep 5432
    
    --Port açma
    sudo ufw allow 5432/tcp
    
    --Açık portları listeleme
    sudo ufw status verbose
    
    https://computingforgeeks.com/install-postgresql-11-on-ubuntu-18-04-ubuntu-16-04/
    
    ------
    sudo su - postgres
    psql -c "alter user postgres with password 'StrongPassword'"
    
    createuser dbuser1
    createdb testdb -O dbuser1
    
    psql -l  | grep testdb
    
    alter user dbuser1 with password 'DBPassword';
    
    
    Add or edit the following line in your postgresql.conf :
    
    listen_addresses = '*'
    
    Add the following line as the first line of pg_hba.conf. It allows access to all databases for all users with an encrypted 
    sudo nano /etc/postgresql/11/main/pg_hba.conf
    host  all  all 0.0.0.0/0 md5
    
    Restart Postgresql after adding this with service postgresql restart or the equivalent command for your setup.
    
https://dba.stackexchange.com/questions/83984/connect-to-postgresql-server-fatal-no-pg-hba-conf-entry-for-host
https://medium.com/coding-blocks/creating-user-database-and-adding-access-on-postgresql-8bfcd2f4a91e
    
Management Studio yerine pgAdmin4 kurulabilir.

https://www.postgresql.org/ftp/pgadmin/pgadmin4/v4.11/windows/
