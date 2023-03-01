# PROYECTO2DAW

## Se pide la instalación, configuración y puesta en marcha de un servidor que ofrezca servicio de alojamiento web

Deberemos tener instalados los servicios que usamos en la práctica anterior (apache, php, mysql) e instalaremos bind9.

Instalar bind:
```
sudo apt-get update

sudo apt-get install bind9 bind9utils bind9-doc
```

### Se automatizará mediante el uso de scripts: 
* [La creación de usuarios y del directorio correspondiente para el alojamiento web](/crearUsuario.md)
* [Host virtual en apache](/crearHostVirtual.md)
* [Creación de usuario del sistema para acceso a ftp, ssh, smtp,…](/crearUsuarioFtpSsh.md)
* [Se creará un subdominio en el servidor DNS con las resolución directa e inversa](/crearSubdominio.md)
* [Se creará una base de datos además de un usuario con todos los permisos sobre dicha base de datos (ALL PRIVILEGES)](/crearBasedatos.md)
* [Se habilitará la ejecución de aplicaciones Python con el servidor web](/ejecutarAppPython.md)



# DOCKER

## Creación mediante mediante Docker de un contenedor DNS y al menos un contenedor que actuará como servidor (web, mysql, ssh,...)

Lo primero que haremos será instalar Docker desde su sitio oficial:


Luego, crearemos una red en Docker para que los contenedores puedan comunicarse entre sí con este comando:

```docker network create mynetwork```

Para crear un contenedor DNS, usaremos una imagen de DNS pública como la de BIND9. Creamos el contenedor con:

```
docker run -d --name dns --network mynetwork --restart=always sameersbn/bind:latest
```
(Se creará un contenedor con el nombre 'dns', usando la imagen de BIND9. El contenedor se unirá a la red "mynetwork")

## Crear un contenedor servidor:
Para ello podemos usar cualquier imagen de software, por ejemplo, usaré la de Apache. Lo creo con el siguiente comando:
```
docker run -d --name web --network mynetwork --restart=always httpd:latest
```
(Se creará un contenedor con el nombre 'web', usando la imagen de Apache. Este contenedor también se unirá a la red "mynetwork")
 
 
 ### Lo siguiente que haremos será configurar el DNS, para que el contenedor servidor use el contenedor DNS que acabo de crear:
 
 ```
 docker exec -it web bash
echo "nameserver dns" >> /etc/resolv.conf
 ```
 Este comando iniciará una sesión de shell en el contenedor "web" y agregará la dirección IP del contenedor "dns" al archivo de configuración de DNS.
 
 
 
