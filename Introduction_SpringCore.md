# Introduction to Spring Core

## Introduction and overview

> The Spring Framework provides a comprehensive programming and configuration model for modern Java-based enterprise applications on many kind of deployment platforms.

The goal of this document is to offer you an easy and quick way to get started with Spring. We will focus on the basic core concepts of Spring. Following topics will be covered:

**TODO - Check subtitles**

* What is the Spring framework?
* Spring Core, the basics
  * Why Spring?
  * Inversion of control
  * Dependency Injection
  * Spring IOC container overview
  * Beans, what are they and what do they do?
  * Bean scopes
    * Bean lifecycle
    * Inheritance
  * Bean annotations
  * @Autowired
    * Wiring of beans
    * Qualifier
  * Components, services repositories
* Extras
  * (optional): Properties and injecting them @Value
  * (Optional) Profiles 
  * (Optional) Scheduled tasks

---

## What is the Spring framework?

Spring is a modular system, allowing you to pick and choose which modules are applicable to you when building your applications, without having to bring in the rest. The following section provides details about all the modules available in Spring Framework.

![Spring Framework Overview](https://github.com/tvanwinckel/intro-spring-core/blob/main/images/SpringFrameworkOverview.png?raw=true "Spring Framework Overview")

### Core container

This will be the focus of todays presentation. The Core Container consists of the Core, Beans, Context, and Expression Language modules:

* The **Core** module provides the fundamental parts of the framework, including the Inversion of Control (IoC) and Dependency Injection features (DI).
* The **Bean** module provides BeanFactory, which is a sophisticated implementation of the factory pattern.
* The **Context** module builds on the solid base provided by the Core and Beans modules and it is a medium to access any objects defined and configured. The ApplicationContext interface is the focal point of the Context module.
* The **SpEL** module provides a powerful expression language for querying and manipulating an object graph at runtime. (Will be the subject of later Spring presentations, Spring Data?)

### Data Access / Data Integration

The Data Access/Integration layer consists of the JDBC, ORM, OXM, JMS and Transaction modules:

* The **JDBC** module provides a JDBC-abstraction layer that removes the need for tedious JDBC related coding.
* The **ORM** module provides integration layers for popular object-relational mapping APIs, including JPA, JDO, Hibernate, and iBatis.
* The **OXM** module provides an abstraction layer that supports Object/XML mapping implementations for JAXB, Castor, XMLBeans, JiBX and XStream.
* The **Java Messaging Service (JMS)** module contains features for producing and consuming messages.
* The **Transaction** module supports programmatic and declarative transaction management for classes that implement special interfaces and for all your POJOs.

### Web

The Web layer consists of the Web, Web-MVC, Web-Socket, and Web-Portlet modules:

* The **Web** module provides basic web-oriented integration features such as multipart file-upload functionality and the initialization of the IoC container using servlet lis 
* The **Web-MVC** module contains Spring's Model-View-Controller (MVC) implementation for web applications.
* The **Web-Socket** module provides support for WebSocket-based, two-way communication between the client and the server in web applications.
* The **Web-Portlet** module provides the MVC implementation to be used in a portlet environment and mirrors the functionality of Web-Servlet module.

### Miscellaneous

There are few other important modules like AOP, Aspects, Instrumentation, Web and Test modules the details of which are as follows:

* The **AOP** module provides an aspect-oriented programming implementation allowing you to define method-interceptors and pointcuts to cleanly decouple code that implements functionality that should be separated.
* The **Aspects** module provides integration with AspectJ, which is again a powerful and mature AOP framework.
* The **Instrumentation** module provides class instrumentation support and class loader implementations to be used in certain application servers.
* The **Messaging** module provides support for STOMP as the WebSocket sub-protocol to use in applications. It also supports an annotation programming model for routing and processing STOMP messages from WebSocket clients.
* The **Test** module supports the testing of Spring components with JUnit or TestNG frameworks.

---

## Spring Core

So why do we need a framework after all? To be fair, it is not really necessary to use a framework. But, it is often advisable to use one for several reasons:

* Helps you focus on the core task rather than the boilerplate associated with it.
* Brings together years of knowledge in the form of design patterns.
* Helps you adhere to the industry and regulatory standards.
* Brings down the total cost of ownership for the application.

But it can't be all positives, so what's the catch:

* Forces you to write an application in a specific manner.
* Binds to a specific version of language and libraries.
* Adds to the resource footprint of the application.

### Inversion Of Control

![Wikipedia IOC definition](https://github.com/tvanwinckel/intro-spring-core/blob/main/images/WikipediaIOCDefinition.jpg?raw=true "Wikipedia IOC definition")

Inversion of Control is a principle in software engineering which transfers the control of objects or portions of a program to a container or framework. We most often use it in the context of object-oriented programming.

In contrast with traditional programming, where our custom code makes calls to a library, IoC enables a framework to take control of the flow of a program and make calls to our custom code. To enable this, frameworks use abstractions with additional behavior built in. If we want to add our own behavior, we need to extend the classes of the framework or plugin our own classes.

The advantages of this architecture are:

* Decoupling the execution of a task from its implementation.
* Making it easier to switch between different implementations.
* Greater modularity of a program.
* Greater ease in testing a program by isolating a component or mocking its dependencies, and allowing components to communicate through contracts.

We can achieve Inversion of Control through various mechanisms such as: Strategy design pattern, Service Locator pattern, Factory pattern, and **Dependency Injection (DI).**

**TODO Add some articles to the other methods like service locator?**

### Dependency Injection

Dependency injection is a technique in which an object or function receives other objects or functions that it requires, as opposed to creating them internally. Dependency injection aims to separate the concerns of constructing objects and using them, leading to loosely coupled programs.

```java
public class GhostJar {
    private final Skull skull;
 
    public GhostJar() {
        this.skull = new SkullImpl();    
    }
}
```

In the example above, we need to instantiate an implementation of the ``Skull`` interface within the ``GhostJar`` class itself. By using DI, we can rewrite the example without specifying the implementation of the Skull that we want:

```java
public class GhostJar {
    private final Skull skull;

    public GhostJar(final Skull skull) {
        this.skull = skull;
    }
}
```

### The Spring IOC Container

An IoC container is a common characteristic of frameworks that implement IoC. In the Spring framework, the interface ``ApplicationContext`` represents the IoC container. The Spring container is responsible for instantiating, configuring and assembling objects known as beans, as well as managing their life cycles. 

The container gets its instructions on what objects to instantiate, configure, and assemble by reading configuration metadata. The configuration metadata is represented in XML, Java annotations, or Java code. It lets you express the objects that compose your application and the dependencies between those objects.

The following diagram shows a high-level view of how Spring works. Your application classes are combined with configuration metadata so that, after the ApplicationContext is created and initialized, you have a fully configured and executable system or application.

![IOC overview](https://github.com/tvanwinckel/intro-spring-core/blob/main/images/IOCoverview.png?raw=true "IOC overview")

```java
@Configuration
public class GhostConfig {

    @Bean
    public GhostJar ghostJar(final Ghost ghost) {
        return new GhostJar(ghost);
    }

    @Bean
    public Ghost ghost() {
        return new AngrySkull();
    }
}
```

### Spilling the Beans?

https://www.baeldung.com/spring-bean

In Spring, the objects that form the backbone of your application and that are managed by the Spring IoC container are called beans. A bean is an object that is instantiated, assembled, and otherwise managed by a Spring IoC container. **A Spring IoC container manages one or more beans**. These beans are created with the configuration metadata that you supply to the container. Within the container itself, these bean definitions are represented as BeanDefinition objects, which contain (among other information) the following metadata:

* A package-qualified class name: typically, the actual implementation class of the bean being defined.
* Bean behavioral configuration elements, which state how the bean should behave in the container (scope, lifecycle callbacks, and so forth).
* References to other beans that are needed for the bean to do its work. These references are also called collaborators or dependencies. 
* Other configuration settings to set in the newly created object — for example, the size limit of the pool or the number of connections to use in a bean that manages a connection pool.

#### Naming Beans

Every bean has one or more identifiers. These identifiers must be unique within the container that hosts the bean. A bean usually has only one identifier. However, if it requires more than one, the extra ones can be considered aliases.

```java    
@Bean
public Agent lucy() {
    return new Agent("Lucy Carlyle");
}

@Bean
public Agent george() {
    return new Agent("George Cubbins");
}

@Bean(name = "lockwood")
public Agent anthony() {
    return new Agent("Anthony Lockwood");
}
```

> The convention is to use the standard Java convention for instance field names when naming beans. That is, bean names start with a lowercase letter and are camel-cased from there. Examples of such names include accountManager, accountService, userDao, loginController, and so forth.
Naming beans consistently makes your configuration easier to read and understand. Also, if you use Spring AOP, it helps a lot when applying advice to a set of beans related by name.

### Bean Scopes

When you create a bean definition, you create a recipe for creating actual instances of the class defined by that bean definition. The idea that a bean definition is a recipe is important, because it means that, as with a class, you can create many object instances from a single recipe.

You can control not only the various dependencies and configuration values that are to be plugged into an object that is created from a particular bean definition but also control the scope of the objects created from a particular bean definition. This approach is powerful and flexible, because you can choose the scope of the objects you create through configuration instead of having to bake in the scope of an object at the Java class level. Beans can be defined to be deployed in one of a number of scopes. The Spring Framework supports six scopes, four of which are available only if you use a web-aware ApplicationContext.

* Singleton
* Prototype
* Request, Session, Application & Websocket

#### Singleton

When we define a bean with the singleton scope, the container creates a single instance of that bean; all requests for that bean name will return the same object, which is cached. Any modifications to the object will be reflected in all references to the bean. This scope is the default value if no other scope is specified.

![Singleton Scope](https://github.com/tvanwinckel/intro-spring-core/blob/main/images/SingletonScope.png?raw=true "Singleton Scope")

```java
@Bean
@Scope("singleton")
public Ghost ghost() {
    return new AngrySkull();
}

// OR

@Bean
@Scope(value = ConfigurableBeanFactory.SCOPE_SINGLETON)
public Ghost ghost() {
    return new AngrySkull();
}
```

#### Prototype

A bean with the prototype scope will return a different instance every time it is requested from the container. It is defined by setting the value prototype to the @Scope annotation in the bean definition.

![Prototype Scope](https://github.com/tvanwinckel/intro-spring-core/blob/main/images/PrototypeScope.png?raw=true "Prototype Scope")

```java
@Bean
@Scope("prototype")
public Ghost ghost() {
    return new RandomTypeTwoGhost();
}

// OR

@Bean
@Scope(value = ConfigurableBeanFactory.SCOPE_PROTOTYPE)
public Ghost ghost() {
    return new RandomTypeTwoGhost();
}
```

#### Request, Session, Application & Websocket

The Request, Session, Application & Websocket copes are available only if you use a web-aware Spring ApplicationContext.

**Request**

The Spring container creates a new instance of the LoginAction bean by using the loginAction bean definition for each and every HTTP request. 

```java
@RequestScope
@Component
public class LoginAction {
	// ...
}
```

**Session**

The Spring container creates a new instance of the UserPreferences bean by using the userPreferences bean definition for the lifetime of a single HTTP Session. In other words, the userPreferences bean is effectively scoped at the HTTP Session level. As with request-scoped beans, you can change the internal state of the instance that is created as much as you want, knowing that other HTTP Session instances that are also using instances created from the same userPreferences bean definition do not see these changes in state, because they are particular to an individual HTTP Session. When the HTTP Session is eventually discarded, the bean that is scoped to that particular HTTP Session is also discarded.

```java
@SessionScope
@Component
public class UserPreferences {
	// ...
}
```

**Application**

The Spring container creates a new instance of the AppPreferences bean by using the appPreferences bean definition once for the entire web application. That is, the appPreferences bean is scoped at the ServletContext level and stored as a regular ServletContext attribute. This is somewhat similar to a Spring singleton bean but differs in two important ways: It is a singleton per ServletContext, not per Spring ApplicationContext (for which there may be several in any given web application), and it is actually exposed and therefore visible as a ServletContext attribute.

```java
@ApplicationScope
@Component
public class AppPreferences {
	// ...
}
```

### Bean Lifecycle

 When a bean is instantiated, it may be required to perform some initialization to get it into a usable state. Similarly, when the bean is no longer required and is removed from the container, some cleanup may be required.

 To define setup and teardown for a bean, we simply declare the bean with initmethod and/or destroy-method parameters. The init-method attribute specifies a method that is to be called on the bean immediately upon instantiation. Similarly, destroymethod specifies a method that is called just before a bean is removed from the container.

 #### Initialization

 The ``org.springframework.beans.factory.InitializingBean`` interface specifies a single method, you can simply implement this interface and initialization work can be done inside ``afterPropertiesSet()`` method as follows:

 ```java
 public class Agency implements InitializingBean {
    
    private final List<Agent> agents = new ArrayList<>();

    @Override
    public void afterPropertiesSet() throws Exception {
        agents.add(new Agent("Lucy"));
    }
}
 ```

 #### Destruction

 The ``org.springframework.beans.factory.DisposableBean`` interface specifies a single method, you can simply implement the above interface and finalization work can be done inside ``destroy()`` method as follows:

 ```java
public class Agency implements DisposableBean {

    private final List<Agent> agents = new ArrayList<>();

    @Override
    public void destroy() throws Exception {
        agents.clear();
    }
}
 ```

 ### Bean Annotations

 In this chapter, we will discuss the most common Spring bean annotations used to define different types of beans. There are several ways to configure beans in a Spring container. Firstly, we can declare them using XML configuration. But we can also declare beans using the ``@Bean annotation in a configuration class``.

Finally, we can mark the class with one of the annotations from the ``org.springframework.stereotype package``, and leave the rest to ``component scanning``.

#### Component Scanning

Spring can automatically scan a package for beans if component scanning is enabled. ``@ComponentScan`` configures which packages to scan for classes with annotation configuration. We can specify the base package names directly with one of the ``basePackages`` as value arguments

```java
@Configuration
@ComponentScan(basePackages = "com.the.problem")
class GhostConfig {}
```

Also, we can point to classes in the base packages with the ``basePackageClasses`` argument: 

```java
@ComponentScan(basePackageClasses = GhostConfig.class)
``` 

Alternatively, we can use ``@ComponentScans`` to specify multiple @ComponentScan configurations:

```java
@Configuration
@ComponentScans({ 
  @ComponentScan(basePackages = "com.the.problem"), 
  @ComponentScan(basePackageClasses = GhostConfig.class)
})
class GhostConfig {}
```

#### @Component

``@Component`` is a class level annotation. During the component scan, Spring Framework automatically detects classes annotated with ``@Component``:

```java
@Component
public class Agency {

    private final List<Agent> agents;

    public Agency(final List<Agent> agents) {
        this.agents = agents;
    }
}
```

By default, the bean instances of this class have the same name as the class name with a lowercase initial. In addition, we can specify a different name using the optional value argument of this annotation.

Since ``@Repository``, ``@Service``, ``@Configuration``, and ``@Controller`` are all meta-annotations of ``@Component``, they share the same bean naming behavior. Spring also automatically picks them up during the component scanning process.

#### @Repository

Repository classes usually represent the database access layer in an application, and should be annotated with ``@Repository``:

```java
@Repository
class GhostRepository {
    // ...
}
```

One advantage of using this annotation is that it has automatic persistence exception translation enabled. When using a persistence framework, such as Hibernate, native exceptions thrown within classes annotated with @Repository will be automatically translated into subclasses of Spring's DataAccessException.

#### @Service

The business logic of an application usually resides within the service layer, so we will use the ``@Service`` annotation to indicate that a class belongs to that layer:

```java
@Service
public class AgencyService {
    // ...    
}
```

#### @Controller

``@Controller`` is a class level annotation, which tells the Spring Framework that this class serves as a controller in Spring MVC:

```java
@Controller
public class GhostController {
    // ...
}
```

#### @Configuration

``Configuration classes`` can contain bean definition methods annotated with ``@Bean``:

```java
@Configuration
public class GhostConfig {

    @Bean
    public GhostJar ghostJar(final Ghost ghost) {
        return new GhostJar(ghost);
    }

    @Bean
    public Ghost ghost() {
        return new AngrySkull();
    }
}
```

### Autowired

Starting with Spring 2.5, the framework introduced annotations-driven Dependency Injection. The main annotation of this feature is ``@Autowired``. It allows Spring to resolve and inject collaborating beans into our bean.

The Spring framework enables automatic dependency injection. In other words, by declaring all the bean dependencies in a Spring configuration file, Spring container can autowire relationships between collaborating beans. This is called Spring bean autowiring.

To use Java-based configuration in our application, let's enable annotation-driven injection to load our Spring configuration:

```java
@Configuration
@ComponentScan(basePackages = "com.the.problem")
class GhostConfig {}
```

Moreover, Spring Boot introduces the ``@SpringBootApplication`` annotation. This single annotation is equivalent to using ``@Configuration``, ``@EnableAutoConfiguration``, and ``@ComponentScan``.

```java
@SpringBootApplication
public class TheProblemApplication {
    public static void main(String[] args) {
        SpringApplication.run(TheProblemApplication.class, args);
    }
}
```

As a result, when we run this Spring Boot application, it will automatically scan the components in the current package and its sub-packages. Thus it will register them in Spring's Application Context, and allow us to inject beans using @Autowired.

#### Using Autowired on Properties

Let’s see how we can annotate a property using @Autowired. This **eliminates the need for getters and setters**. First, let's define a bean:

```java
@Component("fittes")
public class FittesAgency {

    public String getName() {
        return "The Fittes Agency";
    }
}
```

Then, we'll inject this bean into the AgencyService bean using ``@Autowired`` on the field definition. And as a result, Spring injects ``fittes`` when ``AgencyService`` is created.

```java
@Service
public class AgencyService {

    @Autowired
    private FittesAgency fittesAgency;
}
```

#### Using Autowired on Setters

Now let's try adding @Autowired annotation on a **setter method**. In the following example, the setter method is called with the instance of FittesAgency when AgencyService is created:

```java
@Service
public class AgencyService {

    private FittesAgency fittesAgency;

    @Autowired
    public void setFittesAgency(final FittesAgency agency){
        this.fittesAgency = agency;
    }
}
```

#### Using Autowired on Constructors

Finally, let's use @Autowired on a **constructor**. We'll see that an instance of FittesAgency is injected by Spring as an argument to the AgencyService constructor:

```java
@Service
public class AgencyService {

    private final FittesAgency fittesAgency;

    @Autowired
    public AgencyService(final FittesAgency fittesAgency) {
        this.fittesAgency = fittesAgency;
    }
}
```

#### Autowired and Optional Dependencies

When a bean is being constructed, the @Autowired dependencies should be available. Otherwise, if Spring cannot resolve a bean for wiring, it will throw an exception. Consequently, it prevents the Spring container from launching successfully with an exception of the form:

```text
Caused by: org.springframework.beans.factory.NoSuchBeanDefinitionException: 
No qualifying bean of type [com.theproblem.NoBeanMadeForThisClass] found for dependency: 
expected at least 1 bean which qualifies as autowire candidate for this dependency. 
Dependency annotations: 
{@org.springframework.beans.factory.annotation.Autowired(required=true)}
```

To fix this, we need to declare a bean of the required type:

```java
@Service
public class AgencyService {

    @Autowired(required = false)
    private FittesAgency fittesAgency;
}
```

#### Alternatives to Autowired

There are mainly three annotations, two of the three annotations belong to the Java extension package: ``javax.annotation.Resource`` and ``javax.inject.Inject``. The ``@Autowired`` annotation belongs to the ``org.springframework.beans.factory.annotation package``.

**Resource**

The ``@Resource`` annotation is part of the JSR-250 annotation collection, and is packaged with Jakarta EE. This annotation has the following execution paths, listed by precedence:

* Match by Name
* Match by Type
* Match by Qualifier

These execution paths are applicable to both setter and field injection.

```java
@Resource
private Ghost randomGhost;
```

```java

@Configuration
public class ApplicationContextTestResourceNameType {

    @Bean
    public Ghost randomGhost() {
        final Ghost ghost = GhostFactroy.create();
        return ghost;
    }
}
``` 

**Inject**

The ``@Inject`` annotation belongs to the JSR-330 annotations collection. This annotation has the following execution paths, listed by precedence:

* Match by Name
* Match by Type
* Match by Qualifier

These execution paths are applicable to both setter and field injection. In order for us to access the ``@Inject`` annotation, we have to declare the javax.inject library as a Gradle or Maven dependency.

```gradle
testCompile group: 'javax.inject', name: 'javax.inject', version: '1'
```

```xml
<dependency>
    <groupId>javax.inject</groupId>
    <artifactId>javax.inject</artifactId>
    <version>1</version>
</dependency>
```

```java
@Inject
private Ghost randomGhost;
```

```java

@Configuration
public class ApplicationContextTestResourceNameType {

    @Bean
    public Ghost randomGhost() {
        final Ghost ghost = GhostFactroy.create();
        return ghost;
    }
}
``` 

#### Qualifiers

**By default, Spring resolves @Autowired entries by type.** If more than one bean of the same type is available in the container, the framework will throw a fatal exception. To resolve this conflict, we need to tell Spring explicitly which bean we want to inject.

For instance, let's see how we can use the ``@Qualifier`` annotation to indicate the required bean.

First, we'll define 2 beans of type Agency:

```java
@Component("fittes")
public class FittesAgency implements Agency {

    @Override
    public String getName() {
        return "The Fittes Agency";
    }
}

@Component("lockwood")
public class Lockwood implements Agency {
    
    @Override
    public String getName() {
        return "Lockwood & Co.";
    }
}
```

Now let's try to inject an Agency bean into the AgencyService class:

```java
@Service
public class AgencyService {

    private final Agency agency;

    @Autowired
    public AgencyService(final Agency agency) {
        this.agency = agency;
    }
}
```

In our example, there are two concrete implementations of Agency available for the Spring container. As a result, Spring will throw a ``NoUniqueBeanDefinitionException`` exception when constructing the AgencyService:

```txt
Caused by: org.springframework.beans.factory.NoUniqueBeanDefinitionException: 
No qualifying bean of type [com.theproblem.Agency] is defined: 
expected single matching bean but found 2: fittes,lockwood
```

We can avoid this by narrowing the implementation using a ``@Qualifier`` annotation:

```java
@Service
public class AgencyService {

    private final Agency agency;

    @Autowired
    public AgencyService(@Qualifier("lockwood") final Agency agency) {
        this.agency = agency;
    }
}
```

When there are multiple beans of the same type, it's a good idea to use @Qualifier to avoid ambiguity. Please note that the value of the @Qualifier annotation matches with the name declared in the @Component annotation of our Agency implementation.

### Properties

Spring 3.1 also introduces the new ``@PropertySource`` annotation as a convenient mechanism for adding property sources to the environment. We can use this annotation in conjunction with the ``@Configuration`` annotation:

```java
@Configuration
@PropertySource("classpath:theproblem.properties")
public class TheProblemConfig {
    //...
}
```

The @PropertySource annotation is repeatable according to Java 8 conventions. Therefore, if we're using Java 8 or higher, we can use this annotation to define multiple property locations:

```java
@PropertySource("classpath:ghosts.properties")
@PropertySource("classpath:agencies.properties")
public class TheProblemConfig {
    //...
}



@PropertySources({
    @PropertySource("classpath:ghosts.properties"),
    @PropertySource("classpath:agencies.properties")
})
public class TheProblemConfig {
    //...
}
```

#### Using Properties

We're going to have a look at the ``@Value`` Spring annotation. This annotation can be used for injecting values into fields in Spring-managed beans, and it can be applied at the field or constructor/method parameter level. We will need a ``properties file`` to define the values we want to inject with the @Value annotation. And so, we will first need to define a @PropertySource in our configuration class with the properties file name.

```txt
defence.material.iron=Iron Chain
locationOfOrigin=London
listOfGhostTypes=1,2,3
```

As a basic and mostly useless example, we can only inject “string value” from the annotation to the field:

```java
@Value("string value")
private String stringValue;
```

Using the ``@PropertySource`` annotation allows us to work with values from properties files with the ``@Value`` annotation. In the following example, we get Value got from the file assigned to the field:

```java
@Value("${defence.material.iron}")
private String valueFromFile;
```

Default values can be provided for properties that might not be defined. Here, the value some default will be injected:

```java
@Value("${unknown.param:Silver Net}")
private String someDefault;
```

When we use the @Value annotation, we are not limited to a field injection. We can also use it together with constructor injection.

Let's see this in practice:

```java
@Component
@PropertySource("classpath:values.properties")
public class TheProblem {

    private final String locationOfOrigin;

    @Autowired
    public TheProblem(@Value("${locationOfOrigin}") String locationOfOrigin) {
        this.locationOfOrigin = locationOfOrigin;
    }
}
```

Java 14 introduced records to facilitate the creation of an immutable class. The Spring framework supports @Value for record injection since version 6.0.6:

```java
@Component
@PropertySource("classpath:values.properties")
public record TheProblem(@Value("${locationOfOrigin}") String locationOfOrigin) {}
```

#### Properties in Spring Boot

Spring Boot applies its typical convention over configuration approach to property files. This means that we can simply put an ``application.properties`` file in our ``src/main/resources`` directory, and it will be auto-detected. We can then inject any loaded properties from it as normal. So, by using this default file, we don’t have to explicitly register a PropertySource or even provide a path to a property file.

We can also configure a different file at runtime if we need to, using an environment property:

```bash
java -jar app.jar --spring.config.location=classpath:/another-location.properties
```

### Profiles

Profiles are a core feature of the framework, allowing us to map our beans to different profiles. For example, dev, test, and prod. We can then activate different profiles in different environments to bootstrap only the beans we need.

We use the ``@Profile`` annotation, we are mapping the bean to that particular profile. The annotation simply takes the names of one (or multiple) profiles.

```java
@Component
@Profile("dev")
public class GhostConfig
```

#### Setting Profiles

We can also set profiles directly on the environment:

```java
@Autowired
private ConfigurableEnvironment env;
...
env.setActiveProfiles("someProfile");
```

Similarly, we can define the active profiles in the web.xml file of the web application using a context parameter:

```xml
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>/WEB-INF/app-config.xml</param-value>
</context-param>
<context-param>
    <param-name>spring.profiles.active</param-name>
    <param-value>dev</param-value>
</context-param>
```

The profile names can also be passed in via a JVM system parameter. These profiles will be activated during application startup:

```txt
-Dspring.profiles.active=dev
```

In a Unix environment, profiles can also be activated via the environment variable:

```bash
export spring_profiles_active=dev
```

Spring Boot supports all the profile configurations outlined so far, but makes it easier thanks to the use of the properties file.

```txt
spring.profiles.active=dev
```

### Scheduled tasks

In this chapter, we will illustrate how the Spring ``@Scheduled`` annotation can be used to configure and schedule tasks. The simple rules that we need to follow to annotate a method with @Scheduled are:

* The method should typically have a void return type (if not, the returned value will be ignored).
* The method should not expect any parameters.

To enable support for scheduling tasks and the @Scheduled annotation in Spring, we can use the Java enable-style annotation:

```java
@Configuration
@EnableScheduling
public class TheProblemConfig {
    ...
}
```

#### Fixed Delay

Let's start by configuring a task to run after a fixed delay:

```java
@Scheduled(fixedDelay = 1000)
public void newGhostOutbreak() {
    System.out.println("A new ghost as appeared!);
}
```

In this case, the duration between the end of the last execution and the start of the next execution is fixed. **The task always waits until the previous one is finished**. This option should be used when it’s mandatory that the previous execution is completed before running again.

#### Fixed Rate

Let's now execute a task at a fixed interval of time:

```java
@Scheduled(fixedRate = 1000)
public void newGhostOutbreak() {
    System.out.println("A new ghost as appeared!);
}
```

This option should be used when each execution of the task is independent.

Note that **scheduled tasks don't run in parallel by default**. So even if we used fixedRate, the next task won't be invoked until the previous one is done. If we want to support parallel behavior in scheduled tasks, we need to add the ``@Async`` annotation:  

```java
@EnableAsync
public class ScheduledFixedGhostOutbreak {
    @Async
    @Scheduled(fixedRate = 1000)
    public void newGhostOutbreak() throws InterruptedException {
        System.out.println("A new ghost as appeared!");
        Thread.sleep(2000);
    }
}
```

Now this asynchronous task will be invoked each second, even if the previous task isn't done.

#### Fixed Rate VS Fixed Delay

We can run a scheduled task using Spring's ``@Scheduled`` annotation, but based on the properties ``fixedDelay`` and ``fixedRate``, the nature of execution changes.

The fixedDelay property makes sure that there is a delay of x millisecond between the finish time of an execution of a task and the start time of the next execution of the task. This property is specifically useful when we need to make sure that only one instance of the task runs all the time. For dependent jobs, it is quite helpful.

The fixedRate property runs the scheduled task at every x millisecond. It doesn't check for any previous executions of the task This is useful when all executions of the task are independent. If we don't expect to exceed the size of the memory and the thread pool, fixedRate should be quite handy. Although, if the incoming tasks do not finish quickly, it's possible they end up with “Out of Memory exception”.

#### Schedule a Task With Initial Delay

```java
@Scheduled(fixedDelay = 1000, initialDelay = 1000)
public void newGhostOutbreak() {
    System.out.println("A new ghost as appeared!");
}
```

Note how we're using both fixedDelay as well as initialDelay in this example. The task will be executed the first time after the initialDelay value, and it will continue to be executed according to the fixedDelay. This option is convenient when the task has a setup that needs to be completed.

#### Schedule a Task Using Cron Expressions

Sometimes delays and rates are not enough, and we need the flexibility of a cron expression to control the schedule of our tasks:

```java
@Scheduled(cron = "0 15 10 15 * ?")
public void newGhostOutbreak() {
    System.out.println("A new ghost as appeared!");
}
```

Note that in this example, we're scheduling a task to be executed at 10:15 AM on the 15th day of every month.

---

## Sources and Information

**TODO**

* [Spring Documentation](https://spring.getdocs.org/en-US/spring-framework-docs/docs/spring-web/spring-web.html)
* 