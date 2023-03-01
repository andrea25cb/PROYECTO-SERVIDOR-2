# Se habilitará la ejecución de aplicaciones Python con el servidor web

Deberemos tener instalado el modulo mod_wsgi, que se hace con:

```
sudo apt-get update
sudo apt-get install -y libapache2-mod-wsgi-py3
```

Y lo habilitamos con ```sudo a2enmod wsgi```

## Primero, creamos el directorio de python, le puse de nombre 'mi_sitio':
```sudo mkdir /var/www/mi_sitio```

## Luego creamos el archivo de configuración del sitio:
```sudo nano /etc/apache2/sites-available/mi_sitio.conf```

## Agregaremos la siguiente configuración dentro del fichero mi_sitio.conf:
```<VirtualHost *:80>
    ServerName mi_sitio.com
    WSGIScriptAlias / /var/www/mi_sitio/mi_sitio.wsgi
    <Directory /var/www/mi_sitio>
        Require all granted
    </Directory>
</VirtualHost>
```

## Habilito el nuevo fichero de configuración y deshabilito 000-default.conf

```
sudo a2ensite mi_sitio.conf
sudo a2dissite 000-default.conf
```

## Reiniciamos apache para guardar los cambios:

```sudo service apache2 restart```


QUEDARÍA ASÍ:
![image](https://user-images.githubusercontent.com/92718546/222245695-a8063e8e-cddd-4fad-b73e-e55b8262697d.png)
