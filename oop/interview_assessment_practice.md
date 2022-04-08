# Interview Assessment Preparation

## Explain what is OOP?

## Explain encapsulation with the help of an example.
- Encapsulation refers to the **deliberate hiding of internal representation** of an object and only **selectively exposing methods** and properties to **control** how external users can **interact** with objects belonging to a class. This helps protect information (state) stored within an object from unintended access or change.
- In the example below, when a new credit card object is created, it will auto-generate a 16 digit number. We also deliberately prevent users from changing the name and number on the card after creation, only allowing them to view the last 4 digits and validate the name and numbers on the card.

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

## Explain polymorphism with the help of an example.
Polymorphism is a phenomenon where different object types can all respond to a common method invocation. This can be achieved through class inheritance or duck typing. Polymorphism is useful in programming as it offers developers the flexibility to invoke a method call on a pool of different object types without discerning which type they fall under.

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


## Explain, with examples, the relationship between classes and objects.
A class is akin a mold that outlines attributes and methods that will be present in objects created from that class. Objects of a class are instantiations of that class. They can have different states but all will have the same methods.

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

## What are modules and when are they useful? Mixins, Method container, Namespacing
Modules are containers of code that can house methods to be used as class mixins for interface inheritance, as a generic method container and to namespace related classes to avoid name collision.

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

## Explain Method Lookup Path
Method Lookup path is the sequence of Classes and/or Modules used by Ruby when it is searching for a method. It always starts with the class in question,
followed by the mixins it contains, and then its superclasses and their mixins as required. We can view the lookup method by invoking the class method
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



## Explain the difference between states and behaviours
States represent the values stored in objects while behaviours are actions that can be undertaken by those objects. Objects of the same class can have different states but will definitely have the same behaviours.

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

## Explain the difference between public, private and protected access modifiers with an example
`public`, `private` and `protected` are access modifiers for instance methods of a class. They control who can make use of these methods. `public` methods can be called by any instance of an object outside the class definition. `private` methods cannot be called by objects outside the class. They can only be called by instance methods of a class or `self` within the class definition. `protected` is similar to `private` methods but they can be called by both `self` and other instances of the same class within the class definition.

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


## Why is it preferable to access instance variables using getters and setters, where available, in instance methods?
It is preferable to use getters and setters where available as any built-in customisation can be consistently applied.


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

## Explain the difference between class and instance methods
Instance methods provide functions for objects of a class and are invoked by them. Class methods, which are prefixed with `self.`, provide necessary functions for the class and are invoked on the class directly, without the need to create an object of that class. 

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

## Explain the difference between class, instance variables and constants
Instance variables has a single `@` prefix while class variables have `@@` prefix. Instance variables are scoped at the instance level, with each object having its own instance variable but class variables are scoped at the class level. All class and their subclasses will share a single copy of the class variable. This explains why we obvious the abnormal behaviour when using class variables with inheritance. 
Class constants are variables beginning with capitalize letter and are initialized within class definition but outside instance methods. They have a lexical scope.

```ruby
class Dog
  SIZE = "medium"
  
  def to_s
    "This is a #{self.class::SIZE} sized dog!"
  end
end

class GoldenRetriever < Dog
  SIZE = "large"
end

std_dog = Dog.new
puts std_dog
my_golden = GoldenRetriever.new
puts my_golden
```
## Explain the use of super with examples

## What are different meaning of self




