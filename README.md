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
- A la cual accederemos con la URL que ya teniamos de CozyHosting





