# Data Management
## Core Spring 5.0 Certification Exam Study Guide

[<< Back to Table of Contents](README.md)

:star: Star this project on GitHub — It helps!!

[![Contributions welcome](https://img.shields.io/badge/contributions-welcome-orange.svg)](https://github.com/seanjgildea/CoreSpring5CertificationGuide/issues)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)


### What is the difference between checked and unchecked exceptions?

- Checked exceptions are checked at compile time.

- In Java exceptions under Error and RuntimeException classes are unchecked exceptions, everything else under throwable is checked.

- The DataAccessException heirarchy converts proprietary checked exceptions to a set of runtime exceptions

### Why does Spring prefer unchecked exceptions?

- Checked exceptions add unncessary code and create more dependency.

### What is the data access exception hierarchy?

![heirarchy](https://github.com/seanjgildea/CoreSpring5CertificationGuide/blob/master/img/data-access-exception-hierarchy.jpg)

### How do you configure a DataSource in Spring?

        @Bean
        @ConfigurationProperties(prefix="spring.datasource")
        public DataSource dataSource() {
        return new FancyDataSource();
        }

- Configure an XML bean with the properties: driverClassName, url, username and password


        spring.datasource.url=jdbc:mysql://localhost/test
        spring.datasource.username=dbuser
        spring.datasource.password=dbpass
        spring.datasource.driver-class-name=com.mysql.jdbc.Driver

### Which bean is very useful for development/test databases?

- DriverManagerDataSource: Useful for test or standalone environments outside of a J2EE container, either as a DataSource bean in a corresponding ApplicationContext or in conjunction with a simple JNDI environment.

### What is the Template design pattern and what is the JDBC template?

- JDBC Template Doc: It simplifies the use of JDBC and helps to avoid common errors. It executes core JDBC workflow, leaving application code to provide SQL and extract results. This class executes SQL queries or updates, initiating iteration over ResultSets and catching JDBC exceptions and translating them to the generic, more informative exception hierarchy defined in the org.springframework.dao package.

- JdbcTemplate simplifies the use of JDBC and helps to avoid common errors. It executes core JDBC workflow, leaving application code to provide SQL and extract results. This class executes SQL queries or updates, initiating iteration over ResultSets and catching JDBC exceptions and translating them to the generic, more informative exception hierarchy defined in the org.springframework.dao package. .

### What is a callback? What are the three JdbcTemplate callback interfaces that can be used with queries? What is each used for? (You would not have to remember the interface names in the exam, but you should know what they do if you see them in a code sample).

- The PreparedStatementCreator callback interface creates a prepared statement given a Connection, providing SQL and any necessary parameters. 
- The ResultSetExtractor interface extracts values from a ResultSet. See also PreparedStatementSetter and RowMapper for two popular alternative callback interfaces.

- A callback is any executable code that is passed as an argument to other code, which is expected to call back(execute) the argument at a given time

- RowCallbackHandler – An interface used by JdbcTemplate for processing rows of a ResultSet on a per-row basis

- CallableStatementCreator – One of the three central callback interfaces used by the JdbcTemplate class. This interface creates a - CallableStatement given a connection, provided by the JdbcTemplate class.

- PreparedStatementCreator – callback interface creates a prepared statement given a Connection provided by this class.

### Can you execute a plain SQL statement with the JDBC template?

- Yes, using execute() methods – one of them accepts StatementCallback objects and the other wraps the String you pass in a StatementCallback. StatementCallback in its turn has a doInStatement() method that accepts Statement objects. So you can pass either an anonymous class with overridden doInStatement() method or just a simple String that execute() method of Statement will be run on.

### When does the JDBC template acquire (and release) a connection - for every method called or once per template? Why?

- JdbcTemplate acquires and releases connections for each method
- Connection con = DataSourceUtils.getConnection(getDataSource());
- and released by:
- DataSourceUtils.releaseConnection(con, getDataSource());


### How does the JdbcTemplate support generic queries? How does it return objects and lists/maps of objects?

- JdbcTemplate supports querying for any type of object assuming you supplied a RowMapper interface implementation defining the way database table should be mapped to some entity type.

        query()
        queryForObject() – if you are expecting only one object
        queryForMap() – will return a map containing each column value as key(column name)/value(value itself) pairs.
        queryForList() – a list of above if you’re expecting more results

- You use the update(..) method to perform insert, update and delete operations.

### What is a transaction? What is the difference between a local and a global transaction?

- Transaction is an indivisible unit of work.

- In short a local transaction is a simple transaction that is about one single database; whilst a global one is application server managed and spreads across many components/ technologies.

- Global transactions enable you to work with multiple transactional resources, typically relational databases and message queues. The application server manages global transactions through the JTA, which is a cumbersome API to use (partly due to its exception model). Furthermore, a JTA UserTransaction normally needs to be sourced from JNDI, meaning that you also need to use JNDI in order to use JTA. Obviously the use of global transactions would limit any potential reuse of application code, as JTA is normally only available in an application server environment.

### Is a transaction a cross cutting concern? How is it implemented by Spring?

- Yes as it can affect many components.

- The core concept in Spring transactions world is the transaction strategy that is defined by PlatformTransactionManager. This interface is a service provider interface. It has multiple implementations and the one you choose depends on your requirements. From PlatformTransactionManager through getTransaction() method – by passing the TransactionDefinition in, you get the TransactionStatus that may represent a new or an existing transaction.

### How are you going to define a transaction in Spring?

- You can define transactions in 2 ways: Declarative or Programmatic. Declarative way deals with adding some AOP related to transactions to the methods annotated with @Transactional or that have some tx-advices defined by XML. Programmatic way is about using either TransactionTemplate or directly PlatformTransactionManager.

- Instances of TransactionTemplate are thread safe

### What does @Transactional do? What is the PlatformTransactionManager?

- By using @Transactional, many important aspects such as transaction propagation are handled automatically. In this case if another transactional method is called by businessLogic(), that method will have the option of joining the ongoing transaction.

- PlatformTransactionManager: An interface that defines the transaction strategy through different implementations that match requirements specific to the project they are used in.

- The transaction name can be explicitly set only programmatically

- Instances are threadsafe

            public interface PlatformTransactionManager {
            TransactionStatus getTransaction(TransactionDefinition definition) throws TransactionException;
            void commit(TransactionStatus status) throws TransactionException;
            void rollback(TransactionStatus status) throws TransactionException;
            }

### Is the JdbcTemplate able to participate in an existing transaction?

- If you define a method as @Transactional and internally add some JdbcTemplate code it will run in that transaction; but JdbcTemplate itself cannot manage transactions - that is job of TransactionTemplate.

- Instances of the JdbcTemplate class are threadsafe once configured. This is important because it means that you can configure a single instance of a JdbcTemplate and then safely inject this shared reference into multiple DAOs (or repositories). The JdbcTemplate is stateful, in that it maintains a reference to a DataSource, but this state is not conversational state.

- execute() can execute ddl and stored procedure statements

### What is @EnableTransactionManagement for?

- Used in a @Configuration class. It is necessary for declaring a TransactionManager

        @Bean
        public PlatformTransactionManager  txManager() {
            return new DataSourceTransactionManager( dataSource() ); // for single JDBC datasources
        }


- Registers the necessary Spring components that power annotation-driven transaction management, such as the TransactionInterceptor and the proxy- or AspectJ-based advice that weave the interceptor into the call stack when JdbcFooRepository's @Transactional methods are invoked.

- Springs declarative transaction uses the TransactionInterceptor class in its AOP proxies in applying transactional advice.

### What does transaction propagation mean?

- Defines how transactions relate to each other:

        PROPAGATION_REQUIRED = Uses same Transaction object as Method 1 for Method 2 if exists. Create a new transaction or reuse one if available

        PROPAGATION_MANDATORY = method must run within a transaction. If no existing transaction is in progress, an exception will be thrown

        PROPAGATION_REQUIRES_NEW = Code will always run in a new transaction. Suspend current transaction if one exists.

        PROPAGATION_NOT_SUPPORTED = T1 / M1 , M2 starts / cannot run inside T1

### What happens if one @Transactional annotated method is calling another @Transactional annotated method on the same object instance?

- Since transaction-management is implemented using AOP @Transactional annotation will have no effect on the method being called as no proxy is created. The same behavior is characteristic for AOP aspects.

- If you call method2() from method1() within the same class, the @Transactional annotation of the second method will not have any effect because it is not called through proxy, but directly. Methods are enhanced with transactional behavior only if called through proxy (autowired bean, or some instance injected in any other way).

- But generally speaking, if method1() and method2() were in different classes, and both were annotated with @Transactional (so using REQUIRED propagation), then they would share the same transaction started in method1().

### Where can the @Transactional annotation be used? What is a typical usage if you put it at class level?

- At the class and method levels. Class level automatically applies to all methods.

- Transactional Annotations should be placed around all operations that are inseparable. 
- Typically, transactions belong on the Service layer. It's the one that knows about units of work and use cases. It's the right answer if you have several DAOs injected into a Service that need to work together in a single transaction.

### What does declarative transaction management mean?

- Declarative transaction management is a model built on AOP. Spring has some transactional aspects that may be used to advise methods for them to work in a transactional manner.

- It means that you can specify transaction behavior (or lack of it) down to the individual method level

### What is the default rollback policy? How can you override it?

- Any RuntimeException(Unchecked Ex) triggers rollback, and any checked Exception does not.
- Some possible ways of changing the default settings of the @Transactional annotation:

        rollbackFor
        Type: Array of Class objects, which must be derived from Throwable.
        Description: Optional array of exception classes that must cause rollback.

        rollbackForClassName
        Type: Array of class names. Classes must be derived from Throwable.
        Description: Optional array of names of exception classes that must cause rollback.

        noRollbackFor
        Type: Array of Class objects, which must be derived from Throwable.
        Description: Optional array of exception classes that must not cause rollback.

        noRollbackForClassName
        Type: Array of String class names, which must be derived from Throwable.
        Description: Optional array of names of exception classes that must not cause rollback.

### What is the default rollback policy in a JUnit test, when you use the @RunWith(SpringJUnit4ClassRunner.class) in JUnit 4 or @ExtendWith(SpringExtension.class) in JUnit 5, and annotate your @Test annotated method with @Transactional?

- By default, test transactions will be automatically rolled back after completion of the test; however, transactional commit and rollback behavior can be configured declaratively via the @Commit and @Rollback annotations

- @Commit indicates that the transaction for a transactional test method should be committed after the test method has completed. @Commit can be used as a direct replacement for @Rollback(false) in order to more explicitly convey the intent of the code.

### Why is the term "unit of work" so important and why does JDBC AutoCommit violate this pattern?

- Unit of work maintains in-memory updates and sends these in-memory updates as one transaction to the database. It is important because you can take control over the business rules represented and applies to the ACID principles. When JDBCAutoCommit is active ACID principles are broken violating the unit of works all or nothing pattern.

- JDBC AutoCommit will treat each individual SQL statement as a transaction. This means that if logically or from business point of view you need to make sure some statement is ok before executing another one, it would fail. Transactions are meant to solve this problem by “grouping” operations in some logical way so that you confirm to ACID principle (Atomicity, Consistency, Isolation, Durability).

### What does JPA stand for - what about ORM?

- Java Persistence API. 

- Object Relational Mapping

### What is the idea behind an ORM? What are benefits/disadvantages or ORM?

- The idea behind ORM is that in applications we deal with objects and classes whilst in databases we have to do with tables and rows. ORM is meant to help us map our entities to the database tables.

- Benefits include easy mapping of object model to data model, less code, concurrency support, and auto management of cache, connection pool, transactions and keys. Disadvantages include overhead for simple apps, learning implementation, lower performance, and difficulty with complex queries.

### What is a PersistenceContext and what is an EntityManager. What is the relationship between both?

- A persistence context is a set of entities such that for any persistent identity there is a unique entity instance. Within a persistence context, entities are managed. The EntityManager controls their lifecycle, and they can access datastore resources.

- PersistenceContext is a set of entity instances that correspond to persistent entity identity. All the manipulations with the persistent instances are made using EntityManager. EntityManager writes, deletes and searches entities to database.

### Why do you need the @Entity annotation. Where can it be placed?

- @Entity annotation is used to mark a class as an entity class that will be mapped to the database using ORM.

### What do you need to do in Spring if you would like to work with JPA?

- Include spring-boot-starter-data-jpa

         < bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager" >
                < property name="entityManagerFactory" ref="myEmf" / >
            < / bean >

- For JPA, you can define either an application managed entity manager(LocalEntityManagerFactoryBean) or container managed entity manager (LocalContainerEntityManagerFactoryBean)

### Are you able to participate in a given transaction in Spring while working with JPA?

- Spring JPA also allows a configured JpaTransactionManager to expose a JPA transaction to JDBC access code that accesses the same DataSource, provided that the registered JpaDialect supports retrieval of the underlying JDBC Connection. Out of the box, Spring provides dialects for the EclipseLink, Hibernate and OpenJPA JPA implementations. See the next section for details on the JpaDialect mechanism.

### Which PlatformTransactionManager(s) can you use with JPA?

- You can only use JpaTransactionManager. The Transaction Manager abstraction we are talking about here is Spring's PlatformTransactionManager interface, and JPATransactionManager is the only implementation of that interface that understands JPA.

### What does @PersistenceContext do?

- One question that comes to mind is, how can @PersistenceContext inject an entity manager only once at container startup time, given that entity managers are so short lived, and that there are usually multiple per request. The answer is that it can’t: EntityManager is an interface, and what gets injected in the spring bean is not the entity manager itself but a context aware proxy that will delegate to a concrete entity manager at runtime.

- @PersistenceContext expresses a dependency on a container-managed Entity Manager and its associated persistence context. It does NOT inject an EntityManager - it makes a proxy to the required EntityManager for taking care of correct transaction management and thread-safety.

        A Cache is a copy of data, copy meaning pulled from but living outside the database.
        Flushing a Cache is the act of putting modified data back into the database.
        A PersistenceContext is essentially a Cache. It also tends to have it's own non-shared database connection.
        An EntityManager represents a PersistenceContext (and therefore a Cache)
        An EntityManagerFactory creates an EntityManager (and therefore a PersistenceContext/Cache)

### What do you have to configure to use JPA with Spring? How does Spring Boot make this easier?

- To use JPA in a Spring project, an EntityManager needs to be set up.

- LocalEntityManagerFactoryBean or the more flexible LocalContainerEntityManagerFactoryBean.

- Spring Boot does this automatically when including spring-boot-starter-data-jpa

- Spring Boot configures Hibernate as the default JPA provider, so it’s no longer necessary to define the entityManagerFactory bean unless we want to customize it.

- Spring Boot can also auto-configure the dataSource bean, depending on the database used. In the case of an in-memory database of type H2, HSQLDB and Apache Derby, Boot automatically configures the DataSource if the corresponding database dependency is present on the classpath.

### What is an "instant repository"? (hint: recall Spring Data)

        @Repository
        public interface CategoryRepository extends JpaRepository< Category, Long >

- An instant repository is a repository resulting from extending some class representing an implementation of Repository interface (for example JpaRepository) or annotating some class with @RepositoryDefinition.

### How do you define an “instant” repository? Why is it an interface not a class?

- Either extend some implementation of a repository interface or annotate the future instant repo class with @RepositoryDefinition. Spring data has many built in functions and there is no code needed.

### What is the naming convention for finder methods?

- ![finder methods](https://github.com/seanjgildea/CoreSpring5CertificationGuide/blob/master/img/finder-methods.png)

### How are Spring Data repositories implemented by Spring at runtime?

First of all, there's no code generation going on, which means: no CGLib, no byte-code generation at all. The fundamental approach is that a JDK proxy instance is created programmatically using Spring's ProxyFactory API to back the interface and a MethodInterceptor intercepts all calls to the instance and routes the method into the appropriate places

- Spring Data repositories are implemented by using fragments that form a repository composition. Fragments are the base repository, functional aspects such as QueryDsl and custom interfaces along with their implementation.

### What is @Query used for?

- The Spring @Query annotation is used to customize methods(queries) of some instant repository
- @Query("select p from Person u where p.name like %?1%")
- List findAllByName(String name);


[<< Back to Table of Contents](README.md)
