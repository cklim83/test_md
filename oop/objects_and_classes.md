# Classes and Objects

## Section Links
[Objects](#objects)\
[Classes](#classes)\
[Initializing a New Object](#initializing-a-new-object)\
[Instance Variables](#instance-variables)\
[Instance Methods](#instance-methods)\
[Accessor Methods](#accessor-methods)\
[Class Methods](#class-methods)\
[Class Variables](#class-variables)\
[Constants](#constants)\
[to_s Method](#to_s-method)\
[More About self](#more-about-self)

---


## Objects
In Ruby, anything with a **value** is an object: that include numbers, strings, arrays, even _classes and modules_. The following are however **not** objects: methods, blocks and variables.

```Ruby
# Testing whether something is an object using Object#is_a?

'hello'.is_a?(Object)             # => true
0.is_a?(Object)                   # => true
['one', 2, :three].is_a?(Object)  # => true
Hash.new.is_a?(Object)            # => true
nil.is_a?(Object)                 # => true
false.is_a?(Object)               # => true

module Printable
  def print_to_console
    puts "Printing to console"
  end
end
Printable.is_a?(Object)           # => true

class EmptyClass
end
EmptyClass.is_a?(Object)          # => true
my_object = EmptyClass.new
my_object.is_a?(Object)           # => true
```

Objects are created from classes (the latter are also Objects)

**Class** = template/mold for making objects\
**Object** = something made from a template/mold

Individual objects can contain different information (i.e. states), even when they are instances of the same class
```Ruby
irb :001 > "hello".class
=> String
irb :002 > "world".class
=> String
```

[Back to top](#section-links)


## Classes
In Ruby, a class definition outline states and behaviours of objects constructed from that class. States track attributes for individual objects while behaviors describe what those objects can do. **Instance variables** keep track of state and **instance methods** expose behaviors for objects.

Classes are defined within `class` ... `end` keywords. Their names should be in **CamelCase** and containing files in **snake_case** reflecting the class name. 

```ruby
# good_dog.rb
class GoodDog
 ...
end
```

[Back to top](#section-links)


## Initializing a New Object
We can instantiate an object from a class using the `::new` class method. This will invoke the `initialize` method (aka constructor) of that class under the hood with any arguments forwarded to create the object.
```Ruby
class GoodDog
  def initialize
    puts "This object was initialized!"
  end
end

sparky = GoodDog.new        # => "This object was initialized!"
```

[Back to top](#section-links)


## Instance Variables
Instance variables are prepended with `@` symbol and allows data to be tied to objects. They are responsible for tracking states (values) of an object and exists as long as the object instance exists.

```ruby
class GoodDog
  def initialize(name)
    @name = name
  end
end

sparky = GoodDog.new("Sparky")
```
In above example, the string `"Sparky"` is passed to the `initialize` method via the `new` method and assigned to local variable `name`. Within the body of `initialize`, we initialize the instance variable `@name` to reference the same string referenced by `name`.

[Back to top](#section-links)

## Instance Methods
Instance methods are methods that can be invoked by instances of a class.
```ruby
class GoodDog
  def initialize(name)
    @name = name
  end

  def speak
    "#{@name} says arf!"
  end
end

sparky = GoodDog.new("Sparky")
puts sparky.speak           # => Sparky says arf!
fido = GoodDog.new("Fido")
puts fido.speak             # => Fido says arf!
```
They have direct access to instance variables (and also constants and class variables) defined within a class

[Back to top](#section-links)


## Accessor Methods
To allow an object to access the value of an instance variable, we need to define accessor method for that instance variable. Without that, a `NoMethodError` will be raised
```ruby
puts sparky.name
NoMethodError: undefined method `name' for #<GoodDog:0x007f91821239d0 @name="Sparky">
```

Here, we define `get_name` getter method to return the value in the `@name` instance variable and `set_name=` setter method to modify object referenced by `@name`. `sparky.set_name = "Spartacus"` is actually _syntactical sugar_ for invoking the setter method and equivalent to `sparky.set_name=("Spartacus")`.
```ruby
class GoodDog
  def initialize(name)
    @name = name
  end

  def get_name
    @name
  end

  def speak
    "#{@name} says arf!"
  end
	
  def set_name=(name)
    @name = name
  end
end

sparky = GoodDog.new("Sparky")
puts sparky.speak     # Sparky says arf!
puts sparky.get_name  # Sparky
sparky.set_name = "Spartacus"
puts sparky.get_name  # Spartacus
```

By convention, Rubyist typically want to name getter and setter methods using the same name as the instance variable they are exposing and setting. We could thus rename methods `get_name` to `name` and `set_name=` to `name=` since they are for instance variable `@name`.
```Ruby
class GoodDog
  def initialize(name)
    @name = name
  end

  def name                  # This was renamed from "get_name"
    @name
  end

  def name=(n)              # This was renamed from "set_name="
    @name = n
  end

  def speak
    "#{@name} says arf!"
  end
end

sparky = GoodDog.new("Sparky")
puts sparky.speak
puts sparky.name            # => "Sparky"
sparky.name = "Spartacus"
puts sparky.name            # => "Spartacus"
```

**Note**: Setter methods always return the original argument, even if the last evaluated statement is something else. If the argument is mutable and mutated in the setter method, the return value is also mutated
```Ruby
class Dog
  def name=(n)
    @name = n
    "Laddieboy"              # value will be ignored
  end
end

sparky = Dog.new()
puts(sparky.name = "Sparky")  # returns "Sparky"

# return a different value and @name not set
class Cat
  def name
    @name
  end
	
  def name=(n)
    return "test"             # ignored as return value
    @name = n				          # not executed due to return 
  end
end

catty = Cat.new
puts(catty.name = "Catty")   # returns "Catty"
puts catty.name              # nil as @name not set


# Mutable argument mutated in setter
class Cat
  def name
    @name
  end
	
  def name=(n)
    @name = n				         # bind @name to object referenced by n
    n[0] = '!'				       # mutate object reference by n
  end
end

catty = Cat.new
puts(catty.name = "Catty")   # returns "!atty"
puts catty.name              # !atty


# Immutable argument reassigned in setter
class Cat
  def age
    @age
  end
  
  def age=(n)
    n = 20			             # reassignment don't change return value
    @age = n		             # @age set to reassigned value
  end
end

catty = Cat.new
puts(catty.age = 10)   # returns 10
puts catty.age         # 20
```

### Use `attr_*` to create getter and setter methods
Ruby provides **`attr_accessor`** method that takes a **symbol** of the instance variable as argument to automatically create plain vanilla _getter_ and _setter_ methods. 

`attr_reader` is used if only getter methods are required\
`attr_writer` is used if only setter methods are required

```ruby
class GoodDog
  attr_accessor :name       # replicates name and name- methods

  def initialize(name)
    @name = name
  end

  def speak
    "#{@name} says arf!"
  end
end

sparky = GoodDog.new("Sparky")
puts sparky.speak
puts sparky.name            # => "Sparky"
sparky.name = "Spartacus"
puts sparky.name            # => "Spartacus"
```

If we need accessors for more states, we can just pass multiple symbol arguments separated by commas as follows:
```Ruby
attr_accessor :name, :height, :weight
```

Instead of referencing instance variables directly in instance methods, we can use accessor methods too.
```ruby
# Accessing instance variable directly
def speak
  "#{@name} says arf!"
end

# Accessing via getter method name
def speak
  "#{name} says arf!"
end
```

Instead of a plain vanilla getter method provided by `attr_accessor` or `attr_reader`, we can manually define accessor methods with the requisite customisation
- It is good practice to access instance variables using accessor methods and not directly i.e. `@variable_name` to ensure any customisation (e.g. preprocessing/formatting) incorporated into these methods are uniformly applied and we do not have to repeat ourselves
```Ruby
# ssn getter method showing only last 4 digits
def ssn
  # converts '123-45-6789' to 'xxx-xx-6789'
  'xxx-xx-' + @ssn.split('-').last
end
```

### Calling Methods With Self
Unlike getter methods, setter methods needs the `self` prefix when used in instance methods to let Ruby know we are invoking setter methods. Without that, it will be perceived as local variable assignments
```ruby
def change_info(n, h, w)
  self.name = n
  self.height = h
  self.weight = w
end
```

Although we could adopt the same syntax for getter methods, it is not required
```ruby
# Not needed
def info
  "#{self.name} weighs #{self.weight} and is #{self.height} tall."
end

# Preferred
def info
  "#{name} weighs #{weight} and is #{height} tall."
end
```

`self` prefix is not restricted to just accessor methods. They can be used with any instance method.
```ruby
class GoodDog
  # ... rest of code omitted for brevity
  def some_method
    self.info
  end
end
```

[Back to top](#section-links)


## Class Methods
Class methods are methods we call directly on the class itself, without the need to instantiate any objects. Their method name are prepended with the reserved keyword `self.`
```ruby
# ... rest of code ommitted for brevity

def self.what_am_i         # Class method definition
  "I'm a GoodDog class!"
end

GoodDog.what_am_i          # => I'm a GoodDog class!
```
Class methods are used for functionality not pertaining to individual objects (and their states).

[Back to top](#section-links)


## Class Variables
Class variables, prepended by `@@`, are used to capture information for an entire class. The class and all objects of that class refer to a **single instance** of that class variable.
```ruby
class GoodDog
  @@number_of_dogs = 0

  def initialize
    @@number_of_dogs += 1
  end

  def self.total_number_of_dogs
    @@number_of_dogs
  end
	
  def dog_count
    @@number_of_dogs
  end
end

puts GoodDog.total_number_of_dogs   # => 0

dog1 = GoodDog.new
dog2 = GoodDog.new

puts GoodDog.total_number_of_dogs   # => 2
puts dog1.dog_count                 # => 2
puts dog2.dog_count                 # => 2
```
- We initialize class variable `@@number_of_dogs` to 0 and increment this variable every time a GoodDog object is instantiated using `::new`. We then use a class method `self.total_number_of_dogs` to return the value of this class variable
- As shown in both `initalize`, `self.total_number_of_dogs` and `dog_count`, class variables are accessible in **both** class and instance methods. (`initialize` in a special instance method as it is private and cannot be called by any object of that class)
- The `dog_count` instance method confirms that all instances of a class reference a single class variable

[Back to top](#section-links)


## Constants
Constants are used in classes to reference values that never need to change. Although only the first letter of the variable name need to be in upper case, it is common for the entire variable name to be in caps. A warning will be raised by Ruby if they are reassigned to a new value.
```ruby
class GoodDog
  DOG_YEARS = 7

  attr_accessor :name, :age

  def initialize(n, a)
    self.name = n
    self.age  = a * DOG_YEARS
  end
	
  def self.print_constant
    puts DOG_YEARS
  end
	
  def print_constant
    puts DOG_YEARS
  end
end

sparky = GoodDog.new("Sparky", 4)
puts sparky.age             # => 28
spark.print_constant        # => 7
GoodDog.print_constant      # => 7
```

[Back to top](#section-links)


## to_s Method
`to_s` instance method is available to every class in Ruby through inheritance from the root `Object` class. This default implementation output the Class name followed by an encoding of the object's id.

The `to_s` method is called implicitly on the object passed to `puts` or in string interpolation. (Note: if the argument is an `Array`, `puts` will call `to_s` on each element and print them out on separate lines)
```ruby
puts sparky             # => #<GoodDog:0x007fe542323320>
puts "Hello #{sparky}"  # => Hello #<GoodDog:0x007fe542323320>
```

We can cutomise the printout by overriding the default `to_s` in our class definition 
```ruby
class GoodDog
  DOG_YEARS = 7

  attr_accessor :name, :age

  def initialize(n, a)
    @name = n
    @age  = a * DOG_YEARS
  end

  def to_s
    "This dog's name is #{name} and it is #{age} in dog years."
  end
end

puts sparky      # => This dog's name is Sparky and is 28 in dog years.
```

Like `puts`, `p` behaves somewhat similarly. However, instead of calling `to_s` on its argument, it calls another built-in Ruby instance method `inspect` which outputs the **states** of the object on top of its class name and encoded object_id. Hence `p sparky` is equivalent to `puts sparky.inspect`. This ability to display the internal states of an object make `p` very suitable for use in debugging.
```ruby
p sparky         # => #<GoodDog:0x007fe54229b358 @name="Sparky", @age=28>
```

[Back to top](#section-links)


## More About self
- `self` allows Ruby to disambiguate between initializing a local variable and calling a setter method
- `self` is also used for class method definitions

```ruby
class GoodDog
  attr_accessor :name, :height, :weight

  def initialize(n, h, w)
    @name   = n
    @height = h
    @weight = w
  end

  def change_info(n, h, w)
    self.name   = n
    self.height = h
    self.weight = w
  end

  def info
    "#{self.name} weighs #{self.weight} and is #{self.height} tall."
  end
	
  # instance method to test what is self in instance method
  def what_is_self
    self
  end
end
```

**Within a class definition**
1.  `self`, inside of an instance method, references the instance (object) that called the method - the calling object. Therefore, `self.weight=` in `change_info` method is the same as `sparky.weight=`.
	```ruby
	class GoodDog
	  # ... omitted for brevity
	  def what_is_self
	    self
	  end
	end
	```
   
	```ruby
	sparky = GoodDog.new('Sparky', '12 inches', '10 lbs')
	p sparky
	# => #<GoodDog:0x007f83ac062b38 @name="Sparky", @height="12 inches", @weight="10 lbs">
	p sparky.what_is_self
	# => #<GoodDog:0x007f83ac062b38 @name="Sparky", @height="12 inches", @weight="10 lbs">
	```
  This meaning of `self` is also useful for [dynamic constant scope resolution](inheritance_and_variable_scope.md#inheritance-on-constant-variable-scope).


2.  `self`, outside of an instance method, references the class and can be used to define class methods. Therefore if we were to define a `name` class method, `def self.name=(n)` is the same as `def GoodDog.name=(n)`.
	```ruby
	class GoodDog
	  # ... rest of code omitted for brevity
	  puts self
	end

	# outputs GoodDog
	```

**Within Modules**
1. [Modules as Method Containers](inheritance.md#modules-as-method-containers)

[Back to top](#section-links)