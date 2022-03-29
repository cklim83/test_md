# Inheritance on Variable Scope

## Section Links
[Inheritance On Instance Variable Scope](#inheritance-on-instance-variable-scope)\
[Inheritance On Class Variable Scope](#inheritance-on-class-variable-scope)\
[Inheritance On Constant Variable Scope](#inheritance-on-constant-variable-scope)

---

## Inheritance On Instance Variable Scope
Unlike methods, instance variables and their values are **only available if initialized**, else they return `nil` when accessed.


**Example: Instance Variable From A Superclass**
```ruby
class Animal
  def initialize(name)
    @name = name
  end
end

class Dog < Animal
  def dog_name
    "bark! bark! #{@name} bark! bark!"    # can @name be referenced here?
  end
end

teddy = Dog.new("Teddy")
puts teddy.dog_name                       # => bark! bark! Teddy bark! bark!
```
`Dog::new` called `Animal#initalize` with `"Teddy"` as argument and `@name` got initialized to `"Teddy"`. Hence `@name` used in `Dog#dog_name` got interpolated to `"Teddy"`.

```ruby
class Animal
  def initialize(name)
    @name = name
  end
end

class Dog < Animal
  def initialize(name); end

  def dog_name
    "bark! bark! #{@name} bark! bark!"    # can @name be referenced here?
  end
end

teddy = Dog.new("Teddy")
puts teddy.dog_name                       # => bark! bark! bark! bark!
```
Here, `@name` was never initialized as `Animal#initialize` was never called. `Dog::new` called `Dog#initialize` with `"Teddy"` as argument but never do anything with it. Hence `@name` returned `nil` which interpolates to "".


**Example: Instance Variable From Included Module**
```ruby
module Swim
  def enable_swimming
    @can_swim = true
  end
end

class Dog
  include Swim

  def swim
    "swimming!" if @can_swim
  end
end

teddy = Dog.new
teddy.swim                                  # => nil
```
As teddy did not invoke `enable_swimming`, `@can_swim` remain uninitialized which when referenced returns `nil`. Hence `teddy.swim` returns `nil` as the if conditional is falsey.

```ruby
teddy = Dog.new
teddy.enable_swimming
teddy.swim                                  # => swimming!
```
For `#swim` to return `swimming`, we need to invoke `enable_swimming` first to set the instance variable `@can_swim` to `true`.

[Back to top](#section-links)


## Inheritance On Class Variable Scope
- Class variables have a very insidious behavior of allowing sub-classes to override super-class class variables. Further, the change will affect all other sub-classes of the super-class. 
	- This is because all related classes share a single copy of the class variable.
	- Any reassignment of a class variable by subclass affects all related classes inheriting that class variable.
	- The value of the class variable will be based on its last reassigned value.

- As this behaviour is extremely unintuitive, it is recommended we avoid using class variables altogether.


```ruby
class Animal
  @@total_animals = 0

  def initialize
    @@total_animals += 1
  end
end

class Dog < Animal
  def total_animals
    @@total_animals
  end
end

spike = Dog.new
spike.total_animals                           # => 1
```
`Dog::new` invokes `Animal#initialize` which increments `@@total_animals` by 1. `Dog#total_animals` was able to access class variable `@@total_animals` defined in superclass `Animal` to returns 1

**Example: Unexpected Behavior of Class Variable Under Inheritance**
```ruby
class Vehicle
  @@wheels = 4

  def self.wheels
    @@wheels
  end
end

Vehicle.wheels                              # => 4
```
Returns 4 as expected

```ruby
class Motorcycle < Vehicle
  @@wheels = 2
end

Motorcycle.wheels                           # => 2
Vehicle.wheels                              # => 2  Yikes!
```
However, reassignment of class variable `@@wheels` in `Motorcycle` affects the value in superclass too! 

```ruby
class Car < Vehicle
end

Car.wheels                                  # => 2  Not what we expected!
```
New subclass that does not inherit the class that reassigns `@@wheels` also got affected. 

```ruby
class Tricycle < Vehicle
  @@wheels = 3
end

Vehicle.wheels                              # => 3
Car.wheels                                  # => 3
Motorcycle.wheels                           # => 3
Tricycle.wheels                             # => 3
```
All classes sharing the class variable will be referencing the last assigned value.

[Back to top](#section-links)


## Inheritance On Constant Variable Scope
- Constants have lexical scope: Any reference to a constant will be search within the group of code the constant appears in. 

- Constant resolution will look at the lexical scope first, and then look at the inheritance hierarchy

- We can use `self.class::` to reference constants for flexible binding so that it works even in subclasses.

**Example: Unrelated Classes Have Separate Scope**
```ruby
class Dog
  LEGS = 4
end

class Cat
  def legs
    LEGS
  end
end

kitty = Cat.new
kitty.legs                                  # => NameError: uninitialized constant Cat::LEGS
```
Ruby seeks to find `LEGS` referenced in `Cat#leg` within `Cat` (lexical scope) but couldn't find it, resulting in NameError. It does not search for `LEGS` in `Dog` since `Dog` defines another scope 

**Example: Resolution Operator :: Allows Cross Scope Access**
```ruby
class Dog
  LEGS = 4
end

class Cat
  def legs
    Dog::LEGS                               # added the :: namespace resolution operator
  end
end

kitty = Cat.new
kitty.legs                                  # => 4
```
We can refer to constant from another scope using the `::` resolution operator.

**Example: A Child Class Inherits Constant From Its Parent**
```ruby
class Vehicle
  WHEELS = 4
end

class Car < Vehicle
  def self.wheels
    WHEELS
  end

  def wheels
    WHEELS
  end
end

Car.wheels                                  # => 4

a_car = Car.new
a_car.wheels                                # => 4
```
If a constant is not defined in a class, Ruby will look up its parent classes to find the constant.

**Example: Constant Referenced In Mixin Module Have Own Lexical Scope**
```ruby
module Maintenance
  def change_tires
    "Changing #{WHEELS} tires."
  end
end

class Vehicle
  WHEELS = 4
end

class Car < Vehicle
  include Maintenance
end

a_car = Car.new
a_car.change_tires                          # => NameError: uninitialized constant Maintenance::WHEELS
```
Although not explicitly expressed in code, `WHEELS` referenced in `change_tires` is implicitly binded to `Maintenance` due to lexical scope of constants.

We can fix it by being explicit using `::` resolution operator

```ruby
module Maintenance
  def change_tires
    "Changing #{Vehicle::WHEELS} tires."    # this fix works
  end
end
```
or 
```ruby
module Maintenance
  def change_tires
    "Changing #{Car::WHEELS} tires."        # surprisingly, this also works
  end
end
```

or we could use `self.class::` to dynamically resolve this to class of the calling object.
```ruby
module Maintenance
  def change_tires
    "Changing #{self.class::WHEELS} tires."        # surprisingly, this also works
  end
end
```

**Example: Lexical Scope of Constant to Parent Class**
```ruby
class Car
  WHEELS = 4

  def wheels
    WHEELS
  end
end

class Motorcycle < Car
  WHEELS = 2
end

civic = Car.new
puts civic.wheels # => 4

bullet = Motorcycle.new
puts bullet.wheels # => 4, when you expect the out to be 2
```
The unexpected output from `puts bullet.wheels` is due to the lexical scoping of the constant `WHEELS` in `Car` i.e. any reference to `WHEELS` within the `Car` class definition is actually binded to `Car::WHEELS`. Thus even though `Motorcycle` have its own `WHEELS` constant, the inherited `wheels` method is still referencing `Car::WHEELS` value

```ruby
class Car
  WHEELS = 4

  def wheels
    WHEELS
  end
end

class Motorcycle < Car
  WHEELS = 2

  def wheels
    WHEELS
  end
end

civic = Car.new
puts civic.wheels # => 4

bullet = Motorcycle.new
puts bullet.wheels # => 2
```
One way to solve this is to explicitly define a `Motorcycle#wheels` method and return `WHEELS` from it which will bind to `Motorcycle::WHEELS`.

OR
```ruby
class Car
  WHEELS = 4

  def wheels
    puts "self is #{self}"
    self.class::WHEELS
  end
end

class Motorcycle < Car
  WHEELS = 2
end

civic = Car.new
puts civic.wheels 
# self is #<Car:0x00007fc08a1d0cc0>
# 4
# => nil

bullet = Motorcycle.new
puts bullet.wheels
# self is #<Motorcycle:0x00007fc08a1e2ec0>
# 2
# => nil
```
Alternatively, we can also bring the behaviour of constants in line with how we expect other variables by explicitly specifying the scope of the constant, by using `self.class::WHEELS` in the `wheels` instance method.

```ruby
class Car
  WHEELS = 4

  def wheels
    puts "self is #{self}"
    self.class::WHEELS
  end
end

class Motorcycle < Car
end

civic = Car.new
puts civic.wheels 
# self is #<Car:0x00007f842314d9e0>
# 4
# => nil

bullet = Motorcycle.new
puts bullet.wheels
# self is #<Motorcycle:0x00007f8422877be0>
# 4
#=> nil
```
If `Motorcyle` did not have its own `WHEELS` initialized, it will use the `WHEELS` inherited from `Car`

[Back to top](#section-links)