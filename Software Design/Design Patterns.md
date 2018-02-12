------------------------------------------------------------------------------------------
Design Patterns
------------------------------------------------------------------------------------------
http://stackoverflow.com/questions/1673841/examples-of-gof-design-patterns

1. What are some alternatives to inheritance?

    One of general recommendations (or some call it OOP design principles) is to favor composition over inheritance.
    For example in strategy pattern the client object is composed with a behavior object (strategy) and this behavior can be changed in runtime. Favoring composition over inheritance leads to a more flexible design. (Of course in each case a separate decision whether to subclass or to compose with an object has to be made. There is no universal approach)

2. Give an example where you prefer abstract class over interface ?

    One good example comes from java.awt package. There is an abstract class MouseAdapter that implements several interfaces MouseListener, MouseWheelListener, MouseMotionListener. In general there is no need to write a class that implements all the methods from e.g. MouseListener interface. Instead we can subclass from MouseAdapter and override only the methods that we're interested in.

    In general abstract class should be used when there is some default behavior that should be reused. The motivation behind this is changes that are expected to be done in future. If you change some method implementation in an abstract class, all its subclasses will automatically have this updated behavior. If you change an interface -- all classes that implement it will be broken. Another good way to think about abstract class vs interface is thinking about "is a" vs "can be" relation

    Example:

    TODO: find a good example

3. What are wrapper classes? Why do we need wrapper classes?  



4. Can you please explain Strategy design pattern?



5. Can you please explain Observer design pattern? Observer and Observable in Java


.
6. Give example of decorator design pattern in Java ? Does it operate on object level or class level ?



7. What is Adapter design pattern ? Give examples of adapter design pattern in Java?



8. What is Chain of Responsibility design pattern ?



9. What is Singleton design pattern in Java ? Write code for thread-safe singleton in Java



10. Give an insight into such patterns as Façade/Proxy/Decorator/Strategy/Observer (selectively)



11. What is main benefit of using factory pattern ? Where do you use it?



12. What is MVC design pattern ? Give one example of MVC design pattern?



13. Which patterns do you use in a daily basis. Explain their principles.



14. Can you name few design patterns used in standard JDK library? What major patterns do the Java APIs utilize?

    Decorator design pattern is widely used in Java IO and to provide unmodifiable and/or synchronized views for collections

        TODO: example

    Factory methods and factories

        TODO: example

    Observers are used in java.awt to process keyboard and mouse events

        TODO: example

    Prototype: clone() method

    Strategy: interface Comparable and compare() method

        TODO: example

    MVC in Swing libraries

15. What is FrontController design pattern in Java ? Give an example of front controller pattern ?
