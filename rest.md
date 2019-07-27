# REST
## Core Spring 5.0 Certification Exam Study Guide

[<< Back to Table of Contents](README.md)

:star: Star this project on GitHub — It helps!!

[![Contributions welcome](https://img.shields.io/badge/contributions-welcome-orange.svg)](https://github.com/seanjgildea/CoreSpring5CertificationGuide/issues)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)

### What is a resource?

- Anything that can be named is a resource. Usually that nouns that define our model.

### Is REST secure? What can you do to secure it?

- On itself no. Also we can implement some method-level (that is service layer) Spring Security to make it more secure.

### What are safe REST operations?

- Operations that don't change things (OPTIONS, GET, HEAD)

### What are idempotent operations? Why is idempotency important?

- OPTIONS, GET, HEAD, DELETE, PUT

- Operations that always return the same result. Idempotency is important for understanding the limits of effect some operations have on resources.

### Is REST scalable and/or interoperable?

- Because of the stateless nature of REST it is easily scalable and due to its HTTP usage it is highly interoperable because many systems support HTTP.

### Which HTTP methods does REST use?

- GET,PUT,POST,PATCH,DELETE

### What is an HttpMessageConverter?

- HttpMessageConverters convert HTTP requests to objects and back from objects to HTTP responses. Spring has a list of converters that is registered by default but it is customizable - additional implementations may be plugged in.

### Is REST normally stateless?

- REST is stateless because according to its definition it HAS to be stateless; meaning each request must contain all the required information to interact with the server without the server need to store some context between requests

### What does @RequestMapping do?

- A @RequestMapping on the class level is NOT required

- @RequestMapping defines handler methods for calls to certain URI.

### Is @Controller a stereotype? Is @RestController a stereotype?

- @Controller is a stereotype. @RestController is not. Its a convenience annotation that is itself annotated with @Controller and @ResponseBody.

### What is a stereotype annotation? What does that mean?

- Annotations denoting the roles of types or methods in the overall architecture (at a conceptual, rather than implementation, level). When you suggest a role of a particular class being annotated. (Controller classes should be annotated controller, service classes should be annotated service)

### What is the difference between @Controller and @RestController?

- A REST controller is processed by a HttpMessageConverter. The @RestController annotation tells Spring to render the resulting string directly back to the caller for web requests.

- @RestController is used on the server side.

### When do you need @ResponseBody?

- The @ResponseBody annotation tells a controller that the object returned is automatically serialized into JSON and passed back into the HttpResponse object. @ResponseBody is required when you want a controller result to me passed to a message converter rather than to a view resolver.

### What are the HTTP status return codes for a successful GET, POST, PUT or DELETE operation?

- 200, 201 Created, 202 Accepted, 204 No Content

### When do you need @ResponseStatus?

- @ResponseStatus will prevent DispatcherServlet from trying to find a view for the result and will set a status for the response. So for delete methods that return void you can use a response status (no content)

- @ResponseStatus( value = HttpStatus.NOT_FOUND, reason = "Not Found")

### Where do you need @ResponseBody? What about @RequestBody? Try not to get these muddled up!

- The @ResponseBody annotation tells a controller that the object returned is automatically serialized into JSON and passed back into the HttpResponse object.

- ResponseBody indicates a method return value should be bound to the web response body.

- @RequestBody annotation maps the HttpRequest body to a transfer or domain object, enabling automatic deserialization of the inbound HttpRequest body onto a Java object.

### Do you need Spring MVC in your classpath?

- Yes. Spring MVC is the core component for REST support.

### What are the advantages of the RestTemplate?

- RestTemplate is a synchronous client to perform HTTP requests. It is the original Spring REST client and exposes a simple, template-method API over underlying HTTP client libraries. RestTemplate has methods specific to HTTP methods.

### If you saw an example using RestTemplate would you understand what it is doing?

![resttemplate](https://github.com/seanjgildea/CoreSpring5CertificationGuide/blob/master/img/RestTemplate-methods.png)


[<< Back to Table of Contents](README.md)
