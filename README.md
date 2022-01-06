- ## Instalar Docker
https://tame-kick-708.notion.site/Docker-y-Devilbox-4d7006a25ea143b69e3a57002193e1b7
Descargaremos docker desktop, para eso hay que visitar su 👉 sitio web y seleccionar el sistema operativo.

- ## Proceso de instalación Devilbox:
1.- Para instalar devilbox primero hay que descargarlo, por lo tanto nos ayudaremos de git para clonar el repositorio en nuestra computadora.
  1. Localiza la carpeta en la que clonaras el repositorio. En esta caso usaremos la carpeta raíz, por lo tanto hay que moverse a esa carpeta
  en nuestra terminal.
  2. Usaremos el siguiente comando para clonar el repositorio:
    $ git clone https://github.com/cytopia/devilbox
  3. Entramos a la carpeta de devilbox con el comando `$ cd devilbox`
  
- ## Configuración
1.- Ejecutamos el siguiente comando para copiar el archivo de variables de ambiente.  
  $ cp env-example .env
  
2.- En el archivo .env puedes configurar la versión de PHP, MariaDB y el servidor. En este caso usaremos la versión de PHP 7.4, MariaDB-10.3 y 
Nginx como servidor por lo que dejaremos las versiones como vienen por default en este archivo. 
  También configuraremos las rutas para que devilbox encuentre los proyectos con los que trabajaremos, por lo tanto creamos una carpeta en el carpeta 
  raíz, abre tu terminal y ejecuta con el siguiente comando: 
    $ mkdir sites
  
  Dentro del archivo .env buscaremos la siguiente línea HOST_PATH_HTTPD_DATADIR y agregaremos la ruta completa de la carpeta que creamos. Debe quedar 
  algo similar a esto:  
    HOST_PATH_HTTPD_DATADIR=~/sites
    
  En el mismo archivo buscaremos la línea HTTPD_DOCROOT_DIR y una ves identificada agregaremos la carpeta web para que Craft CMS pueda trabajar sin 
  problemas. Algo similar a esto: HTTPD_DOCROOT_DIR=web/
  Con esto ya tenemos la configuración completa y estaremos listos para instalar devilbox.
  
- ## Instalación
1.- Para la instalación asegurate de estar conectado a internet, ya que durante el proceso descargará imagenes de php, nginx y mariadb. Tendremos que 
ejecutar el siguiente comando para que solo descargue las imagenes de las tecnologías que antes mencionamos.

2.- Abre tu terminal y ejecuta el siguiente comando `docker-compose up -d httpd php mysql` esto demorará algunos minutos, por lo que puedes ir a 
preparar café ☕️

3.- Al finalizar ejecuta el siguiente comando `docker ps` para observar lo que esta ejecutando docker, verás que muestra que devilbox se esta ejecutando 
en segundo plano.

4.- Ahora puedes visitar [localhost](http://localhost) y te cargará la interfaz de devilbox.

<----------------------------------------------------------------------------------------------------------------------------------------------------------->

Craft CMS | Instalación básica

- ## Requisitos
  - Conocimientos básicos de terminal
  - Contar con un entorno web que permita ejecutar PHP y MySQL. Documentación recomendada 🐳  
  [Docker y Devilbox](https://www.notion.so/Docker-y-Devilbox-4d7006a25ea143b69e3a57002193e1b7)
  
  - Estos son los requerimientos mínimos para ejecutar Craft. Estos requisitos son cubiertos completamente por Devilbox por lo que no tendrás que preocuparte 
  si es que ya lo tienes instalado.
  [Requirements](https://craftcms.com/docs/3.x/requirements.html#minimum-system-specs)
  
- ## Instalación

1.- Abre tu terminal y muévete a la carpeta de devilbox e inicia el contenedor. Puedes ejecutar los siguientes comandos:
  $ cd devilbox
  $ docker-compose up -d httpd php mysql
  
2.- Ahora que el contenedor se ha iniciado, entraremos con él con el siguiente comando:
  $ ./shell.bat
  
3.- Te darás cuenta que estaremos dentro del contenedor porque mostrará un mensaje con el nombre del contenedor, el cual es Devilbox. La ruta inicial /shared/httpd 
hace referencia a la carpeta que contendrá todos nuestros proyectos, por lo tanto ahí crearemos una carpeta para instalar Craft. Puedes ejecutar el siguiente 
comando `mkdir craft` para crear una carpeta con el nombre craft.

4.- Para instalar Craft usaremos composer, por lo tanto entraremos en la carpeta que acabamos de crear y ejecutaremos dentro el siguiente comando.
  $ composer create-project craftcms/craft craftcms
  
5.- Mientras se descargan las dependencias crearemos la base de datos. Devilbox tiene pre-instalado phpMyAdmin por lo tanto lo usaremos para gestionar nuestras bases de 
datos. Podemos acceder a él visitando a http://localhost/ y buscamos en la navbar → tools → phpMyAdmin abrirá una pestaña y ahí crearemos la base de datos.
Crea tu base de datos como: utf8_general_ci para que acepte caracteres especiales.

6.- A continuación instalaremos Craft CMS, por lo que escribiremos [yes] en la terminal, verás que tendrás que agregar información adicional relacionada con la conexión a 
la base de datos. Agregaremos la siguiente información (no agregue los comentarios, son solo referencia):

Which database driver are you using? [mysql,pgsql,?]: mysql #Estamos usando mysql
Database server name or IP address: [127.0.0.1] #Dejamos identico el host
Database port: [3306] #El puerto también se queda igual
Database username: [root] #Es el usuario que viene por default con devilbox
Database password: #Por defecto devilbox no agrega contraseña, si tu configuraste una agregala
Database name: craft #El nombre de la base de datos que creaste
Database table prefix: #Dejalo vacio

Si agregaste la información correctamente verás que te muestra el siguiente mensaje 👇
Testing database credentials ... success!
Saving database credentials to your .env file ... done

A continuación observarás que pide información del usuario administrador, solo hay que agregar la información que te pide.

- ## Symlink webroot
Hacer un enlace simbólico del directorio webroot real a htdocs es importante. El servidor web espera que la raíz del documento de cada proyecto esté en formato. Esta es 
la ruta donde se ubican los archivos. Esta es también la ruta donde se debe encontrar el punto de entrada de su framework (generalmente ).<vhost dir>/htdocs/index.php
Sin embargo, algunos marcos proporcionan su contenido real en directorios anidados de niveles desconocidos. Esto sería imposible de averiguar por el servidor web, por lo 
que debe vincularlo manualmente a su ruta esperada.
  devilbox@php-7.0.20 in /shared/httpd/my-craft $ ln -s craftcms/web/ htdocs
  
- ## Registro DNS
Si no tiene configurado el DNS automático, deberá agregar la siguiente línea `/etc/hosts` al archivo de su sistema operativo host (o C:\Windows\System32\drivers\etc 
en Windows):
  127.0.0.1 my-craft.loc


