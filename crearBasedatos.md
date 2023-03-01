# Se creará una base de datos además de un usuario con todos los permisos sobre dicha base de datos (ALL PRIVILEGES)


Primero solicitamos el nombre del usuario con el que se creará el usuario en la base de datos. 

```
read -p "Ingrese el nombre del usuario: " USER_NAME
```

Comprobaremos si el usuario existe. Si es así, se configura el nombre de la base de datos, el nombre del usuario y la contraseña para el usuario; si no, mostrará un mensaje de error.

```
if id "$USER_NAME" &>/dev/null; then
    # Configuración de la base de datos
    DB_NAME="nombre_de_la_base_de_datos"
    DB_USER="$USER_NAME"
    DB_PASSWORD="contraseña_del_usuario"
```

Creación de la base de datos
```
    sudo mysql -e "CREATE DATABASE $DB_NAME;"
```

Creación del usuario y otorgamiento de permisos
```
    sudo mysql -e "CREATE USER '$DB_USER'@'localhost' IDENTIFIED BY '$DB_PASSWORD';"
    sudo mysql -e "GRANT ALL PRIVILEGES ON $DB_NAME.* TO '$DB_USER'@'localhost';"
    sudo mysql -e "FLUSH PRIVILEGES;"
else
    # Mostrar mensaje de error si el usuario no existe
    echo "El usuario $USER_NAME no existe."
fi
```

QUEDARÍA ASÍ:

![image](https://user-images.githubusercontent.com/92718546/222242796-49f38e7a-224f-4961-bc9f-5c56d5c5ca7f.png)
