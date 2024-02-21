## Implementing APIs

### MVC

MVC is an acronym for model-view-controller. There are design patterns we can employ to structure code. One such design pattern used to structure the APIs we create is called MVC design pattern.

Assume the entity that calls our API is called the client. If the client wants some information, should they be able to access the database directly? No, as this can lead to security concerns. Instead, the client should go to a server and the server should talk to the database. 

MVC pattern is used to structure the code in the server. The controller is the entity that should first receive the request. The controller layer will parse the data in the request and call the relevant functions in the application. The layer that performs the business logic is called the service layer. So, the controller layer will talk to the service layer to get work done. The service layer can directly talk to the database to create a new record or to read the data stored, however MVC pattern requires a repository layer between the service layer and the database. This allows for flexibility in changing the database in case our requirements change in the future. The respository layer talks to the database. 

In MVC, M stands for model. Models are blueprints of entities that we store in the database. Model class will not have any business logic. 

C stands for controller that is the layer between the client and the service layer.

V stands for view. In the past, backend used to generate the UI for frontend as well. This layer was called the view. These days view is managed by frontend frameworks like React etc. 

In a nutshell, MVC is just SRP for APIs.

### REST Practices

Like we have SOLID/design patterns for structuring general code, MVC for building APIs, we have REST best practices for how APIs should be named. 

All the APIs should be structured around the resource they are working with. The type of action an API is performing should not be a part of the API endpoint. For example, if we are creating a user, the API endpoint should not be /user/create. The type of action should be defined by the HTTP method type and not by the URL. 

Every API is an action on one of the models. 

The type of action should be specified by the method type and not by a verb in the URL.

The types of HTTP methods are

1. GET - To fetch a resource from the server.
2. POST - Create a resource in the server.
3. PATCH - Partially update attributes of a resource on the server.
4. PUT - Replace a resource on the server. This amounts to a complete update of the resource in the server.
5. DELETE - To delete a resource from the server.

Resources are like folders. Think of every entity as an item in the folder. When we create a new user, we are adding a new item in the users folder. So the end point should be /users. When getting details of all/multiple users, then the end point should be /users. If we are getting a particular user, then the end point will be /users/1, where 1 is the user id. 

REST APIs should be stateless. This means that a request should not be dependent on a previous request and that it should be self-sufficient. This is because a request from a user can go to any server in a distributed system. Maintaining the state for a user in a server and ensuring that all requests from a user go to the same server is impractical when the system is serving millions of users. Each HTTP request should contain all information that will help the server respond to the request.

There is no need to maintain a 1:1 mapping of API calls with database tables. A chatty API does not return all the relevant data in one go. In such cases, the client is forced to make multiple API calls to get the relevant information. When building an API, include all relevant information in the response to a request to that API so that the number of API calls can be kept to a minimum. 

REST has no constraint with respect to the data type of response. Response can be sent in multiple formats like JSON (most recommended format), XML, protobuf etc.

## Project Requirements and Microservices

### Microservices - Why do we need them?

In a system like Scaler, what are the various features/functionalities provided?

- Authentication
- Live/Recorded Classes 
- Dashboard
- Assignements
- Career Platform
- Mailing Portal

Assume Scaler creates a single codebase where every functionality is present in a single codebase. This will lead to a huge codebase. What are the issues with a huge codebase? 

1. Compiling a codebase with thousands of classes and application startup will take a lot of time. Hence any fix/update of the codebase will take a lot of time to take effect as the entire codebase will be recompiled and restarted.
2. Code will be overwhelming to understand.
3. Inter team conflicts while deploying etc.
4. Complete application requires a single techstack. This will lead to legacy issues. As technology and frameworks eveolve, there may appear better tools to provide certain features/functionalities. Using a new technology or framework is difficult when we have a single codebase.
5. Inability to selectively scale. Every part of the application does not require the same scale. Using a single codebase will hinder the ability to selectively scale certain applications of the project. 


Once the codebase is split into microservices, each of these services can run on a seperate server and have their own database. These microservices will talk to each other using API calls. 

## Frameworks

Framworks help us build applications efficiently by providing common functionalities out of the box.

### Why do we need frameworks

As codebases evolve, they will have many functionalities and they will be doing many similar things. For example, some of the similarities will be authentication related flow, APIs, talking to databases, logging etc. 

When we build an API, we will need an application to run on a server and receive requests over the internet. The operating system of the server will have to receive the request and pass the request to the appropriate application. The application will have to parse the data received with the request, process it and respond to the request.

If we had to write the code to handle this without a frameowrk, we will collect the data from the request in bytes and convert it into the relevant datatype and then process it in the application. Doing this amounts to reinventing the wheel. 

Instead of this, we should follow the best practices of handling data securely, decrypting data, authentication, talking to databases and many other functions related to processing a request. Frameworks provide these best practices in the form of inbuilt and ready-to-use functionalities. 

### Springboot as a framework

Spring framework is a set of projects that allow easy creation of enterprise level Java applications. 

It is comprised of a core framework (core set of functions) and ADDONs (authentication, kafka, database, cloud). 

The core of spring framework is dependency injection.

#### Dependency Injection

This is a foundational concept in the spring framework. A framework doing dependency injection on our behalf is called inversion of control. 

Consider an attribute in a class. There are two ways the attribute can be assigned a value.

1. Initialize the value in the class. 
2. Inject the value of the attribute from outside the class. 

The benefit of the second approach is that it is better to create a dependency outside the class than creating it within the class. Why?

1. We can resuse the same object. 
2. Satisfies the dependency inversion principle which states that class A and class B should be loosely coupled through an interface. 
3. It makes the code easier to test

Whenever we create an application, there are many common dependencies that have to be injected in multiple classes. For example, multiple classes will have a dependency on a database. Spring provides an easy way to do dependency injection. 

Spring has something called beans, which are special objects that we provide to spring, so that they can be injected automatically when needed. Spring takes these objects and puts them in the spring container which is also called application context. 

While spring framework provided dependency injection, a lot of functionalities required for builing an application are provided by spring add-ons. Initially, to include add-ons we had to create an XML file just to configure them.

Springboot made the entire process of including add-ons easy, by automatically configuring add-ons using best practices, while retaining the ability to override it.  

#### Controllers

#### Services

When we have more than one implementation of a service interface, name each of the implementations and then in the Controller's constructor add @Qualifier("serviceClassName") so that Spring can inject the correct bean into the controller. 

#### Repositories

Repositories are written as interfaces that extend JPA

#### Models

All the classes from the class diagram become models in Springboot. These are the entities that we store in the database. Hence we have to annotate these classes with @entity. 

All tables in SQL need a primary key. We can select an attribute (usually id) and annotate it with @Id. A design choice we can follow here is the use of a BaseModel mapped superclass for common attributes like id, auditing attributes like createdAt and updatedAt and a boolean attribute isDelete that enables us to implement soft delete. 

##### Implementing Inheritance Types

1. Mapped Superclass
    
    - Annotate parent class with @MapperSuperclass
    - Annotate child classes with @Entity

2. Table per class

    - Annotate parent class with @Entity and @Inheritance(strategy = InheritanceType.TABLE)
    - Annotate child classes with @Entity

3. Joined Table

    - @Entity on the parent class.
    - @Inheritance(strategy=InheritanceType.JOINED) on the parent class
    - @Entity on the child class
    - @PrimaryKeyJoinColumn(name="parent_id") on the child class. This is the join column with the parent class.

4. Single Table

    - Annotate the parent class with @Entity.
    - Annotate the parent class with @Inheritance(strategy = InheritanceType.SINGLE_TABLE).
    - Annotate the parent class with @DiscriminatorColumn(name="userType",discriminatorType=DiscriminatorType.INTEGER).
    - We can also add @DiscriminatorValue(value="0") to the parent table
    - Annotate the child class with @Entity. Adding this will not lead to a table of the child in the database. It ensures that the attributes of child class will be there in the main table. 
    - Annotate the child class with @DiscriminatorValue(value="1").
    



### First Application using a Framework

We are going to use spring boot to develop a basic application. Initialize a springboot project using spring initializr extension in vscode. Select the springboot version, java version and build a maven project with the default names (com.example and demo) provided by the initializr . Add spring dev tools, spring configuration processor, lombok and spring web as add-ons in the project and save the project to your desired folder. Later, we will see what each of these add-ons do. 

Once you have opened the project in the code editor, create a new folder in the demo folder called controllers. Then create a file called HelloController.java. 


Three references on springboot

https://docs.spring.io/spring-framework/reference/overview.html

https://docs.spring.io/spring-framework/reference/core/beans/introduction.html

https://spring.io/guides/gs/rest-service



### Journey of a request in Spring

When a request comes to springboot, the request goes to a class called Dispatcher Servlet. This class routes the request to the appropriate controller by talking to someone called Handler Mapping class. It asks for the method to serve the particular request. The handler mapping has a map of the routes and their corresponding methods in the corresponding controllers. 

Upon receiving the method, the servlet will call the controller method with the relevant arguments. Once the controller passes on the request to the service and the service responds to the request, the controller sends the response back to the servlet. The servlet then sends the response back to the client.

### Fakestore API

https://fakestoreapi.com/docs

Some of the features of a product service are

1. Create product
2. Get details of a product
3. Search product
4. delete product

There are a couple of ways we can implement the product service

1. Using a database. Here, we store details of products in the database. When we create a product, the product service will create an entry in the database. When we request the details of a product, the product service will get it from the database.

2. Using a third party API provider. When a client requests the details of a product, the product service will get these details from the third party api.

We are going to focus on using a third party API provider to build the product service in this section. The third party API provider we will use is the Fakestore API. We will implement proxy APIs for the APIs provided by Fakestore. 

#### Building our Proxy APIs

The product service should support CRUD operations for products. Inorder to create the class diagram, we need to find the nouns in the requirements. 

1. Product. Fakestore stores id, title, price, category, description and image url of a product. The datatype of id is integer, title is string, price is double, category is another entity, description is a string and image url is also a string.

2. Category. This entity will store the id and name of the category. The datatype of id is integer and name is a string. 

Both these classes will become models in our codebase. These are the tables that will be created in the database. 

In general, the service in the controller should be an interface because we retain the flexibility to choose the implementation we want. 

We should use the RestTemplate library to call third party APIs. We need only one instance of such a class to make third party API calls. This one instance can be used by multiple services to make these API calls. So, it makes sense to instantiate this object as a bean that can then be used by multiple services. 

Important classes should be annotated so that Spring knows the existence of these classes. This helps Spring create the necessary beans and inject them to constructors and methods specified by us with the autowired annotation.

The code for getting all products from the fakestore api can be written like below

```
public List<Product> getAllProducts() {
        List<FakeStoreProductDto> response = restTemplate.getForObject("https://fakestoreapi.com/products/", List<FakeStoreProductDto>.class, null);
        List<Product> answer = new ArrayList<>();
        for (FakeStoreProductDto dto: response) {
            answer.add(convertFakeStoreProductDtoToProduct(dto));
        }
        return answer;
    }
```

However this will not work. Java will not allow converting to a list of objects, in this case fakestoredto, because of the way generics in Java work. 

A datatype that takes another data type as parameter is generics. These are generic datatypes. The class for List is defined as follows

```
class List <T> {

    add(T data) {}

    T get(int index) {}
}
```

Here T is a variable data type. This type of definition was introduced from Java 5. Before that the class was defined like this.

```
class List {
    add(Object a) {}
}

```

Java tries to maintain as much backward compatibility as possible. This means that a code written in previous version of Java should continue to work in the current version of Java. Inorder to do this, Java developers implemented type erasure. This means that paramterized type was checked only during compile time and not at run time.

If someone has declared a list<Animal> or list<Integer>, the parameter information is removed at runtime. At runtime, Java recognizes the list but not the datatype of data in the list.

You can check this by running the following code

```
List<Integer> l1 = List.of(1, 2, 3, 4);
List<Object> l2 = List.of(new Object(), new Object());

System.out.println(l1.getClass());
System.out.println(l2.getClass());
```

The output in both cases will be the same. 

So, runtime will not know that each item of the response should be converted to an object of FakeStoreProductDto and added to a list. As Java does not know this, each item is converted to a hashmap by default. 

Since the issue is with using lists, we can replace it wiht array and the problem goes away.

```
public List<Product> getAllProducts() {
        FakeStoreProductDto[] response = restTemplate.getForObject("https://fakestoreapi.com/products/", FakeStoreProductDto.class, null);
        List<Product> answer = new ArrayList<>();
        for (FakeStoreProductDto dto: response) {
            answer.add(convertFakeStoreProductDtoToProduct(dto));
        }
        return answer;
    }
```

Put method in restTemplate does not return the replaced object. Even though the fakestore API returns the replaced object, the put method ignores it.

Internally the put method in the restTemplate call the execute method. All the other HTTP methods call the execute method internally. Execute method is a low level method that even we can use to achieve customization while using restTemplate. 

```
public Product replaceProduct(Long id, Product product) {
        
        RequestCallback requestCallback = restTemplate.httpEntityCallback(product,          FakeStoreProductDto.class);
		HttpMessageConverterExtractor<FakeStoreProductDto> responseExtractor =
				new HttpMessageConverterExtractor<>(FakeStoreProductDto.class, restTemplate.getMessageConverters());
		FakeStoreProductDto response = restTemplate.execute("https://fakestoreapi.com/products/"+id, HttpMethod.PUT, requestCallback, responseExtractor);
        return convertFakeStoreProductDtoToProduct(response);
    }
```

The httpEntityCallback method takes the request body and the respnse type as inputs and returns a requestCallback. Then we call the execute method with the relevant arguments and get the response as an object of FakeStoreProductDto. Thus we can now respond with the replaced object which was previously not possible with the put method. 

In order to make patch requests using resttemplate, we need to add a dependency

```
<dependency>
	<groupId>org.apache.httpcomponents.client5</groupId>
	<artifactId>httpclient5</artifactId>
	<version>5.2.1</version>
</dependency>
```

#### Handling Exceptions in APIs

Any request goes to the controller first. Exceptions can be thrown from controller. For example, in the absence of mandatory parameters or if the values of some parameters are wrong, then exceptions can be thrown. 

Services can also throw exceptions. For example, in case of a request to get all the orders of a user, if the user does not exist, then the service should throw an exception. 

Repositories can also throw exceptions. For example, if the database connection was interrupted, then repositories should throw exception. 

Exceptions can be thrown from any point in a request path. When an excpetion is thrown from a method, then these excpeptions should be caught and handled. This means, the method call should be wrapped in a try catch block. In the catch block, we handle the exception. 

If an excpetion is thrown from the controller, then the client cannot handle the exception. Frontend cannot catch exception from the backend. This means that, when an exception is thrown, the entire stack trace will be visible at the frontend. Even if exceptions are thrown from the service or repository layer, if it is not handled, the entire stack trace will be sent to the client side. This is a security issue. 

We should handle excpeption gracefully and send meaningful responses to the client in case an exception is thrown. 

Handling different types of exceptions in the controller will lead to the code becoming large and clunky. 

To handle the above issues, Spring provides Controller Advice. Any exception thrown in the spring application will pass through the controller. Controller Advice is an additional check on responses returned by the controller. It can also modify data that is being returned to the client.

To make a class a controller advice, we have to annotate the class with @ControllerAdvice. Now we can define exception handlers within this class. The code will look something like this.

```
@ControllerAdvice
public class ControllerAdvice {

    @ExceptionHandler(ArrayIndexOutOfBounds.class)
    public Response arrayOutOfBounds() {
        ...
    }
}
```

We can have exception handlers in the controller class itself in case we want to send controller class specific messages while handling exceptions. 

```
// When exception is thrown from the controller, this class level handler will be 
// called.
@ExceptionHandler(ProductNotFoundException.class)
public ResponseEntity<Void> handleProductNotFoundException() {
    return new ResponseEntity<>(HttpStatus.FORBIDDEN);
}
```

### Introduction to Spring Data JPA

When interacting with a database, an application does the following things

1. Create tables
2. CRUD operations

In relational database, there is a close relation between class diagram and schema design. This means if there is a class, then there will be a corresponding table in the db. The attributes of the class will have corresponding columns in the database. 

Writing SQL queries for CRUD operations ourselves can be cumbersome. There are libraries called ORM libraries that make this task easier. ORM stands from Object Relational Mapping. They provide an easy way to interact with database based on the models in our codebase. 

They provide functionalities like

1. Automatically create tables based on the models we have defined
2. Automatically get/update data from the db.
3. Based on the name of the method, ORM can create queries for us.

Hibernate is the most popular ORM library in Java. There are other ORM libraries like MyBatis, JOOQ.

Java also provides an interface that are implemented by different ORMs called JPA. JPA is called Java Persistence API. This way we can code our application to the interface rather than a direct coupling with Hibernate or MyBatis etc. 

Hibernate supports multiple databases like MySql, PostgresSql etc. Hibernate does not have code for talking to every database. This is where JDBC comes in. JDBC is an interface that has common methods like connect, save, insert, executeQuery etc. This interface is implemented by individual databases. For example, MySQL will have a MySQLJDBCConnector, PostgreSql will have a PostgreSQLJDBCConnector and so on.

The code in these connectors talks to the database. Any database that has JDBC implementation will work with Hibernate because Hibernate talks to JDBC.

### Database Setup

Add Spring-starter-jpa dependency to pom.xml from mvnrepository. After that add the mysql-connector dependency to the pom file. 

### JPA Queries

#### Query Methods

In declared queries, we write a method name and the ORM will create a query based on method name. There are two parts to the method name, the part before by and the part after by. We can find the prefixes in supported query method subject keywords. We can find the part after by in supported query method predicate keywords and modifiers. 

The part before by is the type of query we want to run. The part after by is the condition based on which records are fetched.

In derived query, suppose we want to query for products of a particular category by category id. One of the ways to do this, it to query the categories table with category id and get the category object and then use the object to query the product table to get the product list. This may make the API slow because there are 2 DB calls. We can instead write a method like this.

```
List<Product> findByCategory_Id(Long id);
```

The '_' allows us to access the attributes of the entity mentioned. 

There is one method that is used to create and update. This is the save method. If we pass an object to save without id, then the object record will be created and saved in the database. If the object is passed with id, then first check for presence, if yes, then update else save.


Hibernate Query Language, which is like SQL, is used when we dont want the ORM to create the query.

```
@Query("select p from Product p where price = ? and description like %?%")
List<Product> someName();
```

When do we use HQL? When creating a query method becomes complex or long. 

An advantage of using HQL is we can query for specific attributes instead of getting the entire row.

```
@Query("select p.id, p.title from Product p where price = ? and description like %?%")
List<Product> someName();
```

For this to work, the return type of the method should be an interface with get method of id and title attribute.

```
public interface ProductWithIdAndTitle {
    Long getId();
    String getTitle();
}
```
This interface is called projection.

Now the HQL method will look like this

```
@Query("select p.id, p.title from Product p where price = ? and description like %?%")
List<ProductWithIdAndTitle> someName();
```

HQL is database independent. It works across databases as hibernate converts HQL to a database specific query.

We can write native queries by including nativeQuery = true in the @query annotation.

```
@Query(value = "select * from product p where p.id = 5", nativeQuery = true)
List<ProductWithIdAndTitle> someName();
``` 

How do we get the parameters?

```
@Query(value = "select * from product p where p.id = :id", nativeQuery = true)
List<ProductWithIdAndTitle> someName(@param("id") Long id);
```

We might want to write specific SQL queries to have control over the query being executed. We may want to do this for enhanced performance. 

> Reference: https://docs.spring.io/spring-data/jpa/reference/jpa/query-methods.html

#### Representing Cardinalities

Consider products in an ecom store. Assume a product can belong to only one category. A category can have many products. The cardinality of this relationship is m:1. How do we represent this relation ship in Spring?

In the product class, we will have the following.

```
@Entity
class Product {
    @ManyToOne
    private Category category;
}
```

In the category class, we will have the following.

```
@Entity
class Category {
    private List<Product> products
}
```

The product attribute in the category class and the category attribute in the product class represent the same relationship. However, Spring does not know they are the same relationship by default. Hence Spring will end up representing the relationships twice. So, we have to tell Spring that they are the same relation. 

```
@Entity
class Category {
    @OneToMany(mappedBy = "category")
    private List<Product> products
}
```

This means that the relationship between products and category is mapped by the category attribute of the product class. This is possible with @OneToMany annotation and not @ManyToOne annotation.

Once the tables are setup, the products table will have a foreign key which is the category id. 

What happens when we delete a row from the categories table? There are a few ways we can handle this. 

1. Update the product category column to null.
2. Delete all the product rows associated with the category row itself.
3. Do not allow category to be deleted.

These are the generic ways to handle foreign key deletion. 

Spring provides us some ways to handle these operations.

```
@Entity
class Product {
    @ManyToOne(cascade = {CascaseType.All})
    private Category category;
}
```

In this way, if the product is deleted, then the category is also deleted. If product is updated and there is an update in the category, then the category is also updated. If product is created and category details are also provided, then a new category is created. What happens if the category already exists and it is not handled in the code. 

CascadeType.PERSIST will transmit the save operation. CascadeType.MERGE will transmit the update operation. CacadeType.REMOVE will transmit the delete operation. 

```
@Entity
class Product {
    @ManyToOne(cascade = {CascaseType.PERSIST, CascadeType.MERGE})
    private Category category;
}
```

The same concepts apply in many to many relationships as well. 

> Cascade Types: https://www.baeldung.com/jpa-cascade-types

#### FetchType and FetchMode

When a class has an attribute of another class, to fetch the details of that attribute, it will require us to do a join. This will make the query slow. If we need the values of the attribute, then we have no choice but to do the join and fetch the values. 

However, in cases where we do not these values, performing the join operation and fetching the values of the attribute will be wasteful. 

JPA supports two ways to fetch attributes of a child class. 

1. Eager fetch
2. Lazy fetch

In eager fetch, we fetch the details of the child class attributes while fetching the main object. Here we will need to perform a join operation. 

In lazy fetch, we do not fetch the details of the child class attributes while fetching the main object. These details will be fetched lazily. What does this mean? This means the details will be fetched in a seperate query if those details are requested. 

While lazy fetch, which means making a seperated database call, is not as efficient as a join operation, we prefer lazy fetch when the number of times we request for the attributes of the child class constitute about 10% of the number of times we request the main object. 

By default, all attributes are eager fetched other than collection attributes. Where can we change the default behaviour JPA? When we specify the cardinality of a relationship we can specify the fetch type as well.

```
@Entity
public class Category {
    @OneToMany(fetch=FetchType.EAGER, mappedBy="category")
    private List<Product> products;
}
```

Now let us talk about fetch modes. JPA ignores fetch modes. It is relevant when we directly use hibernate.

Consider this situation.

```
class Category {
    Long id;
    String name;
    List<Product> products
}

List<Category> categories = categoryRepo.findByNameStartsWith("s");
for (Category c: categories) {
    for (Product p: c.getProduct()) {
        sout(p.title());
    }
}

```

Let us say there are m categories and each category has n products. The number of ways to get the categories and all the products of the categories is as shown in the table below.

|2                                                                                            |m+1                                                                                           |m*n + 1                                                                                       |
|---------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------|
| Select * from categories where name like "s%"                                              | Select * from categories where name like "s%"                                              |  Select * from categories where name like "s%"                                             | 
| Select * from products p join categories c on p.c_id = c.id where c.id in []               |
Select * from products p join categories c where c.id = {1}. Here we run these queries for each category id | Here select the product from categories, but for each product send queries to get details of product |

The code is the same but JPA could decide to execute the code in any of the three ways. 

1. Whenever using ORM, read the queries that the ORM runs. If not optimal go for native queries. Usually ORMs run m+1 queries. 

2. We can also do this by telling JPA the fetch mode. JPA usually ignores fetch mode. Fetch mode usually works with hibernate.

> https://www.baeldung.com/hibernate-fetchmode, https://www.baeldung.com/spring-data-jpa-query, https://thorben-janssen.com/jpql/


## Repository Pattern

Code to interact with the persistence layer should be seperate from the business logic. Service layer takes care of business logic. The repository layer takes care of interacting with the database. If we need to change the persistence layer for any reason, we only need to implement a new repository code to interact wwith the new database. The service layer will stay the same. Hence the repository pattern promotes loose coupling.

## UUID

UUID is universally unique identifier. 

In an SQL database, every table needs to have a primary key that is unique. 

What sort of column should be make the primary key? An index is automatically created for a column that is the primary key. Incase the primary key column has the string datatype, the writes will become slower. 

The rule of thumb is that having a seperate id column as the primary key is almost always the better choice.

So, what should be the datatype of such a column?

1. Integer - An integer occupies 4 bytes. It can store 2^32 values. There can be cases where a table can have more than 2 billion rows.

2. BigInt/Long - It occupies 8 bytes. It can store 2^64 or 10^18 values. It is unlikely that we will need a table that will need to store more than these number of values. While it can accomodate a large number of records in a tables, there are couple of issues. Records with long id can be scraped easily if the id is publically visibile. This can be done by iterating across all the possible values. If data is distributed across multiple databases, then auto-incrementing the id will not work. This will make it difficult to ensure uniqueness of the id.  

UUID is a 128-bit number created from a function of multiple parameters. It is represented as a hexadecimal number. Every 4 bits from 128 bits is combined to get a hexadecimal char. Thus the UUID will have 32 characters.

UUID version 7 ensures that every UUID generated is greater than the previous UUID. This ensures that writes into the database do not lead to performance issues. The version 7 ensures this by making the first few characters the milliseconds since epoch. 

## HTTP Response

When we make an http request, we get a response that has different types of information in it. 

1. Status Code. 1xx is informational, 2xx conveys succes, 3xx is redirect, 4xx says client is wrong and 5xx says server is wrong.
2. Response body

In spring boot, we can add different types of information in the response by returning a ResponseEntity<>. If we are return a product object, then we can specify the return type to be ResponseEntity<Product>. 


## Inheritance Relationship in Database

There are 4 ways to represent inheritance relationships in database.

1. Mapped Superclass

    - When there is no object of the parent class. 
    - Parent class is an abstract class
    - One table of each child class with its corresponding attributes and the parent's attributes.

2. Joined Table

    - Every data with respect to parent class will be in parent table. 
    - For each child class, there will be a table with its attributes.
    - Each child table will also have the parent id as foreign key. 
    - This is useful when we want to query based on parent's attribute.
    - In order to get a child attribute in the parent table, we have to join the child and parent table on the foreign key and query the data.

3. Table Per Class

    - This approach is similar to mapped superclass except that in this way, we will have the parent table as well. 

4. Single Table

    - Create one table with all columns of child classes as well.
    - Add one more attribute called type to identify the child class a record belongs to.
    - There will be many null values. It wastes space. 

> Read https://www.baeldung.com/hibernate-inheritance 



