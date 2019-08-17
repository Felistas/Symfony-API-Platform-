# Create a CRUD API with Symfony and API-Platform (Part 2)
A continuation on creating a CRUD API using API platform and Symfony 4
## Outline Takeaways
1. Introduction
2. Adding a Custom Controller
3. Pagination
4. Implement Search Queries in the GET endpoints
5. Serialize and deserialize data in your application
6. Summary.

## Introduction
Now that we have created a simple CRUD API, let's now learn how we can retrieve the data we want using query parameters, serialize and deserialize the results and paginate them for better display. Other than that, we will also add a custom controller with business logic that will suit our application.


## Adding a Custom Controller
 As we have learnt, API platform automatically creates CRUD operations from the resources created if no operation is specified. It however, also allows creation of custom operations on specific routes. There are two types operations, collection and items operations. Collection operations are operations that act on a group of resources ,e.g retrieving all bucketlists while item operations are operations that act on a single resource, e.g retrieving one bucketlist. For collection operations, the GET and POST routes are implemented with the GET operation being enabled by default. In item operations, the GET, PUT and DELETE routes are defined with the GET route enabled by default. In order to specify the default collection and items operations, add the following annotation before the class defination and refresh your browser. N/B: this can also be done using XML or YAML.
 
 ```
<?php

namespace App\Entity;

use ApiPlatform\Core\Annotation\ApiResource;
use Doctrine\ORM\Mapping as ORM;
use Symfony\Component\Validator\Constraints as Assert;

/**
 * ...
 * @ApiResource(
 *     collectionOperations={"get"},
 *     itemOperations={"get"}
 * )
 */
class BucketList
{
  //...
}
```
Your routes should now appear as shown below:
//upload the picture here

This will make the resource ReadOnly. If you want to add the POST endpoint on the collectionOperations, customize the `collectionOperations` to  `collectionOperations={"get", "post"}`. The same can also be applied to the itemOperations to enable the PUT and DELETE endpoints. 

Note: Both item and collection operations work independently. This means that if the collection operations are configured and the item operations are not, the GET, PUT and DELETE operations will be automatically configured and vice versa. For instance, add the following piece of code:

 ```
<?php

namespace App\Entity;

use ApiPlatform\Core\Annotation\ApiResource;
use Doctrine\ORM\Mapping as ORM;
use Symfony\Component\Validator\Constraints as Assert;

/**
 * ...
 * @ApiResource(
 *     collectionOperations={"get"}
 * )
 */
class BucketList
{
  //...
}
```

You should expect to see the all the item operations enabled by default as shown in the image below:

//insert the image here

API platform also enables adding custom URLs to your routes and overriding the default ones. In order to do that, add the following piece of code.

 ```
<?php

namespace App\Entity;

use ApiPlatform\Core\Annotation\ApiResource;
use Doctrine\ORM\Mapping as ORM;
use Symfony\Component\Validator\Constraints as Assert;

/**
 * ...
 * @ApiResource(
 *     collectionOperations={"get" ={"path"="/customCollectionBucketListEndPoint"}},
 *     itemOperations={"get" ={"path"="/customItemBucketListEndPoint"}}
 * )
 */
class BucketList
{
  //...
}
```
After refreshing your browser, the resulting screen should be simillar to:

//insert image



 
 ## Pagination
 - Implement pagination when retrieving huge sets of data.
 
 ## Implement Search Queries in the GET endpoints
 - Add query parameters to our GET endpoints for custom search instances.
 
 ## Serialize and deserialize data in your application
 - Transform PHP entities to hypermedia API responses.

 ## Summary
 In this tutorial we have learnt how to customize the CRUD API we created with symfony 4 and API platform to suite our business needs.



