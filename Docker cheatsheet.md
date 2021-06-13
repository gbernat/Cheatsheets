# Docker cheatsheet

- Create your docker image. From within your development folder (cd fn_xxx), run:
# docker build . -t resilient/<fn_example>:1.0.0
(Compruebo que se creo con #docker images)


- Runnig a container:
```
# docker run -v /path/to/your/local/app.config:/etc/rescircuits/app.config resilient/fn_example:1.0.0  

Run a container of amazonlinux interactive, with current path as persistent volume (pull amazonlinux image if not already exists):  
$ docker run -it --name alinux -v $(pwd):/opt/lambda/ amazonlinux /bin/bash
```




>>> DOCKER <<<

Ver dockers running:
# docker ps 

Ver todos los dockers:
# docker ps -a

Entrar por ssh a un docker running:
# docker exec -it <nombre> bash

Informacion detallada del docker o imagen:
# docker inspect <nombre>

Iniciar/detener un docker:
# docker start/stop <nombre>

Ver si una imagen esta configurada para que inicie automaticamente (always) al iniciar el servicio de Docker:
# docker inspect <nombre> | grep -A2 RestartPolicy
            "RestartPolicy": {
                "Name": "always",

List all containers (only IDs)
# docker ps -aq 

Stop all running containers
# docker stop $(docker ps -aq)

Remove all containers
# docker rm $(docker ps -aq)

Remove all images
# docker rmi $(docker images -q)

Remove some images
# docker image rm [image_id1] [image_id2]

Sintaxis equivalente:
docker image ls <=> docker images



OBSERVACIONES:
Los datos internos que se modifican dentro de un contenedor (no necesariamente en un volumen) persisten ante stop/start de la imagen y 
stop/start del servicio de Docker. No resisten, obvio luego de rm de la imagen.

En linux (lab redhat y ansessrweb01) pude hacer ping, telnet a la ip del container desde host y existe creada una interfaz docker0. En macos, no (ni siquiera aparece la ip en ifconfig) 

En /etc/containers/registries.conf esta por prioridades, de que registros docker/podman puede tomar imagenes (lo mas confiable es registry.access.redhat.com, pero tambien puede ser quay.io)
  en el caso de entorno seguro deberia ser un registro privado local. <--- No vi este dir en linux, ni mac. VER ESTO PARA OPENSHIFT
  En docker linux, "parece" estar en  /var/lib/docker/image/devicemapper/repositories.json
