# Aspect Oriented Programming
## Core Spring 5.0 Certification Exam Study Guide

[<< Back to Table of Contents](README.md)

:star: Star this project on GitHub — It helps!!

[![Contributions welcome](https://img.shields.io/badge/contributions-welcome-orange.svg)](https://github.com/seanjgildea/CoreSpring5CertificationGuide/issues)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)


### What is the concept of AOP? Which problem does it solve? What is a cross cutting concern?

Aspect Oriented Programming (alias AOP) is a concept of separating cross-cutting parts of an application. For example suppose you want to add logging or some security functions to different elements of your application. Usually you would add some code to each element you want to add that functionality to. But using AOP you can externalize/separate these functions into something called aspects.

### What two problems arise if you don't solve a cross cutting concern via AOP?

1. Brittle object hierarchy – when seemingly safe modification of superclass causes subclasses to malfunction.
2. Cumbersome delegation – you call one thing but that thing doesn’t do the job … instead it asks some 3rd party to do the required job.

### What is a pointcut, a join point, an advice, an aspect, weaving?

        Joinpoints are options on the menu and Pointcuts are items you select.
        Aspect: a modularization of a concern that cuts across multiple classes. Transaction management is a good example of a crosscutting concern in enterprise Java applications. In Spring AOP, aspects are implemented using regular classes (the schema-based approach) or regular classes annotated with the @Aspect annotation (the @AspectJ style).

        Join point: a point during the execution of a program, such as the execution of a method or the handling of an exception. In Spring AOP, a join point always represents a method execution.

        Advice: action taken by an aspect at a particular join point. Different types of advice include "around," "before" and "after" advice. (Advice types are discussed below.) Many AOP frameworks, including Spring, model an advice as an interceptor, maintaining a chain of interceptors around the join point.

        Pointcut: a predicate that matches join points. Advice is associated with a pointcut expression and runs at any join point matched by the pointcut (for example, the execution of a method with a certain name). The concept of join points as matched by pointcut expressions is central to AOP, and Spring uses the AspectJ pointcut expression language by default.

        Introduction: declaring additional methods or fields on behalf of a type. Spring AOP allows you to introduce new interfaces (and a corresponding implementation) to any advised object. For example, you could use an introduction to make a bean implement an IsModified interface, to simplify caching. (An introduction is known as an inter-type declaration in the AspectJ community.)

        Target object: object being advised by one or more aspects. Also referred to as the advised object. Since Spring AOP is implemented using runtime proxies, this object will always be a proxied object.

        AOP proxy: an object created by the AOP framework in order to implement the aspect contracts (advise method executions and so on). In the Spring Framework, an AOP proxy will be a JDK dynamic proxy or a CGLIB proxy.

        Weaving: linking aspects with other application types or objects to create an advised object. This can be done at compile time (using the AspectJ compiler, for example), load time, or at runtime. Spring AOP, like other pure Java AOP frameworks, performs weaving at runtime.

### How does Spring solve (implement) a cross cutting concern?

- In comparison to some AOP frameworks like AspectJ, Spring allows for aspects to be applied only at methods join points (AspectJ can advise fields, types etc.). Spring AOP is done using proxy objects that in their turn are created by 2 different means: JDK dynamic proxying (creating some class that implements the same interface as the proxied bean) CGLib proxying (creating subclass for the proxied bean) important to mention here is the fact that starting from version 3.2 you don’t have to attach any additional libraries to support CGLib proxying.

### What are the limitations of the two proxy-types?

- Common limitations:
- Proxying works only when calls to methods are beyond the limits of a bean. If one method calls another on internally in a bean – that call will not be intercepted. Proxied objects must be created by the Spring IoC container. Proxies are not serializable. 
- JDK dynamic proxying has the following limitations:
- can be applied only on objects that implement an interface 
- CGLib proxying limitations: 
- final methods cannot be advised (remember you have to create a subclass for the proxied object so it won’t be possible because of the final modifier) the proxied class must have a default constructor resulting proxy object is final – you won’t be able to proxy a proxy

### What visibility must Spring bean methods have to be proxied using Spring AOP?

- JDK Proxies: Only public interface calls on proxy are intercepted. 
- CGLIB Proxies: Public , protected and package level method calls on proxies will be intercepted. 
- Final methods will not be intercepted however.

- "Due to the proxy-based nature of Spring's AOP framework, calls within the target object are by definition not intercepted. For JDK proxies, only public method calls on the proxy can be intercepted. With CGLIB, public and protected method calls on the proxy will be intercepted, and even package-visible methods if necessary."

### How many advice types does Spring support. Can you name each one? What are they used for?

- Before, After, Around, AfterReturning, AfterThrowing

### Which two advices can you use if you would like to try and catch exceptions?

- You have to use @AfterThrowing with 2 attributes:
- pointcut (alias value) – for selecting a named pointcut or writing an inline one
- throwing – for choosing a name to be passed to the advice method. This name must match the name of the parameter of the @AfterThrowing annotated method.

### What do you have to do to enable the detection of the @Aspect annotation?

- JAVA: Add @EnableAspectJAutoProxy annotation to the @Configuration annotated class
- XML: Add < aop:aspectj-autoproxy/> (will require the aop namespace added to the < bean> tag)

### What does @EnableAspectJAutoProxy do?

- @EnableAspectJAutoProxy enables @Aspect usage.
- It may have an additional parameter – proxyTargetClass=true will cause the enforced usage of CGLib.

- If shown pointcut expressions, would you understand them? For example, in the course we matched getter methods on Spring Beans, what would be the correct pointcut expression to match both getter and setter methods?

- Match all methods with getters and setters
- execution(* someService.get*(..)) && execution(* someService.set*(..))

- Match all public methods on EmployManager
- execution(public * EmployeeManager.*(..))

- pointcut expressions can be combined with the operators && (and), || (or), and ! 

    @Pointcut("execution(* com.xyz.someapp..service.*.*(..))")
    public void businessService() {} 

    @Pointcut("within(com.xyz.someapp.service..*)") 
    public void inServiceLayer() {} 

    @Pointcut("target(org.baeldung.dao.BarDao)") 

    @Pointcut("this(org.baeldung.dao.FooDao)") 

    @Pointcut("@args(org.baeldung.aop.annotations.Entity)")
    public void methodsAcceptingEntities() {} 

    @Pointcut("@annotation(org.baeldung.aop.annotations.Loggable)")
    public void loggableMethods() {}

### What is the JoinPoint argument used for?

- Any advice method may declare as its first parameter, a parameter of type org.aspectj.lang.JoinPoint (please note that around advice is required to declare a first parameter of type ProceedingJoinPoint, which is a subclass of JoinPoint. The JoinPoint interface provides a number of useful methods such as:

        getArgs() (returns the method arguments)
        getThis() (returns the proxy object)
        getTarget() (returns the target object)
        getSignature() (returns a description of the method that is being advised)
        toString() (prints a useful description of the method being advised).

### What is a ProceedingJoinPoint? When is it used?

- Used ONLY in the @Around method
- proceed() will call the advised method

    @Around("execution(* com.mumz.test.spring.aop.BookShelf.addBook(..))")
    public void aroundAddAdvice(ProceedingJoinPoint pjp){...

- ProceedingJoinPoint is used to reference the method that is being advised with an @Around advice. Call the ProceedingJoinPoint’s proceed() method for the advised method to run.


[<< Back to Table of Contents](README.md)
