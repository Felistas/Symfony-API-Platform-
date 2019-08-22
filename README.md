# HOW TO BUILD A CRUD REST API WITH API PLATFORM AND SYMFONY 4 (Part II).

Now that we have created a simple 
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

API Platform also enables adding custom URLs to your routes and overriding the default ones. In order to do that, add the following piece of code.

```php
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
API platform uses Symfony's routing system to register custom operations and controllers. Let's get straight into it!
Add the following code snippet in  `api/src/Controller/CustomController.php`

```
<?php

namespace App\Controller;

use Symfony\Component\HttpFoundation\JsonResponse;
use Symfony\Component\Routing\Annotation\Route;

class CustomController
{
    /**
     * @Route(
     *     name="custom_controller",
     *     path="/customController",
     *     methods={"GET"},
     *     defaults={"_api_item_operation_name"="custom_controller_example"}
     * )
     */
    public function __invoke()
    {
        return new JsonResponse('Hi there!');
    }
}
```

Next, we need to create an Entity that will use the custom controller. 

```
<?php

namespace App\Entity;

use ApiPlatform\Core\Annotation\ApiResource;
use Doctrine\ORM\Mapping as ORM;
use App\Controller\CustomController;


/**
 * @ApiResource(
 *     itemOperations={
 *         "custom_controller_example"={
 *             "route_name"="custom_controller",
 *             "swagger_context"={
 *                  "parameters"={}
 *              }
 *         }
 *     },
 *    collectionOperations ={}
 * )
 * @ORM\Entity
 */
class BucketListItems
{
    /**
     * @var int The id of a bucketlist.
     *
     * @ORM\Id
     * @ORM\GeneratedValue
     * @ORM\Column(type="integer")
     */
    private $id;
}
```

Once you refresh your browser, you should see the resulting screen below. 

![Api platform dashboard](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-2/CustomController.png)

Hit the `Try out` button and click `Execute`. You should expect to see the screen below.

![Api platform dashboard](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-2/executeCustomController.png)

You can more logic to the custom controller to suit your business needs.

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
 API platform already provides a way of applying filters to collections so you don't have to write a custom endpoint for filters unless you really have to. For example if we want to filter by name in our API, add the following annotation and refresh your browser.

```
<?php

namespace App\Entity;

use ApiPlatform\Core\Annotation\ApiResource;
use Doctrine\ORM\Mapping as ORM;
use Symfony\Component\Validator\Constraints as Assert;
use ApiPlatform\Core\Annotation\ApiFilter;
use ApiPlatform\Core\Bridge\Doctrine\Orm\Filter\SearchFilter;


/**
*
* @ApiResource()
* @ApiFilter(SearchFilter::class, properties={"name": "exact"})
* @ORM\Entity
*/
class BucketList
{
  //
}
```

![Api platform dashboard](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-2/search_results.png)

As you can see above, all bucket lists with the name `Sky Diving` are returned in the response. You may wish to combine filters and that can be done by adding the property name of the second filter. i.e `@ApiFilter(SearchFilter::class, properties={"name": "exact", "description": "exact"})`. The resulting filter results will be as follows:

![Api platform dashboard](https://github.com/Felistas/Symfony-API-Platform-/blob/Part-2/combined_filters.png)

Note the URL: ` http://localhost:8081/bucket_lists?name=Sky%20Diving&description=Sky%20dive%20in%20Dubai%20with%20Keisha`
The query parameters are automatically appended. The SearchFilter class supports a number of filter strategies such as:
- `partial` searches for fields containing the text you will provide on the API client. It uses the MYSQL `LIKE %searchText%` operator. 
- `start` searches for fields that start with the text you will provide on the API client.It uses the MYSQL `LIKE searchText%` operator. 
- `end` searches for fields that end with the text you will provide on the API client.It uses the MYSQL `LIKE %searchText` operator. 
- `word_start` searched for fields containing words that start with the text provided. It uses the MYSQL `LIKE text% OR LIKE % text%` operator. 

 ## Summary
 In this tutorial we have learnt how to add custom operations and URLs on the resource we created,pagination and adding filters to our search queries. If you would like to dive deep into more API platform features you can have a look at their [documentation](https://api-platform.com/docs/core/) page. As always,I would love to hear from you! You can reach me on [Twitter](https://twitter.com/WaceeraN) or [LinkedIn](https://www.linkedin.com/in/felistas-ngumi-b6063192/LinkedIn) . Happy hacking!




