# Spring Controllers

## What is a Controller?

A controller is a class or method in our Java code that is the destination point a web request can reach. There are 4 basic types of endpoints within controllers: GET, PUT, POST, and DELETE (or CRUD: Create, Receive, Update, and Delete). 

Let us understand what are standard CRUD Operations:

- *POST*: Creates a new resource
- *GET*: Reads/Retrieve a resource
- *PUT*: Updates an existing resource
- *DELETE*: Deletes a resource

CRUD is data-oriented and the standardized use of HTTP methods

****
## Basic Controller Setup

From within the folder that contains your Main class create a new folder names ```controllers```. Within that folder create a new java file called ```Hello.java```.
```java
// your package will be unique to your application
package foo.bar.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class Hello{

    @GetMapping("/hello")
    public String hello(){
        return "Hello World."
    }
}
```
With this controller we can start our app using ```gradle bootRun```. Once the app has started, on port 8080, you can open a browser and navigate to ```localhost:8080/hello``` or ```127.0.0.1:8080/hello``` and you can see the return string from our controller. 

This works good and well with a just strings, but we will need to return objects, JSON, so we can properly consume the data. 
