# Encapsulation and Polymorphism

## Section Links
[Encapsulation](#encapsulation)\
[Polymorphism](#polymorphism)

---

## Encapsulation
Encapsulation refers to the **hiding of internal representations** of an object and **only exposing selected methods and properties** that external users need.

**Benefits of Encapsulation**
- Users do not need to know how internal states or methods are implemented for a class. They just need to know the public methods available for their use.
- By decoupling usage from implementation, implementations can change without affecting users as long as public method signatures remain unchanged. This reduces interdependency, making software more maintainable and scalable.
- Enables high level of abstraction during class design. Designers can think in terms of nouns (classes) and their behaviours (methods).
- Protects the integrity of information stored within objects from unintended manipulation or alteration. This implies that a class should keep the number of public methods to a minimum to protect that class and internal data from undesirable changes from the external world 

**Example**
```ruby
class Dog
  def initialize(n)
    @nickname = n
  end

  def change_nickname(n)
    self.nickname = n
  end
	
  def nickname
    @nickname.clone
  end

  def greeting
    "#{nickname.capitalize} says Woof Woof!"
  end

  private
    attr_writer :nickname
end

dog = Dog.new("rex")
dog.change_nickname("barny") # changed nickname to "barny"
puts dog.greeting # Displays: Barny says Woof Woof!
```
Users of `Dog` class need not know how it is implemented. They just need to be know they can change a dog object's nickname through `change_nickname` and also get a greeting message in the form of a string through a `greeting` method call. While users can retrieve a dog's nickname using the `nickname` method call, the developer can also protect the object's state by returning a clone of the instance variable, a safety feature that the user need not know.

[Back to top](#section-links)

## Polymorphism
Polymorphism refers to the phenomenon where different object types can respond to a **common method invocation**. To achieve this, participating classes would need to implement that common interface. 

**Benefits**
- There enables flexibility during software development to get a pool of object types to invoke a common method without discerning their respective types.

Polymorphism can be achieved through: 
1. Inheritance
2. Duck typing

### Polymorphism through Inheritance
- A method can be acquired through inheritance from a superclass
```ruby
class Animal
  def move
  end
end

class Fish < Animal
  def move
    puts "swim"
  end
end

class Cat < Animal
  def move
    puts "walk"
  end
end

# Sponges and Corals don't have a separate move method - they don't move
class Sponge < Animal; end
class Coral < Animal; end

animals = [Fish.new, Cat.new, Sponge.new, Coral.new]
animals.each { |animal| animal.move }
```

- Overriding inherited methods with a different method signature can break polymorphism (See exammple below)
```ruby
class Cheetah < Animal
  def move(speed)
    puts "sprinting at #{speed} mph."
  end
end

animals << Cheetah.new
animals.each { |animal| animal.move }

# => swim
# => walk
# ArgumentError (wrong number of arguments (given 0, expected 1))
```


### Polymorphism through Duck Typing
- Duck typing occurs when objects of **unrelated classes** (i.e. no superclass and subclass relationship) can all respond to a common method invocation.

**Incorrect Way to Implement Polymorphism**
- Need to be explicit about the type of each object and the corresponding method to invoke
```ruby
class Wedding
  attr_reader :guests, :flowers, :songs

  def prepare(preparers)
    preparers.each do |preparer|
      case preparer
      when Chef
        preparer.prepare_food(guests)
      when Decorator
        preparer.decorate_place(flowers)
      when Musician
        preparer.prepare_performance(songs)
      end
    end
  end
end

class Chef
  def prepare_food(guests)
    # implementation
  end
end

class Decorator
  def decorate_place(flowers)
    # implementation
  end
end

class Musician
  def prepare_performance(songs)
    #implementation
  end
end
``` 

**Correct Way to Implement Polymorphism**
```ruby
class Wedding
  attr_reader :guests, :flowers, :songs

  def prepare(preparers)
    preparers.each do |preparer|
      preparer.prepare_wedding(self)
    end
  end
end

class Chef
  def prepare_wedding(wedding)
    prepare_food(wedding.guests)
  end

  def prepare_food(guests)
    #implementation
  end
end

class Decorator
  def prepare_wedding(wedding)
    decorate_place(wedding.flowers)
  end

  def decorate_place(flowers)
    # implementation
  end
end

class Musician
  def prepare_wedding(wedding)
    prepare_performance(wedding.songs)
  end

  def prepare_performance(songs)
    #implementation
  end
end
```
- All classes implements a common `prepare_wedding` method with the same signature. When invoking this method, there is no need to check the specific type of an object.
- In the duck idiom, we do not need to check whether an object is truly a duck. As long as it quacks like a duck and walks like a duck, it is as good as a real duck.

[Back to top](#section-links)