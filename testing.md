# Testing
## Core Spring 5.0 Certification Exam Study Guide

[<< Back to Table of Contents](README.md)

:star: Star this project on GitHub — It helps!!

[![Contributions welcome](https://img.shields.io/badge/contributions-welcome-orange.svg)](https://github.com/seanjgildea/CoreSpring5CertificationGuide/issues)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)

### Do you use Spring in a unit test?

- You do not need to, no.

### What type of tests typically use Spring?

- Integration tests, testing profiles, testing databases

### How can you create a shared application context in a JUnit integration test?

- By default, once loaded, the configured ApplicationContext is reused for each test. Thus, the setup cost is incurred only once per test suite, and subsequent test execution is much faster. 
@SpringJUnitConfig(classes=SystemTestConfig.class)

### When and where do you use @Transactional in testing?

- On the method level where you want to test a service out.
- You must declare Spring’s @Transactional annotation either at the class or the method level for your tests. Annotating a test method with @Transactional causes the test to be run within a transaction that is, by default, automatically rolled back after completion of the test. If a test class is annotated with @Transactional, each test method within that class hierarchy runs within a transaction. Test methods that are not annotated with @Transactional (at the class or method level) are not run within a transaction. Furthermore, tests that are annotated with @Transactional but have the propagation type set to NOT_SUPPORTED are not run within a transaction.

### How are mock frameworks such as Mockito or EasyMock used?

- Spring Boot includes a @MockBean annotation that can be used to define a Mockito mock for a bean inside your ApplicationContext. You can use the annotation to add new beans or replace a single existing bean definition. The annotation can be used directly on test classes, on fields within your test, or on @Configuration classes and fields. When used on a field, the instance of the created mock is also injected. Mock beans are automatically reset after each test method.

### How is @ContextConfiguration used?

- Most commonly in integration tests rather than Unit tests

      @ContextConfiguration defines class-level metadata that is used to determine how to load and configure an ApplicationContext for integration tests. Specifically, @ContextConfiguration declares the application context resource locations or the annotated classes used to load the context.
      @ContextConfiguration("/test-config.xml")
      @ContextConfiguration(classes = TestConfig.class)
      @ContextConfiguration(initializers = CustomContextIntializer.class)
      @ContextConfiguration provides support for inheriting resource locations or configuration classes as well as context initializers that are declared by superclasses.

### How does Spring Boot simplify writing tests?

- It allows easy mocking of objects and dependencies. There are also many built in annotations specifically for testing different aspects of an application. (test configs and setup is done by spring boot)
- Spring Boot provides a number of utilities and annotations to help when testing your application. Test support is provided by two modules: spring-boot-test contains core items, and spring-boot-test-autoconfigure supports auto-configuration for tests. Most developers use the spring-boot-starter-test “Starter”, which imports both Spring Boot test modules as well as JUnit, AssertJ, Hamcrest, and a number of other useful libraries.

- TestPropertyValues is a Spring Boot utility thats assists in adding properties.

### What does @SpringBootTest do?

- Integration testing focused - bootstraps entire container

- Used alongside @TestPropertySource to load files for test configs

- It sets up the same config for tests that the app uses. It automatically searches for springBootConfiguration for the SpringBootApplication. The @SpringBootTest annotation can be used as an alternative to the standard spring-test @ContextConfiguration annotation when you need Spring Boot features. The annotation works by creating the ApplicationContext used in your tests through SpringApplication. In addition to @SpringBootTest a number of other annotations are also provided for testing more specific slices of an application. Don’t forget to also add @RunWith(SpringRunner.class) to your test, otherwise the annotations will be ignored. By default, @SpringBootTest will not start a server.

### How does @SpringBootTest interact with @SpringBootApplication and @SpringBootConfiguration?

- The search algorithm works up from the package that contains the test until it finds a class annotated with @SpringBootApplication or @SpringBootConfiguration.


[<< Back to Table of Contents](README.md)










