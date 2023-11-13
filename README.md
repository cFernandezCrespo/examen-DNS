# examen-DNS

### Explica métodos para 'abrir' una consola/shell a un contenedor que se está ejecutando

Existen dos opciones, una de ellas sobre el contenedor hacer click derecho "attach shell", de esta forma se nos abriría la ventana con la terminal del contenedor, la otra usando el comando "exec", que tendría una sintaxis tal que asi.
~~~
docker exec -it contenedor bash
~~~

###  En el contenedor anterior con que opciones tiene que haber sido arrancado para poder interactuar con las entradas y salidas del contenedor


~~~
docker run -it --name miubuntu ubuntu bash
~~~

###  ¿Cómo sería un fichero docker-compose para que dos contenedores se comuniquen entre si en una red solo de ellos?

Pues lo principal para hacer esto seria que ambos estuvieran en la red, tendrias que tener un docker compose que se pareciera al siguiente.
~~~
services:
 asir_contenedor1:
   image: httpd:2.4
   ports:
     - "8000:80"
   volumes:
     - ./paginas:/usr/local/apache2/htdocs 
   container_name: asir_contenedor1
   networks:
     my-network:

 asir_contenedor2:
   image: httpd:2.4
   ports:
     - "9080:80"
   volumes:
     - ./paginas:/usr/local/apache2/htdocs 
   container_name: asir_contenedor2
   networks:
     my-network:

networks:
   my-network:
     ipam:
       config:
         - subnet: 172.28.0.0/16
           gateway: 172.28.0.1
~~~

De esta forma se encontrarian todos los clientes en la misma subred, y como no tienen ips se les asignaria por dhcp.

###  ¿Qué hay que añadir al fichero anterior para que un contenedor tenga la IP fija?

Muy sencillo en el espacio de networks my network tendriamos que añadir una linea que fuera ipv4 con una sintaxis asi:
~~~
 asir_contenedor2:
   image: httpd:2.4
   ports:
     - "9080:80"
   volumes:
     - ./paginas:/usr/local/apache2/htdocs 
   container_name: asir_contenedor2
   networks:
     my-network:
       ipv4_address: 172.28.0.40
~~~

### ¿Que comando de consola puedo usar para saber las ips de los contenedores anteriores? Filtra todo lo que puedas la salida.

~~~
docker inspect --format '{{.Name}} - {{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $(docker ps -aq)
~~~

### ¿Cual es la funcionalidad del apartado "ports" en docker compose?

La sección ports en un archivo docker-compose.yml se utiliza para especificar cómo se deben mapear
los puertos entre el host y el contenedor. Permite definir cómo las aplicaciones dentro de los
contenedores pueden ser accedidas desde el host o desde fuera de la red del contenedor.

Esto es sumamente importante si trabajamos con varios contenedores, pues si no especificamos los puertos todos irian por el mismo

### ¿Para que sirve el registro CNAME? Pon un ejemplo

El registro de "CNAME" significa nombre canónico y su función es hacer que un
dominio sea un alias para otro. El CNAME generalmente se utiliza para asociar nuevos
subdominios con dominios ya existentes de registro A.</br></br>
Supongamos que tienes dos nombres de dominio:</br></br>
www.ejemplo.com: Este es tu dominio principal donde está alojado tu sitio web.</br>
blog.ejemplo.com: Quieres que este subdominio apunte al mismo lugar que www.ejemplo.com.</br></br>
Para lograr esto, puedes usar un registro CNAME en tu configuración DNS:</br></br>
blog.ejemplo.com. IN CNAME www.ejemplo.com.</br>
En este ejemplo:</br>
blog.ejemplo.com: Es el subdominio para el cual estás configurando el CNAME.</br>
CNAME: Indica que este es un registro CNAME.</br>
www.ejemplo.com.: Es el nombre canónico al cual el subdominio apunta.</br>

###  ¿Como puedo hacer para que la configuración de un contenedor DNS no se borre si creo otro contenedor?

Mientras lo hagas con un docker compose lo tienes guardado siempre con la misma configuración,
además de que docker de base guarda los contenedores a no ser que tu de forma activa busques
eliminarlos.

###  Añade una zona tiendadeelectronica.int en tu docker DNS que tenga
www a la IP 172.16.0.1 </br>
owncloud sea un CNAME de www</br>
un registro de texto con el contenido "1234ASDF"</br>
Comprueba que todo funciona con el comando "dig"</br>
Muestra en los logs que el servicio arranca correctamente</br></br></br></br>



###  Realiza el apartado 9 en la máquina virtual con DNS





