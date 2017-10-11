
Configuracion del host docker

para este directorio encontraremos el archivo Dockerfile para la creacion de imagen la cual nos permitira crear los contenedores del docker ademas del archivo (authorized_keys) que esto sirve para la autenticacion por llaves en el servicio ssh de cada contenedor.


Configuraciones básicas y creación de dockers de prueba los veremos en los siguientes pasos

-construir un docker personalizado que incluye el servidor openssh

-Creación de la imagen en la que se basará el contenedor.

ejecutaremos el siguiente comando:

$ (sudo) docker build -t {{ nombre imagen }} .

En mi caso el {{ nombre contenedor }} será server_parcial

$ (sudo) docker build -t server_parcial .

para el siguiente paso seria el despliegue

Creacion de contenedores.

crearemos un conjunto de maquinas para el despliegue, se creará un servidor web (apache) y uno de bases de datos (mysql).

Nombre del contenedor web = web_server

$ (sudo) docker run -d -P --name web_server -p 2221:22 -p 80:80 server_parcial

Nombre del contenedor mysql = mysql_server

$ (sudo) docker run -d -P --name mysql_server -p 2222:22 -p 3306:3306 server_parcial

el paso siguiente seria la configuración de los alias

Opción 1: edita el archivo /etc/hosts y adiciona 3 alias a localhost

127.0.0.1 web_server mysql_server

Opción 2: adición automática en el archivo de hosts del sistema

echo "127.0.0.1 server01 server02 server03" | sudo tee -a /etc/hosts

Adicionar las llaves ssh

ssh -o StrictHostKeyChecking=no root@web_server -p 2221 -i ../key.private hostname ssh -o StrictHostKeyChecking=no root@mysql_server -p 2222 -i ../key.private hostname

y por ultimo seria la  confirmación

Realizamos una prueba de conexión a las maquinas que se crearon recientemente, en el paso anterior donde secrearon dos maquinas con los puertos:
-2221
-2222
abirto para la conexion esto es mas que todo verificar si realmente estan conectadas:

ssh root@web_server -p 2221 -i ../key.private ssh root@mysql_server -p 2222 -i ../key.private

Si la conexión se establece, ya está listo para seguir con ansible.
