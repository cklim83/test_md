# Interview Assessment Preparation

## Section Links
[Explain Encapsulation With An Example](#explain-encapsulation-with-an-example)\
[Explain Polymorphism With An Example](#explain-polymorphism-with-an-example)\
[Explain The Relationship Between Classes And Objects](#explain-the-relationship-between-classes-and-objects)\
[What Are Modules And How Are They Useful?](#what-are-modules-and-how-are-they-useful)\
[Explain Method Lookup Path](#explain-method-lookup-path)\
[Explain The Difference Between States And Behaviours](#explain-the-difference-between-states-and-behaviours)\
[Explain The Difference Between Public, Private And Protected](#explain-the-difference-between-public-private-and-protected)\
[Why Is It Preferable To Access Instance Variables Through Getters And Setters Whenever Available?](#why-is-it-preferable-to-access-instance-variables-through-getters-and-setters-whenever-available)\
[Explain The Difference Between Class And Instance Methods](#explain-the-difference-between-class-and-instance-methods)\
[Explain The Difference Between Class, Instance Variables And Constants](#explain-the-difference-between-class-instance-variables-and-constants)\
[Explain The Use Of `super`](#explain-the-use-of-super)\
[What Are The Different Meanings Of Self?](#what-are-the-different-meanings-of-self)\
[Complex Example Involving Self, Inheritance, Method Lookup and Class and Instance Methods](#complex-example-involving-self-inheritance-method-lookup-and-class-and-instance-methods)


## Explain Encapsulation With An Example
- Encapsulation involves restricting direct access to some components (data and/or methods) of an object of a class by controlling the public interfaces available to external users. This helps to safeguard an object's internal state from unintentional modification and is also used to hide implementation details users do not need to know about.  
- In the example below, we intentianally hid `generate_number` method from users of `CreditCard` class as they do not need to be concerned with how the number is generated and in what form. Getters and setters were also not available as we do not want users to be able to reveal any sensitive information or made any changes of these field once object is created. Users are limited to viewing the last four digits or validate the card details.
```ruby
class CreditCard
  def initialize(name)
    @name = name
    @number = generate_number
  end
  
  def last_four_digits
    ending_digits = number.split("-").last
    
    "Card ends with: #{ending_digits}"
  end
  
  def valid_card?(card_name, card_number)
    name == name && number == card_number
  end
  
  private

  attr_reader :name, :number
  
  def generate_number
    numbers = []
    4.times { numbers << ('0000'..'9999').to_a.sample }
    numbers.join("-")
  end

end

david_card = CreditCard.new("David")
david_card.last_four_digits

```

[Back to top](#section-links)


## Explain Polymorphism With An Example
Polymorphism is a phenomenon where **different object types** can all **respond to a common method invocation**. This can be achieved through **class inheritance** or **duck typing**. Polymorphism offers **flexibility** in programming as we can **call a method on a pool of object types** without discerning what type each object belongs to.
```ruby
# Polymorphism by Inheritance
class Animal
  def move
    "walking"
  end
end

class Dog < Animal
end

class Kangaroo < Animal
  def move
    "hopping"
  end
end

class Fish < Animal
  def move
    "swimming"
  end
end

animals = []
animals << Dog.new
animals << Kangaroo.new
animals << Fish.new

animals.each do |animal|
  puts animal.move
end
```

```ruby
# Polymorphism by duck typing (Unrelated types i.e. no inheritance relationship with same interface)
class Guitarist
  def perform
    "play guitar"
  end
end

class Dancer
  def perform
    "dance"
  end
end

class Comedian
  def perform
    "tell jokes"
  end
end

talents = [Guitarist.new, Dancer.new, Comedian.new]
talents.each { |talent| puts talent.perform }
```

[Back to top](#section-links)


## Explain The Relationship Between Classes And Objects
A class is akin to a **mold** that outlines the **type of attributes and methods present** in objects created from that class. Objects of a class are instantiations of that class. They can have different states but all will have the same methods.

In the example below, we define a `Dog` class and created 2 objects (`spotty` and `rex`) with different names and ages from that class.
```ruby
class Dog
  DOG_TO_HUMAN_YEARS = 7
  
  def initialize(name, age_years)
    @name = name
    @age_years = age_years   
  end
  
  def speak
    puts "#{@name} says Woof!"
  end
  
  def age_in_human_years
    @age_years * DOG_TO_HUMAN_YEARS
  end
end

spotty = Dog.new("Spots", 3)
rex = Dog.new("Rex", 1)

spotty.speak
puts spotty.age_in_human_years

rex.speak
puts rex.age_in_human_years
```

[Back to top](#section-links)


## What Are Modules And How Are They Useful?
Modules are containers of code and they useful as mixins, method container or in namespacing.
- **Mixins** are modules that can be included in multiple classes to share common methods. They are particularly useful in situations those methods do not fit nicely into a hierachical structure demanded of class inheritance.
- Modules can also be use as **class containers** to hold generic methods to be used in any program without inclusion in a class.
- Modules are also used in **namespacing** to house related classes to avoid name collision.
```ruby
# As mixin
module Movable
  def move
    "moving"
  end
end

class Car
  include Movable
end

class Tree
end

class Boat
  include Movable
end

Car.new.move
Boat.new.move
```

```ruby
# As Generic Method Container
module Computable
  def self.add(a, b)
    a + b
  end
  
  def self.subtract(a, b)
    a - b
  end
end

puts Computable.add(3, 5)
puts Computable.subtract(5, 8)
```

```ruby
# As Namespace for related Classes
module Shape
  class Rectangle
    def initialize(length, breadth)
      @length = length
      @breadth = breadth
    end
    
    def area
      @length * @breadth
    end
  end
  
  class Square < Rectangle
    def initialize(side)
      super(side, side)
    end
  end
  
  class Circle
    def initialize(radius)
      @radius = radius
    end
    
    def area
      (Math::PI * @radius * @radius).round(2)
    end
  end
end

my_square = Shape::Square.new(5)
p my_square.area

my_circle = Shape::Circle.new(10)
p my_circle.area
```

[Back to top](#section-links)


## Explain Method Lookup Path
Method lookup path is the sequence of Classes and/or Modules visited by Ruby when searching for a method. It always starts with the class in question,
followed by the mixins it contains, and then its superclasses and their mixins. The search stops once the first same named method is found. If no same named method is found after Ruby exhausted the list of classes/modules, a `NoMethodError` is raised. We can view the lookup method by invoking the class method
::ancestors on the class of interest.

```ruby
module Formattable
  def format
  end
end

module Printable
end

class MyClass
  include Formattable
  include Printable
end

MyClass.ancestors   # => [MyClass, Printable, Formattable, Object, Kernel, BasicObject] 
```

[Back to top](#section-links)


## Explain The Difference Between States And Behaviours
States represent the values stored in objects while behaviours are actions that can be undertaken by those objects. Objects of the same class can have different states but have the same behaviours.

In the example below, we have two objects of `Person` class. They have different value for their name (state) but both are able to greet (behaviour).

```ruby
class Person
  def initialize(name)
    @name = name
  end
  
  def greet
    puts "hello"
  end
end

tom = Person.new("Tom")
david = Person.new("David")
p tom
p david
tom.greet
david.greet
```

[Back to top](#section-links)


## Explain The Difference Between Public, Private And Protected
`public`, `private` and `protected` are **access modifiers** for instance methods of a class. They **control** who can **access** of these methods.
- `public` methods can be called by **any instance** of that class **outside the class definition**.
- `private` methods **cannot** be called by objects outside the class. They can only be called by instance methods of a class or `self` within the class definition.
- `protected` is similar to `private` methods but they can be called by **both `self` and other instances** of that class **within** the class definition.

```ruby
class Circle
  def initialize(radius)
    @radius = radius
  end
  
  def print_area
    puts "The circle has an area of #{area}"
  end
  
  def ==(other)
    radius == other.radius
  end
  
  protected
   
  attr_reader :radius
  
  private
  
  def area
    (Math::PI * @radius * @radius).round(2)
  end
end

circle_a = Circle.new(10)
circle_b = Circle.new(5)

circle_a.print_area
circle_a == circle_b
circle_a.area
```

[Back to top](#section-links)


## Why Is It Preferable To Access Instance Variables Through Getters And Setters Whenever Available?
It is preferable to use getters and setters where available as any **built-in customisation** can be **consistently applied**. In the example below, we process any input string to remove any non-letters before the assignment. This ensure all inputs are cleaned up before assigning to instance variables.
```ruby
  class Person
    attr_reader :name
    
    def name=(given_name)
      @name = given_name.gsub(/[^A-Za-z]/, "").capitalize
    end
  end
  
  tom = Person.new
  tom.name = " tommy 35!"
  tom.name
```

[Back to top](#section-links)


## Explain The Difference Between Class And Instance Methods
- **Instance methods** provide functions for **objects** of a class and are invoked by them.
- **Class methods**, which are prefixed with `self.`, provide necessary functions for the **class** and are invoked on the class directly, without the need to create an object of that class. 

```ruby
class Dog
  @@dog_count = 0
  
  attr_reader :name
  
  def initialize(name)
    @name = name
    @@dog_count += 1
  end
  
  def bark
    "#{name} barking: Woof Woof!!"
  end
  
  def self.total
    @@dog_count
  end
end

Dog.total            # => 0

spotty = Dog.new("Spotty")
puts spotty.name     # => "Spotty"
Dog.total            # => 1

spotty.bark          # => "Spotty barking: Woof Woof!!"
browny = Dog.new("Browny")
puts browny.name     # => "Browny"
Dog.total            # => 2

class Shiba < Dog
  @@dog_count = 10
end

Shiba.total          # => 10
Dog.total            # => 10
```

[Back to top](#section-links)


## Explain The Difference Between Class, Instance Variables And Constants
- **Instance variables** have a single `@` prefix and are used to reference values for an object. They are scoped at the instance level.\
- **Class variables** have `@@` prefix, and are scoped at the class level. They are used to store value for the class. There is only a single copy of class variable even when the class variable is made available through inheritance to subclasses. This explains why we may see unintuitive behaviour when multiple related classes all operate on a particular class variable.\ 
- **Class constants** are variables beginning with a **capital letter**. They are initialized within the boundaries of a class definition but outside of instance methods. They have a **lexical scope** (i.e. they tend to be binded to the group of code they are defined in).

```ruby
# Example for Class Constant and Lexical Scope
class Dog
  SIZE = "medium"
  
  def to_s
    "This is a #{SIZE} sized dog!"
  end
end

class GoldenRetriever < Dog
  SIZE = "large"
end

std_dog = Dog.new
puts std_dog                     # => "This is a medium sized dog!"
my_golden = GoldenRetriever.new
puts my_golden                   # => "This is a medium sized dog!"
```

[Back to top](#section-links)


## Explain The Use Of `super`
`super` is a keyword used to invoke a same named method next up in the method lookup path. It is often used to reuse functionality already defined in the superclass and potentially add-on extra functionality. It can be involved in one of the following forms:
- `super`: all arguments to the method it resides in is forwarded to the same named method
- `super(a, b)`: only selected arguments are forwarded to the same named method
- `super()`: no arguments are forwarded to the same named method
```ruby
class Car
  def initialize(brand)
    @brand = brand
  end
  
  def to_s
    "#{self.class} by #{@brand}!"
  end
end

class ElectricVehicle < Car
  def initialize(brand, battery_rating)
    super(brand)
    @battery_rating = battery_rating
  end
  
  def to_s
    super + " Go Green!"
  end
end

ev = ElectricVehicle.new("Tesla", 1000)   # => #<ElectricVehicle:0x0000000001253450 @brand="Tesla", @battery_rating=1000> 
puts ev                                   # => "ElectricVehicle by Tesla! Go Green!"
```

[Back to top](#section-links)


## What Are The Different Meanings Of Self?
`self` can refer to one of the following
- the object calling the instance method. It is this identity that we use to call setter to differentiate from local variable assignment.
- the class itself
- the module `self` is in

```ruby
class Car
  @@car_count = 0
  
  puts self
  
  def initialize(brand)
    @brand = brand
    @@car_count += 1
  end
  
  def self.car_count
    @@car_count
  end
  
  def print_self
    puts self
  end
end

Car.car_count              # => 0
my_car = Car.new("Audi")
Car.car_count              # => 1
my_car.print_self          # => #<Car:0x0000000001c35e00>
```

```ruby
# self in Module Method Container
module Movable
  puts self
  
  def self.move
    puts "moving"
  end
end

Movable.move               # => "moving"
```

```ruby
module MyMixin
  def special
    puts "#{self} within special"
  end
end

class MyClass
  include MyMixin
end

my_class = MyClass.new     # => #<MyClass:0x0000000001ed85e0> 
my_class.special           # => "#<MyClass:0x0000000001ed85e0> within special"
```

Beside the various meaning of `self`.  We need to prefix `self` if we need to call the setter method to distinguish it from local variable assignment. This is optional when calling getters
```ruby
class GoodDog
  attr_accessor :name, :height, :weight

  def initialize(n, h, w)
    self.name   = n
    self.height = h
    self.weight = w
  end

  def change_info(n, h, w)
    self.name   = n
    self.height = h
    self.weight = w
  end

  def info
    "#{self.name} weighs #{self.weight} and is #{self.height} tall."
  end
end
```

[Back to top](#section-links)


## Complex Example Involving Self, Inheritance, Method Lookup and Class and Instance Methods

In the example below, modify only `GrandParent` class to get the output "From GrandParent class method, we want to print Tom". Explain how we arrive at this output with the solution provided.

```ruby
class GrandParent
  # Add your modifications below

end

class Parent 
  attr_reader :name
  
  def initialize(name)
    @name = name
  end
  
  def which_method
    "From Parent instance method"
  end
end

class Child < Parent
  def info
    "#{self.class.which_method}, we want to print #{person.name}"
  end
end

tom = Child.new("Tom")
tom.info  # => "From GrandParent class method, we want to print Tom" 
```


**Solution**
```ruby
class GrandParent
  # Add your modifications below
  def self.which_method
    "From GrandParent class method"
  end
  
  def person
    self
  end
end

class Parent < GrandParent
  attr_reader :name
  
  def initialize(name)
    @name = name
  end
  
  def which_method
    "From Parent instance method"
  end
end

class Child < Parent
  def info
    "#{self.class.which_method}, we want to print #{person.name}"
  end
end

tom = Child.new("Tom")
tom.info  # => "From GrandParent class method, we want to print Tom" 
```
- `tom.info` will execute `Child#info`
- Interpolation of `self.class.which_method` will invoke class method `::which_method` rather than instance method`#which_method` since `self.class` returns a `Child` class. Even though `Child` does not have a class method `which_method`, it can get this from the `GrandParent` class through inheritance. Hence we can add `which_method` class method in `GrandParent` to get the first part of the required string. `#{self.class.which_method}` returns `"From GrandParent class method"``. From this example, we can see that **class methods, like instance methods, are also inherited.**
- Interpolation of `#{person.name}` caused Ruby to search for a variable or method named `person`. To get this to return the value referenced by `@name` without changing `Child` or `Parent` class, we create a `#parent` method in `GrandParent` that returns `self`. This caused `parent.name` to be evaluated to `self.name`, with `self` referring to `tom`, a `Child` object, which also have access to `#name` attribute reader from `Parent` to return value referenced by `@name`.

[Back to top](#section-links)