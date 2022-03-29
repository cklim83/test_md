# Method Access Control

## Private Protected and Public
Method access control involves granting or restricting access to methods defined in a class. This is achieved through the use of `public`, `protected` and `private` **access modifiers**.

All methods in a class definition are `public` by default. **Public** methods are available for the rest of the program to use and comprise the class' _interface_ (how other classes and objects will interact with this class and its objects)

**Private** methods are not available to the rest of the program but can only be used within the class (e.g. as helper methods to other methods within the class to process some data). To make methods private, we use the `private` method call and anything below it is made private (unless another access modifier method call such as `protected` is called after it to negate it).
```Ruby
class GoodDog
  DOG_YEARS = 7

  attr_accessor :name, :age

  def initialize(n, a)
    self.name = n
    self.age = a
  end

  private

  def human_years
    age * DOG_YEARS
  end
end

sparky = GoodDog.new("Sparky", 4)
sparky.human_years
```

```irb
NoMethodError: private method `human_years' called for
  #<GoodDog:0x007f8f431441f8 @name="Sparky", @age=4>
```
The method `human_years` was made private as it was placed under the `private` method. Hence it is not accessible outside the class definition. Nonetheless, `human_years` can be invoked by other instance methods of the class. We can have `public_disclosure`, a public method, make use of the private method in its body.
```Ruby
# assume the method definition below is above the "private" method

def public_disclosure
  "#{self.name} in human years is #{human_years}"
end
```

**Protected** methods are in between public and private methods and exhibit the following characteristics: 
- Inside the class definition, `protected` methods are accessible just like public methods
- Outside the class definition, `protected` methods act like private methods
- Protected methods can be called by **any instance (self or otherwise) of the defining class or its subclasses** but private methods can only be called by `self` in the class definition. We cannot access another object's private method directly.

**Example 1**
```Ruby
class MyClass
  ...
  
  def compare_to(x)
    self.some_method <=> x.some_method
  end
  
  protected
  
  def some_method
    # ...
  end
end
```
`some_method` cannot be private but must be protected so that another instance `x` can also call it in the class definition. Helper methods that need not be called by other instances can be private.


**Example 2**
```Ruby
class Card
  NON_NUMERIC_CARDS = { 
    'Jack' => 11, 'Queen' => 12,
    'King' => 13, 'Ace' => 14
  }
  
  include Comparable      # will generate other comparison methods using <=>
  
  attr_reader :rank, :suit
  
  def initialize(rank, suit)
    @rank = rank
    @suit = suit
  end
  
  def <=>(other_card)
    return 1 if self.relative_rank > other_card.relative_rank
    return -1 if self.relative_rank < other_card.relative_rank
    0
  end
  
  protected
  
  def relative_rank
    return rank unless rank.to_i == 0
    NON_NUMERIC_CARDS[rank]
  end
end
```
Similarly, `relative_rank` has to be `protected` so that it can also be called upon by another instance of the same class `other_card` within the class definition. Otherwise a `NoMethodError` will be raised by `other_card.relative_rank` as we tried to call private method `relative_rank` in `<=>`.


### Invoking Private Method With Self Prefix
Before Ruby 2.7, private method cannot be invoked with a `self.` prefix.

**Example: Calling Private Method With Self Prefix Pre Ruby 2.7**
```Ruby
class Animal
  def public_protected_method
    "Will this work? " + self.protected_method
  end

  def public_private_method_1
    "Will this work? " + private_method
  end
		
  def public_private_method_2
    "Will this work? " + self.private_method
  end
		
  protected

  def protected_method
    "Yes, I'm protected!"
  end
		
  private
		
  def private_method
    "Yes, I'm private!"
  end
end
```

```Ruby
fido = Animal.new
fido.public_protected_method  # => "Will this work? Yes, I'm protected!"    
fido.public_private_method_1  # => "Will this work? Yes, I'm private!"
fido.public_private_method_2  # => `public_private_method_2': private method `private_method' called for #<Animal:0x000000000114c598> (NoMethodError)
```

**Example: Calling Private Method With Self Prefix From Ruby 2.7 onwards**
```Ruby
class Animal
  def public_protected_method
    "Will this work? " + self.protected_method
  end

  def public_private_method_1
    "Will this work? " + private_method
  end
		
  def public_private_method_2
    "Will this work? " + self.private_method
  end
		
  protected

  def protected_method
    "Yes, I'm protected!"
  end
		
  private
		
  def private_method
    "Yes, I'm private!"
  end
end
```

```Ruby
fido = Animal.new
fido.public_protected_method  # => "Will this work? Yes, I'm protected!"    
fido.public_private_method_1  # => "Will this work? Yes, I'm private!"
fido.public_private_method_2  # => "Will this work? Yes, I'm private!"
```

