# Seguridad_U3
## busqueda de informacion
**libpam-cracklib:** Mejora la seguridad de contraseñas en un sistema al integrarse con el sistema de autenticación PAM. Permite configurar reglas y restricciones para la creación y modificación de contraseñas de usuarios.

**rainbowcrack:** Herramienta de cracking de contraseñas que utiliza tablas arcoíris precalculadas para acelerar el descifrado. Recupera contraseñas comparando hash almacenados con tablas arcoíris, siendo eficiente en ataques contra contraseñas débiles, pero menos efectiva con salting y contraseñas complejas.

**Pwdump:** Extrae y muestra contraseñas en formato hash almacenadas localmente en sistemas Windows. Facilita el acceso a hashes de contraseñas para análisis o ataques, pero su uso ético es crucial debido a posibles violaciones de privacidad y términos de servicio.

**L0phtCrack:** Herramienta de auditoría de contraseñas que realiza análisis de fuerza bruta y diccionario para evaluar la fortaleza de las contraseñas en un sistema, detectando vulnerabilidades de seguridad.

**NTPWEdit:** Utilidad que permite editar contraseñas de usuarios en sistemas Windows modificando el Registro del sistema, útil en casos de pérdida de contraseña o acceso a cuentas.

**John the Ripper:** Programa de prueba de contraseñas que utiliza métodos de fuerza bruta y ataques de diccionario para descifrar contraseñas, versátil en auditorías de seguridad.

**Shadow Defender:** Crea un entorno virtual aislado en el sistema, permitiendo probar software o navegar de forma segura. Los cambios se revierten al reiniciar, evitando afectar el sistema real.

**Ofris:** Herramienta de seguridad que protege sistemas Linux bloqueando la escritura en directorios importantes, previniendo modificaciones no autorizadas en archivos cruciales.

**Grub2:** Gestor de arranque utilizado en sistemas basados en Unix. Configurable para arrancar varios sistemas operativos, es esencial en el inicio de sistemas Linux y otros compatibles con Unix.

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
minlen = 8            # Longitud mínima de la cadena: 8 caracteres
minclass = 4          # Mínimo de clases (tipos de caracteres) requeridas: 4
maxrepeat = 2         # Máxima repetición de un mismo carácter o grupo: 2 veces
maxclassrepeat = 1    # Máxima repetición de la misma clase (tipo de carácter): 1 vez
maxsequence = 3       # Número máximo de secuencias permitidas: 3

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
en mi caso utilizare kali linux 
instalacion de rainbow-crack
``` bash
sudo apt install rainbowcrack
```
creamos una tabla

``` bash
sudo rtgen ntlm lowealpha-numeric 1 5 0 2400 10000 0
```

donde los siguientes parametros son
``` bash
- rtgen: Invoca la herramienta RainbowCrack para generar tablas arcoíris.

- ntlm: Especifica el algoritmo de hash, en este caso, NTLM.

- loweralpha-numeric: Define el conjunto de caracteres que se utilizará para generar contraseñas, en este caso, letras minúsculas y dígitos.

- 1: Longitud mínima de contraseña.

- 5: Longitud máxima de contraseña.

- 0: Desplazamiento de reducción, que en este caso es 0.

- 2400: Número de cadenas reducidas generadas por cada cadena original.

- 100000: Número total de cadenas originales generadas.

- 0: Semilla aleatoria inicial para generar cadenas originales.
```

Ahora hay que ordenar la tabla para poder descifrar el hash (da igual desde que carpeta se lance rtgen la tabla se guarda en esta ruta)

``` bash
sudo rtsort /usr/share/rainbowcrack
```

ahora visualizaremos el fichero con un cat para mostrar los datos y selecionar la ultima parte del cada administrador para descrifrar (esta señalado en la imagen)
 <p align="center">
    <img src="imagenes/A3_3.PNG" alt="captura1_actividad3" width="430" height="318">
</p>

Ahora podemos descifrar el has de una de las contraseña del SAM, el has es la parte en amarillo
con el siguiente comando


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

### Protege el arranque de los sistemas operativos
primero tendremos que crear un hash de la siguiente manera
Abrimos una terminal y ejecutamos el siguiente comando:
```bash
    sudo grub-mkpasswd-pbkdf2
```
despues abrimos /etc/default/grub y añadimos el hash y el usuario a la siguiente linea
Una vez abierto el editor de textos localizamos la siguiente linea:

 <p align="center">
    <img src="imagenes/A3_3.PNG" alt="captura1_actividad3" width="430" height="318">
</p> 

para cada modificacion que hagamos deberemos hacer un update-grub

tambien tendremos que añadir el usuario y el hashen la ruta /etc/grub.d/40_custom, como en la siguiente imagen
 <p align="center">
    <img src="imagenes/A3_3.PNG" alt="captura1_actividad3" width="430" height="318">
</p> 

y ya tendremos configurado un unico usuario para iniciar los sistemas operativos del grub, si queremos añadir mas usuarios simplemente repetiremos los pasos.
