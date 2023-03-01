# Se creará un subdominio en el servidor DNS con las resolución directa e inversa


## Configuración del servidor DNS

```
sudo apt-get update
sudo apt-get install -y bind9

echo "Introduce el nombre del subdominio"
read subdominio
```

## Creación de la zona del subdominio

```
sudo sh -c "echo '
zone \"subdominio.ejemplo.com\" {
    type master;
    file \"/etc/bind/db.subdominio\";
};
' >> /etc/bind/named.conf.local"
```

### Añadimos la informacion necesaria:

```
sudo sh -c "echo '
$TTL 86400
@       IN      SOA     ns1.$subdominio.com. admin.$subdominio.com. (
                        1       ; Serial
                        3600    ; Refresh
                        1800    ; Retry
                        604800  ; Expire
                        86400 ) ; Minimum TTL
;
@       IN      NS      ns1.$subdominio.com.
@       IN      A       192.168.1.100' > /etc/bind/db.subdominio"

sudo systemctl restart bind9
```

# Creación de la zona de resolución inversa

```
sudo sh -c "echo '
zone \"1.168.192.in-addr.arpa\" {
    type master;
    file \"/etc/bind/db.192.168.1\";
};
' >> /etc/bind/named.conf.local"

sudo sh -c "echo '
$TTL    86400
@       IN      SOA     ns1.$subdominio.com. admin.$subdominio.com. (
                        1       ; Serial
                        3600    ; Refresh
                        1800    ; Retry
                        604800  ; Expire
                        86400 ) ; Minimum TTL
;
@       IN      NS      ns1.$subdominio.com.' > /etc/bind/db.192.168.1"

sudo systemctl restart bind9

```

EL SCRIPT QUEDARÍA ASÍ:

![image](https://user-images.githubusercontent.com/92718546/222239949-2bc70e2c-c7ac-417b-a27c-fde6c71486ff.png)

![image](https://user-images.githubusercontent.com/92718546/222240051-de759ce3-c36e-4639-8bde-622805bef972.png)

![image](https://user-images.githubusercontent.com/92718546/222240111-25cf3125-d299-44c0-9b8a-9a1f5c73787d.png)

