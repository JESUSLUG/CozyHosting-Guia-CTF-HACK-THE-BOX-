```markdown
# CozyHosting-Guia-CTF-HACK-THE-BOX

En este repositorio se busca explicar paso a paso la búsqueda de la bandera. Pensé en crear esto para las personas nuevas en el mundo del hacking ético y que no se sientan tan abrumadas con tantos conceptos.

**Lo primero que haremos es identificar nuestra máquina que querremos atacar, que en este caso es la CozyHosting. Esta tiene la siguiente información:**

- **IP: 10.10.11.230**

## Editar el archivo /etc/hosts

Para comenzar, necesitamos editar nuestro archivo `/etc/hosts` utilizando el siguiente comando:

```bash
sudo nano /etc/hosts
```

- **`sudo`**: "sudo" es un comando en sistemas Unix y Linux que significa "superusuario do". Permite a un usuario ejecutar un comando con privilegios elevados, generalmente como el superusuario o administrador del sistema. El uso de "sudo" es necesario para editar archivos que requieren permisos de administrador, como el archivo `/etc/hosts`.

- **`nano`**: "nano" es un editor de texto en la línea de comandos. Es una herramienta simple y fácil de usar para editar archivos de texto en la terminal. En este caso, se utiliza para abrir y editar el archivo `/etc/hosts`.

- **`/etc/hosts`**: "/etc/hosts" es la ruta completa al archivo que estás tratando de abrir y editar. Este archivo es importante porque se utiliza para resolver nombres de host a direcciones IP en sistemas Unix y Linux. Cuando tu computadora intenta acceder a un sitio web o un recurso en la red, primero verifica si la dirección IP correspondiente está en este archivo antes de recurrir a un servidor DNS público. Agregar entradas en este archivo te permite personalizar la resolución de nombres en tu sistema local.

**Ya dentro, procedemos a agregar nuestra IP junto con la URL.**

*(para guardar con Ctrl+O, Ctrl+X, yes, te preguntará si quieres cambiar el nombre, simplemente dale enter)*
```

En este formato, solo los comandos de Kali Linux son copiables, mientras que el resto del texto está en formato Markdown para una mejor legibilidad.
