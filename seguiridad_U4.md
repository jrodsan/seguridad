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
