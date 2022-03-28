# Fake Operators

## Section Links
[List of Operators and Methods](#list-of-operators-and-methods)\
[Equality Methods](#equality-methods)\
[Comparison Methods](#comparison-methods)\
[The `<<` and `>>` Shift Methods](#the-`<<`-and-`>>`-shift-methods)\
[The `+` Method](#the-plus-method)\
[Element Setter and Getter Methods](#element-setter-and-getter-methods)

---



## List of Operators and Methods
In Ruby, many methods are written to look like operators due to **syntactic sugar**. `==` is one example. Because they are methods, we can implement them in our classes and take advantage of the special syntax for our own objects. When we do that, we must be careful to follow conventions established in the Ruby standard library. Otherwise, using those methods will be very confusing.


In the table below, we show which operators are real operators and which are methods disguised as operators (listed by order of precedence, highest first) 

| Method | Operator | Description |
|---|---|---|
|no| `.`, `::`| Method/constant resolution operators |
|yes| `[]`.`[]=` | Collection element getter and setter |
|yes| `**` | Exponential operator |
|yes| `!`, `~`, `+`, `-`| Not, complement, unary plus and minus (method names for the last two are +@ and -@) |
|yes| `*`, `/`, `%` | Multiply, divide, modulo |
|yes| `+`, `-` | Plus, minus |
|yes| `>>`, `<<` | Right and left shift |
|yes| `&` | Bitwise "and" |
|yes| `^`, `|` | Bitwise exclusive "or" and regular "or" (inclusive "or") |
|yes| `<=`, `<`, `>`, `>=` | Less than/equal to, less than, greater than, greater than/equal to |
|yes| `<=>`, `==`, `===`, `!=`, `=~`, `!~` | Equality and pattern matching (`!=` and `!~` cannot be directly defined) |
|no| `&&` | Logical "and" |
|no| `..`, `...` | Inclusive range, exclusive range |
|no| `? :` | Ternary if-then-else |
|no| `=`, `%=`, `/=`, `-=`, `+=`, `!=`, `&=`, `>>=`, `<<=`, `*=`, `&&=`, `!!=`, `**=`, `{` | Assignment(and shortcuts) and block delimiter |

## Equality Methods

## Comparison Methods
Implementing comparison methods allow us to compare objects.
```ruby
class Person
  attr_accessor :name, :age

  def initialize(name, age)
    @name = name
    @age = age
  end
	
  def >(other_person)
    age > other_person.age
  end
end
```

```ruby
bob = Person.new("Bob", 49)
kim = Person.new("Kim", 33)
puts "bob is older than kim" if bob > kim
```

In Ruby, if we implement `<=>` method that returns `-1, 0, 1 or nil`, and we include [`Comparable`](https://docs.ruby-lang.org/en/3.1/Comparable.html) module in our custom class, we get all comparison methods `<`, `<=`, `==`, `>=`, and `>`) and the method `between?` for free.


## The `<<` and `>>` Shift Methods
The `<<` that we thought is a Ruby operator used to append elements to an array is actually the instance method `Array#<<`. By convention, `<<` mutates the caller by appending the argument.

Example of Implementing `<<` for custom class
```ruby
class Team
  attr_accessor :name, :members

  def initialize(name)
    @name = name
    @members = []
  end

  def <<(person)
    members.push person
  end
end

cowboys = Team.new("Dallas Cowboys")
emmitt = Person.new("Emmitt Smith", 46)     # suppose we're using the Person class from earlier

cowboys << emmitt                           # will this work?

cowboys.members                             # => [#<Person:0x007fe08c209530>]
```

`Team#<<` provides an easy method to add new members to an collection type object. It is also common to add a guard clause to reject adding a member unless some criteria is met.

```ruby
def <<(person)
  return if person.not_yet_18?              # suppose we had Person#not_yet_18?
  members.push person
end
```

## The Plus Method
```ruby
1 + 1                                       # => 2
1.+(1)                                      # => 2
```
The `+` operation is actually a instance method. `1 + 1` is actually `1.(1)` using `Integer#+`. `1.0 + 2` is different because the caller is a `Float` and we are invoking `Float#+` and passing in an integer as argument.

Different standard classes have their own `+` implementations:
- `Integer#+`: increments the value by value of argument, returning a new integer.
- `String#+`: concatenate with argument, returning a new string
- `Array#+`: Concatenate with argument, returning a new array
- `Date#+`: Increments the date in days by value of argument, returning a new date.

So if we are implement `+` for our custom class, it should increment or concatenate with the argument and return a new object of that class.
```ruby
class Team
  attr_accessor :name, :members

  def initialize(name)
    @name = name
    @members = []
  end

  def <<(person)
    members.push person
  end

  def +(other_team)
    temp_team = Team.new("Temporary Team")
    temp_team.members = members + other_team.members
    temp_team
  end

  end
end

# we'll use the same Person class from earlier

cowboys = Team.new("Dallas Cowboys")
cowboys << Person.new("Troy Aikman", 48)
cowboys << Person.new("Emmitt Smith", 46)
cowboys << Person.new("Michael Irvin", 49)


niners = Team.new("San Francisco 49ers")
niners << Person.new("Joe Montana", 59)
niners << Person.new("Jerry Rice", 52)
niners << Person.new("Deion Sanders", 47)
```

```ruby
dream_team = niners + cowboys
puts dream_team.inspect                     # => #<Team:0x007fac3b9eb878 @name="Temporary Team" ...
```

In our `Team` class, we use `Array#+` within `Team#+` to concatenate the members, then return it as a new Team object to follow the `+` convention.

## Element Setter and Getter Methods
`[]` and `[]=` commonly used to retrieve and set array elements are actually getter and setter instance methods `Array#[]` and `Array#[]=`. `arr[n]` is actually `arr.[](n)` and `arr[n]=value` is actually `arr.[]=(n, value)`
```ruby
my_array = %w(first second third fourth)    # ["first", "second", "third", "fourth"]

# element reference
my_array[2]                                 # => "third"
my_array.[](2)                              # => "third"


# element assignment
my_array[4] = "fifth"
puts my_array.inspect                            # => ["first", "second", "third", "fourth", "fifth"]

my_array.[]=(5, "sixth")
puts my_array.inspect                            # => ["first", "second", "third", "fourth", "fifth", "sixth"]
```

For our `Team` collection type class, we can use `Array#[]` and `Array#[]=` to implement the getter and setter functions since we represented the collection using and Array as collaborator object.
```ruby
class Team
  # ... rest of code omitted for brevity

  def [](idx)
    members[idx]
  end

  def []=(idx, obj)
    members[idx] = obj
  end
end
```

```ruby
# assume set up from earlier
cowboys.members                           # => ... array of 3 Person objects

cowboys[1]                                # => #<Person:0x007fae9295d830 @name="Emmitt Smith", @age=46>
cowboys[3] = Person.new("JJ", 72)
cowboys[3]                                # => #<Person:0x007fae9220fa88 @name="JJ", @age=72>
```