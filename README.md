# HOW TO BUILD A CRUD REST API WITH API PLATFORM AND SYMFONY 4.
A simple application to illustrate how to create CRUD API's with Symfony 4 and API Platform

## Introduction
As per the official documentation, Api platform is a “powerful but easy to use full stack framework dedicated to API driven projects”. Api platform helps developers significantly speed up their development process, building complex and high performance hypermedia driven APIs. 
It ships with Symfony 4, Doctrine ORM, a dynamic Javascript admin created with React and React Admin, a progressive web application skeleton and a docker setup providing Nginx servers to run the API and JavaScript apps, Varnish Cache server and Helm Chart to deploy the API in a Kubernetes cluster.Oh and did i mention that API platform also supports OpenAPI natively for your project documentation? So why not give it a try!
In this tutorial, I will take you through how to create a simple bucket list API with CRUD 

## Prerequisites
1. PHP - Version 7.0 or higher.
2. Docker
3. Postgres

## Getting Started
 Follow the instructions below to setup your development environment:

```
$ mkdir demo-app
$ cd demo-app
```
Download the compressed .tar.gz distribution then extract it inside our working directory then run the commands below:

```
$ docker-compose pull
$ docker-compose up -d
```
The `docker-compose pull` downloads the  images specified in the `docker-compose.yml` file. In order to start the containers, we run `docker-compose up -d`. The `-d` flag runs the containers in detach mode meaning they run in the background. In order to view the container logs, you can run this command  `docker-compose logs -f`. In your browser, paste the following urls `http://localhost` in order to view the application. 
You should expect to view the following screens:



 ## Design and Create Entities
 - Create ToDo entities that will create the relevant tables for us in the database.
 - Inspect routes created
 
 ## Database Setup
 - Initialize the database.
 - Load fake data into the db.
 
 ## Add CRUD Logic
  -  Add logic to support the different CRUD operations in the app including data validation. i.e
     - Add a ToDo list
     - Get a ToDo list
     - Edit a ToDo list
     - Delete a ToDo list
  ## Test The Application
  - Run different tests on the API platform UI from the browser.
  
 ## Summary
 In this tutorial we have learnt how to create a simple CRUD API with symfony 4 and API platform.



