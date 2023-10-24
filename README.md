# ServerFlix - Servidor multimedia sobre Docker

Este repositorio sirve para crear un servidor multimedia con el cual puedes descargar automáticamente películas y series, una vez descargado el contenido, es copiado a su respectiva carpeta dentro del directorio `media/` desde el cual nuestro Plex se sincronizará y agregará todo a nuestra biblioteca.

Así mismo se agrega un servidor samba para compartir los archivos dentro de la red (el uso de este no es obligatrio para el funcionamiento del servidor multimedia).

## Primeros pasos

Instalar Docker

```
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
echo "deb [arch=armhf] https://download.docker.com/linux/debian \
     $(lsb_release -cs) stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list
sudo apt-get update && sudo apt-get install -y --no-install-recommends docker-ce docker-compose
```

Modifica tu docker config para que guarde los temps en el disco:

```
sudo vim /etc/default/docker
# Agregar esta linea al final con la ruta de tu disco externo montado
export DOCKER_TMPDIR="/mnt/storage/docker-tmp"
```

Montar el disco (es necesario ntfs-3g si es que tenes tu disco en NTFS)
NOTA: en este [link](https://youtu.be/OYAnrmbpHeQ?t=5543) del gran Pelado Nerd pueden ver la explicación en vivo

```
# usamos la terminal como root porque vamos a ejecutar algunos comandos que necesitan ese modo de ejecución
sudo su
# buscamos el disco que querramos montar (por ejemplo la partición sdb1 del disco sdb)
fdisk -l
# pueden usar el siguiente comando para obtener el UUID
ls -l /dev/disk/by-uuid/
# y simplemente montamos el disco en el archivo /etc/fstab (pueden hacerlo por el editor que les guste o por consola)
echo UUID="{nombre del disco o UUID que es único por cada disco}" {directorio donde queremos montarlo} (por ejemplo /mnt/storage) ntfs-3g defaults,auto 0 0 | \
     sudo tee -a /etc/fstab
# por último para que lea el archivo fstab
mount -a (o reiniciar)
```

## Cómo correrlo

Simplemente bajate este repo y modifica las rutas de tus archivos en el archivo (oculto) .env, y después corre:

`docker-compose up -d`

## IMPORTANTE

Este servidor multimedia sirve tanto para correrlo desde Linux como desde Windows.

Agradecimientos a [Akrista](https://github.com/akrista/) y a [Pablokbs](https://github.com/pablokbs/).