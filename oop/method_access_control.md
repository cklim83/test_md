# Method Access Control

## Private Protected and Public
Method access control involves granting or restricting access to methods defined in a class. This is achieved through the use of `public`, `protected` and `private` _access modifiers_.

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
- Other objects of same class can invoke an `protected` method but not a `private` method of that class within the class definition (Differentiator between `protected` and `private` method)

**Example: `self` calling protected and private method in class definition**
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

**Note**: Prior to Ruby 2.7, `self.private_method` will have resulted in `NoMethodError`. However, we still **cannot** call a private method using another object of the same type in the class definition.

**Example: Other object calling protected and private method in class definition**
```Ruby
class MyNumber
  attr_reader :value

  def initialize(value)
    @value = value
  end

  def <=>(other)
    return 1 if self > other
	return 0 if self == other # self.==(other), where == is private
	-1
  end

  protected

  def >(other)
    self.value > other.value
  end

  private

  def ==(other)
    self.value == other.value
  end
end
```

```Ruby
MyNumber.new(100) <=> MyNumber.new(3)  # => 1
MyNumber.new(10) <=> MyNumber.new(10)  # NoMethodError (private method `==' called for #<MyNumber:0x00007fdb4993b760 @value=10>
```
Within `<=>`, `self > other` works because `>` is a protected method. `self == other` results in `NoMethodError` because `==` is a private method.

