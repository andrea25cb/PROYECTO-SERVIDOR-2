# Creación de usuario del sistema para acceso a ftp, ssh, smtp

## Solicitamos el nombre del usuario

```read -p "Introduce el nombre del usuario: " username```

Debemos tener instalado vsftpd, openssh-server y openssl, que se hace con:

```sudo apt-get install -y vsftpd openssh-server openssl```

## Creamos el archivo de configuración de vsftpd
```sudo nano /etc/vsftpd.conf```

## Aquí agregamos lo siguiente:
```
listen=YES
listen_ipv6=NO
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
use_localtime=YES
xferlog_enable=YES
connect_from_port_20=YES
chroot_local_user=YES
secure_chroot_dir=/var/run/vsftpd/empty
pam_service_name=vsftpd
ssl_enable=YES
allow_anon_ssl=NO
force_local_data_ssl=YES
force_local_logins_ssl=YES
ssl_tlsv1=YES
ssl_sslv2=NO
ssl_sslv3=NO
rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
```


## Reiniciamos vsftpd
```
sudo service vsftpd restart
```

## Creamos el usuario y su contraseña
```
sudo useradd -m $username -s /bin/bash
sudo passwd $username
```

## Añadimos el usuario al grupo ftp:
```
sudo usermod -a -G ftp $username
```

## Establecemos los permisos correctos en el directorio del usuario
```
sudo chown -R $username:ftp /home/$username
sudo chmod -R 755 /home/$username
```

# Creamos un certificado SSL para vsftpd
```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.key -out /etc/ssl/certs/vsftpd.crt
```

# Añadiremos la configuración necesaria para permitir el acceso mediante FTP con TLS en el archivo de configuración de vsftpd
```
sudo nano /etc/vsftpd.conf
```

# Agregamos las siguientes líneas al archivo de configuración de vsftpd
```
ssl_enable=YES
allow_anon_ssl=NO
force_local_data_ssl=YES
force_local_logins_ssl=YES
ssl_tlsv1=YES
ssl_sslv2=NO
ssl_sslv3=NO
rsa_cert_file=/etc/ssl/certs/vsftpd.crt
rsa_private_key_file=/etc/ssl/private/vsftpd.key
```

## Reiniciamos vsftpd para guardar los cambios

```
sudo service vsftpd restart
```

## Añadimos el usuario al grupo ssh y al grupo sftp

```
sudo usermod -a -G ssh $username

sudo usermod -a -G sftp $username
```

## Añadimos la configuración necesaria para permitir el acceso mediante SFTP en el archivo de configuración de sshd
```
sudo nano /etc/ssh/sshd_config
```

## Luego agregamos las siguientes líneas al archivo de configuración de sshd:
```
Match Group sftp
    ChrootDirectory %h
    ForceCommand internal-sftp
    AllowTcpForwarding no
    X11Forwarding no
```


## EL SCRIPT QUEDARÍA ASÍ:
![image](https://user-images.githubusercontent.com/92718546/222251765-9ef8d95e-034c-4921-8ac2-2a6d2258aeab.png)
![image](https://user-images.githubusercontent.com/92718546/222251881-a5098a58-9517-4ee8-b2c9-7edb8d47073e.png)
![image](https://user-images.githubusercontent.com/92718546/222251925-749af346-cf37-4cfe-b131-a0b92b791efe.png)
![image](https://user-images.githubusercontent.com/92718546/222251971-cf64725f-9168-4558-9e89-5c36fa1a3aa2.png)


