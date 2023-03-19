# Core Technologies
## The IoC Container
### Introduction
- IoC is Inversion of Control, is also known as dependency injection.
- It is a process whereby object define their dependencies. (constructor, factory method and set properties)
- From bean itself controlling the instantiation to container controlling.
#### Basis packages
##### org.springframework.beans
- A bean is an object that is instantiated, assembled, and managed by a Spring IoC container. Otherwise, a bean is simply one of many objects in your application. 
- Beans, and the dependencies among them, are reflected in the configuration metadata used by a container.
###### BeanFactory interface 
- provides an advanced configuration mechanism capable of managing any type of object.
##### org.springframework.context
###### ApplicationContext
- is a sub-interface of BeanFactory, It adds:
    1. Easier integration with Spring’s AOP features
    2. Message resource handling (for use in internationalization)
    3. Event publication
    4. Application-layer specific contexts such as the WebApplicationContext for use in web applications.
## Container Overview
- The ```org.springframework.context.ApplicationContext ```interface represents the Spring IoC container and is responsible for instantiating, configuring, and assembling the beans by reading configuration metadata which is represented in XML, Java annotations or Java code.
- Servral implementations of the ```ApplicationContext``` interface are supplied with Spring:
    1. ```ClassPathXmlApplicationContext```
    2. ```FileSystemXmlApplicationContext```
- While XML has been the traditional format for defining configuration metadata, you can instruct the container to use Java annotations or code as the metadata format by providing a small amount of XML configuration to declaratively enable support for these additional metadata formats.
- In most application scenarios, explicit user code is not required to instantiate one or more instances of a Spring IoC container. For example, in a web application scenario, a simple eight (or so) lines of boilerplate web descriptor XML in the web.xml file of the application typically suffices.
- Business Objects & Configuration metadata -> The Spring Container -produce-> Fully configured system (Ready for use)
### Configuration Metadata
- XML
    ```
    #The id attribute is a string that identifies the individual bean definition.
    #The class attribute defines the type of the bean and uses the fully qualified class name.
    <beans>
        <import resource="xxx.xml">
        <bean id="" class="">
            <property name="" ref=""/>
        </bean>
    </beans>
    ```
- Annotation-based configuration
- Java-based configuration
### Instantiating a Container
- supply the location path or paths to a constructor to let container load configuration metadata.
    ```
    ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");
    ```
### Using the Container
    ```
    // create and configure beans
    ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");

    // retrieve configured instance
    PetStoreService service = context.getBean("petStore", PetStoreService.class);

    // use configured instance
    List<String> userList = service.getUsernameList();
    ```
## Bean Overview
- Within the container itself, these bean definitions are represented as BeanDefinition objects, which contain (among other information) the following metadata:
    1. A package-qualified class name: typically, the actual implementation class of the bean being defined.
    2. Bean behavioral configuration elements, which state how the bean should behave in the container (scope, lifecycle callbacks, and so forth).
    3. References to other beans that are needed for the bean to do its work. These references are also called collaborators or dependencies.
    4. Other configuration settings to set in the newly created object — for example, the size limit of the pool or the number of connections to use in a bean that manages a connection pool.
- The bean definition
    1. Class
    2. Name
    3. Scope
    4. Constructor arguments
    5. Properties
    6. Autowiring mode
    7. Lazy initialization mode
    8. Initialization method
    9. Destruction method
### Determining a Bean's Runtime Type
- is non-trivial to determine.
- a specified class in the bean metadata definition is just an initial class reference.
- potentially combined with a declared factory method or being a FactoryBean class which may lead to a different runtime type.
- Additionally, AOP proxying may wrap a bean instance with an interface-based proxy with limited exposure of the target bean’s actual type 
- The recommended way to find out about the actual runtime type of a particular bean is a BeanFactory.getType call for the specified bean name. 
## Dependency Injection
- Code is cleaner with the DI principle, and decoupling is more effective when objects are provided with their dependencies. 
- The object does not look up its dependencies and does not know the location or class of the dependencies. 
- As a result, your classes become easier to test, particularly when the dependencies are on interfaces or abstract base classes, which allow for stub or mock implementations to be used in unit tests.
### Constructor-based Dependency Injection
- it is a good rule of thumb to use constructors for mandatory dependencies.
### Setter-based Dependency Injection
- use setter methods or configuration methods for optional dependencies.
- only it can resolve circular dependencies.
### Dependency Resolution Process
- ApplicationContext is created and initialized with configuration metadata.
- For each bean, its dependencies are expressed in the form of properties, constructor arguments, or arguments to the static-factory method. These dependencies are provided to the bean, when the bean is actually created.
- Each property or argument is an actual definition of the value to set, or a reference to another bean in the container.
- Each property or constructor argument that is a value is converted from its specified format to the actual type of that property or constructor argument. By default, Spring can convert a value supplied in string format to all built-in types, such as int, long, String, boolean, and so forth.
- The Spring container validates the configuration of each bean as the container is created. However, the bean properties themselves are not set until the bean is actually created. 
### Using depends-on
### Lazy-initialized Beans
- pre-instantiation is desirable, because errors in the configuration or surrounding environment are discovered immediately
### Autowiring Collaborators
- Autowiring can significantly reduce the need to specify properties or constructor arguments.
- Autowiring can update a configuration as your objects evolve. For example, if you need to add a dependency to a class, that dependency can be satisfied automatically without you needing to modify the configuration. 
#### Autowiring modes
- no
- byNmae
- byType
- constructor
#### Disadvantage
- Explicit dependencies always override autowiring
- is less exact than explicit wiring
- may not be available to tools that may generate documentation from a Spring container
- Multiple bean definitions may match the type specified.
    1. Abandon autowiring in favor of explicit wiring
    2. Avoid autowiring for a bean definition by setting its autowire-candidate attributes to false.
    3. Designate a single bean definition as the primary candidate by setting the primary attribute of its bean element to true.
    4. Implement the more fine-grained control available with annotation-based configuration. 
### Method Injection
```
package fiona.apple;

// no more Spring imports!

public abstract class CommandManager {

    public Object process(Object commandState) {
        // grab a new instance of the appropriate Command interface
        Command command = createCommand();
        // set the state on the (hopefully brand new) Command instance
        command.setState(commandState);
        return command.execute();
    }

    // okay... but where is the implementation of this method?
    protected abstract Command createCommand();
}
```
```
<!-- a stateful bean deployed as a prototype (non-singleton) -->
<bean id="myCommand" class="fiona.apple.AsyncCommand" scope="prototype">
    <!-- inject dependencies here as required -->
</bean>

<!-- commandProcessor uses statefulCommandHelper -->
<bean id="commandManager" class="fiona.apple.CommandManager">
    <lookup-method name="createCommand" bean="myCommand"/>
</bean>

```
#### Arbitrary Method Replacement (less use)
## Bean Scopes
### singleton
- (Default) Scopes a single bean definition to a single object instance for each Spring IoC container.
### prototype
- Scopes a single bean definition to any number of object instances.
### request
- Scopes a single bean definition to the lifecycle of a single HTTP request. That is, each HTTP request has its own instance of a bean created off the back of a single bean definition. Only valid in the context of a web-aware Spring ApplicationContext.
### session
- Scopes a single bean definition to the lifecycle of an HTTP Session. Only valid in the context of a web-aware Spring ApplicationContext.
### application
- Scopes a single bean definition to the lifecycle of a ServletContext. Only valid in the context of a web-aware Spring ApplicationContext.
### websocket
- Scopes a single bean definition to the lifecycle of a WebSocket. Only valid in the context of a web-aware Spring ApplicationContext.
### Scopes Beans as Dependencies
```
<bean id="userPreferences" class="com.something.UserPreferences" scope="session">
    <aop:scoped-proxy/>
</bean>

<bean id="userManager" class="com.something.UserManager">
    <property name="userPreferences" ref="userPreferences"/>
</bean>
```
### Custom Scopes
#### Creating a Custom Scope
- need to implement the org.springframework.beans.factory.config.Scope interface
```
Object get(String name, ObjectFactory<?> objectFactory)
Object remove(String name)
void registerDestructionCallback(String name, Runnable destructionCallback)
String getConversationId()
```
#### Using a Custom Scope
- register first
```
# This method is declared in ConfigurableBeanFactory
void registerScope(String scopeName, Scope scope);
Scope threadScope = new SimpleThreadScope();
beanFactory.registerScope("thread", threadScope);
```
## Customizing the Nature of a Bean
