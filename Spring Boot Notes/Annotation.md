# Annotation

# What is Annotation?

In Spring Boot, annotations are used to provide metadata and configuration for your application, making it easier to define the behavior and structure of your components.

- `@SpringBootApplication`
    
    ```java
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    
    @SpringBootApplication
    public class FirstApp{
    	public static void main(String[] args){
    		SpringApplication.run(FirstApp.class,args);
    	}
    }
    ```
    
    @SpringBootApplication is a combination of three other annotations used to configure Spring Boot Application: `@EnableAutoConfiguration`, `@ComponentScan`, `@Configuration`.
    
- `@EnableAutoConfiguration`
    
    Enable Spring Boot’s auto-configuration mechanism, which attempts to configure your application automatically based on the jar dependencies you have added.
    
- `@ComponentScan`
    
    By default, it Scans the current java package for all other components of the application and then register them as bean in the Spring context.
    
- `@Configuration`
    
    This designates the class as a source of bean definitions for the application context. It’s part of the Spring Framework’s Java-based configuration support
    
    ```java
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
    import org.springframework.context.annotation.ComponentScan;
    import org.springframework.context.annotation.Configuration;
    
    @Configuration
    @EnableAutoConfiguration
    @ComponentScan
    public class MySpringBootApplication {
        public static void main(String[] args) {
            SpringApplication.run(MySpringBootApplication.class, args);
        }
    }
    ```
    
- `@Component`
    
    Components refers to a class that is managed by the Spring IOC Container. 
    
    They are typically Java objects that perform specific roles within the application and are defined as beans in the Spring context.
    
    ```java
    import org.springframework.stereotype.Component;
    
    @Component
    public class MyComponent {
        public void performTask() {
            System.out.println("Performing a task...");
        }
    }
    ```
    
    Using Component in an Application using context
    
    ```java
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.context.ConfigurableApplicationContext;
    
    @SpringBootApplication
    public class FirstApp{
    	public static void main(String[] args){
    		ConfigureApplicationContext context = SpringApplication.run(FirstApp.class,args);
    		
    		MyComponent component = context.getBean(MyComponent.class);
    		component.performTask();
    	}
    }
    ```
    
    Using Component in an Application using `@AutoWired`
    
    ```java
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.beans.factory.annotation.Autowired;
    
    @SpringBootApplication
    public class FirstApp{
    
    	@AutoWired
    	MyComponent component;
    	public static void main(String[] args){
    		SpringApplication.run(FirstApp.class,args);
    		
    		component.performTask();
    	}
    }
    
    //Getting Error: Cannot make a static reference to the non-static field task
    ```
    
- `@Service`
    
    It is specialized type of `@Component` used in the Service layer. Services typically contains business logic.
    
- `@Repository`
    
    It is specialized type of `@Component` used in data access layer. Repository or Doa is typically responsible for interacting with the database.
    
- `@Controller`
    
    It is specialized type of `@Component` used in the presentation layer. Controllers typically handle web requests and return web responses.
    
- `@RestController`
    
    It is combination of `@Controller` and `@ResponseBody` . They are typically used in the production of REST Api.
    
- @GetMapping, @PostMapping, @PutMapping, @DeleteMapping, @PatchMapping
    
    Used to map HTTP GET, POST, PUT, DELETE, and PATCH request to specific handler methods in a controller, respectively.
    
- `@Autowired`
    
    Used to inject dependencies automatically by type.
    
- `@Value`
    
    Used to inject values from properties files or environment variables into fields, methods, or constructor parameters.
    
- `@Bean`
    
    Indicates that a method produces a bean to be managed by the Spring Container.
    
- `@ConditionalOnProperty`
    
    Configures a bean to be registered in the application context based on the presence of a configuration property.