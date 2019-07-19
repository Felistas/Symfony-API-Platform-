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
$ cd api-platform-2.4.5
$ docker-compose pull
$ docker-compose up -d
```
The `docker-compose pull` downloads the all images specified in the `docker-compose.yml` file. In order to start the containers, we run `docker-compose up -d`. The `-d` flag runs the containers in detach mode meaning they run in the background. In order to view the container logs, you can run this command  `docker-compose logs -f` on a separate terminal. In your browser, paste the following url `http://localhost` in order to view the application.
You should expect to view the following screens:

Api Platform Homepage

![Api platform dashboard](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-1/Screen%20Shot%202019-07-19%20at%2015.14.24.png)

Api Platform Dashboard 
Click the API either `HTTP` or `HTTPS` to view the screen below or navigate to `http://localhost:8080/`

![Api platform dashboard](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-1/Screen%20Shot%202019-07-19%20at%2015.16.20.png)

Api Platform Admin
To view the Admin, Click the Admin button in the dashboard or navigate to `https://localhost:444/#/greetings`

![Api platform admin dashboard](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-1/Screen%20Shot%202019-07-19%20at%2015.16.28.png)

