# Spring Web

Build web, including RESTful, applications using Spring MVC. Uses Apache Tomcat as the default embedded container.

# What Does Spring Web Provides?

The Spring Web Starter dependency is part of the Spring Boot project and is designed to facilitate the development of web applications. When you include the Spring Web Starter dependency in your project, it provides a comprehensive set of libraries and frameworks that are essential for building web applications, including RESTful APIs.

## **Core Components**
1. **Spring MVC**
   1. The core web framework that provides model-view-controller (MVC) architecture and RESTful web services.
   2. Enable building web applications and RESTful services.
2. **Embedded Servlet Container**
   1. By default, includes Tomcat, but you can switch to Jetty or Undertow.
   2. Allows you to run your application as a standalone application without requiring an external server.
## **Key Features**
1. **RESTful API Development**
   1. Annotations like `@RestController`, `@RequestMapping`, `@GetMapping`, `@PostMapping` , etc., to easily create REST endpoints.
2. **Spring MVC Annotations**
   1. Annotations like `@Controller`, `@ResponseBody`, `@PathVariable`, `@RequestParam`, `@RequestBody`, etc., to handle web requests and responses.
3. **Static Content Support**
   1. Automatically serves static content like HTML, CSS, and JavaScript from directories such as `/static`, `/public, `/resources`` m and `/META-INF/resources` .
    

# StudentTrack

In this tutorial, we will be building StudentTrack to understand how RESTful applications are made using Spring Web. StudentTrack is a simple project designed to manage student records, demonstrating key concepts such as creating REST endpoints, handling HTTP requests and responses, and performing CRUD operations using Spring Boot and Spring Data JPA. Through this hands-on example, you'll gain a solid foundation in developing RESTful web services with the Spring Framework.

# Base Code

## Spring Boot Entry Point

As we know Spring Boot Initializer creates a base code template for the application in the `/src/main/java` folder that is given below:

```java
package com.example.StudentTrack;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class StudentTrackApplication {

	public static void main(String[] args) {
		SpringApplication.run(StudentTrackApplication.class, args);
	}

}
```

In this code, `@SpringBootApplication` Annotation automatically set all the configurations required for the application to run whereas 

[`SpringApplication.run`](http://SpringApplication.run) method creates an appropriate `ApplicationContext` instance, configures and registeres all the `beans` into the Context [Container].

### General Student Class

I have saved this file in `/Model` folder

```java
package com.example.StudentTrack.Model;

import org.springframework.stereotype.Component;

public class Student {
    private Long id;
    private String name;
    private String email;
    private String course;

    // Constructors
    public Student() {}

    public Student(Long id, String name, String email, String course) {
        this.id = id;
        this.name = name;
        this.email = email;
        this.course = course;
    }

    // Getters and Setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }

    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }

    public String getCourse() { return course; }
    public void setCourse(String course) { this.course = course; }

    @Override
    public String toString() {
        return "Student{id=" + id + ", name='" + name + '\'' + ", email='" + email + '\'' + ", course='" + course + '\'' + '}';
    }
}
```

In this code, `@Component` registers this class as bean during the initialization of the container or context. Further in the tutorial, we will be learning how to inject this bean as a dependency in any other Component of the application.

**Note: For classes that will be managed by Container, should have a default Constructor in its code**

### Basic Rest Controller

we will use `@RestController` annotation to describe our first Controller.

In this code we are initializing a `Hash Map` to save the data of students in key-value Pair format. In further tutorials we will be using JPA and ORM such as Hibernate to save this data in a database.

`@GetMapping` and `@PostMapping` is used to create `/student` route which will help us to create new Student data as well as in return all the students. 

`@RequestBody` is used to extract the body of request. It maps the Json body into parameter a function.

```java
package com.example.StudentTrack.Controller;

import java.util.HashMap;
import java.util.Map;

import com.example.StudentTrack.Model.Student;

import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.DeleteMapping;

@RestController
public class StudentController {

    private Map<Long, Student> studentRepo = new HashMap<>();

    @GetMapping("/Student")
    public Map getAllStudent(){
        return studentRepo;
    }

    @PostMapping("/Student")
    public String createStudent(@RequestBody Student student) {
        studentRepo.put(student.getId(), student);
        return "Student added to the database";
    }
}
```

### Other HTTP Methods

Going Deeper into our RestController by learning about other important HTTP Methods

```java
package com.example.StudentTrack.Controller;

import java.util.HashMap;
import java.util.Map;

import com.example.StudentTrack.Model.Student;

import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.DeleteMapping;

@RestController
public class StudentController {

    private Map<Long, Student> studentRepo = new HashMap<>();

    @GetMapping("/Student")
    public Map getAllStudent(){
        return studentRepo;
    }

    @PostMapping("/Student")
    public String createStudent(@RequestBody Student student) {
        studentRepo.put(student.getId(), student);
        return "Student added to the database";
    }

    @GetMapping("/getStudent")
    public Student getStudentById(@RequestParam Long studentId) {
        Student response = studentRepo.get(studentId);
        return response;
    }

    @PutMapping("/Student/{id}")
    public Student putUpdateStudent(@PathVariable Long id, @RequestBody Student student) {    
        Student response = studentRepo.put(id, student);
        return response;
    }

    @DeleteMapping("/Student/{id}")
    public Map deleteStudent(@PathVariable Long id){
        studentRepo.remove(id);
        return studentRepo;
    }
}
```

### Difference Between Path Variable and Request Param

| @PathVariable (/Student/{id}) | @RequestParam (/Student?id=1) |
| --- | --- |
| It is used to extract query parameters from URL path. | It is used to extract query parameters directly from URL. |
| It maps the URL path variable directly to method parameters. | It binds query String Parameters from the URL to method parameters. |
| Typically used in RESTful web services to capture dynamic values from the URL. | Commonly used to capture values from query strings in URLs. |

## Initializing studentRepo as a Bean

```java
package com.example.StudentTrack;

import java.util.HashMap;
import java.util.Map;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

import com.example.StudentTrack.Model.Student;

@SpringBootApplication
public class StudentTrackApplication {

	public static void main(String[] args) {
		SpringApplication.run(StudentTrackApplication.class, args);
	}

	@Bean
    public Map<Long, Student> studentRepo() {
        return new HashMap<>();
    }

}
```

In this code `@Bean` registers studentRepo as a Bean.

### Injecting bean into the Controller

Here are the different ways through which you can inject `studentRepo` in `StudentController` 

```java
package com.example.StudentTrack.Controller;

import java.util.HashMap;
import java.util.Map;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class StudentController {

	// 1. By Using Annotation
    @Autowired
    private Map<Long, Student> studentRepo;
    
  // 2. Using Contructor
    private Map<Long, Student> studentRepo;
    
    public StudentController( Map<Long, Student> studentRepo){
        this.studentRepo = studentRepo;
    }
 
}
```

### Advantages of using studentRepo as bean

we can use this same instance of `studentRepo` in `AnotherController` 

```java
package com.example.StudentTrack.Controller;

import java.util.Map;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RestController;

import com.example.StudentTrack.Model.Student;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;

@RestController
public class AnotherController {
    
    @Autowired
    Map<Long, Student> studentRepo;

    @GetMapping("/getName")
    public String getStudentName(@RequestParam Long id) {
        Student student = studentRepo.get(id);
        return student.getName();
    }
}
```

## Going Deeper into RESTful Response

Till now, we have developed an Api which can perform all CRUD operations with the Student Data, But Still we cannot consider it RESTful as it does not return data in correct manner.

**Problem:** Currently our Api cannot be able to inform us about the conditions such as Not Found.

```java
@RestController
public class StudentController {

		@Aotowired
    private Map<Long, Student> studentRepo;

    @GetMapping("/getStudent")
    public Student getStudentById(@RequestParam Long studentId) {
        Student response = studentRepo.get(studentId);
        if(response == null){
		        return "Student Not Found";
		    // Now Here we will get Error: cannot convert from String to Student
		    }
        return Student;
    }
}
```

As Response type of our `getStudentByID` method is Student It will not be able to return a String saying, “Student Not Found”.

**Solution:** Now we can Wrap all of our responses in `ResponseEntity` Object.

```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;

@RestController
public class StudentController {

		@Aotowired
    private Map<Long, Student> studentRepo;

    @GetMapping("/getStudent")
    public ResponseEntity<?> getStudentById(@RequestParam Long studentId) {
        Student response = studentRepo.get(studentId);
        if(response == null){
            return new ResponseEntity<>("Student Not Found",HttpStatus.NOT_FOUND);
        }
        return new ResponseEntity<>(response,HttpStatus.OK);
    }
}
```

It will also help us to return Correct Http Status code as well.

[HTTP Status Codes](/Important_Topic/StatusCode.md)

### Student Controller

```java
package com.example.StudentTrack.Controller;

import java.util.Map;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import com.example.StudentTrack.Model.Student;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.PathVariable;

@RestController
public class StudentController {

    @Autowired
    private Map<Long, Student> studentRepo;

    @GetMapping("/Student")
    public ResponseEntity<Map<Long, Student>> getAllStudent(){
        return new ResponseEntity<>(studentRepo,HttpStatus.OK);
    }

    @PostMapping("/Student")
    public ResponseEntity<String> createStudent(@RequestBody Student student) {
        if(studentRepo.containsKey(student.getId()))
            return new ResponseEntity<>("Student already exists", HttpStatus.CONFLICT);
            
        studentRepo.put(student.getId(), student);
        return new ResponseEntity<>("Student added to the database", HttpStatus.CREATED);
    }

    @GetMapping("/getStudent")
    public ResponseEntity<?> getStudentById(@RequestParam Long studentId) {
        Student response = studentRepo.get(studentId);
        if(response == null){
            return new ResponseEntity<>("Student Not Found",HttpStatus.NOT_FOUND);
        }
        return new ResponseEntity<>(response,HttpStatus.OK);
    }

    @PutMapping("/Student/{id}")
    public ResponseEntity<?> putUpdateStudent(@PathVariable Long id, @RequestBody Student student) {    
        
        if(studentRepo.containsKey(id))
            return new ResponseEntity<>("Student Not Found", HttpStatus.NOT_FOUND);

				student.setId(id);
        Student response = studentRepo.put(id, student);
        return new ResponseEntity<>(response,HttpStatus.OK);
    }

    @DeleteMapping("/Student/{id}")
    public ResponseEntity<?> deleteStudent(@PathVariable Long id){

        if(studentRepo.containsKey(id))
            return new ResponseEntity<>("Student Not Found", HttpStatus.NOT_FOUND);

        studentRepo.remove(id);
        return new ResponseEntity<>(HttpStatus.NO_CONTENT);
    } 
}
```

**Note: Above Code does not contain best practices for handling cases such as User Not Found, In Further chapters we will learn about Exception Handling which will handle these cases.**
