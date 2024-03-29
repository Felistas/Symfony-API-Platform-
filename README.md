# HOW TO BUILD A CRUD REST API WITH API PLATFORM AND SYMFONY 4.

## Introduction
As per the official documentation, [API platform](https://api-platform.com/) is a “powerful but easy to use full-stack framework dedicated to API driven projects”. API platform helps developers significantly speed up their development process, building complex and high performance, hypermedia-driven APIs.

It ships with Symfony 4, the Doctrine ORM, a dynamic Javascript admin created with React, and React Admin, Varnish Cache server, Helm Chart to help deploy the API in a Kubernetes cluster and a Progressive Web Application skeleton. It also includes a Docker setup for providing Nginx servers to run the API and JavaScript apps. Most inspiring is the ability of API platform to natively generate project documentation with support of OpenAPI!

In this tutorial, I will take you through how to create a simple bucket list API with CRUD operations.


## Prerequisites
1. PHP - Version 7.0 or higher.
2. [Docker](https://docs.docker.com/compose/install/)
3. Postgres

## Getting Started
Follow the instructions below to setup your development environment:

```
$ mkdir demo-app
$ cd demo-app
```
Download the latest compressed `.tar.gz` [distribution](https://github.com/api-platform/api-platform/releases). Then extract it inside of our working directory and run the commands below:
```
$ cd api-platform-2.4.5
$ docker-compose pull
$ docker-compose up -d
```
The `docker-compose pull` command downloads all images specified in the `docker-compose.yml` file. In order to start the containers, run `docker-compose up -d`. The `-d` flag runs the containers in detached mode, meaning they run in the background. In order to view the container logs, you can run this command  `docker-compose logs -f` in a separate terminal.

In your browser, paste the following url `http://localhost` in order to view the application.
You should expect to view the following screens:

##### Api Platform Homepage

![Api platform dashboard](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-1/Screen%20Shot%202019-07-19%20at%2015.14.24.png)

##### Api Platform Dashboard 

Click the API, either `HTTP` or `HTTPS` to view the screen below or navigate to `http://localhost:8080/`

![Api platform dashboard](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-1/Screen%20Shot%202019-07-19%20at%2015.16.20.png)

##### Api Platform Admin

To view the Admin, click the Admin button in the dashboard or navigate to `https://localhost:444/#/greetings`

![Api platform admin dashboard](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-1/Screen%20Shot%202019-07-19%20at%2015.16.28.png)

We are all set to start developing now


## Creating BucketList Models

First, let’s remove the Greeting model since we will not be needing it.  Navigate to `api/src/Entity/Greeting.php` and delete the file. Your API dashboard will now be empty once you reload your browser. 
 
Now, let's create our first model in `api/src/Entity/BucketList.php`. This model will be responsible for storing the items of our bucketlist. Add the following lines of code:

```
<?php

namespace App\Entity;

use Doctrine\ORM\Mapping as ORM;

/**
*
*
* @ORM\Entity
*/
class BucketList
{
   /**
    * @var int The id of a bucketlist.
    *
    * @ORM\Id
    * @ORM\GeneratedValue
    * @ORM\Column(type="integer")
    */
   private $id;

   /**
    * @var string The name of the bucketlist.
    *
    * @ORM\Column(nullable=true)
    */
   public $name;

    /**
    * @var string The description of the bucketlist.
    *
    * @ORM\Column(type="text")
    */
   public $description;

     /**
    * @var \DateTimeInterface When the bucketlist was updated.
    *
    * @ORM\Column(nullable=true)
    */
   public $updatedAt;

   /**
    * @var \DateTimeInterface When the bucketlist was created.
    *
    * @ORM\Column(type="datetime")
    */
   public $createdAt;

   public function getId(): ?int
   {
       return $this->id;
   }
}
```

Next, we need to navigate into the PHP container and run migrations in order to update the database with the newly created models. In order to do that, run `docker-compose exec php bin/console doctrine:schema:update --force`.

Alternatively, you can:
1. List the docker containers in your terminal by typing `docker ps` in order to grab the name of the container.
2. Exec into the PHP container by running `docker exec -it <container-name> /bin/sh`
3. Run `php bin/console doctrine:schema:update --force` to update the database

Log into your preferred Postgres client and confirm if the table has been created. Here is my table and all the fields have been created.

![Postico](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-1/Screen%20Shot%202019-07-19%20at%2016.02.13.png)

After confirming the models have been created, it's now time to create endpoints with CRUD operations. In order to do that, we need to mark the class we created using the `@ApiResource` annotation. Our class will look like:

```
<?php

namespace App\Entity;

use ApiPlatform\Core\Annotation\ApiResource;
use Doctrine\ORM\Mapping as ORM;

/**
*
* @ApiResource
* @ORM\Entity
*/
class BucketList
{
   /**
    * @var int The id of a bucketlist.
    *
    * @ORM\Id
    * @ORM\GeneratedValue
    * @ORM\Column(type="integer")
    */
   private $id;

   /**
    * @var string The name of the bucketlist.
    *
    * @ORM\Column(type="text")
    */
   public $name;

   /**
   * @var string The description of the bucketlist.
   *
   * @ORM\Column(type="text")
   */
   public $description;

   /**
    * @var null When the bucketlist was updated.
    *
    * @ORM\Column(nullable=true)
    */
   public $updatedAt;

   /**
    * @var \DateTimeInterface When the bucketlist was created.
    *
    * @ORM\Column(type="datetime")
    */
   public $createdAt;

   public function getId(): ?int
   {
       return $this->id;
   }
}
```

Go ahead and refresh your browser with the API dashboard and you'll see that we have the CRUD endpoints created. How cool is that?

![Postico](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-1/bucket_list.png)

Let’s try performing some CRUD operations to confirm if everything works as expected. Expand the POST method and try creating a new bucketlist as shown below by clicking on the `Try out` button:
Copy the JSON below under the description text area and click `Execute`.
```
{
  "name": "Sky Diving",
  "description": "Sky dive in Dubai with Keisha",
  "createdAt": "2019-07-17T22:24:59.525Z"
}
```
A new bucketlist should now be created and persisted to the database. The response code returned is 201 (representing that a new resource has been created).

![add bucketlist](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-1/add_a_bucketlist.png)

Let's utilize the GET method to retrieve the bucketlist created. Expand the GET `/bucket_lists` endpoint and click on `Execute` to retrieve the list of bucket lists you have created. 

![all bucketlists](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-1/get_all_bucketlists.png)

In order to get a particular bucketlist using its id, expand the GET `/bucket_lists/{id}` endpoint and click on `try out`. In the resulting dashboard, enter the `id` of the bucketlist of the id you would want to retrieve. In my case, I have retrieved the bucket list with id 4.

![get one bucketlist](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-1/get_one_bucketlist.png)

Suppose a user wants to retrieve a resource that doesn't exist, for example, a bucketlist with 9. The resulting status code is 404 with the error message, `Not Found`. API Platform takes care of such validations out of the box. 

![not found bucketlist](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-1/resource_not_found.png)

Deleting a bucketlist is also straight forward. Expand the DELETE `/bucket_lists/{id}` endpoint and enter the id of the resource you would like to delete. If that resource does not exist, a status code of 404 is returned. 

![delete bucketlist](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-1/delete_bucketlist.png)

The final part would be editing a particular bucketlist. Expand the PUT `/bucket_lists/{id}` endpoint and enter the following JSON in the resulting text area after clicking `Try out` then click `Execute`. 

```
{
  "name": "test bucketlist",
  "description": "testing updating the bucketlist",
  "updatedAt": "2019-07-19T15:35:44.772Z",
  "createdAt": "2019-07-19T14:35:44.772Z"
}
```
![edit bucketlist](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-1/edit_bucketlist.png)

## Conclusion
In this tutorial, we learned how to create a CRUD API with API platform. As you have seen, it's pretty straight forward with most of the validation already done for you. In the next article, I will talk about data validation and serialization, creating custom endpoints and how to add pagination to your API. I would love to hear from you! You can reach me on [Twitter](https://twitter.com/WaceeraN),  [LinkedIn](https://www.linkedin.com/in/felistas-ngumi-b6063192/LinkedIn) or drop me an [email](felistaswaceera@gmail.com). Happy hacking!

