## Introduction:
### The need of Spring framework
- In object-oriented programming(OOP) `cohesion and coupling` are two important `design principles` used to evaluate the design and organization of classes and their relationships within a system.

#### Cohesion: 
Cohesion refers to the degree to which the elements (methods and attributes) within a single module (such as a class) are related and work together to achieve a common purpose or functionality. High cohesion is desirable because it leads to more modular, understandable, and maintainable code. Modules with high cohesion tend to have **tightly focused responsibilities**, meaning that `each class or module` does `one thing` and `does it well`. 
**It is always good to have HIGH COHESION**

#### Coupling:
Coupling refers to the degree of interdependence between modules or classes. It measures how `closely connected classes or modules are to each other`. `Low coupling` is `desirable` because it `promotes flexibility`, `reusability`, and `maintainability`. When classes are loosely coupled, changes to one class are less likely to require changes to other classes.
**It is always good to have LOOSE/LOW COUPLING**
In summary, `cohesion` focuses on how closely related and focused the elements within a module are, while `coupling` focuses on the degree of interdependence between modules or classes. 
Good design principles advocate for `high cohesion and low coupling` to create modular, maintainable, and flexible software systems.

In current situation in lossely coupled and tightly coupled we are directly or indirectly dependeing on another class in tightly coupled (has a relationship) one class is direct depending upon another class and we need to `create the object of the other class` and provide it.
Same in lossly coupled we provide the object using setter or constructor method but still we need to create the object by ourself(using new keyword)/ on runtime the object is created and provide the whole object on runtime.

**So when we want the responsibility to creating the object or the dependency of object creation is removed**:
Hence we take the help of `spring framework` which is capable to `create an object` and we can ask it to pass the object when we need it.
So, all the classes object is stored inside a **container** which is called **spring container**. These container is capable of creating the object and provide it to us. This the object is created on runtime and provide to the code. 
- With the help of `spring framework` we are `reversing the control`, this phenomena is called **Inversion of Controll**.
- When we want any object we can ask the spring for the object and spring will `inject the object` to the perticular class so this is called **Dependency Injection**.
We can achive this things in **2 ways**:
1. Dependency Injection
2. Autowiring

### Depecndency Injection:
- We create the container using `ApplicationContext`:
```java
ApplicationContext applicationContext = 
				new ClassPathXmlApplicationContext
				("com/accenture/lkm/resources/my_springbean.xml");
```
- In `ClassPathXmlApplicationContext` we need to pass the path where we store the object
- With this single particular object `applicationContext` we can `access all the objects`. It is the entry point of all the object





