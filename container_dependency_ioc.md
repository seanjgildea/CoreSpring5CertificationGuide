# Container, Dependency and IOC
## Core Spring 5.0 Certification Exam Study Guide

[<< Back to Table of Contents](README.md)

:star: Star this project on GitHub — It helps!!

[![Contributions welcome](https://img.shields.io/badge/contributions-welcome-orange.svg)](https://github.com/seanjgildea/CoreSpring5CertificationGuide/issues)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)

### What is a pattern? What is an anti-pattern? Is dependency injection a pattern?

- A software design pattern is a general, reusable solution to a commonly occurring problem within a given context in software design. Dependency injection is a programming technique that makes a class independent of its dependencies. An anti pattern is a common response to a recurring problem that is usually ineffective and risks being highly counterproductive

### What is an interface and what are the advantages of making use of them in Java?

- A way of implementing multiple inheritance (polymorphism), interfaces only contain abstract methods and cannot be instantiated. Advantages include providing different implementations at runtime, the ability to inject dependencies, and polymorphism. An interface is a reference type in Java. It is similar to a class. It is a collection of abstract methods. Interfaces are Java's way to implement multiple inheritance, ie: A set of methods you can call without any knowledge of their implementation.

### What is meant by “application-context?

- The ApplicationContext is the central interface within a Spring application for providing configuration information to the application. It is a container used for IoC(inversion of control) over beans. The BeanFactory interface provides an advanced configuration mechanism capable of managing any type of object. ApplicationContext is a sub-interface of BeanFactory. In short, the BeanFactory provides the configuration framework and basic functionality, and the ApplicationContext adds more enterprise-specific functionality. The ApplicationContext is a complete superset of the BeanFactory.

### What is the concept of a “container” and what is its lifecycle?

- The container will create the objects, wire them together, configure them, and manage their complete life cycle from creation till destruction. The Spring container uses DI to manage the components that make up an application.

### How are you going to create a new instance of an ApplicationContext?

- ApplicationContext is just an interface so first you have to choose the implementation that best suits your needs. In case of annotations you will need a configuration class annotated with @Configuration. In case of XML you just supply an Spring XML Configuration Metadata file.

### Can you describe the lifecycle of a Spring Bean in an ApplicationContext?

- ![spring-bean-lifecycle](https://github.com/seanjgildea/CoreSpring5CertificationGuide/blob/master/img/springbeanlifecycle.png)

### How are you going to create an ApplicationContext in an integration test?

- For automatic creation of an ApplicationContext for test purposes you will need the configuration class or XML configuration metadata file and 2 annotations.

      @RunWith(SpringRunner.class)
  
      @ContextConfiguration(classes = Config.class) 
  
      or @ContextConfiguration(classes={A.class,B.class})
  
      or @ContextConfiguration(locations = {"classpath:config.xml"})

### What is the preferred way to close an application context? Does Spring Boot do this for you?

    context.close() 
    context.registerShutdownHook(); 

- Yes Spring does. In springApplication.run()

### Can you describe: Dependency injection using Java configuration?

- One bean method should call another bean method. 

    @Bean public Foo foo() { return new Foo(bar()); } 
  
    @Bean public Bar bar() { return new Bar(); }
    
- ![spring-framework](https://github.com/seanjgildea/CoreSpring5CertificationGuide/blob/master/img/annotations.png)

### Can you describe: Dependency injection using annotations (@Component, @Autowired)?

- @Component marks the class as a Java Bean which signals Spring to pick it up and pull it into the Application Context so that it can be injected into @Autowired instances.

- @Autowired has a Required property to indicate if the value being injected is optional

- @Required dependencies that are not set raise a corresponding exception

### Can you describe: Component scanning, Stereotypes and Meta-Annotations?

- Components are scanned at startup based off the specified classpath or marked with @Component
  
- Stereotypes: An annotation classification for classes. @Component @Controller @Service @Repository
  
- A Stereotype is an annotation that can be used to annotate other annotations. 

- Meta-Annotations: An annotation that is used as part of another annotation. For example, @RestController is not a Stereotype. It is a convenience annotation composed of @Controller and @Responsebody.

### Can you describe: Scopes for Spring beans? What is the default scope?

- Scopes define how the bean will be used. For example singleton scope will use the same instance of the requested object each time. Common scopes are singleton, prototype, session, and request. The default scope is singleton.

### Are beans lazily or eagerly instantiated by default? How do you alter this behavior?

- Beans are eagerly instantiated by default. You can override this by marking the bean @Lazy

### What is a property source? How would you use @PropertySource?

    @PropertySource("classpath:/com/myco/app.properties")
  
    public class ConfigMySqlDB 

    @Value("${mongodb.url}")

    private String mongodbUrl;

    @Autowired

    Environment env;

    env.getProperty("testbean.name");

- @PropertySource is an Annotation providing a convenient and declarative mechanism for adding a PropertySource to Spring's Environment. To be used in conjunction with @Configuration classes.

      To resolve ${ } in @Value
  
          @Bean

          public static PropertySourcesPlaceholderConfigurer propertyConfigInDev() {

          return new PropertySourcesPlaceholderConfigurer();

          }

### What is a BeanFactoryPostProcessor and what is it used for? When is it invoked?

- BeanFactoryPostProcessor is an interface and beans that implement it are actually beans that undergo the Spring lifecycle but these beans don't take part of the other declared beans' lifecycle. A bean implementing BeanFactoryPostProcessor is called when all bean definitions will have been loaded, but no beans will have been instantiated yet. This allows for overriding or adding properties even to eager-initializing beans. This will let you have access to all the beans that you have defined in XML or that are annotated (scanned via component-scan).

### Why would you define a static @Bean method?

- You would use static @Bean when defining post-processor beans, e.g. of type BeanFactoryPostProcessor or BeanPostProcessor, since such beans will get initialized early in the container lifecycle and should avoid triggering other parts of the configuration at that point. Static @Bean methods will never get intercepted by the container, not even within @Configuration classes. This is due to technical limitations: CGLIB subclassing can only override non-static methods. As a consequence, a direct call to another @Bean method will have standard Java semantics, resulting in an independent instance being returned straight from the factory method itself.

### What is a PropertySourcesPlaceholderConfigurer used for?

-  Specialization of PlaceholderConfigurerSupport / Resolves @Value annotations

    < bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer" > < property name="locations" value="classpath:jdbc.properties" / > < / bean >

-   This bean is used for defining the location of the properties file to be used in assigning values to bean properties. But it also allows for searching the required value in the system and/or environment variables. Resolves ${...} placeholders within bean definition property values and @Value annotations

-   SpEL is abbreviation of Spring Expression Language which can be used to query property value from properties file (Use $),' or manipulate java object and it’s attributes at runtime( Use #).

### What is a BeanPostProcessor and how is it different to a BeanFactoryPostProcessor?

-  BeanPostProcessor operates on bean instances whereas the BeanFactoryPostProcessor does it's work BEFORE the container instantiates any beans.

-   BeanFactoryPostProcessor implementations are "called" during startup of the Spring context after all bean definitions will have been loaded while BeanPostProcessor are "called" when the Spring IoC container instantiates a bean (i.e. during the startup for all the singleton and on demand for the proptotypes one)

### What does a BeanPostProcessor do? When are they called?

- A bean implementing BeanPostProcessor operates on bean (or object) instances which means that when the Spring IoC container instantiates a bean instance then BeanPostProcessor interfaces do their work.

### What is an initialization method and how is it declared on a Spring bean?

    @Bean(initMethod="init",destroyMethod="destroy")
  
- Initialization method is a method that does some initialization work after all the properties of the bean were set by the container. It can be declared using the initMethod of @Bean, using @PostConstruct, or implementing InitializingBean and overriding afterPropertiesSet(discouraged). The order the methods are called is @PostConstruct, then afterPropertiesSet, and then init-method.

### Consider how you enable JSR-250 annotations like @PostConstruct and @PreDestroy? When/how will they get called

     @PostConstruct and @PreDestroy -

    @Resource(name = "spellChecker") // Interprets as bean name to be injected 
  
    public void setSpellChecker( Spellchecker spellChecker) { this.spellchecker = ...}

- For these annotation to be available you must have the CommonAnnotationBeanPostProcessor registered in context. They are declared with the init or destroy methods and also are called upon instantiation or just before beans are removed from the container.


![jsr330](https://github.com/seanjgildea/CoreSpring5CertificationGuide/blob/master/img/jsr330.png)



[<< Back to Table of Contents](README.md)



