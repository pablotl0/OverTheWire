# **Memoria del Wargame 'Bandit' - OverTheWire**

## **Introducción**
Bandit es un wargame de seguridad informática desarrollado por OverTheWire. Su objetivo es enseñar los fundamentos de la línea de comandos en Linux, la manipulación de archivos, permisos, conexiones remotas y otras técnicas esenciales para la seguridad informática. Cada nivel consiste en encontrar la contraseña para el siguiente nivel utilizando diversas herramientas y comandos de Linux.

Este documento proporciona soluciones detalladas para todos los niveles de Bandit hasta el nivel 34, explicando los comandos utilizados y su propósito.

---

## **Niveles y Soluciones**

### **Nivel 0 -> Nivel 1**
**Objetivo:** Conectarse al servidor mediante SSH y obtener la contraseña del siguiente nivel.

**Comando:**
```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```
**Solución:**
Ingresamos la contraseña inicial (`bandit0`) y ejecutamos:
```bash
cat readme
```
La contraseña para el siguiente nivel se muestra en pantalla.

---

### **Nivel 1 -> Nivel 2**
**Objetivo:** Leer un archivo con nombre especial (`-`).

**Comando:**
```bash
cat ./-
```
**Solución:**
Usamos `cat ./-` para evitar que el `-` sea interpretado como una opción de comando.

---

### **Nivel 2 -> Nivel 3**
**Objetivo:** Leer un archivo en una carpeta con espacios en su nombre.

**Comando:**
```bash
cat "spaces in this filename"

cat spaces\ in\ this\ filename
```
**Solución:**
Usamos comillas para manejar el nombre con espacios o simplemente tabulando se autocompleta.

---

### **Nivel 3 -> Nivel 4**
**Objetivo:** Encontrar y leer un archivo oculto en la carpeta `inhere`.

**Comando:**
```bash
ls -a inhere
cat inhere/...Hiding-From-You
```
**Solución:**
Listamos archivos ocultos con `ls -a` y usamos `cat` para leer `...Hiding-From-Youca`.

---

### **Nivel 4 -> Nivel 5**
**Objetivo:** Encontrar un archivo con texto ASCII en el directorio inhere.

**Comando:**
```bash
for dir in inhere/*
do
  file $dir
done
cat inhere/-file07
```
**Solución:**
Utilizamos un bucle for para iterar sobre todos los archivos y directorios dentro de inhere/. El comando file se usa para identificar el tipo de archivo. Este comando nos ayudará a encontrar cuál de los archivos contiene texto ASCII y luego lo leemos con `cat`.

---

### **Nivel 5 -> Nivel 6**
**Objetivo:** Encontrar un archivo con permisos específicos dentro de `inhere`: 
- Fichero
- Tamaño: 33 bytes
- No ejecutable


**Comando:**
```bash
find inhere -type f -size 1033c ! -executable
```
**Solución:**
Usamos `find` para ubicar el archivo con los permisos correctos y luego lo leemos con `cat`.

---

### **Nivel 6 -> Nivel 7**

**Objetivo:** Encontrar un archivo que tenga las siguientes propiedades:
- Propietario: `bandit7`
- Grupo: `bandit6`
- Tamaño: 33 bytes

**Comando:**
```bash
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
cat /var/lib/dpkg/info/bandit7.password
```

**Solución:**
Utilizamos el comando `find` para buscar archivos que coincidan con las siguientes condiciones:
- El archivo debe ser propiedad del usuario `bandit7` (`-user bandit7`).
- El archivo debe pertenecer al grupo `bandit6` (`-group bandit6`).
- El archivo debe tener un tamaño exacto de 33 bytes (`-size 33c`).

El comando redirige cualquier mensaje de error a `/dev/null` para evitar mostrar permisos denegados. Una vez que se encuentra el archivo, podemos leerlo con `cat`.

---

### **Nivel 7 -> Nivel 8**
**Objetivo:** Encontrar la contraseña en un archivo de datos.

**Comando:**
```bash
grep "millionth" data.txt
```
**Solución:**
Usamos `grep` para filtrar la línea que contiene la palabra millionth.

---

### **Nivel 8 -> Nivel 9**
**Objetivo:** Identificar la única línea que no se repite dentro de un archivo.

**Comando:**
```bash
sort data.txt | uniq -u
```
**Solución:**
Primero, utilizamos el comando `sort` para ordenar las líneas del archivo data.txt. Esto es importante porque uniq solo puede identificar líneas duplicadas si están adyacentes, por lo que ordenarlas previamente asegura que las líneas repetidas estén juntas.

Luego, usamos `uniq -u` para filtrar y mostrar solo las líneas que son únicas, es decir, aquellas que no tienen duplicados en el archivo. Esta línea única será la que contiene la contraseña para el siguiente nivel.

---

### **Nivel 9 -> Nivel 10**
**Objetivo:** Encontrar una cadena con varios `=` en un archivo binario.

**Comando:**
```bash
strings data.txt | grep "=="
```
**Solución:**
Utilizamos `strings` para extraer las secuencias de texto legibles en archivos binarios y `grep` para filtrar la contraseña.

---

### **Nivel 10 -> Nivel 11**
**Objetivo:** Decodificar una contraseña codificada en Base64.

**Comando:**
```bash
base64 -d data.txt
```
**Solución:**
Usamos `base64 -d` para decodificar el contenido y obtener la contraseña.

---

### **Nivel 11 -> Nivel 12**
**Objetivo:** La contraseña para el siguiente nivel está almacenada en el archivo `data.txt`, donde todas las letras en minúsculas (a-z) y mayúsculas (A-Z) han sido rotadas 13 posiciones (ROT13).

**Comando:**
```bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

**Solución:**
El archivo `data.txt` contiene texto cifrado usando ROT13, un tipo de cifrado simple que rota las letras 13 lugares en el alfabeto. Para descifrarlo, utilizamos el comando `tr` (transliterate) para aplicar la transformación ROT13. 

- El comando `tr 'A-Za-z' 'N-ZA-Mn-za-m'` rota las letras del alfabeto:
  - Las letras mayúsculas de la A a la Z se rotan de A a M y N a Z.
  - Las letras minúsculas de la a a la z se rotan de la a la m y n a la z.

--- 

### **Nivel 12 -> Nivel 13**
**Objetivo:** Encontrar la contraseña del siguiente nivel en el archivo `data.txt`, un hexdump multiples veces comprimido.

**Comando:**
```bash
# Crear un directorio temporal y copiar el archivo data.txt
mktemp
cp data.txt /tmp/temp_dir

# Navegar al directorio temporal
cd /tmp/temp_dir/

# Convertir el archivo binario a su formato original
xxd -r data.txt > data1

# Verificar el tipo de archivo
file nombre_datos

# Descomprimir el archivo usando gzip
gzip -cd data1 > data2

# Descomprimir usando bzip2
bzip2 -d data

# Extraer archivos usando tar
tar -xvf data
```

**Solución:**
1. **`mktemp`**: Crea un directorio temporal para trabajar con los archivos.
2. **`cp data.txt /tmp/temp_dir`**: Copia el archivo `data.txt` a un directorio temporal para evitar modificaciones accidentales.
3. **`cd /tmp/temp_dir/`**: Navegamos al directorio donde hemos copiado el archivo.
4. **`xxd -r data.raw > data1`**: Usamos `xxd` para convertir el archivo `data.raw` (que puede estar en un formato hexadecimal) de vuelta a su formato binario original y lo guardamos como `data1`.
5. **`file nombre_datos`**: Verificamos el tipo de archivo con el comando `file` para asegurarnos de qué formato tiene el archivo y cuál es la siguiente acción a realizar.
6. **Descomprimir multiples veces con las distintas herramientas de descompresion**
---

### **Nivel 13 -> Nivel 14**
**Objetivo:** Iniciar sesión en el siguiente nivel utilizando una clave SSH privada.

**Comandos:**
```bash
 ssh -i sshkey.private bandit14@localhost -p 2220
```
**Solución:**
Se nos proporciona una clave SSH privada (`sshkey.private`) para autenticar nuestra conexión al siguiente nivel desde local.

---

### **Nivel 14 -> Nivel 15**
**Objetivo:** Establecer una comunicación de red utilizando Netcat.

**Comando:**
```bash
 cat /etc/bandit_pass/bandit14 | nc bandit.labs.overthewire.org 30000
 cat /etc/bandit_pass/bandit14 | telnet localhost 30000
```
**Solución:**
Usamos `nc` (Netcat) para conectarnos al puerto 30000 del servidor y tras pasarle la contraseña del nivel recibimos la contraseña del siguiente nivel.
También se podría resolver usando telnet.
---

### **Nivel 15 -> Nivel 16**
**Objetivo:** Establecer una comunicación segura utilizando OpenSSL.

**Comando:**
```bash
echo 8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo | openssl s_client -connect bandit.labs.overthewire.org:30001
```
**Solución:**
Utilizamos `openssl s_client` para establecer una conexión SSL/TLS con el servidor y obtener la contraseña del siguiente nivel proporcionando la del nivel actual.

---

### **Nivel 16 -> Nivel 17**
**Objetivo:** Realizar un escaneo de puertos y servicios en el rango 31000-32000 utilizando nmap, identificar un puerto con un servicio SSL y conectarse a él para obtener la contraseña.

**Comando:**
```bash
nmap -sT localhost -p 31000-32000 | grep "open" | awk '{print $1}' | cut -d '/' -f 1 | xargs -I {} nmap -sV localhost -p {}
echo kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx | openssl s_client -quiet -connect localhost:31790
```
**Solución:**
1. **Escaneo de puertos abiertos:**  
   - Ejecutamos `nmap -sT localhost -p 31000-32000` para escanear el rango de puertos especificado.  
   - Filtramos los puertos abiertos con `grep "open"`.  
   - Extraemos los números de puerto con `awk '{print $1}' | cut -d '/' -f 1`.  
   - Para cada puerto abierto, realizamos un escaneo detallado con `nmap -sV` para identificar los servicios que se ejecutan en ellos.  

2. **Conexión a un servicio SSL:**  
   - Tras identificar el puerto 31790 como un servicio SSL, usamos `openssl s_client -quiet -connect localhost:31790` para conectarnos a él.  
   - Enviamos la clave `kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx` como entrada estándar para autenticarnos y obtener la contraseña del siguiente nivel.  
---

### **Nivel 17 -> Nivel 18**
**Objetivo:** Encontrar diferencias entre archivos.

**Comando:**
```bash
diff passwords.old passwords.new
```
**Solución:**
Usamos `diff` para comparar dos archivos y encontrar las diferencias que nos revelarán la contraseña del siguiente nivel.

---


### **Nivel 18 -> Nivel 19**
**Objetivo:** Acceder a un archivo readme en el home del usuario, evitando que `.bashrc` cierre la sesión automáticamente.

**Comando:**
```bash
ssh  -p2220 bandit18@bandit.labs.overthewire.org "cat /home/bandit18/readme"

```
**Solución:**
Al intentar conectarse normalmente por SSH, `.bashrc` cierra la sesión automáticamente. Para evitarlo, se ejecuta directamente el comando `cat /home/bandit18/readme` en la conexión SSH, lo que permite leer la contraseña sin iniciar una sesión interactiva.
---

### Nivel 19 -> Nivel 20

**Objetivo:** Utilizar el binario setuid en el directorio home para obtener la contraseña del siguiente nivel.

**Comando:**

```bash
./bandit20-do cat /etc/bandit_pass/bandit20
```

**Solución:** El binario `bandit20-do` permite ejecutar comandos con privilegios elevados. Al utilizarlo para leer el archivo `/etc/bandit_pass/bandit20`, se obtiene la contraseña del siguiente nivel.

---

### Nivel 20 -> Nivel 21

**Objetivo:** Utilizar el binario setuid `suconnect` para obtener la contraseña del siguiente nivel.

**Comando:**

Terminal 1:

```bash
echo password | nc -l -p 12345
```

Terminal 2:

```bash
./suconnect 12345
```

**Solución:** En la Terminal 1, se inicia un servidor que escucha en el puerto 12345 utilizando `nc` y devuelve la contraseña con `echo` en caso de conexion al puerto. En la Terminal 2, se ejecuta el binario `suconnect` especificando el puerto en el que `nc` está escuchando. Si la contraseña es correcta, el servidor enviará de vuelta la contraseña para el siguiente nivel.

---

### **Nivel 21 → Nivel 22**  

**Objetivo:** Encontrar la contraseña en los archivos de cron.  

**Comando:**  

```bash
cd /etc/cron.d/
ls -l
cat cronjob_bandit22
cat /usr/bin/cronjob_bandit22.sh
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

**Solución:**  
El archivo `cronjob_bandit22` ejecuta periódicamente el script `cronjob_bandit22.sh`, que almacena la contraseña en un archivo temporal dentro de `/tmp/`. Al revisar este archivo, obtenemos la contraseña del siguiente nivel.  

---

### **Nivel 22 → Nivel 23**  

**Objetivo:** Obtener la contraseña generada por un script cron.  

**Comando:**  

```bash
cd /etc/cron.d/
ls -l
cat cronjob_bandit23
cat /usr/bin/cronjob_bandit23.sh
whoami
myname=bandit23
mytarget=$(echo "I am user $myname" | md5sum | cut -d ' ' -f 1)
cat /tmp/$mytarget
```

**Solución:**  
El script `cronjob_bandit23.sh` genera un hash MD5 con el formato `"I am user <usuario>"` y guarda la contraseña en un archivo en `/tmp/`. Calculamos el hash correspondiente a `bandit23` y lo usamos para localizar el archivo con la contraseña.  

---

### **Nivel 23 → Nivel 24**  

**Objetivo:** Aprovechar un cronjob para extraer la contraseña.  

**Comando:**  

```bash
cd /etc/cron.d/
ls -l
cat cronjob_bandit24
cat /usr/bin/cronjob_bandit24.sh
mkdir /tmp/myscript
cd /tmp/myscript
nano pass.sh
  #!/bin/bash
  cat /etc/bandit_pass/bandit24 > /tmp/myscript/password" 
chmod +x pass.sh
cp pass.sh /var/spool/bandit24/foo
cat /tmp/myscript/password
```

**Solución:**  
El script de cron ejecuta cualquier archivo ubicado en `/var/spool/bandit24/`. Creamos un script que copia la contraseña a un archivo accesible en `/tmp/`, lo colocamos en esa carpeta y esperamos a que sea ejecutado automáticamente. Luego, leemos el archivo generado.  

---

### **Nivel 24 → Nivel 25**  

**Objetivo:** Realizar un ataque de fuerza bruta contra un servicio en el puerto 30002.  

**Comando:**  

```bash
mkdir /tmp/brute-force
cd /tmp/brute-force
nano brute.sh
  #!/bin/bash
  for i in {0000..9999}; 
    do echo \"password \$i\"; 
  done | nc localhost 30002" > brute.sh
chmod +x brute.sh
./brute.sh
```

**Solución:**  
El servidor en el puerto 30002 requiere la contraseña anterior seguida de un código PIN de 4 dígitos. Generamos todas las combinaciones posibles y las enviamos con `nc`. Cuando encontramos el PIN correcto, el servidor nos devuelve la nueva contraseña.  

---

### **Nivel 25 → Nivel 26**  

**Objetivo:** Acceder al siguiente nivel usando una clave privada SSH.  

**Comando:**  

```bash
ls -l
file bandit26.sshkey
chmod 600 bandit26.sshkey
ssh -i bandit26.sshkey bandit26@localhost
cat /etc/bandit_pass/bandit26
```

**Solución:**  
El archivo `bandit26.sshkey` es una clave privada SSH que permite autenticarse como `bandit26` sin necesidad de contraseña. Se usa para conectarse al servidor y leer el archivo con la nueva contraseña.  

---

### **Nivel 26 → Nivel 27**  

**Objetivo:** Utilizar un binario con permisos especiales para obtener la contraseña.  

**Comando:**  

```bash
ls -la
file bandit27-do
cat text.txt
./bandit27-do id
./bandit27-do whoami
echo "cat /etc/bandit_pass/bandit27" > /tmp/getpass.sh
chmod +x /tmp/getpass.sh
./bandit27-do /tmp/getpass.sh
```

**Solución:**  
El binario `bandit27-do` tiene permisos especiales que permiten ejecutar comandos como `bandit27`. Creamos un script en `/tmp/` que lee la contraseña y lo ejecutamos con `bandit27-do` para obtener la contraseña del siguiente nivel.  

---

### Nivel 27 -> Nivel 28

**Objetivo:** Encontrar la contraseña del siguiente nivel en un repositorio de Git.

**Comando:**

```bash
mkdir /tmp/git27
cd /tmp/git27
git clone ssh://bandit27-git@localhost/home/bandit27-git/repo
cd repo
cat README
```

**Solución:** Se clona el repositorio de Git proporcionado y se lee el archivo `README` para obtener la contraseña del siguiente nivel.

---

### Nivel 28 -> Nivel 29

**Objetivo:** Encontrar la contraseña del siguiente nivel en un repositorio de Git con múltiples commits.

**Comando:**

```bash
mkdir /tmp/git28
cd /tmp/git28
git clone ssh://bandit28-git@localhost/home/bandit28-git/repo
cd repo
git log
# Identificar el commit que contiene la contraseña
git show <commit_id>
```

**Solución:** Se clona el repositorio de Git proporcionado, se revisa el historial de commits para identificar aquel que contiene la contraseña y se muestra su contenido para obtener la contraseña del siguiente nivel.

---

### **Nivel 29 → Nivel 30**  

**Objetivo:** Encontrar la contraseña en el historial de cambios de un repositorio Git.  

**Comando:**  

```bash
mkdir /tmp/git29
cd /tmp/git29
git clone ssh://bandit29-git@localhost/home/bandit29-git/repo
cd repo
cat README.md
git branch -a
git checkout dev
git log
git diff 33ce2e95d9c5d6fb0a40e5ee9a2926903646b4e3 a8af722fccd4206fc3780bd3ede35b2c03886d9b
```

**Solución:**  
El repositorio contiene varias ramas. Cambiamos a la rama `dev` y revisamos el historial con `git log`. Luego, usamos `git diff` para comparar los cambios entre dos commits, lo que revela la contraseña del siguiente nivel.  

---

### **Nivel 30 → Nivel 31**  

**Objetivo:** Encontrar la contraseña en una etiqueta (`tag`) oculta en Git.  

**Comando:**  

```bash
mkdir /tmp/git30
cd /tmp/git30
git clone ssh://bandit30-git@localhost/home/bandit30-git/repo
cd repo
cat README.md
git branch -a
git show-branch --all
git log
cat .git/packed-refs
git show-ref --tags -d
git show --name-only secret
```

**Solución:**  
El repositorio tiene una etiqueta (`tag`) llamada `secret` que no aparece en `git log`. Usamos `git show-ref --tags -d` para encontrarla y `git show secret` para revelar la contraseña.  

---

### **Nivel 31 → Nivel 32**  

**Objetivo:** Subir un archivo ignorado por `.gitignore` al repositorio.  

**Comando:**  

```bash
mkdir /tmp/git31
cd /tmp/git31
git clone ssh://bandit31-git@localhost/home/bandit31-git/repo
cd repo
ls -la
cat README.md
touch key.txt
echo "May I come in?" > key.txt
cat .gitignore
git add -f key.txt
git commit -m 'add key'
git push origin master
```

**Solución:**  
El repositorio tiene un archivo `.gitignore` que impide subir archivos `.txt`. Usamos `git add -f` para forzar la inclusión de `key.txt`, lo confirmamos con `git commit` y lo subimos con `git push`. Al hacerlo, el servidor nos muestra la contraseña.  

---

### **Nivel 32 → Nivel 33**  

**Objetivo:** Ejecutar un shell interactivo para leer la contraseña.  

**Comando:**  

```bash
ls
clear
$0
pwd
ls -la 
file uppershell
cat uppershell
cat /etc/bandit_pass/bandit33
```

**Solución:**  
Este nivel nos permite ejecutar un shell restringido. Ejecutamos `$0` para abrir una nueva instancia del shell sin restricciones y luego usamos `cat` para leer la contraseña.  

---
