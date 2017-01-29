# Resources

## {{ book.must }} Avoid Actions — Think About Resources

REST is all about your resources, so consider the domain entities that take part in web service interaction, and aim to model your API around these using the standard HTTP methods as operation indicators. For instance, if an application has to lock articles explicitly so that only one user may edit them, create an article lock with PUT or POST instead of using a lock action.

Request:

    PUT /article-locks/{article-id}

The added benefit is that you already have a service for browsing and filtering article locks.

## {{ book.should }} Define *useful* resources

As a rule of thumb resources should be defined to cover 90% of all its client's use cases. A *useful* resource should
contain as much information as necessary, but as little as possible. A great way to support the last 10% is to allow
clients to specify their needs for more/less information by supporting filtering and
[embedding](../hyper-media/Hypermedia.md#should-allow-embedding-of-complex-subresources).

## {{ book.must }} Keep URLs Verb-Free

The API describes resources, so the only place where actions should appear is in the HTTP methods.
In URLs, use only nouns.

## {{ book.must }} Use Domain-Specific Resource Names

API resources represent elements of the application’s domain model. Using domain-specific nomenclature for resource names helps developers to understand the functionality and basic semantics of your resources. It also reduces the need for further documentation outside the API definition. For example, “sales-order-items” is superior to “order-items” in that it clearly indicates which business object it represents. Along these lines, “items” is too general.

## {{ book.must }} Identify resources and Sub-Resources via Path Segments

Basic URL structure:

    /{resources}/[resource-id]/{sub-resources}/[sub-resource-id]

Examples:

    /carts/1681e6b88ec1/items
    /carts/1681e6b88ec1/items/1


## {{ book.should }} Only Use UUIDs If Necessary

Generating IDs can be a scaling problem in high frequency and near real time use cases. 
UUIDs solve this problem, as they can be generated without collisions in a distributed, 
non-coordinated way and without additional server roundtrips.

However, they also come with some disadvantages:

* pure technical key without meaning; not ready for naming or name scope conventions 
that might be helpful for pragmatic reasons, e.g. we learned to use names for 
product attributes, instead of UUIDs 
* less usable, because...
   * cannot be memorized and easily communicated by humans
   * harder to use in debugging and logging analysis
   * less convenient for consumer facing usage
* quite long: readable representation requires 36 characters and comes with 
higher memory and bandwidth consumption 
* not ordered along their creation history and no indication of used id volume
* may be in conflict with additional backward compatibility support of legacy ids

UUIDs should be avoided were not needed for large scale id generation. 
Instead, for instance, server side support with id generation can be preferred (POST on id resource, 
followed by idempotent PUT on entity resource). 
Usage of UUIDs is especially discouraged as primary keys of master and configuration data, 
like brand-ids or attribute-ids which have low id volume but widespread steering functionality. 

In any case, we should always use string rather than number type for identifiers. 
This gives us more flexibility to evolve the identifier naming scheme. 
Accordingly, if used as identifiers, UUIDs should not be qualified using a format property.

Hint: Usually, random UUID is used - see UUID version 4 in [RFC 4122](https://tools.ietf.org/html/rfc4122). 
Though UUID version 1 also contains leading timestamps it is not reflected by its lexicographic sorting.
This deficit is addressed by [ULID](https://github.com/alizain/ulid) (Universally Unique Lexicographically Sortable Identifier). 
You may favour ULID instead of UUID, for instance, for pagination use cases ordered along creation time. 


## {{ book.could }} Consider Using (Non-) Nested URLs

If a sub-resource is only accessible via its parent resource and may not exists without parent resource, consider using a nested URL structure, for instance:

    /carts/1681e6b88ec1/cart-items/1

However, if the resource can be accessed directly via its unique id, then the API should expose it as a top-level resource. For example, customer is a collection for sales orders; however, sales orders have globally unique id and some services may choose to access the orders directly, for instance:

    /customers/1681e6b88ec1
    /sales-orders/5273gh3k525a

## {{ book.should }} Limit number of Resources

To keep maintenance and service evolution manageable, we should follow "functional segmentation" and "separation of concern" design principles and do not mix different business functionalities in same API definition. In this sense the number of resources exposed via API should be limited - our experience is that a typical range of resources for a well-designed API is between 4 and 8. There may be exceptions with more complex business domains that require more resources, but you should first check if you can split them into separate subdomains with distinct APIs.

## {{ book.should }} Limit number of Sub-Resource Levels

There are main resources (with root url paths) and sub-resources (or “nested” resources with non-root urls paths). Use sub-resources if their life cycle is (loosely) coupled to the main resource, i.e. the main resource works as collection resource of the subresource entities. You should use <= 3 sub-resource (nesting) levels -- more levels increase API complexity and url path length. (Remember, some popular web browsers do not support URLs of more than 2000 characters)
