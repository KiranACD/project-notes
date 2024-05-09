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

1. We can resuse the same object (Object here is the value of the attribute). 
2. Satisfies the dependency inversion principle which states that class A and class B should be loosely coupled through an interface. 
3. It makes the code easier to test

Whenever we create an application, there are many common dependencies that have to be injected in multiple classes. For example, multiple classes will have a dependency on a database. Spring provides an easy way to do dependency injection. 

Spring has something called beans, which are special objects that we provide to spring, so that they can be injected automatically when needed. Spring takes these objects and puts them in the spring container which is also called application context. 

While spring framework provided dependency injection, a lot of functionalities required for building an application are provided by spring add-ons. Initially, to include add-ons we had to create an XML file just to configure them.

Springboot made the entire process of including add-ons easy, by automatically configuring add-ons using best practices, while retaining the ability to override it.  

#### Controllers

#### Services

When we have more than one implementation of a service interface, name each of the implementations and then in the Controller's constructor add @Qualifier("serviceClassName") so that Spring can inject the correct bean into the controller. 

#### Repositories

Repositories are written as interfaces that extend JPA.

#### Models

All the classes from the class diagram become models in Springboot. These are the entities that we store in the database. Hence we have to annotate these classes with @entity. 

All tables in SQL need a primary key. We can select an attribute (usually id) and annotate it with @Id. A design choice we can follow here is the use of a BaseModel mapped superclass for common attributes like id, auditing attributes like createdAt and updatedAt and a boolean attribute isDelete that enables us to implement soft delete. 

##### Implementing Inheritance Types

1. Mapped Superclass
    
    - Annotate parent class with @MapperSuperclass.
    - Annotate child classes with @Entity.
    - Parent class is an abstract class. This means no object of parent class.
    - 1 table of each child class with its own and parent's attributes. 

2. Table per class

    - Annotate parent class with @Entity and @Inheritance(strategy = InheritanceType.TABLE).
    - Annotate child classes with @Entity.
    - There will be a table for all classes with its own and parent attributes. 
    - There will be a users table, a mentors table, an instructors table, a TAs table with all the attributes in all the tables unlike the joined table.
    - The Users table will have data of all users who are not mentors or instructors or TAs. 
    - If a user is both instructor and mentor, their entry will be in the instructors and the mentors table.

3. Joined Table

    - @Entity on the parent class.
    - @Inheritance(strategy=InheritanceType.JOINED) on the parent class.
    - @Entity on the child class.
    - @PrimaryKeyJoinColumn(name="parent_id") on the child class. This is the join column with the parent class.
    - Every data with parent attributes will be in the parent table. 
    - Each child class will have a table with attributes specific to the child class. 
    - The query to get email of every instructor will be slower in this design than the mapped superclass because there we had to query just one table. 
    - The query to get enail of every user will be faster in this design than the mapped superclass because we have to query a single table here, whereas in the mapped superclass design, we have to do union of all child tables to get emails of all users. 
    - This is the best design choice in 99% of the cases. 

4. Single Table

    - Annotate the parent class with @Entity.
    - Annotate the parent class with @Inheritance(strategy = InheritanceType.SINGLE_TABLE).
    - Annotate the parent class with @DiscriminatorColumn(name="userType",discriminatorType=DiscriminatorType.INTEGER).
    - We can also add @DiscriminatorValue(value="0") to the parent table
    - Annotate the child class with @Entity. Adding this will not lead to a table of the child in the database. It ensures that the attributes of child class will be there in the main table. 
    - Annotate the child class with @DiscriminatorValue(value="1").
    - There will be one table with all attributes of all parent and child classes. 
    - We will need to add an attribute called type to identify whether a row is an instructor or TA or mentor. 
    



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
| Select * from products p join categories c on p.c_id = c.id where c.id in [] | Select * from products p join categories c where c.id = {1}. Here we run these queries for each category id | Here select the product from categories, but for each product send queries to get details of  product |

The code is the same but JPA could decide to execute the code in any of the three ways. 

1. Whenever using ORM, read the queries that the ORM runs. If not optimal go for native queries. Usually ORMs run m+1 queries. 

2. We can also do this by telling JPA the fetch mode. JPA usually ignores fetch mode. Fetch mode usually works with hibernate.

> https://www.baeldung.com/hibernate-fetchmode, https://www.baeldung.com/spring-data-jpa-query, https://thorben-janssen.com/jpql/

#### Schema Migration and Versioning

ORMs handle table creation in general.

1. We might want to handle the table creation ourselves inorder to make optimizations that the ORM cannot do. 
2. ORMs do not do versioning of schema. We want to have versioning of database schema for multiple reasons. The primary one is being able to revert to previous schema when we revert to a previous code. In general, we want to track schema changes along with code changes. There are two schema migration/versioning libraries called flybase and liquibase. 

When we use a migration library, we have to maintain a db.migrations folder. When we make code change that also has schema change, we have to create a new SQL file with SQL query of that change. The SQL query is to create or update tables based on the schema changes. These files will be versioned. An additonal table is created in the database to track the versions of schema that have been applied. When we run code, flyway checks the table for last applied schema and will run the queries from subsequent versions.

When we use a schema migration library, we can set ddl-auto to none or validate. Validate is better because then JPA will check if all the required tables have been created for the app to run. 

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

## Why Testing?

As size of codebase increases, interdependence between different parts of the app will increase. Cost of change will increase with increasing interdependence.

No one person can be expected to know about the complete codebase. This can lead to anxiety about changing any part of the code and hence reduces the pace of development. 

Technical debt arises when we make an unoptimal change and in the future continue to rely on this change. 

This where testing helps because it tells us whether making a particular change could break another part of the application. A developer should not just write a feature, but also write automated test cases for the feature. Before releasing a new feature, if we run all the test cases and they pass, then we can confidently release the new feature. Here the underlying assumption is that the test cases are comprehensive which means all the edge cases are also covered by the tests.

There are two ways of writing tests.

1. Write feature -> Write tests -> Submit
2. Write tests -> Write feature -> Submit

The second is called test-driven development (TDD). TDD forces us to first think about how a user might want to use the feature we developed. 

### Flaky Tests

Flaky tests are unreliable because in some cases they pass and in other cases they fail. Tests become flaky when the code it is testing is itself flaky or the code in the test is flaky. Code is flaky when concurrency leads to inconsistent results.

If our code has concurrency or using random, we need to double check the code for flakiness. 

### Types of Testing

#### Unit Testing

A test for a function A should fail only if the code in the function has bugs. If A calls another function B and there are bugs in B but not in A, the tests for A should not fail. 

The way to do this is to mock function B in the tests so that the test is limited to code in A. Code in B should be tested by another set of tests that cover only B.

Test coverage is the percentage of code being tested by one or more test.

A good test case is input/output pairs. It should not have the function logic in the test function itself. 

#### Integration Testing

Here we test external dependencies of certain functions. Here, all dependencies are also called as in the read world. In an integration test, we will not mock external dependency. Hence, functional tests are usually slower than unit tests. We should not have as many functional tests as unit tests. 

In functional testing, we will mock third party dependencies because these are not usually our code. 

#### Functional Testing

Here we test end-to-end functionality of an app. We will make an API call with an input JSON and compare the output JSON with expected JSON.

> https://www.baeldung.com/junit-5, https://www.baeldung.com/mockito-annotations

### Testing Scenarios 

1. Happy scenarios
    - These are the inputs we typically expect from a user.

2. Bad scenarios
    - These are inputs we never expect

3. Corner scenarios 
    - Here the input provided has a huge potential to result in a bug in the code. 

### Qualities of Unit Tests

A unit test should be fast. It should be written in 3 different sections. First one is arrange, second is act and third is assert. 

In the arrange section, we need to create all the inputs to feed into the function that we want to test. In the act section, we will call the function to test by passing the inputs we created. In the assert section, we will compare the result with the expected result. The expected result should be hardcoded in the test code. 

A unit test should be isolated. Another test should not affect the output of the current test. 

A unit test should be repeatable. The output of the test should be the same for the same input. 

All tests should be self-checking. This means we should not take input from the user for tests. 

Tests should test behaviour and not implementation. This is because even though implementation can change, the behaviour of the function should never change. 

> https://testing.googleblog.com/2013/08/testing-on-toilet-test-behavior-not.html


### Mocking

Mocking is hardcoding the response of an external dependency in the function that we want to test.

When mocking, we create test doubles. These are objects that will replace the real objects. For example, when testing controller, we will create doubles of service and repository. 

#### Types of Test Double

1. Mock
    - A double is where you hardcode the return value. 
    ```
    when (productRepo.findById(1L))
        thenReturn(new Product())
    ```
    ```
    class ProductServiceTest {
        test() {
            when(pr.findById())
                thenReturn(new Product());
            when(pr.getCount())
                thenReturn(5);
            
        }
    }
    ```
    - We cannot maintain state in a Mock. 

2. Stub
    - A class that tries to replicate the behaviour of the real class. It implements the same interface as the dependency.
    ```
    class ProductRepositoryStub {
        int count = 0;
        
        createProduct() {
            count += 1;
        }

        getCount() {
            return count;
        }
    }
    ```
    - The above stub will be used to test the function to get count of products.
    ```
    ProductServiceTest {
        test() {
            ProductRepositoryStub pr = new ProductRepositoryStub();
            ProductService ps = new ProductService(pr);
            ps.createProduct();
            int c = ps.getCount();
            assert(c == 1);
            ps.createProduct();
            c = ps.getCount();
            assert(c == 2);
        }
    }
    ```
    - Here, we pass the ProductRepositoryStub instead of the real ProductRepository.

3. Fake
    - It is similar to a stub, but the implementation is a lot closer to the real object than the implementation of the stub. It is less hacky than a stub
    ```
    ProductRepoFake implements ProductRepo {
        HashMap<Integer, Product> pc;
        save(p) {
            int s = pc.size();
            pc.put(s+1, p);
        }
    }
    ```

> https://blog.pragmatists.com/test-doubles-fakes-mocks-and-stubs-1a7491dfa3da, https://learn.microsoft.com/en-us/dotnet/core/testing/unit-testing-best-practices


### Testing in Spring

All tests should be written in the test folder. The test folder will have the same structure as the main folder. When writing tests for controllers, services, repositories and other pieces of code, we should follow the same package structure as in the main folder. 

Create a method for each test. Annotate the method for tests with @Test. 

The assertj library provides a variety of semantic assert statements. 

The way to create a mock bean. 

```
@SpringBootTest
class ProductControllerTest {
    @Autowired
    private ProductController productController;

    @MockBean
    private ProductService productService

    @MockBean
    private ProductRepository productRepository;

    @Test
    void testProductsSameAsService() {

        List<Product> products = new ArrayList<>();

        Product p1 = new Product();
        p1.setTitle("iPhone 15");
        products.add(p1);

        Product p2 = new Product();
        p2.setTitle("iPhone 15 Pro");
        products.add(p2);

        Product p3 = new Product();
        p3.setTitle("iPhone 15 Pro Max");
        products.add(p3);

        when(
            productService.getAllProducts()
        ).thenReturn(
            products
        )

        ResponseEntity<List<Product>> response = productController.getAllProducts();

        List<Product> productsInResponse = response.getBody();
        assertEquals(products.size(), productsInResponse.size());
    }

    @Test 
    void testNonExistingProductThrowsException() {
        
        when(
            productRepository.findById(10L)
        ).thenReturn(
            Optional.empty()
        );

        assertThrows(
            ProductDoesNotExistException.class,
            () -> productController.getSingleProduct(10L)
        );
    }
}
```

## Authentication and Authorization

The concept of identifying a user is authentication. The concept of granting authority to a user to access select resources. Authentication comes before authorization. 

Authentication based access control and role based access control.

### Authentication Flow

When you need to authenticate, you need an id and a way to validate the id. 

1. Signup
    - Create an account with name, email, password.
    - Password is the way the website can authenticate our id in the future. 
    - Website sends email to verify.
    - After we verify, our record in the db is marked as verified. 


2. Login
    - Send email and password. If email and password match record in db, then website returns success, else it returns failure.


Problems with this flow.

Database can be hacked and users' data can be leaked. With this data, the hacker can login to any user's account. 

Solution to this is to hash the password and then store in the database. The problem with this is that a hacker can create multiple accounts with some common passwords and get the hash value of the password from the website database. Then the hacker can check the users that have the same hash value and this way he gets the password of some users.

Another solution is to hash and salt. When using bcrypt library, the password will be encrypted and each time, it will be encrypted to a different value even if the password is the same. 

The signup will look like this.

```
signup(email, password) {
    hp = bcrypt.encrypt(password);
    db.save(email, hp);
}
```

When we try to login, we cannot do something like this.

```
login(email, password) {
    hp = bcrypt.encrypt(password);
    sp = db.get(email);
    if (hp == sp) return true;
}
```

The hp will never equal sp this way. Instead of this, bcrypt provides a method that can verify whether the password could have had the encrypted value stored in the db.

```
login(email, password) {
    sp = db.get(email);
    if (bcrypt.verify(sp, password)) return true;
    return false;
}
```

As HTTP is stateless, we need to send all details needed to fulfil the request. Then the server has to make db calls, run bcrypt and return the response. If done for every request (not just login requests), the server will become slow and hence every request will take lot of time. 

There has to be another solution so that we reduce the time taken for every request. This is where we can use tokens. When a user logs in, the server can create a token for the user if the authentication is successful and store it in the db. This token will have an expiry time. Now, for every subsequent request, the user can send the token to for authentication purpose. 

Even in the above method, we will need to make a db call to get the details associated with the token (username, expiry time etc). We can avoid making the db call by encoding all this information in the token itself. 

However, the above way has a security issue as anyone who gets access to the token can edit information like user id, generate a new token and login as another user. How can resolve this security issue? The solution is JWT. 

What is JWT? It stands for json web token. We first create a json like this.

```
{
    "user_id": 12345,
    "email:: "kiran@scaler.com",
    "role": ["admin"],
    "expiry_at": "Tuesday 27 Feb, 2024"
}
```

Now, we encode the above json using base64 format and create token. This token can be decoded by anyone. Now, we can change some values in the json generated and create a new token and send it to the server. The server cannot trust this token. In order to trust the token, the server will have to use a secret key to create the token. This secret key is only known to the server. Any token encrypted using this method can be decrypted only by someone with the secret key. Now the server can decrypt the token sent by the user and get all the user details encoded in the token. 

The json forms the part B of JWT. Part C contains the base64 encoding of part A and Part B and the secret key. Part A will have information about the encryption algorithm employed. This part is also base64 encoded. 

```
verifyToken(token) {
    a, b, c = token.split(".");
    d = decrypt(c, secret);
    if fails :=> invalid token;
    if (a + "." + b != d) :=> invalid token;
    return true;
}
```

JWT is a self-validating token. It can be validated using the information present in the token itself. We do not need to make database call. 

> https://jwt.io/introduction

Multiple modules of an application may need authentication. 

### OAUTH

Industry wide followed standards for authorization. It defines API contracts with respect to authentication and authorization. It becomes easy for us to use different authentication providers. 

When we build the userservice, we will follow the OAUTH standards to implement authentication and authorization. 

OAUTH says that there are 4 participants in any authorizaton related work.

1. User
    - User wants to access a resource

2. Resource Server
    - The application that has the information that the user wants.

3. Application
    - Service on which user wants to access information.

4. Authorization Server
    - The service that will manage authorization. In our case, it will be the userservice.

 

What is the difference between resource server and application? Consider an application that shows you emails on your gmail. In order to authorize, we will login via google. So google is the authorization server. The emails are in Gmail server, so that is the resource server. So application will talk to resource server. 

```mermaid
sequenceDiagram
User ->> Application: Give me access
Application -->> User: First Login
User ->> Authorization Server: Do Login
Authorization Server -->> Application: Gives token
Application ->> Resource Server: getResource(token)
Resource Server ->> Authorization Server: verifyToken(token)
Authorization Server -->> Resource Server: True
Resource Server -->> Application: returns emails
Application -->> User: Show emails

```

In many cases, the resource server and the application are the same. 

### OAUTH Implementation in Spring 

Tokens come as part of request headers. It will be the value of authenticationToken key in the headers. 

Once our application receives a request for resource, which could either be in the resource server seperate from the application or be in the application itself, it will have to verify the token before responding with the resource. 

So we will create a seperate class that will send request to the authorization server to validate the token. 

```
@Service
public class AuthenticationCommons {
    private RestTemplate restTemplate;

    public AuthenticationCommons(RestTemplate restTemplate) {
        this.restTemplate = restTemplate;
    }

    public UserDTO validateToken(String token) {
        ResponseEntity<UserDTO> userDtoResponse = restTemplate.postForEntity(
            "https://localhost:8181/users/validate" + token,
            null,
            UserDTO.class
        );

        if (userDtoResponse.getBody() == null) {
            return null;
        }
        return userDtoResponse.getBody();

    }
}
```

Now let us implement an authorization server. 

> https://docs.spring.io/spring-authorization-server/reference/getting-started.html

Get the dependency and application properties from the link above. It also helps us setup the security config for the server. 

```
import java.security.KeyPair;
import java.security.KeyPairGenerator;
import java.security.interfaces.RSAPrivateKey;
import java.security.interfaces.RSAPublicKey;
import java.util.UUID;

import com.nimbusds.jose.jwk.JWKSet;
import com.nimbusds.jose.jwk.RSAKey;
import com.nimbusds.jose.jwk.source.ImmutableJWKSet;
import com.nimbusds.jose.jwk.source.JWKSource;
import com.nimbusds.jose.proc.SecurityContext;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.annotation.Order;
import org.springframework.http.MediaType;
import org.springframework.security.config.Customizer;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.oauth2.core.AuthorizationGrantType;
import org.springframework.security.oauth2.core.ClientAuthenticationMethod;
import org.springframework.security.oauth2.core.oidc.OidcScopes;
import org.springframework.security.oauth2.jwt.JwtDecoder;
import org.springframework.security.oauth2.server.authorization.client.InMemoryRegisteredClientRepository;
import org.springframework.security.oauth2.server.authorization.client.RegisteredClient;
import org.springframework.security.oauth2.server.authorization.client.RegisteredClientRepository;
import org.springframework.security.oauth2.server.authorization.config.annotation.web.configuration.OAuth2AuthorizationServerConfiguration;
import org.springframework.security.oauth2.server.authorization.config.annotation.web.configurers.OAuth2AuthorizationServerConfigurer;
import org.springframework.security.oauth2.server.authorization.settings.AuthorizationServerSettings;
import org.springframework.security.oauth2.server.authorization.settings.ClientSettings;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.LoginUrlAuthenticationEntryPoint;
import org.springframework.security.web.util.matcher.MediaTypeRequestMatcher;

@Configuration
@EnableWebSecurity
public class SecurityConfig {

	@Bean
	@Order(1)
	public SecurityFilterChain authorizationServerSecurityFilterChain(HttpSecurity http)
			throws Exception {
		OAuth2AuthorizationServerConfiguration.applyDefaultSecurity(http);
		http.getConfigurer(OAuth2AuthorizationServerConfigurer.class)
			.oidc(Customizer.withDefaults());	// Enable OpenID Connect 1.0
		http
			// Redirect to the login page when not authenticated from the
			// authorization endpoint
			.exceptionHandling((exceptions) -> exceptions
				.defaultAuthenticationEntryPointFor(
					new LoginUrlAuthenticationEntryPoint("/login"),
					new MediaTypeRequestMatcher(MediaType.TEXT_HTML)
				)
			)
			// Accept access tokens for User Info and/or Client Registration
			.oauth2ResourceServer((resourceServer) -> resourceServer
				.jwt(Customizer.withDefaults()));

		return http.build();
	}

	@Bean
	@Order(2)
	public SecurityFilterChain defaultSecurityFilterChain(HttpSecurity http)
			throws Exception {
		http
			.authorizeHttpRequests((authorize) -> authorize
				.anyRequest().authenticated()
			)
			// Form login handles the redirect to the login page from the
			// authorization server filter chain
			.formLogin(Customizer.withDefaults());

		return http.build();
	}

	@Bean
	public UserDetailsService userDetailsService() {
		UserDetails userDetails = User.withDefaultPasswordEncoder()
				.username("user")
				.password("password")
				.roles("USER")
				.build();

		return new InMemoryUserDetailsManager(userDetails);
	}

	@Bean
	public RegisteredClientRepository registeredClientRepository() {
		RegisteredClient oidcClient = RegisteredClient.withId(UUID.randomUUID().toString())
				.clientId("oidc-client")
				.clientSecret("{noop}secret")
				.clientAuthenticationMethod(ClientAuthenticationMethod.CLIENT_SECRET_BASIC)
				.authorizationGrantType(AuthorizationGrantType.AUTHORIZATION_CODE)
				.authorizationGrantType(AuthorizationGrantType.REFRESH_TOKEN)
				.redirectUri("http://127.0.0.1:8080/login/oauth2/code/oidc-client")
				.postLogoutRedirectUri("http://127.0.0.1:8080/")
				.scope(OidcScopes.OPENID)
				.scope(OidcScopes.PROFILE)
				.clientSettings(ClientSettings.builder().requireAuthorizationConsent(true).build())
				.build();

		return new InMemoryRegisteredClientRepository(oidcClient);
	}

	@Bean
	public JWKSource<SecurityContext> jwkSource() {
		KeyPair keyPair = generateRsaKey();
		RSAPublicKey publicKey = (RSAPublicKey) keyPair.getPublic();
		RSAPrivateKey privateKey = (RSAPrivateKey) keyPair.getPrivate();
		RSAKey rsaKey = new RSAKey.Builder(publicKey)
				.privateKey(privateKey)
				.keyID(UUID.randomUUID().toString())
				.build();
		JWKSet jwkSet = new JWKSet(rsaKey);
		return new ImmutableJWKSet<>(jwkSet);
	}

	private static KeyPair generateRsaKey() { (6)
		KeyPair keyPair;
		try {
			KeyPairGenerator keyPairGenerator = KeyPairGenerator.getInstance("RSA");
			keyPairGenerator.initialize(2048);
			keyPair = keyPairGenerator.generateKeyPair();
		}
		catch (Exception ex) {
			throw new IllegalStateException(ex);
		}
		return keyPair;
	}

	@Bean
	public JwtDecoder jwtDecoder(JWKSource<SecurityContext> jwkSource) {
		return OAuth2AuthorizationServerConfiguration.jwtDecoder(jwkSource);
	}

	@Bean
	public AuthorizationServerSettings authorizationServerSettings() {
		return AuthorizationServerSettings.builder().build();
	}

}
```

Here, we are using an in memory user model as provided in the userDetailsService function. We want to implment this service with a real model. 

The authorizationServerSettings method sets the default configuration. We can introduce more customization if required.

The jwtDecoder method sets the jwt token decoder method. Here the default implementation uses RSA256 algorithm. This is an asymmetric encryption algorithm. It means the algorithm uses different keys for encryption and decryption. In contrast, a symmetric encryption algorithm uses the same key for encryption and decryption. When we use symmetric encryption algorithms, the encryption and decryption should ideally happen in the same place. 

So, the jwkSource uses RSA256 algorithm to generate a key pair for encryption and decryption. 

Now we look at the registeredClientRepository method. The authorization server will cater to many clients looking to use the APIs provided by the server to perform authentication of users looking to access their resources. 

In our case, productservice, orderservice, paymentservice and the other microservices will be the clients of the authorization server. 

In this case, the clients are being stored in memory, but we will have to implement rela entities to handle the client details and store them in a database. 

UserDetailsService is an interface. In the default implementation, the interface is being used to create users and add them to memory. However, we will want our own implementation so that we can store user details in a database. All we need to do is to provide a class to Spring security that implements the UserDetailsService.

Now we need to implement the models that will enable us to save clients, tokens etc in database. 

> https://docs.spring.io/spring-authorization-server/reference/guides/how-to-jpa.html

Now, we create packages called models, repositories and services. Copy all the relevant classes from above link into files in the respective packages. 

Now, we have to remove the default registeredClientRepository because this saves the client in-memory only. We have a repository that saves the client into the database. 

When we save the client secret into the db, make sure to encrypt it using bcrypt because when we request for authentication, the client secret is converted to bcrypt encryption and then compared with the secret in the db.

We need to connect the authorization server to our user database.

Lazy fetching works when the object is fetched from the same method that the original row was fetched. For example, when the user details are fetched, the roles are lazily fetched and it works when roles are fetched in the same method where the user was fetched. If it is fetched from another method, then it becomes another transaction and we fail to fetch roles. 

Spring security does not let a custom class be converted to JSON using Jackson, we have to explicitly let Spring know that it is safe to convert the custom class to JSON. We can do this by annotating the class with @JsonDeserialize. 

Jakson deserializes/serializes a class by first creating an object of the class by calling a constructor without passing anything. So the class needs to have an empty constructor defined. 

Another good practice, is to create attributes associated with all get methods in the class. Then return the value of these attributes from the correponding get methods.

```
@JsonDeserialize
public class CustomUserDetails implements UserDetails {

    private String password;
    private String username;
    private Long userId;
    private List<CustomGrantedAuthority> authorities;
    private boolean accountNonExpired;
    private boolean accountNonLocked;
    private boolean credentialsNonExpired;
    private boolean enabled;

    
    public CustomUserDetails() {}

    public CustomUserDetails(User user) {
        this.password = user.getHashedPassword();
        this.username = user.getEmail();
        this.userId = user.getId();
        List<CustomGrantedAuthority> grantedAuthorities = new ArrayList<>();
        for (Role role: user.getRoles()) {
            grantedAuthorities.add(new CustomGrantedAuthority(role));
        }
        this.authorities = grantedAuthorities;
        this.accountNonExpired = true;
        this.accountNonLocked = true;
        this.enabled = true;
        this.credentialsNonExpired = true;
    }

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return this.authorities;
    }

    @Override
    public String getPassword() {
        return this.password;
    }

    @Override
    public String getUsername() {
        return this.username;
    }

    @Override
    public boolean isAccountNonExpired() {
        return this.accountNonExpired;
    }

    @Override
    public boolean isAccountNonLocked() {
        return this.accountNonLocked;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return this.credentialsNonExpired;
    }

    @Override
    public boolean isEnabled() {
        return this.enabled;
    }
    
    
}
```

The fields in the JSON associated with the token are called claims. We want to be able to add custom claims in the JSON. We have to add the following bean in the security config.

```
@Bean
public OAuth2TokenCustomizer<JwtEncodingContext> jwtTokenCustomizer() {
    return (context) -> {
        if (OAuth2TokenType.ACCESS_TOKEN.equals(context.getTokenType())) {
            context.getClaims().claims((claims) -> {
                Set<String> roles = AuthorityUtils.authorityListToSet(context.getPrincipal().getAuthorities())
                        .stream()
                        .map(c -> c.replaceFirst("^ROLE_", ""))
                        .collect(Collectors.collectingAndThen(Collectors.toSet(), Collections::unmodifiableSet));
                claims.put("roles", roles);
                claims.put("userId", ((CustomUserDetails)context.getPrincipal().getPrincipal()).getUserId());
            });
        }
    };
}
```

Now we need to send the token to the productservice. We want to be able to restrict access to some endpoints of productservice to authenticated users or authorized users based on role. So we need to add spring security. 

### Userservice

#### LLD of user service

```mermaid
classDiagram

class User {
  Long id
  String name
  String email
  String hashPassword
  List<Role> roles
}

class Role {
    Long id
    String name
}

class Token {
    Long id
    String value
    User user
    Date expiry
}

```

## Deploying Applications

We want to make the application accessible to the outside world. 

### How Internet Works

How is a browser able to go to google.com and get data from there to show the user. We need a way for the browser to find google.com server and get data from there. An equivalent of address is not enough, we need an equivalent of a location on a map. 

The equivalent of location on a map in the internet is called IP address. IP address allows for efficient routing in the internet. IP address looks like 192.168.1.1, but remembering numbers is hard. It is easier to remember names. 

Taking the example of google.com again, how will the browser know the IP address of google.com? We use a DNS. DNS contains the mapping of domain names and their respective IP addess. We configure the OS to add the DNS IP address. Whenever the browser wants to visit a website, it first goes to the DNS and gets the IP address associated with the domain name. Then the browser goes to the IP address to talk to google.com server. 

IP address has 4 numbers that can take numbers from 0 to 255. Each number can be stored in 8 bits. We have 4 such numbers, so the IP address can be stored in 32 bits. There can be 2^32 unique IP addresses, which is around 2 billion unique addresses. The number of devices connected to the internet is more than the number of unique addresses. 

Inorder to meet the demand, we have network address translators (NAT), a server, which allows us to have an entire internet within the NAT. Each NAT can be configured with a unique IP address that is exposed to the public. But within this IP address, we can have another unique billion IP addresses that is managed by the NAT server. Of course, we can have multiple layers of NAT if required. 

For hosting a website, we need public and static IP address. We also need reliable servers, electricity, internet service providers and geographical distribution of servers. Hosting the website ourselves is possible, but expensive. We can instead use cloud providers to host our website. 

### Managed Infrastructure

In a real production setup, we have to install a database, save backups at frequent intervals, maintain replicas and implement sharding. We would also want to update database version while keeping the system available. We might also want to use Kafka for messaging queue or use Redis for caching etc. Cloud providers offer these services so we dont have to setup these functionalities by ourselves. They will ensure the server is consistently available. 

#### AWS Managed Infrastructure

EC2 stands for elastic compute. Every service of AWS use EC2 behind the scenes. 

Make an account on AWS. Then go to console and click on EC2. Then launch an instance. Name it, then select the OS, then select the specs. Then create an instance. Connect to the instance. SSH into the instance and install all that you need. Then run the app on a port. Make the port visible from the AWS console.

Elastic beanstalk is the manager of our application deployment. It brings together multiple services of AWS to reduce the amount of work we have to do to keep the application running. An application runs on multiple servers. If a server goes down, an alternate server with the application should spin up and serve requests. All the servers are sitting behind a load balancer. The client knows the IP address of the load balancer. The load balancer forwards the requests to the servers so that requests are distributed evenly. 

Elastic beanstalk will manage all of the above functionalities. As a developer, we need to only provide the configuration for these tasks to it. We can tell elastic beanstalk the number of servers, when to add servers, when to reduce servers, the application that is running, the programming language used etc.

Java is a compiled language. We need to package the codebase before deploying it on another computer. We need a jar file of the code base and then we can distribute the application and run it using the command below.

```
java -jar name-of-jar.jar
```

AWS's managed infrastructure for database is called RDS. We need to setup a database server that can be accessed from anywhere.

1. Click on create database. 
2. Select standard create. This allows setting all configuration options including ones for availability, security backups and maintenance. 
3. Select the database engine (MySQL, PostgreSQL etc). 
4. Select versions that support the multi-AZ DB cluster. This way there will be data back up in multiple regions of the world for disaster recovery purposes. AZ stands for avaliability zones. 
5. There will be options to select development, development/test and free tier.
6. Then select a name for the database.
7. Fill in the master username and password fields.
8. As we are in free tier, we will use the free-tier instance as it is the only one available. 
9. There is an option to enable autoscaling based on a criteria. If this criteria is met, additional storage will be added to the database. 
10. In connectivitiy, there are options to connect to an EC2 compute resource or not. We will go with not now because we can add a connection later. We will select IPv4. 
11. Ideally, our database server should accept connection only from our application servers. So we should select No in public access, but for now we will select Yes. Later, we will change the setting.
12. We can enable automated backups, choose frequency of creating back up and window for choosing when to take backup.
13. We can enable encryption data. For now, we will choose to disable encryption.
14. After this, create the database.

Now your database instance is ready.

Now that we have the database url, username and password, we should add it to the application.properties file using environment variables. We can have seperate database instances for production and testing. By adding the database URL using environment variables, we ensure that the URLs of production and testing databases never get mixed up. It also ensures that the URL, username and password cannot be seen even if the codebase is public on Github.

In VSCode, we can add environment variables in Java in the launch.json file. Open the launch.json file and add a key "env" and add the environment variables as values.

```
{
    "configurations": [


            {
                "type": "java",
                "name": "Spring Boot-ProductserviceApplication<productservice>",
                "request": "launch",
                "cwd": "${workspaceFolder}",
                "mainClass": "com.example.productservice.ProductserviceApplication",
                "projectName": "productservice",
                "args": "",
                "envFile": "${workspaceFolder}/.env",
                "env": {
                    "PRODUCT_SERVICE_DATABASE_URL": "Value1",
                    "PRODUCT_SERVICE_DATABASE_USERNAME": "Value2",
                    "PRODUCT_SERVICE_DATABASE_PASSWORD": "Value3",
                    "USER_SERVICE_URL": "Value4"
                }
            }
        ]
}
```

Now package your code to a .jar file.

If you have tests and some of the variables are passed in using environment variables, then we have to pass the environment variables with the maven command. 

```
"path-to-mvnw" package -f "path-to-pom.xml" -DPvar1=value1 -Dvar2=value2 
```

Now, let us set up the elastic beanstalk. Choose application name, domain name and upload the jar file. Then choose custom configuration preset and click on next.

Now, we have to configure service access. Behind the scenes, EBS is a wrapper over other services. We have to give EBS permission to setup the other services required to run the application. In AWS, we give permissions to others via service roles. So we have to 

1. Create a service role
2. Give permission to that service role
3. Allow EBS to take that service role

Creating role is fairly straightforward for EBS. Details and steps are provided in the learn more section.

Now, we can configure logging services, security groups and setup the environment variables and launch the environment. EBS will create the required instances. 

The RDS instances that we created for both userservice and productservice are open for connections from anywhere. We should restrict connections to the RDS instances from only the application servers. We will use the virtual private cloud to achieve this. 

Virtual private cloud is a cloud within a cloud. It is a set of virtual machines that are isolated from others. We can configure an instance so that it accepts connections only from those machines that are in the same VPC as it is. 

Every server has an associated security group. A security group is set of rules that determine from where a server can receive requests and to where a server can send requests. We can create a security group where we can specify that the server will receive requests only from specific security groups. We can assign such a security group to a database server so that the server will accept requests only from another specific server or servers that belong to a particular security group. Such a server or group of servers is called jump server. 

How is rolling deployment done? 

1. Bring one server down. (Remove server from load balancer)
2. Deploy the new version on that server. 
3. Wait for server to work fine.
4. Bring the server up. (Connect it to load server)

How will we know if the server is fine? We use health status check. We have to tell EBS the URL that it can use for health check. When the URL responds with 200, then health is fine. By default EBS sends a request to "/". We can set our own endpoint here to gauge health. In springboot projects, we can use an actuator library to setup an endpoint specifically for this purpose. 

Get a domain from namecheap or godaddy or any other website that sells domains. On AWS, use route53 to create a hosted zone with your domain name. Add the namesevers of these hosted zones in the nameserver settings of the website from where you got the domain. 


## Cache

Storing the copy of data at some other place to speed up overall response to a request. 

Implement redis to cache results of a request for fast response when the same request is made next time.

Build APIs with the goal of serving a request in <10ms. 

What steps should we take to implement stateless APIs?

An example on caching is when browsers store the IP addresses of domains when it is first fetched from DNS. 

In Spingboot, get the dependency for redis. Define a configuration file with a bean, which is a redistemplate, where we define the type of key and value. We can configure other properties in redistemplate as well. 

> https://docs.spring.io/spring-data/redis/reference/redis.html

## Message Queues

In our case, we will be using it to send email after successful signup. We can even send a link to the user's email to verify their account. 

Userservice has to talk to emailservice. One way is to send a post request to the emailservice from the userservice. Another way is to use a message queue. 

Case 1: Uploading a video on youtube.

After uploading youtube has to,

1. Check for copyrights
2. Check for adult content
3. Convert video to different resolutions

and many other jobs before making it visible to public. 

A user would have send the request to upload via an API call. Should the server respond only after all work is done? No, because all the jobs will take a few minutes to finish. So, server sends a response after receiving the video. The jobs on the video will start after that. 

Case 2: Placing an order in Amazon

On placing an order, Amazon should

1. Save to DB.
2. Send email.
3. Send SMS.
4. Send Notification.

and other jobs as well. 

In this case, save to DB is an important step and should happen before we respond with success. The other steps can be delayed and hence we can use message queue to perform these tasks.

The rule of thumb from the two case studies above is that we should do as little as required synchronously. If some jobs can be delayed, delay it. This is the async flow. What is the sync flow? When jobs happen sequentially before sending a response to a request. 

In a messaging queue, there are producers and consumers. The producers put into the queue and the consumers pick from the queue. Consumers will subscribe to the events relevant to it. We can categorize events in the queue into topics. Consumers will subscribe to relevant topics. Producers will put events into relevant topics in the queue. 

The event will remain in the queue till the consumers processes it. If there are multiple instances of a service forming a consumer group, the queue will ensure that the event is consumed by only one instance.  

> Install kafka https://www.conduktor.io/kafka/how-to-install-apache-kafka-on-linux/

> Send email https://www.digitalocean.com/community/tutorials/javamail-example-send-mail-in-java-smtp



## Payment Process

Payment systems are complex because of

1. Security and privacy. 
2. There are a large number of payment methods. Our service also talks to a large number of partners. 
3. There are lot of regulatory requirements. There are thrid party services that certifies that the code of the service satisfies all the regulatory requirements. One such certificate is called PCI-DSS.

Because of such complexity, most organizations prefer to buy third party payment systems. Such systems are called payment gateways.

### Interaction with payment gateway

Should we create the order after payment or before payment. Let us explore the two scenarios.

1. Order created after payment. What happens if payment fails, but money is deducted from the account. In this scenario, we will not know the source of the payment as there is no order. 

2. Order created and then payment. This is industry wide standard because tracking order is possible now. In case payment fails, we know the order the payment is associated with and hence it is easy to refund it. If someone clicks on the pay button twice, then having an order id associated with the payment request lets the server track the duplicate request and cancel it. In this case the order id is the idempotency key. Idempotency key is the attribute that allows us to uniquely identify a requirement and handle duplicates. 

3. Partial Payments. Here payment is split between digital wallet and credit card. This is just an example of one type of partial payment. In this case, even if the wallet payment succeeds, but the card payment fails, it will be easy to handle it because of the order id which is an idempotency key.

Given the above reasons, we should first create the order and then process payment for the order.

We should not call the payment url from the front end. We should first send a request to the orderservice, which will create an order and respond with the order id. Then we send a request with the order id to process payment to the paymentservice. The paymentservice will request the orderservice for the order details. Then the payment service will send a request to the payment gateway which will return a link. Along with the request, the paymentservice will send a callback url that the payment gateway will call when the payment status change. The payment service will return the link to the user in the frontend. The user then opens the link and makes the payment. 

When a payment has succeeded, we may want to do a few things. 

1. Order confirmation email.
2. Generate invoice.
3. Update database.

To do this, the server needs to know that the payment is successful. How will the server know?

The callback url is something like "amazon.in/callback?order_id=123". Upon calling the URL, a request is sent to the orderservice for the payment status. The orderservice will send a request for the payment status to the payment gateway. The response is saved in the orders repository and sent to the frontend to update the user. 

A point of failure here is that the callback url may not get called because of any one of the following reasons. 

1. User closed the tab.
2. User pressed the back button or refreshed the tab.
3. Internet goes down.

It is impossible to avoid all of the above, so we need a solution that will work inspite of the above scenarios. We can use webhooks. Webhook is an API of our server that is called by a third party when a particular event happens. We can add these webhooks to the payment gateway and tell it to call the webhooks when certain events happen. In this case, the events would be payment success or payments failure. Even in the case of callback url failure, the webhook will update the orders repository. 

The webhook has a chance of failure as well. To cover for this, there is a reconciliation process. Every few hours, the payment gateway will send a file with all the transactions that have happened in the last X hours for that organization. 


Floating point numbers or doubles do not store the exact value. They store an approximation of the number. In financial systems, if we store numbers as float/double, it could lead to loss. Hence it is better to store as integer. So, if we have a bill of 10.00, then we have to specify it as 1000.

### Summary

1. User sends request to orderservice - Order is created.
2. User sends request to paymentservice to create payment link. Paymentservice requests orderservice for order details. Using these details, payment link is created. 
3. User goes to payment link and makes the payment. 
4. Then redirected to callback URL. Callback URL requests paymentservice for status of payment. 
5. Payment gateway calls the paymentservice using the webhook url. 

In the paymentservice, we have to implement
1. createPaymentLink
2. getPaymentStatus
3. handleWebhookEvent

## Search APIs

Intuitively, GET request seems like the natural choice for search APIs. However, most of them are implemented using the POST request.

A search will usually have a search term, filters, sort order etc. Adding all this data to the URL will make it large. Older browser versions used to have a limit on the size of the URL. HTTP protocol does not support attaching a request body to a GET request. 

### Pros of GET vs POST

1. Shareable URL. POST request URL will not contain all the data from search term, filters etc
2. Ability to cache search results

### Pros of POST vs GET

1. No size limit concerns.
2. We can easily specify filters and ordering.

### Pagination

Paging - Divide the response into multiple parts. Each part is called a page. 

When we fetch results from the DB, to implement paging, we use offset and limit in the SQL queries. 

In search requests, there are two other parameters. These are the page number and the page size. 

Spring JPA supports pagination. 

```
Page<Product> findAll(Pageable pageable);
```

How do we create an object of Pageable?

```
public Page<Product> getAllProducts(int pageNumber, int pageSize) {
    // TODO Auto-generated method stub
    Page<Product> products = productRepository.findAll(PageRequest.of(pageNumber, pageSize));
    return products;
}
```

We take the pageNumber and pageSize via @RequestParams in the controller. 

### Sorting

By default, results are sorted based on the primary key. We can take in paramaters to sort by the attribute that we want and to sort in the ascending or descending order.

While making an object of Pageable, we can specify a third paramter called sort.by() through which we can pass the attribute that we want to sort by and the order we want to sort by.

An example of using this sort interface.

```
Sort sort = Sort.by("price").ascending().and(Sort.by("name").ascending())
```

The above would first sort the results by price and if two or more have the same price, it will sort those by name in the ascending order.

### Implementing Search APIs

When we implement search APIs, we usually get the filters in the request body in a POST request. We should get the filters as a list of Filter objects that have the properties called attribute and value. 

```
class Filter {
    String attribute;
    Object value;
}
```

The request body will be a JSON like this.

```
[
    {
        "attribute":"brand"
        "value":["addidas", "puma", "nike"]
    },
    {
        "attribute":"priceLow",
        "value":2000
    },
    {
        "attribute":"priceHigh",
        "value":5000
    }
]
```

The controller will take the request body as this.

```
public <returnType> controller(@RequestBody List<Filter>) {}
```

### Elastic Search

MySQL is not the ideal storage for performing fast retreival of results when we have multiple filter parameters. It will match the value of each row to select it and this is not a fast process.

We can use Elastic Search, a document database, for this purpose as it is specially optimized for fast reads, filtering and sorting.


## Possible Interview Questions

Had a mock interview with an interviewer having 19 years of experience. He presented scenarios similar to real-world projects and asked questions on top of it. Throughout the session, he posed questions that delved deeper into these scenarios, Overall the discussion was good and I had new learnings about springboot features.

1. @Autoconfiguration? only definition wont work, you need to know the working , why it is used in springBoot , what it does. (All the discussions were like this only)
2. Spring Boot Peer that relies on Spring Framework - Spring Cloud , Spring WebFlux, Spring Batch etc

3. Spring Boot starters
4. Spring Profiles - spring.profiles.active
5. Swagger
6. Health Monitoring
7. transaction Handling in Spring Boot
8. Dependency Injection
9. Necessity of configuration
10. what is @ComponentScan
11. how to create a customized starter
12. relation between spring and spring boot
13. how framework helps
14. spring actuator
15. how u create scripts in CLI(Command Line Interface)
16. Authentication Vs Authorisation
17. How you use authentication and authorization in spring and name a few annotations to work with spring security.
18. Transactions, Isolation levels, Concurrency
19. optimistic and passive-locking
20. how to create DI when there is a need to create 2 objects for a particular request
21. how to achieve SRP in Springboot

He went from beginner to intermediate to advanced concepts.


Attended the Backend Project Module Mock Interview (Java Spring/Spring Boot) and cleared it. Here are the list of questions which were asked in the interview

1. What is Dependency Injection
2. Diff b/w Spring & SpringBoot
3. Spring Profiles
4. Logging and Levels of Logging
6. What are Beans
7. ⁠What are Annotations
8. Component Scan Annotation
9. Swagger
10. Starting point of Spring Boot Application
11. What is Singleton DP. Are Spring Beans thread safe?
12. Can we create Non-Web applications in Spring Boot?
13. Default Application Server of Spring Boot. Can we replace Apache Tomcat with some other App Server?
14. Flow of API requests in Spring
15. Gave a use case and asked to design the backend API implementation for one particular API request
16. Diff b/w @RequestMapping and @GetMapping
17. Diff b/w @ReqstController and @Controller
18. What are the Build Tools use are aware of?
19. Adding dependencies in Maven

Apart from the topics discussed in the classes, there were few other topics being asked. So it is better to have a basic understanding of those topics as well