# Variable Scope
## Section Links
[Instance Variable Scope](#instance-variable-scope)\
[Class Variable Scope](#class-variable-scope)\
[Constant Variable Scope](#constant-variable-scope)

---

## Instance Variable Scope
- Instance variables start with `@` 
- Scoped at **object level**, they are used to track the state of individual objects and do not cross between objects (i.e. each maintains the state of its own object)
- Unlike local variables, instance variables are **accessible by all instance methods** even if they are not initialzed or passed in as argument to that instance method 
```ruby
class Person
  def initialize(n)
    @name = n
  end

  def get_name
    @name                     # @name accessible here
  end
	
  def self.class_get_name
    @name                     # @name NOT accessible here
  end
end

bob = Person.new('bob')
bob.get_name                  # => "bob"

Person.class_get_name         # => nil
```
- They are **NOT accessible in class methods**. Trying to access an instance variable via a class method will return `nil` as seen in `Person.class_get_name` in example above
- **Uninitialized** instance variable will **not form part of the state** of the object when inspected by `inspect`. Trying to access it via an instance method will return `nil` even if it has not been set to `nil`. This is unlike referencing uninitialized local variables, which will result in a `NameError: undefined local variable or method`
```ruby
class Person
  def initialize
    @age = 10
  end
	
  def get_name
    @name         # uninitalized instance variable
  end
end

bob = Person.new  # <Person:0x00007f8ab38dda98 @age=10>
bob.get_name      # => nil
```

- If an instance variable is initialized **outside** an instance method i.e. at the class level, we get a **class instance variable** instead. They are **not accessible** in instance methods
```ruby
class Person
  @name = "bob"              # class level initialization

  def get_name
    @name
  end
end

bob = Person.new
bob.get_name                  # => nil
```

[Back to top](#section-links)


## Class Variable Scope
- Class variables start with `@@`
- Scoped at the class level, they exhibit the following characteristics:
	- All objects share **1 copy** of the class variable
	- **Both** class and instance methods **can access** class variables, regardless of where it is initialized
```ruby
class Person
  @@total_people = 0            # initialized at the class level

  def self.total_people
    @@total_people              # accessible from class method
  end

  def initialize
    @@total_people += 1         # mutable from instance method
  end

  def total_people
    @@total_people              # accessible from instance method
  end
end

Person.total_people             # => 0
Person.new
Person.new
Person.total_people             # => 2

bob = Person.new
bob.total_people                # => 3

joe = Person.new
joe.total_people                # => 4

Person.total_people             # => 4
```
- Both instances of Person `bob` and `joe` are accessing a common copy of class variable `@@total_people` when they call the instance method `total_people`

[Back to top](#section-links)


## Constant Variable Scope
Class constants (constants in short) **begin with a capital letter**. By convention, it is common for the entire name to be in caps. Below are examples of valid constants:
```Ruby
class Blog
  URL = "rubyguides.com" # preferred way to define Constant 
  Category = "sport"     # valid Constant 
  ...
end
```

Constants can only be defined **outside** of methods, otherwise they result in syntax error
```Ruby
class MyClass
  def my_method
    ABC = 1
  end
end
#syntax error: dynamic constant assignment
``` 

Although constants can be reassigned, that is discouraged and will result in a warning
```Ruby
class MyClass
  ABC = 1
  ABC = 2
end
      
(irb):42: warning: already initialized constant MyClass::ABC
(irb):41: warning: previous definition of ABC was here
```


Class constants can be accessed from **both instance and class** methods
```ruby
class Person
  TITLES = ['Mr', 'Mrs', 'Ms', 'Dr']

  attr_reader :name

  def self.titles
    TITLES.join(', ')             # accessible in class method
  end

  def initialize(n)
    @name = "#{TITLES.sample} #{n}" # accessible in instance method
  end
end

Person.titles                   # => "Mr, Mrs, Ms, Dr"

bob = Person.new('bob')
bob.name                        # => "Ms bob"  (output may vary)
``` 

Constants have a **lexical** (aka static) scope. This means that it may only be referenced from within the block of code it is defined in. This gives rise to some peculiarities especially when inheritance is involved:

[Back to top](#section-links)