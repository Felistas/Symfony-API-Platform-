# Create a CRUD API with Symfony and API-Platform (Part II)

Now That we have created a simple 
[CRUD API](https://www.twilio.com/blog/build-crud-restful-api-php-api-platform-symfony-4),let's learn how we can retrieve the data we want using query parameters, customize the pagination of the response then create custom controllers and endpoints.

## Adding Custom Operations
 As we have learnt, API platform automatically creates CRUD operations from the resources created if no operation is specified. It however, also allows creation of custom operations on specific routes. There are two types of operations, collection and items operations. Collection operations are operations that act on a group of resources ,e.g retrieving all bucket lists while item operations are operations that act on a single resource, e.g retrieving one bucket list. For collection operations, the GET and POST routes are defined with the GET operation being enabled by default. In item operations, the GET, PUT and DELETE routes are defined with the GET route enabled by default. In order to specify the default collection and items operations, add the following annotation before the class definition and refresh your browser.
 
 N/B: this can also be done using XML or YAML.
 
 ```
<?php

namespace App\Entity;

use ApiPlatform\Core\Annotation\ApiResource;
use Doctrine\ORM\Mapping as ORM;

/**
 *
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

![Api platform dashboard](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-2/both_collection_and_item_operations.png)


This will make the resource ReadOnly. If you want to add the POST endpoint on the collectionOperations, customize the `collectionOperations` to  `collectionOperations={"get", "post"}`. The same can also be applied to the itemOperations to enable the PUT and DELETE endpoints. 

Note: Both item and collection operations work independently. This means that if the collection operations are configured and the item operations are not, the GET, PUT and DELETE operations will be automatically configured and vice versa. For instance, add the following piece of code:

 ```
<?php

namespace App\Entity;

use ApiPlatform\Core\Annotation\ApiResource;
use Doctrine\ORM\Mapping as ORM;

/**
 * 
 * @ApiResource(
 *     collectionOperations={"get"}
 * )
 */
class BucketList
{
  //...
}
```

You should expect to see all the item operations enabled by default as shown in the image below:

![Api platform dashboard](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-2/collection_get_operation_annotation.png)

## Adding Custom URLs

API platform also enables adding custom URLs to your routes and overriding the default ones. In order to do that, add the following piece of code.

 ```
<?php

namespace App\Entity;

use ApiPlatform\Core\Annotation\ApiResource;
use Doctrine\ORM\Mapping as ORM;

/**
 * 
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
After refreshing your browser, the resulting screen should be similar to:

![Api platform dashboard](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-2/custom_endpoints.png)

## Adding a Custom Controller

## Pagination
API platform enables pagination by default with each collection containing 30 items per page. In my local application, I have created more than thirty bucket list items and when I navigate to the `GET /bucket_lists` endpoint, only thirty items are displayed in the first page. The extra items will be displayed on the next page. 
See the image below:

![Api platform dashboard](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-2/first_thirty_items.png)
As you can see in the image above, API platform displays the total items available in your database and other page routes that have been created automatically. In order to navigate to the next page, enter the page number under `The collection page number` then click `Execute`.

![Api platform dashboard](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-2/second_route.png)

There are instances where one would choose to disable pagination when they have few items to display. This can be done under the file, `api/config/packages/api_platform.yaml` by adding the following few lines of code. 
```
api_platform:
    collection:
        pagination:
            enabled: false
```

The input box for specifying the collection page number will be disabled and the resulting screen will appear as below when the `Execute` button is clicked. 

![Api platform dashboard](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-2/no_pagination.png)

Pagination can also be disabled for a particular resource by adding the following annotation in the particular resource. 

```
<?php

namespace App\Entity;

use ApiPlatform\Core\Annotation\ApiResource;
use Doctrine\ORM\Mapping as ORM;

/**
 * @ApiResource(attributes={"pagination_enabled"=false})
 */
class BucketList
{
  //
}
```

Changing the number of items per page can also be made globally or for a specific resource. In order to change the number of items per page for a particular resource, add the following annotation in your resource and refresh your browser. 

```
<?php

namespace App\Entity;

use ApiPlatform\Core\Annotation\ApiResource;
use Doctrine\ORM\Mapping as ORM;

/**
*
* @ApiResource(attributes={"pagination_items_per_page"=10})
* @ORM\Entity
*/
class BucketList
{ 
  //
}
```

For global configuration, add the following piece of code in `api/config/packages/api_platform.yaml`

```
api_platform:
    collection:
        pagination:
            items_per_page: 10
 ```
 The resulting pagination results will be as shown below (notice the created routes):
 ![Api platform dashboard](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-2/ten_items_per_page.png)
 
 ## Implement Search Queries in the GET endpoints
 API platform already provides a way of applying filters to collections so you don't have to write a custom endpoint for filters unless you really have to.  


 ## Summary
 In this tutorial we have learnt how to customize the CRUD API we created with symfony 4 and API platform to suit our business needs.




