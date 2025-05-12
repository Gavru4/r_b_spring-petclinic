# r_b_spring-petclinic

Lection 17 Docker compose

## 1. At first i created a folder multi-container-app where after i created my docker-compose.yml and index.html file for homework 17 "Docker-compose"

## 2.Then i started all services in the background with this command:

## docker-compose up -d

## and then to view the status of running containers u used this command

## docker-compose ps

## Also, as a confirmation that everything is working fine, I checked port 8080, where I saw the nginx page with the "Hello from Docker" title

## 3. I used "docker network ls" and then "docker volume ls" to view the created networks and volumes, and also use "docker exec container_id" to connect inside current container

## 4. When I needed to scale my web server ( using command => docker-compose up -d --scale web=3 ), I got an error that my newly created container (during the scaling process) was trying to listen to the same port 8080 on the host ( that is not possible because only one container can use port 8080 ).

## To fix this error, firstly, I removed the “port” from the web service, and added a separate load balancer ( Nginx as a proxy that communicates with all web ) that will listen to port 8080 and direct traffic to the scalable containers
