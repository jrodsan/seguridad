# seguridad Tema 4
# cifrado simetrico  GPG
## 1. Crea un documento de texto con cualquier editor o utiliza uno del que dispongas.
## 2. Cifra este documento con alguna contraseña acordada con el compañero de al lado.
Ciframos el documento con el comando **gpg -c “nombre del archivo”** y nos preguntara que contraseña queremos usar y a continuación se creará un **archivo .gpg** ese es el que debemos pasar al compañero.

 <p align="center">
    <img src="imgu4/1.png" width="auto" >
</p>

## 3. Haz llegar por algún medio al compañero de al lado el documento que acabas de cifrar.
## 4. Descifra el documento que te ha hecho llegar tu compañero de al lado.
Para descifrar la contraseña usamos el comando **gpg -d “nombre del archivo”**.gpg 
nos pedira la contraseña y nos mostrará el contenido

 <p align="center">
    <img src="imgu4/2.png" width="auto" >
</p>

## 5. ¿Con qué algoritmo se ha cifrado el fichero? Vuelve a cifrar el fichero usando el algoritmo AES256. ¿Puedes hacer permanente esta configuración?
Si escribimos el comando **gpg --list-packets archivo_cifrado.gpg** nos mostrara con que algoritmo se ha cifrado

 <p align="center">
    <img src="imgu4/3.png" width="auto" >
</p>

en este caso ya se ha cifrado con **AES256**

si quisiéramos usar otro usaríamos el siguiente comando **gpg --cipher-algo cast5 -c archivo_secreto.txt**
siendo:
* --cipher-algo cast5: Especifica el algoritmo de cifrado simétrico que quieres utilizar (en este caso, cast5).

## 6. Instala gpg en windows (Gpg4win), repite el ejercicio en Windows. Puedes encriptar un mensaje en linux y desencriptarlo en windows y al contrario.

Una vez instalado ya podremos utilizar el programa para ello utilizaremos el archivo previamente cifrado con linux y lo descifraremos en windows con esta herramienta
el sistema ya reconoce que esta cifrado asi que le daremos click derecho y en descifrar y verificar e ingresaremos la contraseña

 <p align="center">
    <img src="imgu4/4.png" width="auto" >
</p>
a continuación nos pedirá guardar el archivo nuevo descifrado y lo tendremos ya descifrado
<p align="center">
    <img src="imgu4/5.png" width="auto" >
</p>
de la misma manera para cifrar lo haremos pulsando click derecho al archivo que queramos cifrar y nos aparecerá un asistente que nos ira pidiendo datos y la contraseña, una vez introduzcamos los datos ya tendremos el archivo cifrado 

<p align="center">
    <img src="imgu4/6.png" width="auto" >
</p>

## 7. openssl es otra herramienta que nos permite cifrar mensajes de forma simétrica, investiga como se realiza este ejercicio utilizando esta herramienta.

Para instalar la aplicación de openssl iremos a la web [Win32/Win64 OpenSSL Installer for Windows - Shining Light Productions (slproweb.com)](https://slproweb.com/products/Win32OpenSSL.html)  y descargaremos la versión que queramos utilizar en mi caso usaré la .exe lite
y lo instalaremos de manera habitual como todos los programas de windows con doble clic con su asistente.

En el caso de no haber agregado la variable de entorno deberemos hacerlo en la configuración avanzada de Windows y añadir la ruta al path
<p align="center">
    <img src="imgu4/7.png" width="auto" >
</p>
reinicia el sistema para que refresque las variables.
Una vez tengamos todo listo podremos cifrar con el siguiente comando remplazando archivo.txt por el archivo objetivo.

cifrar el archivo con la clave simétrica
openssl aes-128-cbc -in archivo.txt -out cifrado.enc
donde:
    • **aes-128-cbc** sería el protocolo utilizado mediante modo cbc (podemos utilizar otros tipos de cifrado mencionados en la ayuda del programa, como puede ser aes-256-cbc)
    • **-in: señala** el archivo a cifrar (archivo.txt)
    • **-out:** marcaría la salidad del archivo cifrado (cifrado.enc)
y nos pedirá la contraseña

<p align="center">
    <img src="imgu4/8.png" width="auto" >
</p>
y para descifrarlo seria con el comando openssl aes-128-cbc -d -in cifrado.enc -out descifrado

donde:
    • **aes-128-cbc** es el tipo de cifrado
    • **-d** indica que vamos a descifrar
    • **-in:** nos marca el archivo cifrado (cifrado.enc)
    • **-out:** indica el nombre del archivo ya descifrado (descifrado o cualquier otro que elijáis)
<p align="center">
    <img src="imgu4/9.png" width="auto" >
</p>

# Firma digital con gpg
## 1. Selecciona un documento pdf y encríptalo y fírmalo (opción –sign). Envíalo a un compañero, que debe en primer lugar verificar la firma y posteriormente descifrar el documento.
Cifraremos un archivo en este caso el mismo pdf de la practica
**gpg -c archivo.pdf**

 <p align="center">
    <img src="imgu4/10.png" width="auto" >
</p>
una vez lo tengamos encriptado lo firmaremos.
Antes de hacer nada deberemos asegurarnos de que tenemos alguna clave privada con elsifuiente  comando

**gpg --list-secret-keys**
en el caso de no tener usariamos **gpg --gen-key** para generar una, te pedira el nombre real , un correo electronico  y escribir tu contraseña

una vez ya lo tengamos podemos proceder a firmar

para firmar  un documento lo haremos con el siguiente comando **gpg  --output archivo_firmado.gpg archivo_original.pdf**
el programa nos avisa de que ya hay un archivo con ese nombre y nos da la posibilidad de renombrarlo o machacarlo. 

 <p align="center">
    <img src="imgu4/11.png" width="auto" >
</p>

Y ya tendremos el archivo firmado e encriptado
 <p align="center">
    <img src="imgu4/12.png" width="auto" >
</p>

para verificar la firma usaremos el siguiente comando **gpg --verify archivo_firmado.gpg**

nos imprimirá en pantalla quien es el causante de esta firma entro otros datos como el **RSA**
 <p align="center">
    <img src="imgu4/13.png" width="auto" >
</p>

tenemos que darle la clave publica a nuestro compañero lo haremos de la siguiente manera
gpg --export --armor [jrodsan233y@g.educaand.es](mailto:jrodsan233y@g.educaand.es) > clave_publica.asc
de esta manera exportamos las claves a un .asc con formato de salida ASCII que se lo daremos al compañero.

ahora el compañero debe agrega la clave a su llavero con el comando **gpg --import clave_publica.asc**
 <p align="center">
    <img src="imgu4/14.png" width="auto" >
</p>

y lo descifraremos con la opcion -o para que genere un archivo nuevo con el resultado
**gpg -d -o archivo_descifrado.pdf PRACTICA2FIR.pdf.gpg**

 <p align="center">
    <img src="imgu4/15.png" width="auto" >
</p>

## 2. Realiza el mismo ejercicio pero obteniendo una firma ASCII.
 Seguiremos los mismos pasos que anteriormente pero a la hora de exportar la clave usamos la opcion –armor
gpg --export --armor [jrodsan233y@g.educaand.es](mailto:jrodsan233y@g.educaand.es) > clave_publica.asc

## 3. Ahora sólo queremos firmar un documento. Firma un documento (opción --detach-sign). A continuación envía el documento original y la firma a un compañero para que verifique que el documento está firmado por tí.
Firmaremos el documento con el comando

**gpg --detach-sign documento.txt**

 <p align="center">
    <img src="imgu4/16.png" width="auto" >
</p>

y le enviaremos el documento y la clave publica al compañero
importaremos la clave con **gpg --import clave_publica.asc**
y verificaremos la firma con el comando **gpg --verify documento.txt.asc documento.txt.gpg** 
o lo desencriptamos directamente
**gpg --decrypt archivo_secreto.txt.sig > documentodesencripta**
**do.txt**
 <p align="center">
    <img src="imgu4/17.png" width="auto" >
</p>


# Integridad, firmas y autenticación

## Manda un documento y la firma electrónica del mismo a un compañero. Verifica la firma que tu has recibido.
Con este comando firmamos nuestro documento .gpg
gpg --sign firma2.pdf

 <p align="center">
    <img src="imgu4/18.png" width="auto" >
</p>
verificamos la firma
**gpg --verify firma2.pdf.gpg**

 <p align="center">
    <img src="imgu4/19.png" width="auto" >
</p>

## ¿Qué significa el mensaje que aparece en el momento de verificar la firma?

gpg: Firma correcta de "Pepe D <[josedom24@gmail.com](mailto:josedom24@gmail.com)>" [desconocido]
gpg: ATENCIÓN: ¡Esta clave no está certificada por una firma de confianza!
gpg: No hay indicios de que la firma pertenezca al propietario.
Huellas dactilares de la clave primaria: E8DD 5DA9 3B88 F08A DA1D 26BF 5141 3DDB 0C99 55FC

1. **"gpg: Firma correcta de 'Pepe D** [josedom24@gmail.com](mailto:josedom24@gmail.com)'": Este mensaje indica que la firma del archivo fue verificada con éxito. La firma pertenece al usuario con el nombre "Pepe D" y la dirección de correo electrónico "[josedom24@gmail.com](mailto:josedom24@gmail.com)".
    
2. **"gpg: ATENCIÓN: ¡Esta clave no está certificada por una firma de confianza!"**: Este mensaje indica que GPG no ha establecido una relación de confianza con la clave pública utilizada para firmar el archivo. En otras palabras, GPG no tiene información sobre la autenticidad de la clave pública.
    
3. **"gpg: No hay indicios de que la firma pertenezca al propietario.":** Este mensaje sugiere que GPG no tiene suficientes indicios para confirmar que la firma pertenece realmente al propietario legítimo de la clave. Esto puede deberse a la falta de certificación por parte de otras claves de confianza.
    
4. **"Huellas dactilares de la clave primaria: E8DD 5DA9 3B88 F08A DA1D 26BF 5141 3DDB 0C99 55FC"**: Las huellas dactilares son una forma de identificar de manera única una clave. La secuencia al final es la huella dactilar de la clave pública. Puedes utilizar esta huella para verificar la autenticidad de la clave con el propietario fuera de línea.

## Vamos a crear un anillo de confianza entre los miembros de nuestra clase, para ello.

    • Tu clave pública debe estar en un servidor de claves 
    • Escribe tu fingerprint en un papel y dárselo a tu compañero, para que puede descargarse tu clave pública. 
    • Te debes bajar al menos tres claves públicas de compañeros. Firma estas claves. 
    • Tu te debes asegurar que tu clave pública es firmada por al menos tres compañeros de la clase. 
    • Una vez que firmes una clave se la tendrás que devolver a su dueño, para que otra persona se la firme. 
    • Cuando tengas las tres firmas sube la clave al servidor de claves y rellena tus datos en la tabla Claves públicas PGP 2020-2021 
    • Asegurate que te vuelves a bajar las claves públicas de tus compañeros que tengan las tres firmas. 

**gpg --keyserver keyserver.ubuntu.com --send-key 63BEB5900CC2FF59**

 <p align="center">
    <img src="imgu4/20.png" width="auto" >
</p>

Bajar Clave de compañero →
lo bajamos con el siguiente comando
**gpg --keyserver keyserver.ubuntu.com --recv-key d9ef9ee3e5274086**


 <p align="center">
    <img src="imgu4/21.png" width="auto" >
</p>

mi compañero bajo mi firma


 <p align="center">
    <img src="imgu4/22.png" width="auto" >
</p>

firmamos la firmamos la firma de nuestro compañero

 <p align="center">
    <img src="imgu4/23.png" width="auto" >
</p>

Muestra las firmas que tiene tu clave pública.
**gpg --list-sign**

 <p align="center">
    <img src="imgu4/24.png" width="auto" >
</p>


### Tarea 2: Correo seguro con evolution/thunderbird
haremos un apt install evolution y seguiremos el asistente de instalación para instalarlo
Ahora seguiremos la siguiente ruta: Editar → Preferencias → Preferencias del editor → Firmas → Añadir → Ponemos nuestra id. 

<p align="center">
    <img src="imgu4/25.png" width="auto" >
</p>

ahora enviaremos el correo electrónico con la configuración recordad poner en opciones encriptado gpg

<p align="center">
    <img src="imgu4/26.png" width="auto" >
</p>


###  1. Para validar el contenido de la imagen CD, solo asegúrese de usar la herramienta apropiada para sumas de verificación. Para cada versión publicada existen archivos de suma de comprobación con algoritmos fuertes (SHA256 y SHA512); debería usar las herramientas sha256sum o sha512sum para trabajar con ellos.

Utilizaremos sha512sum -c SHA512SUMS 2> /dev/null | grep netinst
donde:
sha512sum -c SHA512SUMS: Este comando verifica la integridad de los archivos listados en el archivo SHA512SUMS utilizando los hash SHA-512 proporcionados en ese archivo.
    
1. 2> /dev/null: Este redireccionamiento significa que los mensajes de error (stderr) generados por el comando anterior se enviarán a /dev/null, lo que esencialmente los descartará y no los mostrará en la salida estándar.
    
2. |: Es el operador de tubería, que toma la salida del comando anterior y la pasa como entrada al próximo comando.
    
3. grep netinst: Este comando busca líneas que contengan la cadena "netinst" en la salida del comando anterior. En este contexto, se utiliza para filtrar las líneas relacionadas con imágenes de instalación tipo "netinst".

<p align="center">
    <img src="imgu4/27.png" width="auto" >
</p>

Verifica que el contenido del hash que has utilizado no ha sido manipulado, usando la firma digital que encontrarás en el repositorio. Puedes encontrar una guía para realizarlo en este artículo: How to verify an authenticity of downloaded Debian ISO images
Importaremos las claves publicas de debian
gpg --keyserver keyring.debian.org --recv-keys 64E6EA7D
gpg --keyserver keyring.debian.org --recv-keys 6294BE9B
gpg --keyserver keyring.debian.org --recv-keys 09EA8AC3

<p align="center">
    <img src="imgu4/28.png" width="auto" >
</p>

Verificamos clave:
**gpg –verify SHA512SUMS.SIGN**

<p align="center">
    <img src="imgu4/29.png" width="auto" >
</p>


## Tarea 4: Integridad y autenticidad
### ¿Qué software utiliza apt secure para realizar la criptografía asimétrica?

Software de encriptación GPG
GPG  es un software de encriptación utilizado para cifrar y firmar documentos digitales, especialmente el correo, que utiliza criptografía híbrida: combina criptografía simétrica (por su rapidez), con criptografía asimétrica (por no necesitar compartir claves secretas). Sigue el estándar OpenPGP del IETF y está integrado en numerosos programas como Secure-APT, Kmail, Evolution, Mozilla-Thunderbird, etc.

### ¿Para que sirve el comando apt-key? ¿Qué muestra el comando apt-key list? 

Sirve para gestionar la lista de claves que APT usa para autenticar paquetes. Los paquetes autenticados mediante estas claves se consideran de confianza. apt-key list → lista nuestras claves de confianza.

### En que fichero se guarda el anillo de claves que guarda la herramienta apt-key?

El anillo de claves se guarda en el archivo /etc/apt/trusted.gpg

### ¿Qué contiene el archivo Release de un repositorio de paquetes?. ¿Y el archivo Release.gpg?. Puedes ver estos archivos en el repositorio http://ftp.debian.org/debian/dists/Debian10.1/. Estos archivos se descargan cuando hacemos un apt update.

Debian contiene un archivo llamado Release, el cual se actualiza cada vez que cualquiera de los paquetes en el archivo cambian. Entre otras cosas, el fichero Release contiene algunos md5sums por cada paquete listado en él.

### Explica el proceso por el cual el sistema nos asegura que los ficheros que estamos descargando son legítimos.

**Con la firma digital se logra este cometido**

1. Firma Digital:
        ◦ Los desarrolladores de software a menudo firman digitalmente sus archivos con su clave privada. Al firmar un archivo, el desarrollador genera una "firma digital" única que se adjunta al archivo.
    
2. Claves Públicas y Privadas:
        ◦ Cada desarrollador tiene un par de claves GPG: una clave privada y una clave pública. La clave privada se mantiene en secreto, mientras que la clave pública se distribuye libremente.
    
3. Descarga del Archivo y la Firma:
        ◦ Cuando descargas un archivo que ha sido firmado digitalmente, también obtienes la firma digital del desarrollador.
    
4. Descarga de la Clave Pública del Desarrollador:
        ◦ Antes de verificar la firma, necesitas la clave pública del desarrollador. Puedes obtenerla desde el sitio web oficial del desarrollador o desde un servidor de claves GPG.
    
5. Importar la Clave Pública:
        ◦ Importa la clave pública del desarrollador en tu sistema utilizando el comando gpg --import.
    
6. Verificación de la Firma:
        ◦ Utilizando el comando gpg --verify, verificas la firma digital del archivo descargado con la clave pública del desarrollador. Si la firma es válida, significa que el archivo no ha sido modificado desde que fue firmado por el desarrollador.
    
7. Resultados de la Verificación:
        ◦ Si la verificación es exitosa, obtendrás un mensaje indicando que la firma es válida y que el archivo no ha sido modificado. En caso contrario, recibirás un mensaje de advertencia.

## Tarea 5: Autentificación: ejemplo SSH

### Explica los pasos que se producen entre el cliente y el servidor para que el protocolo cifre la información que se transmite? ¿Para qué se utiliza la criptografía simétrica? ¿Y la asimétrica?

Pasos para Cifrar la Información (SSH):
    
1. Establecimiento de la Conexión:
        ◦ El cliente y el servidor negocian la conexión y acuerdan los parámetros iniciales, incluidos los algoritmos criptográficos a utilizar.
    
2. Acuerdo sobre Algoritmos de Cifrado Simétrico:
        ◦ Ambas partes acuerdan un algoritmo de cifrado simétrico para proteger la confidencialidad de los datos durante la transmisión. Este algoritmo simétrico se utilizará para cifrar y descifrar los datos.
    
3. Generación de una Clave de Sesión:
        ◦ Se genera una clave de sesión única para la sesión actual. Esta clave se utiliza para el cifrado y descifrado de los datos.
    
4. Cifrado Simétrico de Datos:
        ◦ Los datos se cifran utilizando la clave de sesión simétrica antes de ser transmitidos. Esto proporciona una comunicación segura y confidencial entre el cliente y el servidor.

### Uso de Criptografía Simétrica y Asimétrica:
    • Criptografía Simétrica:
        ◦ Propósito: La criptografía simétrica se utiliza para cifrar eficientemente grandes cantidades de datos durante la transmisión.
        ◦ Características: Utiliza la misma clave para cifrar y descifrar datos.
        ◦ En SSH: La criptografía simétrica se utiliza para cifrar los datos durante la transmisión una vez que se ha establecido la conexión segura. Los algoritmos como AES (Advanced Encryption Standard) son comunes.
    • Criptografía Asimétrica (o de Clave Pública):
        ◦ Propósito: La criptografía asimétrica se utiliza para autenticar y establecer claves compartidas de forma segura sin necesidad de compartir la clave secreta.
        ◦ Características: Utiliza un par de claves: una pública y otra privada.
        ◦ En SSH: La criptografía asimétrica se utiliza durante la fase de negociación inicial para autenticar el servidor ante el cliente y viceversa. Además, se utiliza para el intercambio seguro de claves de sesión simétricas, garantizando que solo las partes autorizadas puedan descifrar los datos cifrados simétricamente.

### Explica los dos métodos principales de autentificación: por contraseña y utilizando un par de claves públicas y privadas.

Por contraseña:
Utilizando una contraseña antes generada.
Utilizando claves:
Generando una clave pública y privada. La pública se comparte con la máquina a la que queremos acceder.

### En el cliente para que sirve el contenido que se guarda en el fichero ~/.ssh/know_hosts?

Las claves de máquinas se mantienen en etc/sshknownhosts y en el directorio del usuario ~/.ssh/knownhosts.

### ¿Qué significa este mensaje que aparece la primera vez que nos conectamos a un servidor?
$ ssh debian@172.22.200.74
6. The authenticity of host '172.22.200.74 (172.22.200.74)' can't
be established.
7. ECDSA key fingerprint is
SHA256:7ZoNZPCbQTnDso1meVSNoKszn38ZwUI4i6saebbfL4M.
8. Are you sure you want to continue connecting (yes/no)?

Al escribir yes o aceptar la advertencia en la ventana emergente, se agregará entonces la clave a la lista hosts conocidos (archivo known_hosts).

### En ocasiones cuando estamos trabajando en el cloud, y reutilizamos una ip flotante nos aparece este mensaje:

$ ssh debian@172.22.200.74
11. @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
12. @ WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED! @
13. @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
14. IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
15. Someone could be eavesdropping on you right now (man-in-the-
middle attack)!
16. It is also possible that a host key has just been changed.
17. The fingerprint for the ECDSA key sent by the remote host is
18. SHA256:W05RrybmcnJxD3fbwJOgSNNWATkVftsQl7EzfeKJgNc.
19. Please contact your system administrator.
20. Add correct host key in /home/jose/.ssh/known_hosts to get rid
of this message.
21. Offending ECDSA key in /home/jose/.ssh/known_hosts:103
22. remove with:
23. ssh-keygen -f "/home/jose/.ssh/known_hosts" -R
"172.22.200.74"
24. ECDSA host key for 172.22.200.74 has changed and you have
requested strict checking.

La clave del equipo remoto ha cambiado.

### ¿Qué guardamos y para qué sirve el fichero en el servidor ~/.ssh/authorized_keys?

El archivo Authorized_keys en SSH especifica las claves SSH que se pueden usar para iniciar sesión en la cuenta de usuario para la que está configurado el archivo. Es un archivo de configuración muy importante, ya que configura el acceso permanente mediante claves SSH y necesita una gestión adecuada.


# cifrado asimetrico

## 1. Genera un par de claves (pública y privada). ¿En que directorio se guarda las claves de un usuario?

Para generar claves publicas y privadas usamos gpg –gen-key

<p align="center">
    <img src="imgu4/30.png" width="auto" >
</p>

se guardan en ls ~/.gnupg

### 2. Lista las claves públicas que tienes en tu almacén de claves. Explica los distintos datos que nos muestra. ¿Cómo deberías haber generado las claves para indicar, por ejemplo, que tenga un 1 mes de validez?

Para listar las claves usamos gpg --list-keys

<p align="center">
    <img src="imgu4/31.png" width="auto" >
</p>

para generar las claves con un mes de expiración usaremos ese comando 
**gpg --gen-key --expires 1m**

### 3. Lista las claves privadas de tu almacén de claves.

Para listar las claves privadas del almacen usaremos el comando:
**gpg --list-secret-keys**

### 1. Exporta tu clave pública en formato ASCII y guardalo en un archivo nombre_apellido.asc y envíalo al compañero con el que vas a hacer esta práctica.

Para exhortar mi clave publica en formto ASCII y guardarlo en un archivo .asc lo haremos de la siguiente manera

**gpg --armor --export [jrodsan233y@g.educaand.es](mailto:jrodsan233y@g.educaand.es) > jesus_rodriguez.asc**

<p align="center">
    <img src="imgu4/32.png" width="auto" >
</p>

### 2. Importa las claves públicas recibidas de vuestro compañero.

Para importar el archivo usaremos el comando
**gpg --import jesus_rodriguez.asc**

<p align="center">
    <img src="imgu4/33.png" width="auto" >
</p>

### 3. Comprueba que las claves se han incluido correctamente en vuestro keyring.

Para comprobar si se ha añadido correctamente la clave publica usaremos **gpg –list-keys** y para la privada usaremos **gpg –list-secret-keys**


<p align="center">
    <img src="imgu4/34.png" width="auto" >
</p>

## Cifrado asimétrico con claves publicas

### 1. Cifraremos un archivo cualquiera y lo remitiremos por email a uno de nuestros compañeros que nos proporcionó su clave pública.

Para encriptarlo usaremos
**gpg --encrypt --recipient jrodsan@g.educaand.es --output jesuscifrado.gpg parajesus.txt **

<p align="center">
    <img src="imgu4/35.png" width="auto" >
</p>

donde --recipient es el destinatario el cual sera el unico que con su clave privada podra desencriptarlo

### 2. Nuestro compañero, a su vez, nos remitirá un archivo cifrado para que nosotros lo descifremos.

Nuestro compañero lo desencriptara de la siguient manera
**gpg --decrypt archivo_cifrado.gpg**

<p align="center">
    <img src="imgu4/36.png" width="auto" >
</p>

### 3. Tanto nosotros como nuestro compañero comprobaremos que hemos podido descifrar los mensajes recibidos respectivamente.

Con el siguiente comando
**gpg --decrypt archivo_cifrado.gpg**

### 4. Por último, enviaremos el documento cifrado a alguien que no estaba en la lista de destinatarios y comprobaremos que este usuario no podrá descifrar este archivo

descifararemos con **gpg --decrypt archivo_cifrado.gpg**


<p align="center">
    <img src="imgu4/37.png" width="auto" >
</p>

### 5. Para terminar, indica los comandos necesarios para borrar las claves públicas y privadas que posees.

Para borrar las claves privadas y publicas usaremos respectivamente
**gpg --delete-secret-key NOMBRE**
**gpg --delete-key NOMBRE**

## tema 4 Exportar clave a un servidor público de claves PGP

### 1. Genera la clave de revocación de tu clave pública para utilizarla en caso de que haya problemas.
De manera normal el gpg ya crea un par de claves si queremos borrarlas usaremos --gen-revoke <ID> el id lo encontraremos usando el comando gpg –list-keys

para generar la clave usaremos el comando **gpg --gen-revoke nombre**
además del nombre también podemos darle el RSA correspondiente
una vez lo hagamos nos hará un par de preguntas y nos generará la nueva clave


<p align="center">
    <img src="imgu4/38.png" width="auto" >
</p>

### 2. Exporta tu clave pública al servidor pgp.rediris.es

para exportar la clave usaremos el comando

**gpg --send-keys --keyserver pgp.rediris.es RSA**

aun que usaremos el servidor Keys.openpgp.org ya que el anterior no esta activo actualmente

<p align="center">
    <img src="imgu4/39.png" width="auto" >
</p>

### 3. Borra la clave pública de alguno de tus compañeros de clase e impórtala ahora del servidor público de rediris

**gpg --delete-key "RSA"**
**para descargarlo lo haremos de la siguiente manera**
**gpg --keyserver Keys.openpgp.org --recv-key **

## Tema 5 cifrado asimétrico con openssl

### 1. Genera un par de claves (pública y privada).

Usaremos el algoritmo RSA por lo que tenemos que indicar el opción genrsa 
**sudo openssl genrsa -aes128 -out key.pem 2048**


<p align="center">
    <img src="imgu4/40.png" width="auto" >
</p>

### 2. Envía tu clave pública a un compañero.
Para extraer la clave publica usaremos el comando
**sudo openssl rsa -in key.pem -pubout -out key.public.pem**

donde:
-in <pardeclaves> para indicar el par de claves del que queremos extraer la clave pública
-pubout para indicar que extraiga la clave pública
-out <ficherosalida> para extraer la clave a un fichero .public.pem

<p align="center">
    <img src="imgu4/41.png" width="auto" >
</p>

si hacemos un cat veremos la clave

<p align="center">
    <img src="imgu4/42.png" width="auto" >
</p>

### 3. Utilizando la clave pública cifra un fichero de texto y envíalo a tu compañero.

Para cifrar un archivo usaremos el comando:

**openssl pkeyutl -encrypt -in fichero.txt -out fichero.enc -inkey key.public.pem -pubin**

    • -pkeyutl : Este comando de OpenSSL se utiliza para realizar operaciones de cifrado y descifrado con RSA.
    • -encrypt: Indica que la operación que deseas realizar es el cifrado de datos. En este caso, estás cifrando el contenido del archivo especificado en la opción -in.
    • -in fichero.txt: Especifica el nombre del archivo de entrada que deseas cifrar. En este caso, fichero.txt contiene los datos que serán cifrados.
    • -out fichero.enc: Indica el nombre del archivo de salida que contendrá los datos cifrados. En este caso, se ha especificado que el archivo cifrado se llame fichero.enc.
    • -inkey clave.public.pem: Especifica la clave pública RSA que se utilizará para cifrar los datos.

<p align="center">
    <img src="imgu4/43.png" width="auto" >
</p>

### 4. Tu compañero te ha mandado un fichero cifrado, muestra el proceso para el descifrado. 
Para descifrar un fichero cifrado usaremos el siguiente comando
**sudo openssl pkeyutl -decrypt -in fichero.enc -out secreto.txt -inkey key.pem**
<p align="center">
    <img src="imgu4/44.png" width="auto" >
</p>
