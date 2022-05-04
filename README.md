# Instalación y configuración del servidor web Nginx: *Virtual Hosts*

## · Instalando Nginx
Para la práctica se ha optado por instalar Nginx en Windows, de cara al desarrollo del proyecto final. 
En primer lugar, descargamos la última versión de Nginx desde la página oficial y después llevamos a cabo la siguiente secuencia de comandos para su instalación y posterior ejecución.
~~~
cd C:/Program Files
unzip nginx-1.21.6.zip
cd nginx-1.21.6
start nginx
~~~
Una vez se ha ejecutado Nginx, realizamos la comprobación de su correcto funcionamiento escribiendo **localhost** en el *browser*.

![alt_text](https://github.com/carlosblancoj/Practica_Nginx_Stmas/blob/main/img_nginx/nginx1.PNG)
## · Configurando Nginx
Para la configuración de *virtual hosts* en Windows, primero crearemos en el directorio **conf/sites-available** un fichero **.conf** por cada sitio web que queremos que Nginx aloje, en el caso de la práctica serán 2, ejemplo1 y ejemplo2.
El contenido **.conf** de cada ejemplo debe estar dentro de un *server block*, a continuación se muestra el código para el ejemplo1.
~~~
server {
    listen       80;
    server_name  ejemplo1.com;
    root         C:/Program Files/nginx/html/ejemplo1;
    index        index.html;
 }
~~~
Una vez hemos creado los archivos necesarios, vamos a generar *symlinks* en el directorio **conf/sites-enabled** (más adelante se explica de que forma incluimos estos directorios en Windows) que conformarán el enlace fijo y directo a esos archivos **.conf**.
Para ello, hacemos uso del siguiente comando, con el ejemplo1.
~~~
mklink /h conf/sites-enabled/ejemplo1.com.conf conf/sites-available/ejemplo1.com.conf
~~~
Tras esto, es necesario generar *symlinks* a los directorios desde donde se comunicará la información para cada sitio web.
~~~
mklink /j html\ejemplo1 C:/Program Files/nginx/conf/sites-available/ejemplo1.com.conf
~~~
Una vez realizados ambos pasos, nos encontramos con el directorio ejemplo1 anidado dentro del directorio **html** predeterminado en la raíz de la carpeta Nginx.

Llegamos al punto en el que debemos editar el archivo **conf/nginx.conf** incluyendo 2 líneas de código en el *http block*.

![alt_text](https://github.com/carlosblancoj/Practica_Nginx_Stmas/blob/main/img_nginx/nginx4.PNG)
~~~
include conf/upstreams-enabled/*.conf;
include conf/sites-enabled/*.conf;
~~~
Estas líneas son las que incluyen en el servidor los *symlinks* a los archivos **.conf** creados anteriormente.

Por último, antes de reiniciar el servidor Nginx y aplicar así los cambios, es necesario configurar el archivo **hosts** de Windows, el cual encontramos en la siguiente ruta.
~~~
C:\Windows\System32\drivers\etc\hosts
~~~
![alt_text](https://github.com/carlosblancoj/Practica_Nginx_Stmas/blob/main/img_nginx/nginx5.PNG)

Una vez hemos incluido el *servername* especificado anteriormente podemos llevar a cabo el siguiente comando para reiniciar Nginx.
~~~
nginx.exe  -s  reload
~~~
## · Revisión
A continuación, se muestran capturas de el correcto despliegue de los dos ejemplos de sitios web tratados durante el proceso, ejemplo1 y ejemplo2.

![alt_text](https://github.com/carlosblancoj/Practica_Nginx_Stmas/blob/main/img_nginx/nginx2.PNG)

![alt_text](https://github.com/carlosblancoj/Practica_Nginx_Stmas/blob/main/img_nginx/nginx3.PNG)
