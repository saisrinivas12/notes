

refer :  https://docs.docker.com/ for the official documentation 



Docker :

it enforces the sense of packaging , shipping and running the software applications.
it packages all the dependencies along with your main project in a container.
now you will send this container to any other server(different environment) ..now your project will run without any issues.


problem which docker resolves:

  lets say you are developing a java project which uses jdk11 in your personal laptop. now you moved your project to office laptop which has jdk8 installed now if you run your application, your application will not run as it has mismatched dependcies.

  To address this,

  we will create a container , In that we will install jdk11 and develop your project inside that container.
  now if you want to run your project, you will send the whole container to other environment . your project will run perfectly as you are running it from the same container.



Important :

Dockerfile:
 a text file which contains all the commands which user can call from command line to assemble an image.

Dockerimage:
   template to create a container

Docker Container:
Running an instance of docker image results in an container. this contains overall packages of your application 



  Important Docker commands:
  1. docker -v / docker --version :
      gives docker version
  2. docker pull imageName
        specifies the image to pull from docker hub and store it in the local
       eg : docker pull openjdk:18
         pulls the openjdk version 18 
3. docker images:
     to get the list of images installed in your system
4 docker search imageName:
     to search the given imageName

5. docker ps-a :
    to get the list of containers
6. docker run --name nameofyourcontainer -d imagename
     -d meaning in the detached mode
   -it : in an interactive mode to make status up
   Example:
   docker run --name nameofyourcontainer -it -d imagename
7 docker exec -it containerId imageName(exact)
   this will run your python in your container
8. docker inspect containerid
      gives all the information related to container

   Example for nginx
   git pull nginx

   then


   git run --name nginxServer -d -p 8080:80 nginx

   this will run create an nginxServer container

to stop the containers :
docker stop containername/containerid

if you see the containers using 
docker ps it will not show your containers

but 

docker ps -a will still show your container statuses


in order to remove from this

use 
docker rm containerid

to remove images 

docker rmi imageName 



in order to restart container 

use 

docker restart containerName/containerId
![image](https://github.com/saisrinivas12/notes/assets/59176223/baaf0e1b-05e3-44e3-9b1a-ce89f361a7a9)


   build your own umage :

   docker build -t nameofyourimage path 




Dockerfile: example
FROM openjdk // you need to specify the base image 
WORKDIR /usr/src/myapp // you need to specify the working directory 
COPY . /usr/src/myapp    // copy all your assets to the working directory 
RUN javac test.java // when you build for getting an image this command will be run 
CMD ["java","test"] // when you run your image i.e creating a container  this command will be run


docker build -t javaproject .   -> this will build your project and gives a image name as javaproject 

next run your image with creating container

docker run --name javaProject javaproject 

this will create a container named javaProject and your application will be running inside your container


DockerFile for springboot project
 first buildyour springboot project and get the jar lets say jar name as "boot.jar"
 FROM openjdk
 WORKDIR /usr/src/boot
 COPY . /usr/src/boot
 CMD ["java","-jar","boot.jar"]  // same like java -jar boot.jar
 EXPOSE PORT 9595  // this is where your boot project is running 


 docker build -t bootproject .   // this will build your project and get the image with name as bootproject

 then run your image inside the container

 docker run --name bootProject -it -d -p 9595:9595 bootproject 
  -p : is the port for which the docker container will refer to 
        so 9595 will refer to the actual project which is running on 9595

  you can check logs if project is running or not 


  to give the existing image a new name 

  docker tag existingimagename newimagename

  docker logs containerName(bootProject)


  docker volumes:

  docker container uses its own file system to create, update and delete the files . what if we remove this container , then docker completely isolates this container and all previous changes are lost.

  with docker volumes , you can get access to specific file system which is running on your host machine(your computer) so that changes will not be lost 

  Example:
  By default, the todo app stores its data in a SQLite database at /etc/todos/todo.db in the container's filesystem.  It's simply a relational database that stores all the data in a single file. While this isn't the best for large-scale applications, it works for small demos. You'll learn how to switch this to a different database engine later.

  With the database being a single file, if you can persist that file on the host and make it available to the next container, it should be able to pick up where the last one left off. By creating a volume and attaching (often called "mounting") it to the directory where you stored the data, you can persist the data. As your container writes to the todo.db file, it will persist the data to the host in the volume.

 As mentioned, you're going to use a volume mount. Think of a volume mount as an opaque bucket of data. Docker fully manages the volume, including the storage location on disk. You only need to remember the name of the volume.


 Creating a volume  using docker 

 docker volume create volumeName(todo-db)

 starting a container with mounting 
 docker run -dp 127.0.0.1:3000:3000 --mount type=volume,src=todo-db,target=/etc/todos getting-started

 you can check where docker is storing the data when using volume

 docker volume inspect 
 mountpoint will be the location where docker is storing the data



 Bind Mounts :

 this is another type of mount which shares the host's file system to the container. we will mount the container with hosts file system so that if changes occur in the hosts file system will immediately trigger the processes in the container.eg( nodemon)

                    Named volumes	Bind mounts
Host location	Docker chooses	You decide
Populates new volume with container contents	Yes	No
Supports Volume Drivers	 yes	No




Multicontainer apps:


Container networking
Remember that containers, by default, run in isolation and don't know anything about other processes or containers on the same machine. So, how do you allow one container to talk to another? The answer is networking. If you place the two containers on the same network, they can talk to each other.

Creating a network 

docker network create networkName


run a container which contains to the same network that has been created 
docker run -d \
    --network todo-app --network-alias mysql \
    -v todo-mysql-data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=secret \
    -e MYSQL_DATABASE=todos \
    mysql:8.0


docker compose :
https://docs.docker.com/get-started/08_using_compose/


 
