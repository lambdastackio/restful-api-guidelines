# General Guidelines

The titles are marked with the corresponding labels: {{ book.must }}, {{ book.should }}, {{ book.could }}.

## {{ book.must }} Follow API First Principle

As mentioned in the introduction, API First is one of our architecture principles, as per our Architecture Rules of Play. 
In a nutshell API First has two aspects:

- define APIs outside the code first using a standard specification language
- get early review feedback from peers and client developers

By defining APIs outside the code, we want to facilitate early review feedback and also a development 
discipline that focus service interface design on...

- profound understanding of the domain and required functionality
- generalized business entities / resources, i.e. avoidance of use case specific APIs
- clear separation of WHAT vs. HOW concerns, i.e. abstraction from implementation aspects — APIs should be stable even if we replace complete service implementation including its underlying technology stack

Moreover, API definitions with standardized specification format also facilitate...

- single source of truth for the API specification; 
  it is a crucial part of a contract between service provider and client users
- infrastructure tooling for API discovery, API GUIs, API documents, automated quality checks

An element of API First are also this API Guidelines and a [lightweight API review process \[internal link\]](https://github.bus.zalan.do/ApiGuild/ApiReviewProcedure) as to get early review feedback from peers and client developers. 
Peer review is important for us to get high quality APIs, to enable architectural and design alignment 
and to supported development of client applications decoupled from service provider engineering life cycle. 

It is important to learn, that API First is **not in conflict with the agile development principles** that we love. 
Service applications should evolve incrementally — and so its APIs. Of course, our API specification will 
and should evolve iteratively in different cycles, each starting with draft status and early team 
and peer review feedback. 

API may change and profit from implementation concerns and automated testing feedback. 
API evolution during development life cycle may include breaking changes for not yet productive features 
and as long as we have aligned the changes with the clients. 
Hence, API First does *not* mean that you must have 100% domain and requirement understanding and can never produce code 
before you have defined the complete API and get it confirmed by peer review. On the other hand, API First obviously is 
in conflict with the practice of publishing API definition and asking for peer review after the service integration 
or even the service productive operation has started. 
It is crucial to request and get early feedback — as early as possible, but not before the API changes are comprehensive 
with focus to the next evolution step and have a certain quality (including API Guideline compliance), 
already confirmed via team internal reviews. 


## {{ book.must }} Provide API Reference Definition using OpenAPI

We use the [OpenAPI specification](http://swagger.io/specification/) (aka Swagger spec) as standard for our REST API definitions. 
You may choose YAML or JSON as a format of your OpenAPI API definition file; 
however, YAML is generally preferred due to its improved readability. 

We also call the OpenAPI API definition the "API Reference definition" (or "API definition"); 
it provides all information needed by an experienced API client developer to use this API.

The OpenAPI API specification file should be subject of version control together with source code management. 
Services also have to support an
[endpoint to access the API Reference definition](../api-operation/ApiOperation.md#must-Provide-Online-Access-to-OpenAPI-Reference-Definition) for their external API(s). 


## {{ book.should }} Provide User Manual Documentation

In addition to defining the API as OpenAPI Reference Definition, it’s good practice to provide 
further documentation to improve client developer experience, especially of engineers that 
are novices in using this API. A helpful API User Manual documentation typically describes 
the following API aspects:

- API’s scope, purpose and use cases
- concrete usage examples 
- architecture context - including figures and sequence flows
- edge cases and possible error situations

The User Manual must be posted online, e.g. via GitHub Enterprise pages, on specific 
team web servers, or as a Google doc. And don't forget to include a link to this 
user manual documentation into your OpenAPI definition using the “externalDocs” property.

## {{ book.must }} Write APIs in U.S. English
