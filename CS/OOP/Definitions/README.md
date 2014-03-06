Object Orientated Programming Methodology and Definitions
=========

####What is a Class?
A Class is a blue print for an Object and is defined only once. Its defines the initial values for State and implementations of behaviour

####What is an Object?
An Object is an Instance of a class. it is  defined by a class but created many times, as required.

####What is instantiation?
Is the Creation of a real Instance ... It is a particular realisation of an Abstract Object.

####What is a Method?
a Method is a sub route, procedure or function defined as part of a class and operates on an Object and its data members

####What is a Virtual Method?
Is an Abstract method, (interface method), whose signature (i.e. name, return type and parameters) is defined without any implementation. It is inherited an Implemented by another (child class);

####What is a Static or Class Method/Variable?
A Static method or variable is a method or variable that is defined only once. it does not require instantiation and cannot variy with an instaniated object's data members i.e does not vary across instances
e.g. Math.PI is a static variable that always returns the constant value of PI

####What is an Instance Method?
are defined one but implementation can variy for each instance based on the instances data members/ variables.
e.g. Shape.getArea() would depend on the size or type of the shape


####What is 'this' in OOP?
The ‘this’ is a reference to the current Object and is used inside the Object class to refer/access to other members of the class from within.

####What is Static VS Class initialisers?
Static Initialisers preform once for all Instances of a Class while Class Initialisers are preformed with each initialisation

####What is a Constructor?
A Constructor is the Method 1st called when an Object is being created.

####What is a Destructor and a Finalize Method?
A Destructor is the Method called when an Object is being destroyed. In Java this method doesn't exist as we cannot control when an object is being destroyed but, there is a Finalize method which is called before Garbage collection (no Guarantee of when this will happen)

####What is a Super/Base Class?
A super class is the Parent class from which the current class Inherits some behaviours/members from

####What is a Sub/derived Class?
A Sub/derived Class is a Child Class that inherits members from another class (parent)

####What is Inheritance?
It is a paradigm for reusing code by "inheriting" some or all members of another class for reuse, extension and overriding.

####What is Encapsulation?
Encapsulation  is a paradigm used to hide information. Members can be defined as Private preventing them from being directly accessed and can be interacted using Accessor Methods such as getter and setters.

####What is Multiple Inheritance?
This is Occurs when we need to inherit functionality / members form more then one class. In Java we cannot explicitly inherit from more then one class but this can be achieved using interfaces (if a class implements another class it indirectly inherits from the class)

####What is Delegation/Forwarding?
Delegation/Forwarding is a Paradigm used to call some piece of code that is not known till run time. It allows us to define an object’s behaviour in terms of another object’s behaviour. Alternative to inheritance
example:
```java
public interface Worker() {
    public Result work();
}
public class Secretary() implements Worker {
    public Result work() {
        Result myResult = new Result();
        return myResult;
    }    
}
public class Boss() implements Worker {
    private Secretary secretary;
    public Result work() {
        return secretary.work();
    }   
}
```

####What is Aggregation?
Is a Special form of Association, when an Object A "Has a" another Object B, A Aggregates B 
 -----<> (empty Diamond)

####What is Composition?
Composition is a Special form of Aggregation where Object A "has a" another Object B and Object B cannot exist without Object A
------<%> (filled in Diamond)

####What is Abstraction?
Abstraction is a pattern specifying a Framework without an implementation.  e.g. an Abstract Class cannot be instantiated but can be inherited by another class which may or may not be instantiated 

What is an Interface or Protocol?
----
is a way to communicate between unrelated Classes. It is also a way to indirectly inherit.

####What is method overriding?
Method overriding is when a Subclass inherits a method but provides its own implementation, which may or may not include a call to the Original Inherited method using super()

####What is Method Overloading?
Method Overloading is when the same Method name is used for multiple methods but with a different parameter list (can also have a different return type)

####What is Polymorphism?
* means many forms
* is the ability to present and use the same interface for different underlying forms (data types) e.g. the Arithmetic interfaces (+, -, /, *) can be used with double, int, BigDecimal...etc. 
* As well as the use of Subtypes using their parents methods, such as a ford and a BMW are both subclasses of Car so we can just call the Car's method to go, this way we can treat both as a Car and ignore the details when they are needed (even if things are implemented differently, we do not need to be aware of it as the go method for both are either calling the parent or overriding the parent and we do not need to be aware of this when calling the method)

###What is a Methods Signature?
A methods Signature consists of a Methods name, return type and parameter list. It is all that is included when declaring in an abstract call or interface

###What are the different Method visibilities and what access do they provide?
public - any class can access it
protected - any subclass(child) or any class part of the same package may access it
private - only accessible by the members of the class where the private method is defined.

###What is Loose Coupling?
Loose Coupling refers to when modules/components require little knowledge of other modules in order to use them.
In a Loosely coupled system, components can be swapped out for alternative implementations without updating other modules (e.g. if you program to an interface you may have a test interface for unit testing and another interface for production but the calling code should not need to be aware of either case, just supplied the interface to call)

###What is High Cohesion?
Refers to When a Class has a clearly defined Job (low Cohesion would occur when a class has a lot of jobs/procedures that don't have enough in common with them. e.g. CompileAndPrint Class should be 2 separate classes, Compile class and  Print class)

#SOLID:

* #####S - Single Responsibility principle
	A Class should only have a single responsibility ( only a single reason why it might change)
	-> based on Principle of Cohesion
    
* #####O - Open close Principle 
    Entities should be Open for Extension and Closed for modification ( in order to modifiy we should inherit class or implement an interface).
    
* #####L- Liskov principle 
    An Object should be replicable with Instances of Subtypes without altering the correctness of the program.( think of a Square as a child of a Rectangle and the GetWidth method)

* #####I - interface Segregation 
   Many Client specific interfaces are better than an all-purpose Interface who has methods not required by the implementing class

* #####D - Dependency Inversion 
    One should depend on Abstractions and Not Concrete Classes (program to interfaces)

