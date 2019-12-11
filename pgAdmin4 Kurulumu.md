pgAdmin4 Kurulumu 
---------------------------------------------------------------------------------------------------------------------------
    sudo apt update -y && sudo apt upgrade -y
    
    sudo apt-get install pgadmin4 pgadmin4-apache2
    
    K.Adı ve şifre oluştur
    Giriş
    http://example.com/pgAdmin4
        
    ---------------------------------------------------------------------------------------------------------------------------
    Postgresql Tipleri
    ---------------------------------------------------------------------------------------------------------------------------
    C#				Postgresql
    Guid			uuid_generate_v4();    -> İlk olarak "CREATE EXTENSION "uuid-ossp";" çalıştırılacak. Otomatik guid oluşturma
    Datetime		date
    
https://tecadmin.net/install-pgadmin4-on-ubuntu/
https://linoxide.com/linux-how-to/how-install-pgadmin4-ubuntu/
https://cloudcone.com/docs/article/how-to-install-pgadmin4-on-ubuntu-18-04/
https://www.digitalocean.com/community/tutorials/how-to-install-configure-pgadmin4-server-mode
