# Spring Boot
## Core Spring 5.0 Certification Exam Study Guide

[<< Back to Table of Contents](README.md)

:star: Star this project on GitHub — It helps!!

[![Contributions welcome](https://img.shields.io/badge/contributions-welcome-orange.svg)](https://github.com/seanjgildea/CoreSpring5CertificationGuide/issues)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)

### What are the advantages of using Spring Boot?

- Easy Configuration Integrated web containers that allow for easy testing Starters – sets of dependencies that help setting up a “typical” application Decrease amount of boilerplate code

### How does it work? How does it know what to configure?

- Spring Boot doesn’t generate code or make edits to your files. Instead, when you start up your application, Spring Boot dynamically wires up beans and settings and applies them to your application context.

### What things affect what Spring Boot sets up?

- Presence of dependencies in the POM as well as the @EnableAutoConfiguration / @SpringBoot annotations

### How are properties defined? Where is Spring Boot’s default property source?

- application.properties or application.yml , the default property source is on the classpath , or src/main/resources

- Properties are considered in the following order:

- Devtools global settings properties on your home directory (~/.spring-boot-devtools.properties when devtools is active).
- @TestPropertySource annotations on your tests.
- @SpringBootTest#properties annotation attribute on your tests.
- Command line arguments.
- Properties from SPRING_APPLICATION_JSON (inline JSON embedded in an environment variable or system property).
- ServletConfig init parameters.
- ServletContext init parameters.
- JNDI attributes from java:comp/env.
- Java System properties (System.getProperties()).
- OS environment variables.
- A RandomValuePropertySource that has properties only in random.*.
- Profile-specific application properties outside of your packaged jar (application-{profile}.properties and YAML variants).
- Profile-specific application properties packaged inside your jar (application-{profile}.properties and YAML variants).
- Application properties outside of your packaged jar (application.properties and YAML variants).
- Application properties packaged inside your jar (application.properties and YAML variants).
- @PropertySource annotations on your @Configuration classes.
- Default properties (specified by setting SpringApplication.setDefaultProperties).

### Would you recognize common Spring Boot annotations if you saw them in the exam?

- @SpringBootApplication –-> @Configuration, @EnableAutoConfiguration, @ComponentScan

- ![spring-boot-annotations](https://github.com/seanjgildea/CoreSpring5CertificationGuide/blob/master/img/spring-boot-annotations.png)

### Would you recognize Configuration Properties if you saw them in the exam?

- First declare the Config File

        @Configuration
        @EnableConfigurationProperties(AcmeProperties.class)
        public class MyConfiguration {}


-  Then define the properties

        @Component
        @ConfigurationProperties(prefix="acme")
        public class AcmeProperties {}

### What is the difference between an embedded container and a WAR?

- WAR's allow you to use JSP as Tomcat's JSP support is tightly coupled with that of a WAR layout.
- WAR's give the option of deploying your app to a standalone container or app server
- JAR's are for an embedded container that comes with the resulting application
- Change 'packaging' value to 'war' in the maven pom.xml

### What embedded containers does Spring Boot support?

- TomCat, Jetty and Undertow

- To build a war file that is both executable and deployable into an external container, you need to mark the embedded container dependencies

        < dependency >
            < groupId>org.springframework.boot< / groupId>
            < artifactId>spring-boot-starter-tomcat< / artifactId>
            < scope>provided< / scope>
        < /dependency>

- SpringBootServletInitializer

### What does @EnableAutoConfiguration do?

- This annotation tells Spring Boot to “guess” how you want to configure Spring, based on the jar dependencies that you have added.

### What about @SpringBootApplication?

1. Turns on autoconfig
2. Enables auto-scanning
3. Defines a configuration class

### Does Spring Boot do component scanning? Where does it look by default?

- @SpringBootApplication automatically includes @ComponentScan which searches in the current and child packages for @Components

### What is a Spring Boot starter POM? Why is it useful?

- Starter POMs are a set of convenient dependency descriptors that you can include in your application. You get a one-stop-shop for all the Spring and related technology that you need, without having to hunt through sample code and copy paste loads of dependency descriptors.

- Spring Boot supports both Java properties and YML files. Would you recognize and understand them if you saw them?

- Java properties files come in application.properties file; YAML come in application.yml

### Can you control logging with Spring Boot? How?

- The various logging systems can be activated by including the appropriate libraries on the classpath and can be further customized by providing a suitable configuration file in the root of the classpath or in a location specified by the following Spring Environment property: logging.config.

- You can force Spring Boot to use a particular logging system by using the org.springframework.boot.logging.LoggingSystem system property. The value should be the fully qualified class name of a LoggingSystem implementation. You can also disable Spring Boot’s logging configuration entirely by using a value of none.

- Since logging is initialized before the ApplicationContext is created, it is not possible to control logging from @PropertySources in Spring @Configuration files. The only way to change the logging system or disable it entirely is via System properties.


[<< Back to Table of Contents](README.md)
