#### Local Variable Scope Rules in Ruby
##### How local variables interact with method invocations with blocks and method definitions
- Local variables initialized outside of a block is accessible inside a block
- Local variables initialized within a block cannot be accessed outside that block
- Variable shadowing occurs when a block parameter shares the same name as an local variable initialized outside the block. This prevents the outer scoped local variable from being accessed inside the block
- Nested scopes are relative in nature and follows the same outer and inner scope rules
- A block within a method definition obeys the same local variable scoping rules
- Method definitions create their own scope. Local variables initialized outside method definitions cannot be accessed inside the method. Similarly, local variables initialized within the method cannot be accessed outside the method. The only way for an outer scoped object to made available within a method is to pass that object as an argument to the method during a method call.
- A block that follows a method invocation acts as an argument to the method
- Methods with parameters or block may or may not make use of them. It depends on the method definition:
	- Control is passed from a method to an accompanying block when the `yield` keyword is encountered in the method definition
	- When block execution completes, the block returns a value and execution resume after the `yield` keyword in the method definition
	- Some methods make use of block return values (e.g. `map`, `select`)
- Blocks passed to method calls can allow local variables initialized outside both the method and block to be accessed by the method via the block
	 
#### Mutating vs non-mutating methods, pass-by-reference vs pass-by-value
- Mutating methods are those that change the value (state) of objects passed to them. A method may be mutating with respect to the caller and/or its arguments.
- In Ruby, most mutating methods ends with `!` by convention. However, not all mutating methods follow this convention. For example: `#<<`, `#[]=` (index assignment), setter methods `something=` and `delete` are mutating but does not end with `!`
- Collection or String objects are considered mutated once underlying elements or characters are reassigned to new objects or have their values changed. Not clear ...
- Non-mutating methods do not mutate the state of objects. Assignment `=` or assignment operators `+=, -=, *= or /=` do not mutate objects. For example, assuming `a = hello`, then `a += ' world'` is simply syntactic sugar for `a = a + ' world'`, with the right-hand side not an operator but `String#+` method call with ` world` passed in as argument i.e. `a.+(' world')`. This returns a new String object with value `hello world` which is then reassigned to variable `a` on the left of the assignment operator. Hence the expression is merely an assignment to a new string object, with the original `hello` unmutated.
- Element reference methods `String#[index]`, `Array#[index]` and `Hash#[key]` are non-mutating. `String#[index]` returns a new character with same value as the object at that index while `Array#[index]` or `Hash#[key]` returns the actual element at the index/key. 
	- Note: A `String` object has a different representation in memory compared to collection (`Array` or `Hash`) objects: a String is just a single object while a **collection is an object with objects within**. This difference manifest when we call `String#[index]` or `String#each_char` as the method would create a **new String object** having the same value as the character at the given index in the original String. `Array#[index]` or `Hash#[index]` on the other hand could just retrieve and return the **actual component object**. This can be verified with `String#[index].object_id`, which returns a unique id each time its called on the same String object despite having the same value. 
	- This distinction is important because calling `String#[index].upcase!` will not mutate the original String object but `Array#[index].upcase!` would. 
	- The distinction disappears once we call a slice of String or collection objects `String/Array#[start, length]` or `String/Array#[range]` as **both** returns a **new object** as they both call the `slice` method for their respective classes

- Pass by value refers to the passing of copies of arguments during a method call. Since it is a copy rather than the original object that is passed into the method call, the original object CANNOT be mutated by the method. 
- Pass by reference occurs when references to the original object is passed to the method during invocation so that the method retains the ability to access and mutate the original object. Original mutated will reflect its new value even after the method call is over.
- Ruby is largely a pass by reference programming language. Copies of references to objects are passed into methods at invocation. Hence, original objects can be changed by the method. However, in Ruby variables are merely pointers to object but not containers for the object itself, and since the references of the original argument cannot be altered by the method, reassignment of method parameters cannot mutate the object the argument is pointing to. Hence pass by reference value is a more precise description of how Ruby pass objects during method calls.
 
#### Working with Array, Hash and String and Popular Collection Methods (each, map, select, etc)
- `each` method iterates through each element of the calling object, pass it to the parameter in an accompanying block, executes the block. It ignores the return value of the block and returns the original caller object
- `map` method iterates through each element of the calling object, pass it to the parameter in the accompanying block and executes the block. It **stores the return value of the block** into a **new array** and returns that array
- `select` method iterates through each element of the calling object, pass it to the parameter in the accompanying block and executes the block. It **stores the elements whose block's return value evaluates to true** into a **"new array"** and returns that array.
```Ruby
a = ["hello", "bye"]
a.object_id			# => 70216084941460				
a[0].object_id		# => 70216084941500
a[1].object_id		# => 70216084941480

b = a.select { |word| word.length > 3 }
b					# => ["hello"]
b.object_id			# => 70216088501460, different from a
b[0].object_id		# => 70216084941500, same as a[0] object_id
```



#### Variables as pointers
- In Ruby, a variable does not contain the value but instead is a labelled pointer to an address space in memory that holds the value

#### Puts vs Return
- Ruby methods ALWAYS return the **evaluated result of the last executed expression** unless an explicit `return` comes before it
- `return` reserved word in not required to return something from a method
- `puts` converts the argument passed to it to String by calling `#to_s` on it and outputs the result to the console but it returns `nil`

#### False vs Nil and The Idea of "Truthiness"
- Any value that is not `false` or `nil` is `truthy` in Ruby
- `Truthy` values **evaluates to `true`** in the context of conditional expressions but may not equate to the `true` value. Similarly, `falsey` values may not equate to `false` since both `false` and `nil` are `falsey`. `Falsey` values **evaluates to `false`** in conditional expressions. 

Example
```ruby
a = "Hello"

if a
  puts "Hello is truthy"
else
  puts "Hello is falsey"
end
```
- Correct
	- `a` evaluates as `true` in the conditional statement and so 'Hello is truthy' is output OR
	- `a` is `truthy` and so 'Hello is truthy' is output
- Incorrect
	- `a` is `true` and so 'Hello is truthy' is output
	- `a` is equal to `true` and so 'Hello is truthy' is output
- Summary
	- Use `evaluates to true`, `evaluates as true` or `is truthy` when discussing an expression that evaluates as true in a boolean context
	- Do not use `is true` or `is equal to true` unless specifically discussing the boolean `true`
	- Use `evaluates to false`, `evaluates as false`, `is falsey` when discussing an expression that evaluates as false in a boolean context
	- Do not use `is false` or `is equal to false` unless specifically discussing the boolean `false`

#### Method Definition and Method Invocation
- Method definition refers to the specifications/implementation of what a Ruby method does using the `def` keyword. Method invocation refers to the calling and execution of code defined within the method definition.

- Method invocation is when we **call a method**. The method can be an existing method in the Ruby Core API or a custom method
```ruby
greeting # Calling the greeting method outputs "Hello"
```

- Calling methods with a block
```ruby
[1, 2, 3].each { |num| puts num }
```
- a block *is part of* the method invocation
- The block acts as an argument to the method, the same way a local variable can be passed as argument to a method at invocation.
- The way an argument is used, whether it is a method parameter or a block depends on how the method is defined.
	- Method paramter not used
	```ruby
	def greetings(str)
  	puts "Goodbye"
	end

	word = "Hello"

	greetings(word)

	# Outputs 'Goodbye'
	```

	- Method parameter used
	```ruby
	def greetings(str)
  		puts str
 		puts "Goodbye"
	end

	word = "Hello"

	greetings(word)

	# Outputs 'Hello'
	# Outputs 'Goodbye'
	```
	In both of the above examples, the method definition is such that the method has a parameter `str`. This allows the method to access the string `"Hello"` since it is passed in as an argument at method invocation in the form of the local variable `word`. Example 1 doesn't do anything with this string, Example 2 outputs it.
	
	- block not executed
	```ruby
	def greetings
  		puts "Goodbye"
	end

	word = "Hello"

	greetings do
  	puts word
	end

	# Outputs 'Goodbye'
	```
	
	- block executed
	```ruby
	def greetings
  		yield
  		puts "Goodbye"
	end

	word = "Hello"

	greetings do
  	puts word
	end

	# Outputs 'Hello'
	# Outputs 'Goodbye'
	```

In Example 3 the `greetings` method is invoked with a block, but the method is not defined to use a block in any way and so the block is not executed.

In Example 4 the `yield` keyword is what controls the interaction with the block, in this case it executes the block once. Since the block has access to the local variable `word`, `Hello` is output when the block is executed. 

The important take-away for now is that blocks and methods can interact with each other; the level of that interaction is set by the method definition and then used at method invocation.

- When invocating method with a block, we are not limited to just executing code within a block, depending on the method definition, the method can use the return value of the block to perform certain actions
```ruby
a = "hello"

[1, 2, 3].map { |num| a } # => ["hello", "hello", "hello"]
```
- The `Array#map` method is defined in such a way that it uses the return value of the block to perform transformation on each element in an array. In the above example, the `#map` method doesn't have direct access to the `a` variable. However, the block that we pass to `map` (and that `map` calls) does have access to `a`. Thus, the block can use the value of `a` to determine the transformation value for each array element.
- Method definitions _cannot_ directly access local variables initialized outside of the method definition, nor can local variables initialized outside of the method definition be reassigned from within it. A block _can_ access local variables initialized outside of the block and can reassign those variables. We already know that methods can access local variables passed in as arguments, and now we have seen that methods can access local variables through interaction with blocks.
- Given this additional context, we can think of **method definition** as setting a certain scope for any local variables in terms of the parameters that the method definition has, what it does with those parameters, and also how it interacts (if at all) with a block. We can then think of **method invocation** as using the scope set by the method definition. If the method is defined to use a block, then the scope of the block can provide additional flexibility in terms of how the method invocation interacts with its surroundings.

Summary
- The `def..end` construction in Ruby is method definition
- Referencing a method name, either of an existing method or subsequent to definition, is method invocation
- Method invocation followed by `{..}` or `do..end` defines a block; the block is _part of_ the method invocation
- Method definition _sets_ a scope for local variables in terms of parameters and interaction with blocks
- Method invocation _uses_ the scope set by the method definition

#### Implicit Return Value of Method Invocations and Blocks
- In the absence of an explicit `return` keyword in a Ruby method, the **value of the last evaluated expression** is returned by the method. Otherwise the value of the `return` expression is being returned.   


#### How Array#sort method works
- Strings DO NOT have sorting methods, but can be converted to an Array of characters to use `Array#sort`
	```Ruby
	[2, 5, 3, 4, 1].sort 	# => new array [1, 2, 3, 4, 5]
	```

- Sorting a collection relies on **object comparisons** in order to place each element in the correct order

- To compare two objects, the `<=>` method (aka spaceship operator) has to be implemented. `a <=> b` is actually method call `a.<=>(b)` where `a` is the caller and `b` is the argument. The class that implement this method shall return one of the following values:
	- `-1` when caller is smaller than argument
	- `0` when caller is equal to argument
	- `1` when caller is greater than argument
	- `nil` when caller cannot be compared to argument
	```Ruby
	2 <=> 1		# => 1 
	1 <=> 2		# => -1
	2 <=> 2		# => 0
	'b' <=> 'a' # => 1
	'a' <=> 'b'	# => -1, caller is 
	'b' <=> 'b'	# => 0, equal
	1 <=> 'a'	# => nil, not comparable
	```

- When we call `sort` on an `Array`, the sort algorithm iteratively call the `<=>` method of collection elements, then use the return values to order the items. If `nil` is returned in any comparison, an argument error is thrown
	```Ruby
	['a', 1].sort # => ArgumentError: comparison of String with 1 failed
	```
- Hence when we try to sort a collection, we need to know
	1. Does the object type implement a `<=>` comparison method?
	2. What is the specific implementation of that method for that objct type (e.g. `String#<=>` is implemented differently from `Integer#<=>`)
- `String#<=>` returns `-1, 0, +1 or nil` depending on whether `string` is less than, equal to, or greater than `other_string`
- A String order is determined by a character's position in the ASCII table. In general
	- Uppercase letters comes before lowercase letters
	- Digits and (most) punctuation come before letters
	- Accented and other characters are in extended ASCII table that comes after main ASCII table
	- We can use `String#ord` to know the position of a character in the ASCII table
	```Ruby
	'b'.ord 	# => 98
	'}'.ord		# => 125
	
	'b' <=> '}'	# => -1
	```


##### The `sort` method
- When we call sort on an array, it returns a **new array** of ordered items
- `sort` can be called with a block to control how items are sorted (e.g. ascending vs descending). The block shall accept **two** block parameters, and return `-1, 0, 1 or nil`
	```Ruby
	[2, 5, 3, 4, 1].sort do |a, b|
	  ... 		# Optional additional code, e.g output
	  a <=> b	# block return value
	end
	# => [1, 2, 3, 4, 5]

	[2, 5, 3, 4, 1].sort do |a, b|
	  ...		# optional additional code
	  b <=> a	# switching comparison order result in descending order
	end
	# => [5, 4, 3, 2, 1]
	```
- `String#<=>` has the following rules
	- Compares multi-character strings **character by character**: strings beginning with `a` comes before those beginning with `b`
	- If both characters are the same, the next characters in the strings are compared
	- If comparable characters are all equal, the longer name is considered greater i.e. `cap` <=> `cape` returns -1


- `Array#<=>` array comparisons are done on an **element-wise** manner: 
	- we compare the first object in each array, followed by the next for arrays whose position are still to be firmed.
	```Ruby
	[['a', 'cat', 'b', 'c'], ['b', 2], ['a', 'car', 'd', 3], ['a', 'car', 'd']].sort

	# => [["a", "car", "d"], ["a", "car", "d", 3], ["a", "cat", "b", "c"], ["b", 2]]
	```
	- After comparing the first element amongst subarrays, `['b', 2]`'s final position is confirmed since `'b'` is greater than `'a'`, the first elements of all other subarrays. 
	
	- Because of this, `["b", 2]` did not participate in the second element comparison. This **avoided the comparison between `2` and strings** which would have return `nil` and triggered an `ArgumentError`. If we have `["a", 2]` instead of `["b", 2]`, ArgumentError would have been raised
		```ruby
		[['a', 'cat', 'b', 'c'], ['a', 2], ['a', 'car', 'd', 3], ['a', 'car', 'd']].sort
		# => ArgumentError: comparison of Array with Array failed
		```

	- Comparing `['a', 'car', 'd', 3]` and `['a', 'car', 'd']` DID NOT generate an ArgumentError since `3` came into play in terms of a longer length array rather than a comparison between non-comparable objects

##### The `sort_by` Method
- `sort_by` is similar to `sort`, except that it usually called with a block, with return value (`-1, 0, 1, nil`) of the block determining the relative order
	```Ruby
	['cot', 'bed', 'mat'].sort_by do |word|
	  word[1]	# sort by the character in the first index only
	end
	# => ["mat", "bed", "cot"]
	```
- When we call `sort_by` on a hash, two arguments need to be passed to the block. `sort_by` **always returns an Array**
	```Ruby
	people = { Kate: 27, john: 25, Mike: 18 }
	people.sort_by do |_, age|
	  age
	end
	# => [[:Mike, 18], [:john, 25], [:Kate, 27]]
	```

- `Symbol#<=>` is essentially `String#<=>` since it first call `Symbol#to_s` on each symbol to convert it to string first before the `<=>` comparison
 	```Ruby
	people = { Kate: 27, john: 25, Mike: 18 }
	people.sort_by do |name, _|
	  name.capitalize 	# so that 'j' comes before 'K'
	end
	# => [[:john, 25], [:Kate, 27], [:Mike, 18]]
	```

#### Nested Data Structure
##### Mental Model of Variables Pointing to Objects
![Variables as Pointers 1](https://d1b1wr57ag5rdp.cloudfront.net/images/collections/variables-as-pointers-1.png)

##### Shallow Copy
- `dup` and `clone` are Ruby methods to create a _shallow copy_ of an object: only the object that the method is called on is copied, **nested objects** contained within it are **shared, not copied**
```Ruby
arr1 = ["a", "b", "c"]
arr2 = arr1.dup
arr2[1].upcase!

arr2	# => ["a", "B", "c"]
arr1	# => ["a", "B", "c"]
```

```Ruby
arr1 = ["a", "b", "c"]
arr2 = arr1.dup
arr2.map! do |char|
  char.upcase
end

arr1	# => ["a", "b", "c"]
arr2	# => ["A", "B", "C"]
```
- `arr2` is changed but `arr1` is not. This is because the array copy referenced by `arr2` is mutated by the destructive method `Array#map!`: each element in `arr2` will be reassigned to a new object returned by `char.upcase`. Since the original objects are not mutated by `#upcase`, `arr1` is left unchanged.

```Ruby
arr1 = ["a", "b", "c"]
arr2 = arr1.dup
arr2.each do |char|
  char.upcase!
end

arr1 # => ["A", "B", "C"]
arr2 # => ["A", "B", "C"]
```
- Both `arr1` and `arr2` are changed. Here, we call the destructive `String#upcase!` method on each _element_ of `arr2`. However, every element of `arr2` is a reference to the object referenced by the corresponding element in `arr1`. Thus, when `#upcase!` mutates the element in `arr2`, `arr1` is also affected; we change the Array elements, not the Array.

- We need to draw distinction whether the mutation is at the array level or at objects within the arrays

##### Freezing Objects
- Ruby objects can be frozen to prevent their state from being modified.
- `clone` will **preserve the frozen state** of an object while `dup` does not. Any attempts to change a clone will raise a `RuntimeError`
	```ruby
	arr1 = ["a", "b", "c"].freeze
	arr2 = arr1.clone
	arr2 << "d"
	# => RuntimeError: can't modify frozen Array
	```

	```ruby
	arr1 = ["a", "b", "c"].freeze
	arr2 = arr1.dup
	arr2 << "d"

	arr2 # => ["a", "b", "c", "d"]
	arr1 # => ["a", "b", "c"]
	```
- Only mutable objects can be frozen since immutable objects are by default already frozen
- We can check if an object is already frozen with `#frozen?`
	```ruby
	5.frozen? # => true
	```

- `freeze` only freezes the object it is called on. If there are nested objects within it, those will not be frozen.
	```ruby
	arr = [[1], [2], [3]].freeze
	arr[2] << 4
	arr # => [[1], [2], [3, 4]]
	
	arr << 10	# => FrozenError, cant modify frozen Array
	```
#### Others
##### Difference between Puts, Print & P
- `print` output the message to screen while `puts` does the same but also adds a newline. **Both** methods return `nil`
```Ruby
puts 123
puts 456
123
456

print 123
print 456
123456
```
- For arrays, `puts` output each element on its own line while `print` does not
```Ruby
puts [1, 2]
1
2

print [1, 2]
[1, 2]
```

- `puts` attempts to convert everything to a string by calling `#to_s` before printing but print does not
``` Ruby
a = [1, nil, nil, 3]
puts a		# nil.to_s returns "" for put to output "" on newline
1


3

print a
[1, nil, nil, 3]
```
- `p` output a more 'raw' version of an object. It is best used for debugging as normally invisible characters e.g. newline, quotes are made visible
```Ruby
> puts "Ruby is Cool"
Ruby is Cool

> p "Ruby is Cool"
"Ruby is Cool"
```

- `p` returns the object passed to it while `puts` returns `nil` 
### Written Assessment Guide
#### Non-technical part of assessment
- Use markdown backticks(\`\`) to mark **code**, **variable** and **method names** and **values**.
- Divide answers into **short paragraphs** for better readability
- Focus on what each question is asking and keep answer **concise**. For example, if question is to explain how variable shadowing affects given code, we can describle how/where variable shadowing is demonstrated in the code and what effects it has

#### Technical part of assessment
- Use correct programming terms
	- Variable initialization
	- Assignment and reassignment
	- Referencing (synonym of "pointing to")
	- Methods are defined with parameters but called with arguments
	- Output (synonym of "print to console")
	- Returns
	- We call `method_name` on the `object_type` object referenced by `variable_name` and NOT we call `method_name` on `variable_name` as we can't call methods on variables.

- Concepts
	- Variable Scoping: local variables initialized in the outer scope CAN be accessed in an inner scope, but local variables initialized in an inner scope CANNOT be accessed in the outer scope
	
	- Variable Shadowing: Happens when a block parameter has the same name as that of an outer scope local variable initialized outside the block. This prevents the outer scope variable from being accessed within the block
	
	- Each, Map and Select methods
		- `#each` method iterates through the caller, passing each element of the caller to the block and runs the block. The block return value for each element iteration are ignored and `each` simply returns the caller object at the end of method call. 
			- Note: `String` does not have an `each` method implemented unlike arrays and hashes. To use `each`, we can use `String#chars` (shorthand for `str.each_char.to_a`) to convert it to an Array of characters before using `Array#each`
	 
		- `map` method iterates through the caller, passing each element of the caller to the block and runs the block. The block return value for each element iteration are pushed into a **new array** that serves as the return value of the `map` method call. 
			- Note: `map` always returns a new array even if the caller is not an array (e.g. hash). `map!` (only available to Array and not Hashes which uses Enumerable which only have `map`) mutates the current array by replacing existing elements with the block return value in each iteration
		
		- `select` method iterates through the caller, passing each element of the caller to the block and runs the block. The block return value for each element iteration are then used to determine if that element will be included in a new array to be returned by the method. Only elements whose block return value **evaluates** to true (i.e. truthy) are included 

	- Mutating/Non-mutating Ruby Methods
		- Mutating methods are those that **change the value** of an object. A method can be mutating with respect to the calling object and/or arguments. 
			- Examples include `<<`, `String#concat`, `#[]=`, `something=`
		- Non-mutating methods do not change the value of an object. Instead the changes could be make on copies, new objects and the original is unchanged.
			- Example include `=`, `+=`, `-=`, `*=`, `/=`
		- We can use `#object_id` to determine the id of the object referenced by a variable to see if it is the same or different object
		- Collections and Strings may be mutated even thought it is still referencing the same object (same object_id). The happens as changes might occur at the element/ character level


#### Model Answers
```Ruby
a = 'hello'
b = a
a = 'goodbye'
```
?
On line `1`, local variable `a` is **initialized** to a string object with value 'hello'.

On line `2`, local variable `b` is initialized to the same object **referenced** by local variable `a`.

One line `3`, local variable `a` is **reassigned** to a different string object with value 'goodbye'. Now `a` is pointing to a string object with value 'goodbye' while local variable `b` is pointing to another string object with value 'hello'.

```Ruby
def example(str)
  i = 3
  loop do
    puts str
	i -= 1
	break if i == 0
  end
end

example('hello')
```
?
- On line `10`, we call custom method `example` and pass it a string literal object with value of 'hello' as argument. This is assigned to local variable `str`, a parameter of method `example`
- On line `2`, we initialize a local variable `i` with an integer object of value `3`
- On line `3`, we call the `loop` method and pass it a `do .. end` block as argument
- On line `4`, we call the `puts` method and pass local variable `str` as argument. 'hello' is being output.
- On line `5`, `i -= 1` is essentially syntatic sugar for `i = i.-(1)`. `i` calls the method `Integer#-` and pass `1` in as an argument. The return value is then reassigned to local variable `i`. This line essentially decrement `i` by 1 every time it is executed
- On line `6`, we execute `break` to exit the loop if `i` has a value of 0. Together with line `5` and the initial value of `i`, it ensures the loop will be executed 3 times before exiting, causing 'hello' to be output `3` times
- As there is no explicit `return` expression in `example`, `break if i == 0` is the last expression to be executed and returns `nil` when the method call completes.

```Ruby
a = 4

loop do
  a = 5
  b = 3
  break
end

puts a 
puts b
```
Without running the code, what will this code output and why?
?
- This code will output `5` and then raise a `NameError` undefined local variable or method `b`
- In this example, we have local variables of different scopes. Local variable `a` has an outer scope since it is initialized outside the block on line `4-8`. Hence it can be reassigned on line `5` and accessed again on line `10` to be printed.
- On the other hand, local variable `b` is initialized within the block and has an inner scope. Hence it cannot be accessed outside the block and caused an error when we tried to reference it on line `11` 


 ```Ruby
a = 4
b = 2

loop do
  c = 3
  a = c
  break
end

puts a
puts b
```
Without running this code, what will this code output and why?
?
- This code will output `3` followed by `2`
- Both local variables `a` and `b` have outer scopes since they are initialized outside of the `do..end` block on lines `4-8`.
- `a` can be accessed inside the block and got reassigned to `3` on line `6`
- On line 10, `puts a` output the value of `a` which was already set to `3`
- Since `b` was not reassigned, `puts b` will output `2`


```Ruby
a = 4
b = 2

2.times do |a|
  a = 5
  puts a
end

puts a
puts b
```
Without running the code, what will this code output and why?
? 
- The code will output `5`,`5`, `4` and `2` in that order.
- We initialize local variables `a` and `b` to values `4` and `2` on lines `1` and `2` respectively
- Next, we call times method on integer 2 and pass it a block as argument. The block has a parameter `a`. As it has the same name as the outer scope local variable `a`, variable shadowing occurs and the outer scope variable `a` cannot be accessed within the block.
- Hence within the block, we are dealing with an inner scoped local variable `a` which is assigned a value of 5. puts a  then output this value. This is repeated a total of times by the times method call
- The `do .. end` block passed to the `times` method on lines `4-7` create an inner scope. As the block parameter has the same name as the outer scoped local variable `a`, it results in **variable shadowing**, which caused the outer scoped local variable `a` to be inaccessible within the block. The local variable `a` within the block is a different variable and is inner scoped. Hence `a` cannot be changed by the block.
- Since both outer scoped `a` and `b` remain unchanged by lines `4-7`, lines `9` and `10` will output their original values `4` and `2` respectively.


```Ruby
[1, 2, 3, 4].each { |num| puts num }
```

```Ruby
[1, 2, 3, 4].map { |num| puts num }
```

```Ruby
[1, 2, 3, 4].select { |num| puts num }
```

```Ruby
a = 'hello'

puts a
puts a.object_id

a.upcase!

puts a
puts a.object_id
```

```Ruby
a = 'hello'

puts a
puts a.object_id

a.upcase

puts a 
puts a.object_id
```


```Ruby
a = 'name'
b = 'name'
c = 'name'

# Are these three local variables pointing to the same object?

puts a.object_id
puts b.object_id
puts c.object_id

# And when we add these two lines of code... ?

b = c
b = a

puts a.object_id
puts b.object_id
puts c.object_id

# What about now?
a = 5
b = 5
c = 5

puts a.object_id
puts b.object_id
puts c.object_id
```

```Ruby
a = :dog
b = :dog
c = :dog

puts a.object_id
puts b.object_id
puts c.object_id
```

```Ruby
a = 'hello '
puts a
puts a.object_id

a += 'world'
puts a
puts a.object_id
```

Study Session
```Ruby
[[1, 2], [3, 4]].map { |arr| puts arr[0] }
```
My answer (4 mins)
- Multi-dimensional array calling the method `map` and passing it a block with parameter `arr`
- At iteration 1, `arr` is assigned a value of `[1, 2]` and `puts arr[0]` output 1 and returns `nil`
- At iteration 2, `arr` is assigned a value of `[3, 4]` and `puts arr[0]` output 3 and returns `nil`
- Since `map` uses the return value of the block as elements of a new array to be returned, the above code will return `[nil, nil]`

Given answer:
- [nil, nil]
- Map returns new array
- Elements is the return value of block
- Return value is nil


```Ruby
['ant', 'bear'].map do |elem|
  if elem.length > 3
    elem
  end
end
```
My answer: 4 min 16 seconds
- Array `['ant', 'bear']` call the method `map` and pass it a block with parameter `elem` as argument
- Within the block, it return the value of `elem` if the length of `elem` is longer than `3`, `nil` otherwise
- In iteration 1, elem is assigned value of 'ant'. Since 'ant'.length is not greater than 3, `nil` is returned by block
- In iteration 2, elem is assigned value of 'bear'. Since its length is 4, 'bear' is returned by block
- `map` pushed these values into new array and returns `[nil, 'bear']`

Given answer:
- `[nil, 'bear']`


```Ruby
[false, false, false].select { |num| 100 }
```
My answer: (3min 35s)
- `[false, false, false]` calls the method `select` with a block with parameter `num` as argument
- `select` uses the return value of the block to determine if an element should be included in a new array to be returned at the end of method call. 
- If the block return value evaluates to `true`, the element will be included, else it will not
- In this example, the block always return `100`, which evaluates to `true` since it is neither `false` or `nil`
- Hence the above code returns a new array `[false, false, false]`.

Given answer:
`[false, false, false]`
- Wrong Answer:
	- When the element is `falsey`, it is not selected (It is not about the truthiness of element but that of the return value of block)
	- When the block returns true (The return value of the block may not be `true`, e.g. `1` and the element will still get included)
	- When the block is truthy (We are talking about truthiness of the return value of the block, not the block itself)
- Correct Answer:
	- When the return value of the block `evaluates to true` or is `truthy`
	- The return value is a **new array** when we use `select`

```Ruby
animal = 'owl'

loop do |_|
  animal = "snake"
  var = "house"
  break
end

puts animal
puts var
```
My answer: (8m 48s)
- The above code output 'snake', then raise a `NameError`: undefined local variable or method `var`
- On Line `1`, we initialize local variable `animal` ~~with~~ to a string object ~~of~~ with value `'owl'` 
- On lines `3-7`, we invoke method `loop` and pass it  a `do..end` block with unnamed parameter `_` as argument.
- On line `4`, we reassigned `animal` to a new string object with value `"snake"`. This can be done since outer scoped variables can be accessed within an inner scoped block.
- On line `5`, we initialized a new local variable `var` to a new string object with value `"house"`. `var` is inner scoped within the block and cannot be accessed outside of the block
- On line `6`, we exited the block with `break``
- On line `9`, we pass `animal` to `puts` method call which output its reassigned value `'snake'`
- On line `10`, we pass `var` to the `puts` method but since it is inner scoped to be block above and cannot be accessed outside of it, it threw the above error.
-  
Given answer:


```Ruby
def test(str)
  str += '!!!'
  str.upcase!
end

test_str = "Something"
test(test_str)

puts test_str
```
My answer: (7m 40s)
- On line `6`, local variable `test_str` is initialized to a string object with value 'Something'
- On line `7`, we invoke the `test` method and pass it `test_str` as an argument. 
- Upon method call, `str` is assigned to the object that `test_str` is referencing i.e. `Something`
- On line `2`, `str += '!!!'` is actually a non-mutating reassignment which makes a new copy of the object that `str` is referencing, i.e. 'Something' and concatenate it with '!!!' to form a new object 'Something!!!' which is then reassigned back to `str`. From now on, `str` and `test_str` are referencing different objects (`'Something!!!'` and `'Something'` respectively) and `test` can no longer mutate `test_str`
- Thus `puts test_str` output "Something" (forgot to mention return `nil`)


Given answer:
           

Explanation test_str = "Something", and method invocation before going into the relevant processes in the

- Start with

- Test_str is initialized to string object with value “Something”

- Invoke test method with test_str object

Key points (do not need to explain what is done to str in method other than it was reassign to a new object, the rest not relevant)

- # Once the method is invoked `str` and `test_str` variables now point to the same string object. *****

- # On line `2` we are reassigning `str` variable to point to a different string object. From now on, `str` and `test_str` no longer point to the same string object, so it is no longer possible to mutate the object `test_str` variable points to.

- # For that reason line `9` outputs "Something" and returns `nil`.

Common mistakes

- str variable is mutated or we call method upcase! on str. (wrong because we can't mutate variable, and we can't call methods on variables)

Correct language

We are doing that on the object variable points to.

We are initializing variables, we are reassigning them and we are pasing them as the argument to a method call.

- Figure out what is relevant, then go into those processes that resulted in that output. Meaningless processes need not be

- Learn the correct terminology, be precise.


```Ruby
greeting = 'Hello'

loop do
  greeting = 'Hi'
  break
end

puts greeting
```
Examine the code example above. The last line outputs the String `Hi` rather than the String `Hello`. Explain what is happening here and identify the underlying principle that this demonstrates.
?
On line 1, local variable `greeting` was initialized to the String `Hello`. The `do..end` following the `loop` method invocation on lines 3-6 defines a block, within which `greeting` was reassigned to another String `Hi` on line 4. On line 8, the `puts` method was called with local variable `greeting` passed to it as an argument. Since `greeting` is now assigned to `Hi`, this is what is output. This example demonstrates local variable scoping rules in Ruby: local variables initialized outside of a block is accessible inside the block.

def a_method
  puts "hello world"
end
Describe the method
?
The above code shows the method definition of `a_method`. The method has no parameters but simply calls the `puts` method and pass it the String `hello world` as an argument. When invoked, the method will output the `hello world` and returns `nil`.