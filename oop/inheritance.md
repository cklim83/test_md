# Inheritance

## Section Links
[Class Inheritance](#class-inheritance)\
[super](#super)\
[Mixin Modules](#mixing-in-modules)\
[Inheritance vs Modules](#inheritance-vs-modules)\
[Method Lookup Path](#method-lookup-path)\
[Modules for Namespacing](#modules-for-namespacing)\
[Modules as Method Containers](#modules-as-method-containers)\
[Accidental Method Overriding](#accidental-method-overriding)

---


## Class Inheritance
- Inheritance occurs when a class inherit behaviors from another class.
- The class inheriting behaviors is called the **subclass** while the class it inherits from is called the **superclass**.
- Inheritance allows us to place common behaviors of related classes to a common superclass and make them available to those classes without repeating code (DRY principle). Any future changes to these behaviors can also be made at one place, making the code more maintainable.
- In example below, the `speak` behavior is common to all animal types and can thus be extracted to an `Animal` superclass. Whichever animal types that needs it can inherit from this superclass.
```ruby
class Animal
  def speak
    "Hello!"
  end
end

class GoodDog < Animal
end

class Cat < Animal
end

sparky = GoodDog.new
paws = Cat.new
puts sparky.speak           # => Hello!
puts paws.speak             # => Hello!
```

- The `<` symbol indicates the class on the left is inheriting from the class on the right. In our example, both `GoodDog` and `Cat` inherits from `Animal` giving objects of these class access to the `speak` method.
- Ruby also provides the option to **override** inherited methods. In our example below, the `speak` method in `Animal` is overridden by the `speak` method defined in `GoodDog`. This works because Ruby always start its search for a method from the object's class according to the method lookup path.
```ruby
class Animal
  def speak
    "Hello!"
  end
end

class GoodDog < Animal
  attr_accessor :name

  def initialize(n)
    self.name = n
  end

  def speak
    "#{self.name} says arf!"
  end
end

class Cat < Animal
end

sparky = GoodDog.new("Sparky")
paws = Cat.new

puts sparky.speak           # => Sparky says arf!
puts paws.speak             # => Hello!
```

[Back to top](#section-links)


## super
The `super` keyword will call a method with the **same name** as the one it is residing within and that occur one search position after the current class in the method lookup path. In the example below, `super` within `GoodDog#speak` will invoke `Animal#speak` which will return `"Hello!"`. We then append additional text to generate `"Hello! from GoodDog class"`.
```ruby
class Animal
  def speak
    "Hello!"
  end
end

class GoodDog < Animal
  def speak
    super + " from GoodDog class"
  end
end

sparky = GoodDog.new
sparky.speak        # => "Hello! from GoodDog class"
```

`super` is commonly used in `initialize` to ride on parental implementation plus add on functionality. It can take one of 3 forms, depending on the arguments required by the recipient method invoked:

1. All arguments received auto-forwarded i.e. `super`
```ruby
class Animal
  attr_accessor :name

  def initialize(name)
    @name = name
  end
end

class GoodDog < Animal
  def initialize(color)
    super  # equivalent to Animal#initialize(color)
    @color = color
  end
end

bruno = GoodDog.new("brown")        # => #<GoodDog:0x007fb40b1e6718 @color="brown", @name="brown">
```
Here, `super` within `GoodDog#initialize` will invoke `Animal#initialize` and pass it `color` as an argument which is then assigned to `name`.

2. Selected arguments passed i.e. `super(a, b)`
```ruby
class BadDog < Animal
  def initialize(age, name)
    super(name)
    @age = age
  end
end

BadDog.new(2, "bear")        # => #<BadDog:0x007fb40b2beb68 @age=2, @name="bear">
```
In above example, `super` in `BadDog#initialize` only pass the argument `name` up the method lookup chain to `Animal#initialize`. `age` is not passed but assigned to instance variable `@age` since this is only applicable to the `BadDog` class.

3. No arguments forwarded `super()`
```ruby
class Animal
  def initialize
  end
end

class Bear < Animal
  def initialize(color)
    super()
    @color = color
  end
end

bear = Bear.new("black")        # => #<Bear:0x007fb40b1e6718 @color="black">
```
In example 3, `super()` within `Bear#initialize` imply that `Animal#initialize` is invoked with no argument passed in.

[Back to top](#section-links)


## Mixing in Modules
For behaviors that does not fit nicely into a hierarchical structure, modules can be used to house common methods that are only applicable to some classes so that code need not be repeated. We can then include (i.e. mixin) these modules into those classes to grant them access to those methods.
```ruby
module Swimmable
  def swim
    "I'm swimming!"
  end
end

class Animal; end

class Fish < Animal
  include Swimmable         # mixing in Swimmable module
end

class Mammal < Animal
end

class Cat < Mammal
end

class Dog < Mammal
  include Swimmable         # mixing in Swimmable module
end
```

```ruby
sparky = Dog.new
neemo  = Fish.new
paws   = Cat.new

sparky.swim                 # => I'm swimming!
neemo.swim                  # => I'm swimming!
paws.swim                   # => NoMethodError: undefined method `swim' for #<Cat:0x007fc453152308>
```
`Dog`, `Fish` and `Cat` are subclasses of `Animal` but only `Dog` and `Fish` objects can `swim`. 

Modules are commonly named using a verb describing the behavior group + 'able' suffix e.g. `Walkable`, `Swimmable`.

[Back to top](#section-links)


## Inheritance vs Modules
**Class inheritance** refers to the inheritance of behaviors from another type, resulting in the creation of a new type that specializes the superclass.

**Interface inheritance** refers to the inheritance of an interface provided by a mixin module. No new specialized type is created with respect to the module.


Considerations when choosing between class inheritance vs mixins:
- We can only subclass from **one** class but can mix in as many modules as needed
- Use class inheritance for **"is-a"** relationship, interface inheritance for **"has-a"** relationship
- We cannot create objects from modules, they are only used for namespacing and grouping common methods together.

[Back to top](#section-links)


## Method Lookup Path
Method lookup path is the **search order** in which classes and mixin modules are inspected when we call a method. To get the lookup path in array form, we can invoke the class method `::ancestors` on the class of interest

```ruby
module Walkable
  def walk
    "I'm walking."
  end
end

module Swimmable
  def swim
    "I'm swimming."
  end
end

module Climbable
  def climb
    "I'm climbing."
  end
end

class Animal
  include Walkable

  def speak
    "I'm an animal, and I speak!"
  end
end

puts "---Animal method lookup---"
puts Animal.ancestors
```

```irb
---Animal method lookup---
Animal
Walkable
Object
Kernel
BasicObject
```
 This means that when we call a method on any `Animal` object, Ruby will first search for the method in the following order: `Animal` class -> `Walkable` module -> `Object` class -> `Kernel` module -> `BasicObject` class. At any point the method is found, it will be invoked and the search ends. Otherwise it continue its search in the next class or module in the search sequence. If the method is still not found after the entire search sequence is completed, a `NoMethodError` will be raised.
 
 ```ruby
fido = Animal.new
fido.speak                  # => I'm an animal, and I speak!
```
Ruby found `speak` in `Animal` class and looked no further

```ruby
fido.walk                   # => I'm walking.
```
Ruby looked for `walk` in `Animal` and couldn't find it. It then move on to next class/module in the method lookup path, in this case the `Walkable` module, found `walk`, execute it and look no further.

```ruby
fido.swim
  # => NoMethodError: undefined method `swim' for #<Animal:0x007f92832625b0>
```
Ruby search through all the classes and modules in the lookup path of `Animal` but could not find the `swim` method so it threw a `NoMethodError`


```ruby
class GoodDog < Animal
  include Swimmable
  include Climbable
end

puts "---GoodDog method lookup---"
puts GoodDog.ancestors
```

```irb
---GoodDog method lookup---
GoodDog
Climbable
Swimmable
Animal
Walkable
Object
Kernel
BasicObject
```
- Order of module inclusion matters. When multiple modules are included, the **last** included module is searched **first**. Hence `Climbable` appear before `Swimmable` in the method lookup path. 
- Lookup path includes modules included in super class.

[Back to top](#section-links)


## Modules for Namespacing
Namespacing involves the grouping of similar classes using a module to reduce the chance of collision with similarly named classes.
```ruby
module Mammal
  class Dog
    def speak(sound)
      p "#{sound}"
    end
  end

  class Cat
    def say_name(name)
      p "#{name}"
    end
  end
end

buddy = Mammal::Dog.new
kitty = Mammal::Cat.new
buddy.speak('Arf!')           # => "Arf!"
kitty.say_name('kitty')       # => "kitty"
```

[Back to top](#section-links)


## Modules as Method Containers
Modules can also be used as containers to **house methods for direct use (i.e. not via mixin as a method in a class)**. To do that we need to **prefix method names with `self`**.  These methods can then be used directly without inclusion in a class. Similar to class methods, `self` here refers to the module itself.
```ruby
module Mammal
  ...
  puts self
	
  def self.some_out_of_place_method(num)
    num ** 2
  end
end

# outputs Mammal (due to puts self)
```

We could invoke the module method using the following syntax, the former preferred
```ruby
value = Mammal.some_out_of_place_method(4)

# or

value = Mammal::some_out_of_place_method(4)
```

[Back to top](#section-links)


## Accidental Method Overriding
Every class we create inherently subclass from class `Object`. We confirm this by invoking the class method `::superclass` on the class of interest.
```ruby
class Parent
  def say_hi
    p "Hi from Parent."
  end
end

Parent.superclass       # => Object
```

There are important methods from `Object` that we do not want to override. `send` is such an example as it provides a way to invoke a method using the method name (in string or symbol format) as an argument.
```ruby
class Child < Parent
  def say_hi
    p "Hi from Child."
  end
end

child = Child.new
child.say_hi         # => "Hi from Child."
```

```ruby
son = Child.new
son.send :say_hi       # => "Hi from Child."
```

We will lose the default `Object#send` functionality when we accidentally override the `send` instance method in our class, leading to unexpected behavior.
```ruby
class Child
  def say_hi
    p "Hi from Child."
  end

  def send
    p "send from Child..."
  end
end

lad = Child.new
lad.send :say_hi
```

```irb
ArgumentError: wrong number of arguments (1 for 0)
from (pry):12:in `send'
```
Instead of invoking `Object#send` with `:say_hi` as argument, which will invoke `Child#say_hi`, Ruby invokes `Child#send` instead and got a unexpected argument error.

`instance_of?` is another useful instance method from `Object` we do not want to override. This method returns `true` when an object is an instance of a class and `false` otherwise.
```ruby
c = Child.new
c.instance_of? Child      # => true
c.instance_of? Parent     # => false
```

When we override this method in `Child` class, we get an unexpected result and the functionality is lost.
```ruby
class Child
  # other methods omitted

  def instance_of?
    p "I am a fake instance"
  end
end

heir = Child.new
heir.instance_of? Child
```

```irb
ArgumentError: wrong number of arguments (1 for 0)
from (pry):22:in `instance_of?'
```

The only instance method from `Object` that is useful to override is `to_s` to allow us to format the output in a useful way. It is important to familiarize ourselves with common `Object` methods so that we do not override them unintentionally and lose critical functionality.

[Back to top](#section-links)