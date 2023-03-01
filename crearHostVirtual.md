# Host virtual en apache


### Solicitamos el nombre de usuario existente

```read -p "Introduce el nombre del usuario: " username```

### Comprobamos si el usuario existe. Si existe, crea un directorio www en el directorio de inicio del usuario, establece los permisos correctos en el directorio www, crea el archivo de configuración del host virtual, lo habilita y reinicia el servicio de Apache; si no existe, muestra un mensaje de error:

```
if id "$username" >/dev/null 2>&1; then
    # Crear el directorio para el sitio web del usuario
    sudo mkdir /home/$username/www
    sudo chown -R $username:$username /home/$username/www
    sudo chmod -R 755 /home/$username/www
```

### Crea el archivo de configuración del host virtual
 ```
    sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/$username.conf
    sudo sed -i "s/ServerAdmin webmaster@localhost/ServerAdmin $username@localhost/g" /etc/apache2/sites-available/$username.conf
    sudo sed -i "s/ServerName localhost/ServerName $username/g" /etc/apache2/sites-available/$username.conf
    sudo sed -i "s|DocumentRoot /var/www/html|DocumentRoot /home/$username/www|g" /etc/apache2/sites-available/$username.conf
    sudo sed -i "s/<Directory \/var\/www\/>/<Directory \/home\/$username\/www\/>/g" /etc/apache2/sites-available/$username.conf
```
### Habilita el host virtual y reiniciar Apache
```
    sudo a2ensite $username.conf
    sudo service apache2 reload

    echo "El host virtual para $username ha sido creado correctamente."
```
### Mensaje de error:
```
else
    echo "El usuario $username no existe."
fi
```

## EL SCRIPT QUEDARÍA ASÍ:

![image](https://user-images.githubusercontent.com/92718546/222256705-7c74842e-39c9-4f7f-94cc-9619318ad2f2.png)
