# Despliegue de LocalCritic en Docker

Realizaremos un despliegue de la página web que hemos estado desarrollando nuestro grupo (Antoni Xavier Bascuñana, Xavier Borrás y Miguel Marcos). Para ello deberemos dividir las distintas "tecnologías" que se usan en nuestra aplicación web para su correcto funcionamiento (*MySQL*, *phpMyAdmin* y *Tomcat 10*) en distintos contenedores. 

>Todos los archivos de la página web se encuentran en este repositorio de GitHub: https://github.com/abascunana/LocalCritic/tree/docker

# Archivo docker-compose.yml

Iniciaremos con la configuración del archivo `docker-compose.yml`, el cual es necesario para poder desplegar correctamente todos los contenedores en conjunto. En su interior, como veremos a continuación, se incluye la información necesaria para su creación (*nombre de imagen*, *puertos usados*, *nombre de contenedor...*).

## MySQL

![image](https://user-images.githubusercontent.com/91749310/173876220-91b2f4d7-b91a-4d15-9ed5-63b04be9f204.png)

Como podemos ver en la imagen, se ha creado una sección llamada *db* y en el interior de esta, observamos el nombre del contenedor (`container_name: db`), el nombre de la imagen que vamos a usar (`image: mysql`), los puertos usados (`ports: - "3307:3306"`) y la información de la base de datos (nombre de la BBDD, usuario admin y su contraseña, etc), entre otros.

## phpMyAdmin
![enter image description here](https://user-images.githubusercontent.com/91749310/173877772-b747b835-ac67-4538-bfca-453cc6aced15.png)

Al igual que con el contenedor de *MySQL*, es necesaria una configuración en el archivo `docker-compose.yml` para poder usar *phpMyAdmin*. En esta configuración podemos observar el nombre del contenedor, la imagen y los puertos pero además, la primera línea (`depends_on: -db`) indica que esta sección del archivo *.yml* depende del servicio *db*, previamente definido.

## Tomcat 10
![image](https://user-images.githubusercontent.com/91749310/173879298-e6cb0a98-9418-4b52-a4a7-61e5a13b7069.png)

La configuración del servicio de *Tomcat 10* incluye en nombre de la imagen de *Tomcat*, especificando la décima versión, los puertos usados del servicio, el nombre del contenedor y un la creación de un volumen con el *.war* (el cual incluye todos los datos de la aplicación web).

# Despliegue de la aplicación web

A continuación veremos el despliegue de nuestra página web gracias a la configuración del archivo `docker-compose.yml`, el cual permite que los diferentes servicios de la página web se desplieguen de manera conjunta, aunque en diferentes contenedores.

Para ello, nos dirigimos al directorio donde tengamos los datos del proyecto (se encuentra en esta carpeta del repositorio de GitHub: https://github.com/abascunana/LocalCritic/tree/docker/docker) y abrimos la terminal, o en mi caso PowerShell. Una vez tengamos la terminal abierta, ejecutamos el comando `docker-compose up -d`, el cual nos empezará a desplegar nuestro proyecto.

![image](https://user-images.githubusercontent.com/91749310/173882970-16e122e9-8cac-4c83-b525-8d1edf5859c3.png)

Si no tenemos las imágenes de *MySQL*, *phpMyAdmin* o *Tomcat 10* se descargarán solas una vez introducido el comando para el despliegue.

## Comprobación del Despliegue
Si queremos comprobar que el proyecto ha sido desplegado correctamente podemos hacerlo comprobando si realmente se han creado los contenedores; para ello hay dos formas, visualizarlo desde *Docker Desktop* o utilizar el comando `docker ps -a` para visualizarlo en la terminal:

![image](https://user-images.githubusercontent.com/91749310/173883314-c31f4fbc-69f9-4cc4-bfa0-9a5dafe1927a.png)
*Docker Desktop*

![image](https://user-images.githubusercontent.com/91749310/173883535-9be636a0-8b29-45cd-9f8d-05115e7a8e9d.png)
*Terminal*

Una vez hayamos comprobado que los contenedores se han creado correctamente vamos a comprobar si podemos acceder mediante el navegador con el siguiente enlace: `http://localhost:8080/LocalCritic/`.

![image](https://user-images.githubusercontent.com/91749310/173884515-51ffcdef-5de1-457f-89ed-5eaa4593509b.png)

Ahora vamos a comprobar si *phpMyAdmin* funciona correctamente accediendo mediante el siguiente enlace: 
`http://localhost:8082`.

![image](https://user-images.githubusercontent.com/91749310/173885032-7cb24571-a6f2-4d4d-b504-94299cfba36f.png)

![image](https://user-images.githubusercontent.com/91749310/173885263-ffa435f4-6349-4f24-abca-93645da764bc.png)

Ahora comprobaremos si podemos registrarnos e iniciar sesión:
![image](https://user-images.githubusercontent.com/91749310/173885786-d5e7ea5c-a0ba-4bed-8dad-a46adccb343d.png)

![image](https://user-images.githubusercontent.com/91749310/173885882-01048dd8-3d20-47cf-849d-f774e53c510b.png)
*Podemos observar que se ha insertado en la BBDD.*

![image](https://user-images.githubusercontent.com/91749310/173886038-537e191e-71ab-4f0a-894d-1baabb6a3107.png)

![image](https://user-images.githubusercontent.com/91749310/173886110-a3f9057b-51e8-4abd-bd88-efcead22e412.png)

![image](https://user-images.githubusercontent.com/91749310/173886189-cefb2f90-ab4b-4e83-a0c1-faae45ba3e44.png)

*Podemos observar que podemos iniciar sesión correctamente.*

Para acabar de comprobarlo, vamos a probar a insertar una película:

![image](https://user-images.githubusercontent.com/91749310/173886594-f1e7131b-a6fc-485e-9a49-b4733b383a14.png)

![image](https://user-images.githubusercontent.com/91749310/173886699-ab939d3b-2669-4861-a841-433d432099e4.png)

Por el resultado de las pruebas vemos que hemos desplegado el proyecto correctamente.

# Subida a Docker Hub

Para ver el ID de las imágenes que vamos a subir debemos usar el comando `docker images`:

![image](https://user-images.githubusercontent.com/91749310/173887685-9a986b76-15cf-4a98-8594-4b762ef1a0a3.png)

A continuación usamos `docker tag` para otorgarle en nombre correcto:

![image](https://user-images.githubusercontent.com/91749310/173887702-2f9fce9f-0a2f-45b5-bf2c-601b90485d16.png)

Y los subimos con `docker push xborrasmercant/mysql`, `docker push xborrasmercant/phpmyadmin` y `docker push xborrasmercant/tomcat`
![image](https://user-images.githubusercontent.com/91749310/173888794-e771d17e-e83c-4e68-b415-9011ff38b265.png)

![image](https://user-images.githubusercontent.com/91749310/173889476-9dbe3f11-065f-4cfc-8757-809b5f68d0c7.png)

# Conclusiones
Debido a que no lo había casi realizado previamente he tenido que repetirlo varias veces. Te agradezco Máximo que me permitas subirlo incluso después de haber hecho el examen. Te pido disculpas, de verdad.

# Anexos
Perfil de Docker Hub: https://hub.docker.com/u/xborrasmercant
