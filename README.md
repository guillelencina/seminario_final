
# Seminario de Tópicos Avanzados en Datos Complejos
### Especialización en ciencia de datos ITBA  
  
  
  
Docentes:   
Pedro Ferrari | pedro@muttdata.ai  
Juan Martín Pampliega | jpamplie@itba.edu.ar  

Estudiantes:      
Guillermo Lencina | glencina@itba.edu.ar    
Nicolas Arosteguy | narosteguy@itba.edu.ar    
Alexander Chavez | achavezmontano@itba.edu.ar   

## Resumen

Se obtienen los datos a través de dos APIs; una para usuarios y otra para playlist. Para ello se usa python. El resultado de la primera API es almacenado en un .csv.  
  
Dicho archivo es transformado en una BD para almacenar las tablas de usuarios y playlist.

Mediante otro script, también en python, obtiene los datos procesados de las playlist, son embebidos en un _string_ y activa la segunda API que consulta sobre las playlist. Los datos obtenidos son almacenados y transformados con SQL para finalmente vincular a usuarios con artistas.  
  
Una vez vinculados, y a través del procesamiento de datos, se encuentran las relaciones entre usuarios y artistas que se consolidan en una tabla, todo esto con algoritmos de Machine Learning, específicamente de clustering, hasta lograr visualizar las relaciones posibles.  

El orquestamiento de todas éstas tareas son a través de Airflow.  
  
  
## Objetivo    
  
Generar un sistema de recomendaciones basado en relaciones entre usuarios de la plataforma Spotify, ya sea por gustos musicales y/o artistas que se vinculen entre las playlist.  


## Contenido

* [Infraestructura](#Infraestructura)
* [Instalación y puesta en marcha del ambiente](#Pasos-para-instalar)
* [Jupyter Notebook](jupyter/notebook/README.md)
* [Airflow (DAGs configurados en _users_spotify.py_)](dags/README.md)
  
    
## Infraestructura  
  
Nuestro trabajo simula una instalación de producción con múltiples containers en Docker.
_docker-compose.yaml_ contiene las definiciones y configuraciones para los siguientes servicios:

* Interfaz gráfica de Jupyter obtenida de la imagen arjones/pyspark:2.4.5. 

    Una vez los containers estén en Running, podés ingresar desde acá -> [Jupyter](http://localhost:8888)

* Interfaz gráfica de Airflow obtenida de la imagen -apache/airflow:2.4.1. 

    Una vez los containers estén en Running, podés ingresar desde acá -> [Airflow](http://localhost:8080)

* Motor de base de datos postgres obtenida de la imagen postgres:13. 

## Pasos para instalar

1. Clonar repo: git clone https://github.com/guillelencina/seminario_final.git

![](./images/git_clone.jpg)


2. Abrir _Docker Desktop_ para visualizar desde la interfaz.

![](./images/docker_desktop_ini.jpg)


3. Desde VSCode, abrir la carpeta _seminario_final-master_.

![](./images/folder_seminario_final.jpg)


4. Ejecutar una consola de Ubuntu.

![](./images/ubuntu_console.jpg)


5. Ingresar a la carpeta: cd seminario_final-master

6. Ejecutar el comando: docker-compose -f docker-compose.yaml up -d

* El docker-compose es un archivo yaml/yml para crear todos los containers necesarios y a la vez.
* Una vez ejecutado, deben aparecer todos los containers OK como indica la imagen.

![](./images/containers_done.jpg)

* También los podés chequear en la interfaz gráfica de Docker.

![](./images/containers_running.jpg)


Sitios de interés: 

    * https://www.youtube.com/c/PeladoNerd  
    * https://www.youtube.com/c/HolaMundoDev  
    * https://www.youtube.com/c/NetworkChuck
    * Postgres + PGAdmin : https://www.youtube.com/watch?v=uKlRp6CqpDg  


## Acerca de

Nicolás Arostegui | [LinkedIn](https://www.linkedin.com/in/nicol%C3%A1s-arosteguy-a564a97a/) 

Guillermo Lencina | [LinkedIn](https://www.linkedin.com/in/guillermolencina/) 

Alexander Chavez | [LinkedIn](https://www.linkedin.com/in/alexchavez1980/) 

ITBA &copy; 2021/2022 
