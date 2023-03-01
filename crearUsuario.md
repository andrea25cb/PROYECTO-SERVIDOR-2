# La creación de usuarios y del directorio correspondiente para el alojamiento web

creamos: crear_usuario.sh, para que se reconozca como un script debemos escribir como primera línea "#!/bin/bash" (sin las comillas).


Primero, pido por pantalla el nombre del usuario a crear:

```
echo "Inserta el nombre del usuario a crear"
read user
```

Si existe, mostrará mensaje de error 'el usuario ya existe, si no, creamos el usuario, su directorio y un index:

```
if [ grep "$user" /etc/passwd ];
then
  echo "El usuario $user ya existe!"
  clear
  crear_usuario
else
  echo "Añadiendo usuario y su directorio:"
  useradd -m -d /home/$user $user -s /bin/bash
  passwd $user
  sudo mkdir /var/www/html/$user
  sudo cp /var/www/html/index.html /var/www/html/$user/index.html
fi
```

Se vería así:

![image](https://user-images.githubusercontent.com/92718546/222226264-795f78d4-10dd-4720-89d0-eb96446540c4.png)
