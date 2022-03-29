# Equality

## Section Links
[The `==` Instance Method](#the--instance-method)\
[The `equal?` Instance Method](#the-equal-instance-method)\
[The `===` Instance Method](#the--instance-method-1)\
[The `eql?` Instance Method](#the-eql-instance-method)

---

There are different measures of equality for Ruby objects

## The `==` Instance Method
- `==` is **not** an operator but an instance method. `a == b` is actually syntactic sugar for `a.==(b)` and meant to allow developers to write more naturally. `a` is the caller, `b` the argument and `==` the method invoked.

- `==` is mostly implemented to compare **values** of objects
	```ruby
	str1 = "something"
	str2 = "something"
	str1 == str2            # => true

	int1 = 1
	int2 = 1
	int1 == int2            # => true

	sym1 = :something
	sym2 = :something
	sym1 == sym2            # => true
	```

- However the [default `==`](https://docs.ruby-lang.org/en/master/BasicObject.html#method-i-3D-3D) instance method in all Ruby classes are inherited from `BasicObject` (the root superclass in Ruby) and uses `equal?` for comparison, returning **`true`** only if two objects are the **same object**.
	```ruby
	class Person
	  attr_accessor :name
	end

	bob = Person.new
	bob.name = "bob"

	bob2 = Person.new
	bob2.name = "bob"

	bob == bob2                # => false

	bob_copy = bob
	bob == bob_copy            # => true
	```

- Standard built-in classes such as `String` and `Array` have overridden the default `==`. `Array#==` and `String#==` will return `true` as long as their objects have the **same value**, even when they are different objects.
	```ruby
	str1 = "something"
	str2 = "something"
	str1_copy = str1

	# comparing the string objects' values
	str1 == str2            # => true
	str1 == str1_copy       # => true
	str2 == str1_copy       # => true

	# comparing the actual objects
	str1.equal? str2        # => false
	str1.equal? str1_copy   # => true
	str2.equal? str1_copy   # => false
	```

	```ruby
	arr_a = [1, 2, 3]
	arr_b = [1, 2, 3]
	arr_a_copy = arr_a

	# comparing the array objects' values
	arr_a == arr_b 			# => true
	arr_a == arr_a_copy     # => true
	arr_b == arr_a_copy     # => true

	# comparing the actual objects
	arr_a.equal? arr_b      # => false
	arr_a.equal? arr_a_copy # => true
	arr_b.equal? arr_a_copy # => false
	```

- To ensure `==` in custom Ruby classes also compare by value, we need to define our own `==` method to specify what equal value meant in the custom class.
	```ruby
	class Person
	  attr_accessor :name

	  def ==(other)
		name == other.name     # relying on String#== here
	  end
	end

	bob = Person.new
	bob.name = "bob"

	bob2 = Person.new
	bob2.name = "bob"

	bob == bob2                # => true
	```

- When we define `==`, we automatically get `!=` for free.

[Back to top](#section-links)


## The `equal?` Instance Method
- The `equal?` is more stringent than `==` and only returns `true` if two variables are referencing the **same object**.

- This is equivalent to comparing the object_ids of objects referenced by two variables
	```ruby
	# Integers with same value
	a = 1
	b = 1

	a.equal? b                 # => true
	a.object_id                # => 3
	b.object_id                # => 3


	# Symbols with same value
	symbol_1 = :something
	symbol_2 = :something

	symbol_1.equal? symbol_2   # => true
	symbol_1.object_id			   # => 2080988
	symbol_2.object_id			   # => 2080988


	# Strings with same value
	str1 = "something"
	str2 = "something"

	str1.equal? str2           # => false
	str1.object_id             # => 200
	str2.object_id             # => 220


	# Arrays with same value
	arr1 = ['1', 2, :ok]
	arr2 = ['1', 2, :ok]

	arr1.equal? arr2           # => false
	arr1.object_id             # => 420
	arr2.object_id             # => 440
	```
	Note: Objects of `Integer` and `Symbol` are unique in the sense that **if they share the same value, they are the same object**, as seen by their object_ids. This is because they occupy the specific section of the memory block. Hence they are more memory efficient data structures. This does not apply to other object types e.g. arrays and strings.

- Unlike `==`, the `equal?` method [should never be overridden](./inheritance.md#accidental-method-overriding) by subclasses as it is used to establish object identity (i.e. `a.equal?(b)` if and only if `a` is the same object as `b`)

[Back to top](#section-links)


## The `===` Instance Method
- `===` is an instance method and used implicitly in `case` statements
```ruby
num = 25

case num
when 1..50
  puts "small number"
when 51..100
  puts "large number"
else
  puts "not in range"
end
```

is functionally equivalent to:
```Ruby
num = 25

if (1..50) === num
  puts "small number"
elsif (51..100) === num
  puts "large number"
else
  puts "not in range"
end
```

- `a === b` is syntactical sugar for `a.===(b)`. It is read as **is argument `b` a member the caller `a`**
```ruby
String === "hello" # => true
String === 15      # => false
```

- `===` returns true if two objects have the same value and class
```Ruby
a = "hello"
b = "hello"
c = "hel" 
a === b  # true
a === c  # false

Integer === 3 # true
Integer === Integer #false
String === "hi" # true
String === String # false

class Abc end;
Abc === Abc.new # true
Abc === Abc #false
```

[Back to top](#section-links)


## The `eql?` Instance Method
- The `eql?` method determine if two objects contain the same value and are of the same class.

- Used implicity by `Hash` to determine equality among its members. Two hashes are equal if they contain the same keys (even if they are in different order) and each key associates with the same value. It is very rarely used explicitly.

[Back to top](#section-links)