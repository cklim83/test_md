# Open Ended Practice Questions
## Section Links
[OOP](#oop)\
[Classes and Objects](#classes-and-objects)\
[Encapsulation](#encapsulation)\
[Method Access Control](#method-access-control)\
[Polymorphism](#polymorphism)\
[Inheritance](#inheritance)\
[Modules](#modules)\
[Method Lookup Path](#method-lookup-path)\
[Constants](#constants)\
[Getters and Setters](#getters-and-setters)

---


## OOP
### What is OOP and why is it important?
Object oriented programming is a software engineering **approach** to **model a problem domain** as a **system of interacting objects**. Objects, created from classes acting as molds, contain information (states) and perform actions on each other (methods) to fulfill requisite functions within the problem domain. 

This approach is important as it help **overcome the tight dependency drawback of procedural programming**, where small changes made in one location can cause a **big ripple effect** across the entire program (possibly also contributed by the numerous data exchanges via arguments). This meant software systems become increasingly hard to maintain as they grow in scale and complexity.

By utilising abstraction, encapsulation, inheritance and polymorphism, OOP made software easier to design, maintain and scale.

- **Abstraction** allow developers to design solutions based on 'real-world' concepts and create high-level models using basic data structures such as numbers, strings, arrays and hashes.

- **Encapsulation** allow developers to **mask internal representations** of objects and **selectively expose methods to provide requisite functionality**. By controlling the interaction boundary, it confers the following benefits:
	- Safeguard data integrity of objects.
	- Reduce code dependency. Implementation changes can be isolated within classes and not affect the rest of the program as long as public method signatures, i.e. arguments and return objects, remained unchanged.

- **Inheritance** allow developers to build on top of basic classes with large reusability to define more specialized subclasses without repeating code.

- **Polymorphism** gives different object types the ability to respond to a common method invocation, granting developers greater flexibility during development. 


Benefits of OOP
- Creating objects allows programmers to think more abstractly about the code they are writing.
- Objects are represented by nouns so are easier to conceptualize.
- It allows us to only expose functionality to the parts of code that need it, meaning namespace issues are much harder to come across.
- It allows us to easily give functionality to different parts of an application without duplication.
- We can build applications faster as we can reuse pre-written code.
- As the software becomes more complex this complexity can be more easily managed.


[Encapsulation & Polymorphism](encapsulation_and_polymorphism.md)\
[Inheritance](inheritance.md#class-inheritance)


### What is a spike?
A spike refers to temporary code written to explore a problem domain. They are useful in testing hypothesis, and understanding design trade-offs. Learnings can then be used to shape program design such as required classes, types of collaborator objects, methods and their placements.

[Explore Problem before Design](coding_tips.md#explore-problem-before-design)\
[Spike in OOP process](steps_to_model_in_oop.md#steps)


### What may be a sign you are missing a class in OOP?
The presence of **repetitive nouns** in the problem description could be an indication that a class might be used to model that noun. **Actions** undertaken by that noun could form its **methods** and the **information held** could form its **state**.

[Repetitive Nouns](coding_tips.md#lookout-for-repetitive-nouns)


## Classes and Objects
### What is an object?
An object is an instance of a class. It can hold information (state) and have certain behaviours (methods). Different objects can hold different information even though they are of the same class.

```ruby
class Dog
  attr_reader :name
  def initialize(name)
    @name = name
  end
  
  def speak
    "#{name} Woof!"
  end
end

rex = Dog.new("Rex")
teddy = Dog.new("Teddy")
rex.speak                  # => Rex Woof!
teddy.speak                # => Teddy Woof!

rex.class == teddy.class   # => true
```

[Objects](objects_and_classes.md#objects)


### How do you initialize a new object?
We can instantiate a new object from a class by calling the `::new` method on the class and pass in any argument that might be required by the `initialize` method. 

[Initialize New Object](objects_and_classes.md#initializing-a-new-object)

### What is an instance variable, and how is it related to an object?
An instance variable is a variable within an object that begins with a `@`. It references a state of that object and exists as long as the object instance exists.

[Instance Variable](objects_and_classes.md#instance-variables)


### What is an instance method?
An instance method is a method that can be invoked by instances of a class (assuming it is public). They have **direct access** to all instance variables defined in that class.

[Instance Methods](objects_and_classes.md#instance-methods)


### How can you see if an object has instance variables?
We can inspect that object using the `p` method. `p` will invoke `inspect` on that object that will generate the internal state of that object into a string for output into the console, similar to `puts obj.inspect`. The instance variables are those that begin with `@`. Uninitialized instance variables will not be shown (see `@age` below). 

```ruby
class Dog
  attr_accessor :name, :age
  def initialize(name)
    @name = name
  end
end

browny = Dog.new("Browny")
p browny       # => #<Dog:0x00007fd135936120 @name="Browny">
browny.age = 3
p browny       # => #<Dog:0x00007fd135936120 @name="Browny", @age=3>
```


### What is a class? What is the relationship between a class and an object?
A class is similar to a mold used for creating objects. It defines the states and methods of objects created from it. An object is an instantiation of a class. Different objects can hold different values but have a common set of behaviour (methods).

[Classes and Objects](objects_and_classes.md)


### What is the difference between states and behaviours?
States track values of an object while behaviours represent actions (methods) an object can perform. 

### Classes also have behaviours not for objects (class methods). How do you define a class method?
Class methods are actions that can be performed on a class. They are prefixed with `self.` and are invoked by the class themselves rather than instances of a class.
```ruby
class Dog
  @@dog_count = 0 
  
  def initialize
    @@dog_count += 1
  end
	
  def self.count  # class method
    @@dog_count
  end
end

Dog.count  # => 0
rex = Dog.new
Dog.count  # => 1
```

[Class Methods](objects_and_classes.md#class-methods)


### What is a collaborator object? What is the purpose of using collaborator objects in OOP?
A collaborator object is any object stored as states, i.e. assigned to instance variables, within another object. It can be of custom user defined class or non-custom class i.e. standard ruby classes such as `String`, `Integer`, `Array` etc. 

Collaborator objects are used to model relationships between interacting objects of different types. These objects work in conjunction with their host objects to perform certain functions. Their instance methods are also available for use by their host objects.
```ruby
class Person
  def initialize(name)
    @name = name
  end
end

class Cat
  def initialize(name, owner)
    @name = name
    @owner = owner
  end
end

sara = Person.new("Sara")
fluffy = Cat.new("Fluffy", sara)
```
- `"Sara"` is a **non-custom** collaborator object (`String`) within the `Person` object referenced by the local variable `sara`
- `"Fluffy"` is a **non-custom** collaborator object (`String`) within the `Cat` object referenced by the local variable `fluffy`
- The `Person` object referenced by `sara` is a **custom** collaborator object within the `Cat` object referenced by `fluffy`
- The `Cat` object referenced by `fluffy` is **not a** collaborator object since it is not assigned to any instance variable belonging to another object.

[Collaborator Objects](collaborator_objects.md)

## Encapsulation
### What is encapsulation? How does encapsulation relate to public interfaces of a class?

[See above for encapsulation](#what-is-oop-and-why-is-it-important)

The public interfaces of a class are the methods deliberatively made available for users of a class to interact with its objects. 


### How do objects encapsulate state?
States are encapsulated through the use of public methods, which control how we reference and modify an object's state. 

### Why should a class have as few public methods as possible?
As instance methods have **direct access to instance variables that track an object's state**, users may modify an object's state **both intentionally or unintentionally through their public methods**. Hence, it is important to keep the number of public methods to what is minimally required for data integrity sake.

## Method Access Control
### What is a private method call used for?
A `private` method call is an access modifier used within a class definition. When invoked, any methods below (and up till another access modifier invocation) will have their access set to `private`. Private methods can only be used within the class definition by other instance methods or by its caller i.e. `self`. These methods cannot be used outerside the class definition.

### What is a protected method call used for?
A `protected` method call is an access modifier used within a class definition. When invoked, any methods below (and up till another access modifier invocation) will have their access set to `protected`. Protected methods can only be used within the class definition by other instance methods and all instances (`self` or another object) of the same class or their subclasses. These methods cannot be used outerside the class definition.

## Polymorphism
### What is polymorphism?
[See polymorphism above](#what-is-oop-and-why-is-it-important)


### Explain two different ways to implement polymorphism.
Polymorphism can be implemented using either class inheritance or duck typing. 

For related classes that share an is-a relationship, common methods in the more generalized superclass will be inherited by the specialized subclass through class inheritance. Objects of both classes are then able to invoke these common methods. 

In duck typing, unrelated classes that have a common behavior will implement that the common behavior using a common interface (same method name and arguments). In duck idiom, one doesn't need to be sure an object is a duck type, as long as it moves and quacks like a duck, we can assume it is a duck.

Polymorphism either through class inheritance or duck typing provides developers with the flexibility to invoke a common methods on objects of multiple types without the need to check which type an object belongs to.

```ruby
class Cat
  def move
    "walk"
  end
end

class Tiger < Cat
  def move            # inherit and override common method
    "sprint"
  end
end

class Bird
  def move            # duck typing common interface
    "fly"
  end
end

animals = [Cat.new, Tiger.new, Bird.new]
animals.each do |animal|
  puts animal.move    # polymorphism
end
```


### What is duck typing? How does it relate to polymorphism. What problem does it solve?
See above.


## Inheritance
### What is inheritance?
Class inheritance involves the creation of a new, specialized type where behaviours (methods) belonging to the more generalized superclass are made available to the subclass. Ruby practices single inheritance: each class can only have at most one superclass. 

Interface inheritance is another form of behavioural inheritance, but without the creation of a new type. It involves the inclusion of modules within classes so that objects of these classes have access to the methods defined in the included module.

In both cases, behaviours (methods) are made available from the superclass/module to the subclass/containing class.

[Inheritance](inheritance.md)


### What is the difference between a superclass and a subclass?
A superclass is the parent that avails its methods to the subclass. A superclass tends to be more generalized while a subclass tends to be a more specialized form of the superclass.


### When is it good to use inheritance?
It is suitable to class inheritance when two classes has an "is-a" relationship and share similarity in a range of behaviours. The common behaviours can then be placed in the superclass to be inherited by the subclass. Subclass can customise certain inherited methods if there are differences through method overriding.


### Give an example of using the super method, both with and without an argument.
The `super` method invokes a method with same name as the one super resides in and is the next in the method lookup path search sequence in one of 3 forms:
- `super`: all arguments to the current method are forwarded.
- `super(a, b)`: selected arguments are forwarded
- `super()`: the higher up method is invoked without arguments.
```ruby
class Vehicle
  def initialize(manufacturer)
    @manufacturer = manufacturer
  end
	
  def move
    "Moving"
  end
end

class Car < Vehicle
  def initialize(manufacturer, engine_capacity)
    super(manufacturer)
	@engine_cc = engine_capacity
  end
	
  def move
    super + "at faster speed!"
  end
```

[super](inheritance.md#super)


### Give an example of overriding: when would you use it?
```ruby
class Person
  attr_accessor :name, :age
  def initialize(name, age)
    @name = name
	@age = age
  end
	
  def to_s
    "#{name} is #{age} years old"
  end 
end

colleague = Person.new("Cheryl", 35)
puts colleague
```
We override a method when we want to modify the default behaviour of an inherited method to suit the distinct need of a class. For example, we tend to override the `to_s` from `Object` so that it provide a more useful description of our custom object when printed by `puts` or interpolated in strings.

[Overriding to_s](objects_and_classes.md#to_s-method)\
[Accidental Overriding](inheritance.md#accidental-method-overriding)


### In inheritance, when would it be good to override a method?
See above

### Are class variables accessible to subclasses?
Class variables are accessible to subclasses. In fact all related classes share a single copy of a class variable such that any reassignment of a class variable affects all related classes using it.

```ruby
class A
  @@secret = "TopSecret"
  @@name = "AAA"
  
  def self.name
    @@name
  end
end

class B < A
  @@name = "BBB"
  
  def self.secret
    @@secret
  end
end

class C < A
  @@name = "CCC"
end
      
B.secret  # => "TopSecret"
A.name    # => "CCC"
B.name    # => "CCC"
C.name    # => "CCC"
```

[Class Variable Inheritance](inheritance_and_variable_scope.md#inheritance-on-class-variable-scope)


## Modules
### What is a module?
A module is a container used that can be used to house a collection of code for one of the following purposes:
- Namespacing: Related classes are grouped within a module to avoid collision with similarly named classes
```ruby
module Shapes
  class Circle
    # ...
  end
	
  class Rectangle
    # ...
  end
end

my_circle = Shapes::Circle.new
```
- Method containers for common methods to be inherited by multiple classes via mixins
```ruby
module Swimmable
  def swim
    "am swimming"
  end
end

class Dog
  include Swimmable
end

my_dog = Dog.new
my_dog.swim  # => "am swimming"
```
- Method containers for generic methods to be used outright
```ruby
module Computation
  def self.multiply(a, b)
    a * b
  end
  
  def self.addition(a, b)
    a + b
  end
end

Computation.multiply(3, 4)  # => 12
Computation::addition(3, 4) # => 7
```

[Modules](modules.md#modules)\
[Namespacing](inheritance.md#modules-for-namespacing)\
[Mixin Modules](inheritance.md#mixing-in-modules)\
[Method Containers](inheritance.md#modules-as-method-containers)


### What is a mixin?
A mixin is a module included within a class for interface inheritance so that its methods are made available for use by instances of that class.


### What is namespacing?
See above


## Method Lookup Path
### What is the method lookup path?
The method lookup path is a sequence of class/modules by which Ruby will traverse when searching for a method during invocation. We can access the method lookup path on a class of interest by invoking the `::ancestor` on that class. 

The method lookup path will start from the class of interest, any included modules (latest one will appear first), then its superclass and its included modules all the way till the `BasicObject`.
```ruby
module A
end

module B
end

module C
end
class Parent
  include A
end

class Child < Parent
  include B
  include C
end

Child.ancestors # => [Child, C, B, Parent, A, Object, Kernel, BasicObject]
```

[Method Lookup Path](inheritance.md#method-lookup-path)


## Constants
### How can we reference a constant initialized within a different class?
To use that constant in an unrelated class, we need to prepend the class containing the constant followed by `::`
```ruby
class A
  VALUE = 10
end

class B
  def constant_from_a
    A::VALUE
  end
end

B.new.constant_from_a    # => 10
```

[Constant Scope](inheritance_and_variable_scope.md#inheritance-on-constant-variable-scope)


### How are constants used in inheritance?
Constants exhibit lexical scope in inheritance. That means a referenced constant tend to be implicitly binded in the code group (e.g. class or module) it is defined.
```ruby
class A
  SECRET = "TopSecret"
  
  def self.reveal_secret
    SECRET  # implicitly binded to A::SECRET
  end
end

class B < A
  SECRET = "NewSecret"
end

B.reveal_secret  # => "TopSecret"
```

A subclass will search for a constant in the superclass if it is not found in the subclass itself.
```ruby
class A
  SECRET = "TopSecret"
end

class B < A
  def self.reveal_secret
    SECRET
  end
end

B.reveal_secret  # => "TopSecret"
```

[Constant Scope in Inheritance](inheritance_and_variable_scope.md#inheritance-on-constant-variable-scope)


## Getters and Setters
### What is a getter method?
A `getter` method is an instance method meant to retrieve the value of an instance variable. By convention, it is common to name `getter` the same as the instance variable they are referencing. We can either define `getter` method manually or using `attr_reader` to automatically create a plain vanilla `getter`.

[Accessor Methods](objects_and_classes.md#accessor-methods)


### What is a setter method?
A `setter` method is an instance method meant to set the value of an instance variable. By convention, it is common to name `setter` the same as the instance variable they are setting and ending it with an `=` suffix. We can either define `setter` method manually or using `attr_writerr` to automatically create a plain vanilla `setter`.

[Accessor Methods](objects_and_classes.md#accessor-methods)