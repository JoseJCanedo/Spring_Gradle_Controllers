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

Lets start by creating a folder at the same level as your controller folder called ```domain```. The order should look like this:
```
|- controllers
|- domain
|- main.java
```

And within domain we will be creating a new file called ```Greeting.java``` which will contain the following:
```java
package com.example.demo.domain;

public record Greeting(long id, String content) { }
```
This will be our return object. It will return a integer and a string object. 

We need to also make some updates to the controller
```java
package com.example.demo.controller;

import com.example.demo.domain.Greeting;
import java.util.concurrent.atomic.AtomicLong;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class Hello {

    @GetMapping("/hello")
    public String Hello(){
        return "hello";
    }

    private static final String template = "Hello, %s!";
	private final AtomicLong counter = new AtomicLong();

	@GetMapping("/greeting")
	public Greeting greeting(@RequestParam(value = "name", defaultValue = "World") String name) {
		return new Greeting(counter.incrementAndGet(), String.format(template, name));
	}
}
```

In this new controller code we have added a new route, */greeting* and it will return a new Greeting object with a counter, how many times the URL has been hit, and the possibility to return a variable. We are in this case passing a query parameter. There are two ways to pass data from the request to the Java backend. 
- Query variable: seen as a key value pair after a ```? (question mark)``` in the URL. These are optional, but can be required.
- Path variable: as part of the URL. Usualy embedded within the URL itself. These are required

Start the app after the changes and navigate to the /greetings URL. You should see
```json
{
    "id": 1,
    "content": "Hello, World!"
}
```
If we modify the URL and go to ```greeting?name=john``` we should instead see this
```json
{
    "id": 1,
    "content": "Hello, john!"
}
```
In the code above the annotation for request parameters we give it the name of the key "name" and a default value, hence we have "World" in there and we assign it a type "String" followed by a name for the variable "name". 

Lets copy the route for the controller and make it a path variable instead of query for the name and add it below the greeting controller:
```java
    @GetMapping("/greeting/{name}")
	public Greeting greetingPath(@PathVariable String name) {
		return new Greeting(counter.incrementAndGet(), String.format(template, name));
	}
```
Youll have to add the import for ```PathVariable```, but youll notice I didnt add a default parameter, because if it is null we will default to the other path. Youll also notice the URL changed and we gave the variable a name in the path. Start up you app and navigate to ```greeting/john``` and youll get the same response as before, however the name is not mandatory.

For all our HTML methods we can return objects to the requester like this as a response object and we can pass variables from the request to the application using the path and query variables. We will cover the other way to pass information, within the body, when we cover POST and PUT methods. However, before we do that we either need a UI or a tool like [Postman](https://www.postman.com).
