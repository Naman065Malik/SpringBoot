# Spring Boot

# How Does Spring Application Works?

## Initialization of Spring Boot

The initialization of a Spring Boot application involves the following concise steps:

1. **SpringApplication.run()**: Entry point that launches the application.
2. **Environment Preparation**: Sets up the application environment, including configuration properties and profiles.
3. **Application Context Creation**: Creates the `ApplicationContext` (e.g., `AnnotationConfigApplicationContext`).
4. **Bean Definition Loading**: Scans and registers beans and their dependencies.
5. **Listeners and Initializers**: Registers and invokes application listeners and initializers.
6. **Context Refreshing**: Instantiates, configures, and wires beans; starts the embedded web server if applicable.
7. **Post-Processing**: Executes any `CommandLineRunner` and `ApplicationRunner` beans.
8. **Ready State**: Publishes the `ApplicationReadyEvent` signaling the application is ready to handle requests.

## What is Spring Container?

The Spring container is a core component of the Spring Framework. It is responsible creating and managing the lifecycle and configuration of application objects (Beans). The Container reads configuration metadata to know how to initiate, configure, and assemble these beans. This metadata can be supplied in various forms such as XML, annotations, or Java code.

## Responsibility of IOC Container

- **Instantiating beans**: Creating instances of the beans defined in the configuration.
- **Wiring beans**: Managing dependencies between beans.
- **Configuring beans**: Setting properties and managing initialization and destruction callbacks.
- **Managing the bean lifecycle**: Handling the complete lifecycle of a bean from creation to destruction.

![Untitled](/Assets/IOCContainer.png)

Note: To understand more about setting up configuration metadata read this documentation [What is Spring IOC Container with Example (javaguides.net)](https://www.javaguides.net/2018/10/spring-ioc-container-overview.html)

## Life Cycle of Bean

![Untitled](/Assets/BeanLifecycle.png)

- **Instantiation**
    - The Spring container creates an instance of the bean using the bean definition provided in the configuration metadata (XML, annotations, or Java configuration).
- **Populating Properties**
    - The container injects dependencies into the bean’s properties, either through constructor injection, setter injection, or field injection.
- **Bean Name Aware**
    - If the bean implements `BeanNameAware`, the container calls the `setBeanName` method, passing the bean's ID.
- **Bean Factory Aware**
    - If the bean implements `BeanFactoryAware`, the container calls the `setBeanFactory` method, passing an instance of the `BeanFactory`.
- **Post Processing (Before Initialization)**
    - Beans that implement `BeanPostProcessor` have their `postProcessBeforeInitialization` method called.
    - This allows for custom modification of new bean instances before any initialization callbacks are invoked.
- **Initialization**
    - If the bean implements `InitializingBean`, the container calls the `afterPropertiesSet` method.
    - Custom init-methods specified in the configuration (e.g., `init-method` attribute in XML or `@PostConstruct` annotation) are invoked.
- **Post Processing (After Initialization)**
    - Beans that implement `BeanPostProcessor` have their `postProcessAfterInitialization` method called.
    - This allows for custom modification of new bean instances after any initialization callbacks are invoked.
- **Ready to Use**
    - The bean is now fully initialized and ready for use by the application.
- **Destruction**
    - When the container is shut down, it destroys the beans.
    - If the bean implements `DisposableBean`, the container calls the `destroy` method.
    - Custom destroy-methods specified in the configuration (e.g., `destroy-method` attribute in XML or `@PreDestroy` annotation) are invoked.

## Scope of a Bean

![Untitled](/Assets/BeanScope.png)

- **Singleton**
    - A Single instance of the bean is created and shared across the entire Spring Container.
    - Stateless Service such as `Service` and `Repository` or Shared Resources such as DBConfig have this type of scope.
    - It is the default Scope of Beans in Spring Boot Application
- **Prototype**
    - A new instance of a bean is created every time it is requested from container.
    - It is used for `Short-Lived Tasks`
    - Components that are used temporarily and discarded, like data processing objects.
    - `@Scope('prototype')` is used to configure bean of this scope
- **Request**
    - A new instance of a bean is created for each HTTP request such as `request parameters`. Available only in a web-aware Spring application context.
    - `@RequestScope` is used to configure bean of this scope.
- **Session**
    - A new instance of the bean is created for each HTTP session. Available only in a web-aware Spring application context.
    - Example → `shopping cart items` , `user preferences` or `session-specific` settings.
    - It is also used in the Components that handle `user authentication` and maintain user session information.
    - `@SessionScope` is used to configure bean of this scope.
- **Application**
    - A single instance of the bean is created for the lifecycle of a `ServletContext` only in a web-aware Spring application context.
    - Beans that hold configuration or data shared across entire application, such as application settings or constants.
    - `@ApplicationScope` is used to configure bean of this scope.
- **WebSocket**
    - `@WebSocketScope` is used to configure bean of this scope.
    - A new instance of the bean is created of each `WebSocket session`. Available only in web-aware Spring application context.
    - Beans that maintain state for the duration of a WebSocket session, such as `connection details` or `session-specific details`
    - Components involved in handling real-time WebSocket connections and maintain state for each session.