# Seguridad_U3
## busqueda de informacion

## Configuración de Contraseñas Seguras en Windows y Linux
### Windows
####  Acceder a la Configuración de Directivas de Contraseña:
Abre "Administración de directivas de seguridad local" desde el "Administrador del servidor".
Navega a "Configuración del equipo" -> "Directivas" -> "Configuración de seguridad" -> "Directivas de cuenta" -> "Directivas de contraseñas"
imagen

#### Configurar las Directivas de Contraseña:
Longitud Mínima de la Contraseña:
Configura la longitud mínima de la contraseña según tus requisitos.

 <p align="center">
    <img src="imagenes/A3_3.PNG" alt="captura1_actividad3" width="430" height="318">
</p>

Requisitos de Complejidad:
Asegúrate de que estén habilitados los requisitos de complejidad, como mayúsculas, minúsculas, números y caracteres especiales.
 <p align="center">
    <img src="imagenes/A3_3.PNG" alt="captura1_actividad3" width="430" height="318">
</p>
Aplicar Cambios:

    Guarda los cambios realizados en las directivas.
### Linux

```bash
necesitamos instalar el paquete libpam
sudo apt-get update
sudo apt-get install libpam-pwquality
```
#### Editar el archivo de configuración de PAM:

```bash
sudo nano /etc/security/pwquality.conf
```
#### Configurar las Directivas de Contraseña:
Configura las directivas según tus requisitos, por ejemplo:
```bash
minlen = 8
minclass = 3
maxrepeat = 2
minclass = 4
minclass = 4
maxclassrepeat = 0
maxsequence = 3
maxclassrepeat = 1
```
#### Editar el archivo de configuración PAM para contraseñas:
ahora añadiremos o modificaremo esta linea en la ruta "/etc/pam.d/common-password"

```bash
password requisite pam_pwquality.so retry=3
```

Indica que el módulo pam_pwquality es requisito (requisite) y se utiliza para manejar políticas de calidad de contraseña. El parámetro retry=3 significa que se permitirán hasta 3 intentos antes de que la contraseña sea rechazada

#### Guarda los cambios realizados y reinicia el servicio PAM:

```bash
sudo systemctl restart systemd-logind
```
Estas son configuraciones básicas y debes ajustar los valores según tus políticas de seguridad específicas. Además, ten en cuenta que las configuraciones pueden variar dependiendo de las versiones específicas de los sistemas operativos.

## Actividad 3- Ataques contra contraseñas en Sistemas Windows – FICHERO SAM -
tendremos que descargar la herramienta en su pagina web oficial para el sistema operativo que nosotros queramos http://project-rainbowcrack.com/

## proteger el grub
antes de hacer nada es muy recomendable hacer una copia de los ficheros que vamos a modificar
``` bash 
    sudo cp /boot/grub/grub.cfg ~/grub.cfg.old

    sudo cp /etc/grub.d/00_header ~/00_header.old

    sudo cp /etc/grub.d/10_linux ~/10_linux.old

    sudo cp /etc/grub.d/30_os-prober ~/30_os-prober.old
```
### definir los usuarios y contraseñas que podran modificar el grub

Para definir los usuarios y las contraseñas de los usuarios, que podrán usar la línea de comandos del grub, y ejecutar y editar las entradas del grub, abriremos una terminal y ejecutar el siguiente comando:
``` bash
    sudo nano /etc/grub.d/00_header
```
Una vez abierto el editor de textos nano, vamos al final del archivo y tenemos que añadir la lista de usuarios y de contraseñas añadiendo el siguiente texto:
```bash
cat << EOF
set superusers="jesus"
password jesus 4321
EOF
```
definiendo a jesus como el usuario permitido para para modifica y 4321 la contraseña que le identifica.

Una vez definidos los usuarios y las contraseñas ya podemos guardar los cambios.

**Nota:**  Los usuarios y contraseñas que definamos en este apartado pueden ser completamente diferentes a los usuarios del sistema. Cada uno de los usuarios va separado por una coma.

### Protege contraseña el arranque de los sistemas operativos




