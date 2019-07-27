# Spring MVC
## Core Spring 5.0 Certification Exam Study Guide

[<< Back to Table of Contents](README.md)

:star: Star this project on GitHub — It helps!!

[![Contributions welcome](https://img.shields.io/badge/contributions-welcome-orange.svg)](https://github.com/seanjgildea/CoreSpring5CertificationGuide/issues)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)

### MVC is an abbreviation for a design pattern. What does it stand for and what is the idea behind it?

- MVC stands for Model-View-Controller. The idea is to separate these three concepts rather than mixing them – as it is very hard to work when you have a view mixed with a controller and you need to change some details of the view.

### Do you need spring-mvc.jar in your classpath or is it part of spring-core?

- spring-webmvc.jar must be added separately

### What is the DispatcherServlet and what is it used for?

- The job of the DispatcherServlet is to take an incoming URI and find the right combination of handlers (generally methods on Controller classes) and views (generally JSPs) that combine to form the page or resource that's supposed to be found at that location.

### Is the DispatcherServlet instantiated via an application context?

Yes, it is declared in the web.xml of your web application. The DispatcherServlet will take help from ViewResolver to pickup the defined view for the request.

### What is a web application context? What extra scopes does it offer?

- DispatcherServlet expects a WebApplicationContext (an extension of a plain ApplicationContext) for its own configuration. WebApplicationContext has a link to the ServletContext and the Servlet with which it is associated

- A Spring MVC application usually has 2 application context – one for the web controllers etc. and another one for the “base” application components (services, data sources etc.).

### What is the @Controller annotation used for?

- @Controller annotation acts as a stereotype for the annotated class, indicating its role. The dispatcher scans the classes annotated with @Controller and detects @RequestMapping annotations within them. You can only use @RequestMapping on @Controller annotated classes.

- Both @Service and @Controller extend @Component annotation.

### How is an incoming request mapped to a controller and mapped to a method?

- Via the @Controller and @RequestMapping annotations via the servlet dispatcher

### What is the difference between @RequestMapping and @GetMapping?

- @GetMapping is a composed annotation that acts as a shortcut for @RequestMapping(method = RequestMethod.GET).

      GetMapping
      PostMapping
      PutMapping
      DeleteMapping
      PatchMapping

### What is @RequestParam used for?

- In a @RequestMapping, the @RequestParam makes the annotated parameter mandatory. (public String getIndex(@RequestParam String viewName))

### What are the differences between @RequestParam and @PathVariable?

- @PathVariable gets mapping method parameters from the URI itself, while @RequestParam gets them from URI parameters. for @PathVariable: http://www.foo.com/user/33
- for @RequestParam: http://www.foo.com/user?id=33

### What are some of the parameter types for a controller method?

- ServletRequest, HttpServletRequest, HttpSession, WebRequest, NativeWebRequest, Locale, Reader, InputStream, OutputStream, Principal, HttpEntity, Map, ModelMap, RedirectAttributes, Errors, BindingResult, SessionStatus, UriComponentsBuilder

### What other annotations might you use on a controller method parameter? (You can ignore form-handling annotations for this exam)

- @PathVariable, @RequestParam, @RequestHeader, @RequestBody, @RequestPart

### What are some of the valid return types of a controller method?

- ModelAndView , Model, Map, View, String, void, HttpBody, HttpEntity or ResponseEntity, HttpHeaders, Callable< ?>, DeferredResult< ?>, ListenableFuture< ?>, ResponseBodyEmitter, SseEmitter, StreamingResponseBody

### What is a View and what's the idea behind supporting different types of View?

- View is the base interface for different view technologies (XML, PDF, XLS etc.) Model data is passed to the view and rendered for providing different appearance. For passing the model data from controller to the view a view resolver is required – which also come in different flavours, that is different resolvers for different view types.

### How is the right View chosen when it comes to the rendering phase?

- The right view is chosen by the ViewResolver bean that has to be registered in the container.

### What is the Model?

- Model is a map of attributes passed by the controller to the view.

### Why do you have access to the model in your View? Where does it come from?

- Model is created by the Controller and passed back to the DispatcherServlet which in turn checks with the ViewResolver where the Model should be passed to and then passes it to the required View. So basically the Model comes from DispatcherServlet.

### What is the purpose of the session scope?

- Session scope is very similar to HttpSession scope. Beans instantiated based on session scope scope lives through the HTTP session. Similar to request scope, it is applicable only for web aware spring application contexts.

### Why are controllers testable artifacts?

- Controller classes instances can be tested easily using MockMVC. It allows you to make calls of the required type to the controller and assert the response and other parameters.

### What does a ViewResolver do?

- It resolves views by name and passes them to the required view needed.

- Cannot be declared in web.xml

- InternalResourceViewResolver: Convenient subclass of UrlBasedViewResolver that supports InternalResourceView (i.e. Servlets and JSPs) and subclasses such as JstlView.


[<< Back to Table of Contents](README.md)


