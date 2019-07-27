# Security
## Core Spring 5.0 Certification Exam Study Guide

[<< Back to Table of Contents](README.md)

:star: Star this project on GitHub — It helps!!

[![Contributions welcome](https://img.shields.io/badge/contributions-welcome-orange.svg)](https://github.com/seanjgildea/CoreSpring5CertificationGuide/issues)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)

### What are authentication and authorization? Which must come first?

- Authentication is the process of verifying the identity of a user by obtaining some sort of credentials and using those credentials to verify the user's identity. If the credentials are valid, the authorization process starts. Authentication always proceeds to Authorization process.

### Is security a cross cutting concern? How is it implemented internally?

- Yes security is a cross-cutting concern. Spring Security internally is implemented using AOP - the same way as Transactions management.

### What is the DelegatingFilterProxy?

- It can be declared in the web.xml. It provides the link between the web.xml and the application context. It delegates the Filter's methods through to a bean which is obtained from the Spring application context.

### What is the security filter chain?

- @EnableGlobalMethodSecurity can enable annotation-based security

- Spring Security's web infrastructure is based entirely on standard servlet filters. It doesn't use servlets or any other servlet-based frameworks (such as Spring MVC) internally, so it has no strong links to any particular web technology. It deals in HttpServletRequests and HttpServletResponses and doesn't care whether the requests come from a browser, a web service client, an HttpInvoker or an AJAX application.

- The order that filters are defined in the chain is very important. Irrespective of which filters you are actually using, the order should be as follows:

- ChannelProcessingFilter

- SecurityContextPersistenceFilter

- ConcurrentSessionFilter

- UsernamePasswordAuthenticationFilter
- CasAuthenticationFilter
- BasicAuthenticationFilter

- SecurityContextHolderAwareRequestFilter

- JaasApiIntegrationFilter

- RememberMeAuthenticationFilter

- AnonymousAuthenticationFilter

- ExceptionTranslationFilter

- FilterSecurityInterceptor

### What is a security context?

- Context that holds security information about the current thread of execution. This information includes details about the principal. Context is held in the SecurityContextHolder.

### Why do you need the intercept-url?

- It is used to define a URL for the requests we want to have some security constraints.

      < http >
      < intercept-url pattern="/auth/admin/*" access="hasRole('ADMIN')" / >
      < intercept-url pattern="/auth/*" access="hasAnyRole('ADMIN','USER')" / >
      < / http >

### In which order do you have to write multiple intercept-url's?

- Most specific patterns must come first and most general last.

### What does the ** pattern in an antMatcher or mvcMatcher do?

- /admin/** matches any path starting with /admin

- * matches zero or more characters
- ** matches zero or more 'directories' in a path
- ? matches one character

### Why is an mvcMatcher more secure than an antMatcher?

- mvcMatcher can also restrict the URLs by HTTP method

### Does Spring Security support password hashing? What is salting?

- < password-encoder > can be used for supporting encoded passwords

- Spring Security uses PasswordEncoder for encoding passwords. This interface has a Md5PasswordEncoder that allows for obtaining hashes of the password - that will be persisted. Salting is appending a (random) string to the hash to prevent hackers from matching with a hash from the standard dictionary of hashes.

### Why do you need method security? What type of object is typically secured at the method level (think of its purpose not its Java type).

- The @Secured annotation is used to specify a list of roles on a method. 
- Users: A user only can access that method if she has at least one of the specified roles.

### What do @PreAuthorized and @RolesAllowed do? What is the difference between them?

- @RolesAllowed (JAVA based) SPEL NO defines a list of security configuration attributes for business methods.

- @PreAuthorize (Spring based) SPEL YES Writes expressions that limit method invocation based on the roles/permissions, the current authenticated user, and the arguments passed into the method.

- They are used to secure a controller. to limit method invocation based on roles/permissons, the current authenticated user, and arguments passed into the method.

### What does Spring’s @Secured do?

- The Secured annotation is used to define a list of security configuration attributes for business methods. This annotation can be used as a Java 5 alternative to XML configuration. @Secured and @RolesAllowed perform identical functionality in Spring. The difference is that @Secured is a Spring specific annotation while @RolesAllowed is a Java standard annotation (JSR250).

### How are these annotations implemented?

- @PreAuthorize("hasRole('ADMIN') or #user.id == authentication.name")
- @PreAuthorize("hasRole('USER')") I.E: Decides whether a method can actually be invoked or not

### In which security annotation are you allowed to use SpEL?

- PreAuthorize, PostAuthorize, PreFilter, PostFilter

### Is it enough to hide sections of my output (e.g. JSP-Page or Mustache template)?

- You can use a special tag for hiding or not generating parts of JSP depending on access level. You can also verify the user has access to the URL and only allow accessing to the view based on user

### Spring security offers a security tag library for JSP, would you recognize it if you saw it in an example?

- To use any security taglib you need this declared in your JSP
- < % @ taglib prefix="sec" uri="http://www.springframework.org/security/tags" % >

- Authentication Tag: Allows access to the current Authentication object stored in the security context. 

      < sec:authentication property="principal.username" / >

- Authorize Tag: Used to determine whether its contents should be evaluated or not. 
      
      < sec:authorize access="hasRole('supervisor')" > Only supervisors should see this< / sec:authorize >

- Accesscontrollist Tag: This tag is only valid when used with Spring Security’s ACL module. It checks a comma-separated list of required permissions for a specified domain object. If the current user has all of those permissions, then the tag body will be evaluated. If they don’t, it will be skipped

      < sec:accesscontrollist hasPermission="1,2" domainObject="${someObject}" > Shown if has 1 and 2 < / sec:accesscontrollist >



[<< Back to Table of Contents](README.md)
