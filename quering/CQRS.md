# CQRS patten language

In microsevice architecture most of the time we have 1 database that responds both query and update operations.
In other words 1 database is responsible for handling complex join queries, and CRUD operations.

<img src="../images/CQRS/simple-service.svg" />

## <img src="../images/icons/meh.svg" style="position:relative; top:5px;" /> situation

Imagine the situation where your app:

-   requires a query to join more than 10 tables. This will lock the database and cause latency in query computation!
-   Due to the complex database model and validation, CRUD operations will take a long time.

## <img src="../images/icons/sad.svg" style="position:relative; top:5px" /> problem

Read and write have different concerns.
When we write to a DB, we write **entities**,
but when read, we retrieve **aggregated** data, instead of the entity-based model.

## <img src="../images/icons/happy.svg" style="position:relative; top:5px" /> solution

The operations that we perform on our database are: READ, WRITE, UPDATE, DELETE

-   `read` called:
    **_Query_**, **_aggregation data grid_** or **_presentation object_**
-   `write, edit, delete` called:
    **_commands_**. or **_entity based model_**

### Steps:

**1.** Separate the Query and Command models.
(seperate aggregate model from entity-based model)

<img src="../images/icons/warning.svg" style="position:relative; top:5px" /> Good for validation in CRUD operations, but it dosn`t really optimize the database.

<img src="../images/CQRS/service-with-2-models.svg" />

**2.** Split database into read and write DB.
This step, optimizes our databases.

<img src="../images/CQRS/service-with-2-DB.svg" />

#### best solution

**3.** Split the microservice into 2 separate units of deployed software.
Each microservice has own model, own database, ...

<img src="../images/CQRS/app-with-2-service.svg" />

## <img src="../images/icons/star.svg" style="position:relative; top:5px" /> so

##### CQRS: Command Query Responsibility Segregation.

CQRS always has 2 sides: read, write

-   On the write side: commands or events are sent and reliably stored.
-   On the read side: queries are executed to retrieve data.

## <img src="../images/icons/sad.svg" style="position:relative; top:5px" /> problem 2

How to sync two databses and keep it synchronize?

## <img src="../images/icons/happy.svg" style="position:relative; top:5px" /> solution

Using event-driven architecture:
When something is updated in the write database, a message is published.
These messages are consumed by the read database, which then updates itself.

<img src="../images/CQRS/DB-messaging.svg" />

## <img src="../images/icons/dart.svg" style="position:relative; top:5px" /> challenge

Data consistency: Because of using meesagees, data is not immediately reflected.

## <img src="../images/icons/badge.svg" style="position:relative; top:5px" /> some other benefits

-   We can scale each database independently.
    For example: if our app is **read-inactive**, we can focus on scaling only the read DB.
-   The computation of write data does not occur simultaneously with the reading of data.
-   Each computation is done only once, regardless of how many times the value is read.

> **read-inactive** : if our app is mostly reading use case and not writing so much

## <img src="../images/icons/high.svg" style="position:relative; top:2px" /> more performance

The Read DB can utilize its cache to retrieve data faster.
Whenever the Read DB receives a message from the Command DB, it updates itself and can also update its cache.
Subsequently, all data reading operations can be performed directly from the cache,
ensuring quicker access to the data.

## <img src="../images/icons/link.svg" style="position:relative; top:5px" /> related pattern language

-   event sourcing

## <img src="../images/icons/link2.svg" /> links

-   https://medium.com/design-microservices-architecture-with-patterns/cqrs-design-pattern-in-microservices-architectures-5d41e359768c
-   https://www.youtube.com/watch?v=lg6aF5PP4Tc&t=13s
-   https://www.youtube.com/watch?v=pUGvXUBfvEE
