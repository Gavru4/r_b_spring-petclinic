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





Lecture 14 NoSQL MongoDB - Gym Database

## 1- Creating a database:

mongosh
use gymDatabase

# ( command result => { switched to db gymDatabase })

## 2- Creating collections:

db.createCollection("clients")
db.createCollection("memberships")
db.createCollection("workouts")
db.createCollection("trainers")

# ( command result => { ok: 1 })

## 3- Defining the schema and populating the collections with data:

db.[collection_name].insertMany([
{ [name]: value, [name]: value, [name]: value, [name]: value, [name]: value },

## example

{ client_id: 3, name: "Taras Honchar", age: 42, email: "taras@example.com" }
{ membership_id: 102, client_id: 2, start_date: "2024-03-01", end_date: "2024-06-01", type: "Quarterly" }
])

# ( command result => {

# acknowledged: true,

# insertedIds: {

# '0': ObjectId('68178904b57b0620f438e9ef'),

# '1': ObjectId('68178904b57b0620f438e9f0')

# }

# })

# 4 - All clients over 30

     db.clients.find({ age: { $gt: 30 } })

# ( command result =>

# [

# {

# \_id: ObjectId('681788f1b57b0620f438e9ec'),

# client_id: 1,

# name: 'Andrii Shevchenko',

# age: 35,

# email: 'andrii@example.com'

# },

# {

# \_id: ObjectId('681788f1b57b0620f438e9ee'),

# client_id: 3,

# name: 'Taras Honchar',

# age: 42,

# email: 'taras@example.com'

# }

# ]

# )

db.workouts.find({ difficulty: "Medium" })

# ( command result =>

# [

# {

# \_id: ObjectId('68178915b57b0620f438e9f2'),

# workout_id: 202,

# description: 'Cardio & Core',

# difficulty: 'Medium'

# }

# ]

# )

db.memberships.find({ client_id: 1 })

# ( command result =>

# [

# {

# \_id: ObjectId('68178904b57b0620f438e9ef'),

# membership_id: 101,

# client_id: 1,

# start_date: '2024-01-01',

# end_date: '2024-12-31',

# type: 'Annual'

# }

# ]

# )
