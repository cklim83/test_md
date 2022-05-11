# Closures in Ruby
## Section Links
[Summary](#summary)\
[What Is A Closure](#what-is-a-closure)\
[Calling Methods With Blocks](#calling-methods-with-blocks)\
[Writing Methods That Take Blocks](#writing-methods-that-take-blocks)
- [Yielding](#yielding)
- [Tracing Execution Sequence](#tracing-execution-sequence)
- [Yielding With Argument](#yielding-with-argument)
- [Return Value From Yielding to Block](#return-value-from-yielding-to-block)
- [When to Design Methods to Use Blocks](#when-to-design-methods-to_use_blocks)
- [Methods With Explicit Block Parameter](#methods-with-explicit-block-parameter)
- [Using Closures](#using-closures)

[Blocks and Variable Scope](#blocks-and-variable-scope)\
[Two Different and Opposite Usage of &](#two-different-and-opposite-usage-of-&)\
[Exploring Procs, Lambda and Blocks: Definition and Arity](#exploring-procs-lambda-and-blocks-definition-and-arity)
[Use of Splat Operator `*` in Argument Passing](#use-of-splat-operator-*-in-argument-passing)

---

## Summary
- Blocks are one way that Ruby implements closures. Closures are a way to pass around an unnamed "chunk of code" to be executed later.
- Blocks can take arguments, just like normal methods. But unlike normal methods, it won't complain about wrong number of arguments passed to it.
- Blocks return a value, just like normal methods.
- Blocks are a way to defer some implementation decisions to method invocation time. It allows method callers to refine a method at invocation time for a specific use case. It allows method implementors to build generic methods that can be used in a variety of ways.
- Blocks are a good use case for "sandwich code" scenarios, like closing a `File` automatically.
- Methods and blocks can return a chunk of code by returning a `Proc` or `lambda`.

## What Is A Closure
- A **closure** is a general programming concept that allows programmers to **save a chunk of code** for execution at a later time. The chunk code can be passed into methods for execution. 

- This construct is called a closure because it **binds surrounding artifacts** (i.e. variables, constants or methods referenced within the code chunk) and builds an "enclosure" around them so that they (binded artifacts) can be referenced during closure execution.

- In Ruby, a closure can be implemented in one of 3 ways:
	- Instantiating a **`Proc`** object 
	- Using **lambda**
	- Using **blocks**

We will focus on blocks in this lesson.

[Back to Top](#section-links)


## Calling Methods With Blocks
`do ... end` or `{ ... }` that follows a method invocation are blocks passed in as an **argument** to the method call.

```ruby
# Example 1: passing in a block to the `Integer#times` method.

3.times do |num|
  puts num
end
=> 3

# Example 2: passing in a block to the `Array#map` method.

[1, 2, 3].map do |num|
  num + 1
end
=> [2, 3, 4]

# Example 3: passing in a block to the `Hash#select` method.

{ a: 1, b: 2, c: 3, d: 4, e: 5 }.select do |_, value|
  value > 2
end
=> { c: 3, d: 4, e: 5 }
```
- `Integer#times` returns the caller at the end of the method call
- `Array#map` returns a **new array** with elements being the values returned by the block
- `Hash#select` returns a **new hash** with key-value pairs that evaluates to true in the block

As seen in the different method above, whether a block affects the return value of the method depends on the method implementation: how it uses the block and its return values.

[Back to Top](#section-links)


## Writing Methods That Take Blocks
In Ruby, **every method accepts an implict block**. Even if the method doesn't make use of the implicit block, that does not result in an error. 
```ruby
def hello
  "hello!"
end

hello                                         # => "hello!"
hello { puts 'hi' }                           # => "hello!"
hello("hi")                                   # => ArgumentError: wrong number of arguments (1 for 0)
```

```ruby
def echo(str)
  str
end

echo                                          # => ArgumentError: wrong number of arguments (0 for 1)
echo("hello!")                                # => "hello!"
echo("hello", "world!")                       # => ArgumentError: wrong number of arguments (2 for 1)

# this time, called with an implicit block
echo { puts "world" }                         # => ArgumentError: wrong number of arguments (0 for 1)
echo("hello!") { puts "world" }               # => "hello!"
echo("hello", "world!") { puts "world" }      # => ArgumentError: wrong number of arguments (2 for 1)
```

[Back to Top](#section-links)


### Yielding
A method will only execute a passed-in block if it contains the `yield` keyword. When Ruby interpreter ecnounter `yield`, execution flows from the method to the block. After the block completes execution and returns a value, execution will resume after the `yield` keyword.
```ruby
def echo_with_yield(str)
  yield
  str
end
```

```ruby
echo_with_yield { puts "world" }                        # => ArgumentError: wrong number of arguments (0 for 1)
echo_with_yield("hello!") { puts "world" }              # world
                                                        # => "hello!"
echo_with_yield("hello", "world!") { puts "world" }     # => ArgumentError: wrong number of arguments (2 for 1)
```


A `LocalJumpError` is raised when a method expects to `yield` to a block but no block was actually passed in.
```ruby
echo_with_yield("hello!")                          # => LocalJumpError: no block given (yield)
```


To provide a method the flexibility to be invoked with or without a block, `Kernel#block_given?` can be used as a conditional guard for `yield`. This method returns `true` only if a block is passed in during method invocation. 

```ruby
def echo_with_yield(str)
  yield if block_given?
  str
end
```
```ruby
echo_with_yield("hello!")                          # => "hello!"
echo_with_yield("hello!") { puts "world" }         # world
                                                   # => "hello!"
```

[Back to Top](#section-links)


### Tracing Execution Sequence
```ruby
# method implementation
def say(words)
  yield if block_given?
  puts "> " + words
end

# method invocation
say("hi there") do
  system 'clear'
end                                                # clears screen first, then outputs "> hi there"
```

1.  Execution starts at method invocation, on line `8`. The `say` method is invoked with two arguments: a `"hi there"` string and a block (the block is an **implicit parameter** and not part of the method definition).
2.  Execution then jumps to line 2, where the method's local variable `words` is assigned to the string `"hi there"`. The block is passed in implicitly, without being assigned to a variable.
3.  Execution continues into the first line of the method implementation, line 3, which immediately yields to the block.
4.  The block, on line 9, then gets executed, clearing the screen.
5.  After the block is done executing, execution continues in the method implementation on line 4. Executing line 4 results in output being displayed.
6.  The last expression in the method returns `nil`, which is also the return value for the `say` method.

[Back to Top](#section-links)


### Yielding With Argument
Similar to methods, blocks can be defined with parameters. In the example below, the block passed to `#times` has a single parameter `num`.
```ruby
3.times do |num|
  puts num
end
```


**Arity**: the **number of arguments** that we must pass to a `block`, `proc` or `lambda`.

- `Method` and `lambda` objects observe **strict arity**: the number of arguments passed **have to match** what is expected, else an **ArgumentError** will be raised.

- Blocks and `proc` objects have **lenient arity**: mismatches between number of arguments and parameters defined **would not** generate an error.
	1. Passing more arguments than what a block requires just meant that **extra arguments are ignored**.
	```ruby
	# method implementation
	def test
	  yield(1, 2)                           # passing 2 block arguments at block invocation time
	end

	# method invocation
	test { |num| puts num }                 # outputs "1" and second argument is ignored
	```

	2. Passing less arguments than what a block expects just meant that **parameters matched with missing arguments are assigned `nil`**
	```ruby
	# method implementation
	def test
	  yield(1)                              # passing 1 block argument at block invocation time
	end

	# method invocation
	test do |num1, num2|                    # expecting 2 parameters in block implementation
	  puts "#{num1} #{num2}"                # outputs "1 " since num2 is nil
	end
	```

[Back to Top](#section-links)


### Return Value From Yielding to Block
```ruby
def compare(str)
  puts "Before: #{str}"
  after = yield(str)
  puts "After: #{after}"
end

# method invocation
compare('hello') { |word| word.upcase }
```

Output from method invocation:
```plaintext
Before: hello
After: HELLO
=> nil
```
Similar to methods, the last evaluated statement in a block will form its return value. It is up to the method to determine how this value will be used. In above example, the return value is assigned to `after` for subsequent output.

[Back to Top](#section-links)


### When to Design Methods to Use Blocks
1. **To defer implementation code to method invocation time**. For any method, we have a method implementator and a method caller. There are times when the method implementator will just setup the processes (e.g. iterate through elements of a collection) but provide flexibility for the method caller to supply their processing logic at invocation time. `#each`, `#map`, `#select` are good examples of such methods. In general, if we have situations where we are **calling a method multiple times, each time with a minor tweak**, a generic method that yields to a block could be the solution. 

2. Methods that need to perform some **"before" and "after" actions** - sandwich code. Examples include timing methods, opening and closing of files, connecting and disconnect to sql servers.
```ruby
def time_it
  time_before = Time.now
  yield                       # execute the implicit block
  time_after= Time.now

  puts "It took #{time_after - time_before} seconds."
end

time_it { sleep(3) }                    # It took 3.003767 seconds.
                                        # => nil

time_it { "hello world" }               # It took 3.0e-06 seconds.
                                        # => nil
```

When we use `File::open` to open a file, we have to **manually close** it to release system resources.
```ruby
my_file = File.open("some_file.txt", "w+")          # creates a file called "some_file.txt" with write/read permissions
# write to this file using my_file.write
my_file.close
```

`File::open` can also take a block, then **automatically close** the file after block execution. This happens because the method implementator of `File::open` have designed it to open the file, yield to supplied block, then close the file.
```ruby
File.open("some_file.txt", "w+") do |file|
  # write to this file using file.write
end
```

[Back to Top](#section-links)


### Methods With Explicit Block Parameter
An **explicit block** is one that gets **assigned to a method parameter** and is treated like a named object. Like any object, it can be reassigned, passed to another method or invoked multiple times. 

To define an explicit block, the parameter accepting the block should be in the **last position** and be prepended with a `&` character. The `&` signify to Ruby to convert the block into a `Proc` object. A method can only have **one** parameter with `&`.
```ruby
def test(word, &block)
  puts "What's &block? #{block}"
  puts word
end

test("Nothing") { sleep(1) }

# What's &block? #<Proc:0x007f98e32b83c8@(irb):59>
# Nothing
# => nil
```
In above example, `{ sleep(1) }` is assigned to `block`. The `&` converts the block to a `Proc` object and is interpolated within the string using `to_s`, confirming it is a regular `Proc` object.

Explicit block confers the following benefits:
- There is a handle for the block, unlike an implicit block
- The block can be passed to other methods
- The block can be invoked repeated simply using `call`

```ruby
def display(block)
  block.call(">>>") # Passing the prefix argument to the block
end

def test(&block)
  puts "1"
  display(block)
  puts "2"
end

test { |prefix| puts prefix + "xyz" }
# => 1
# => >>>xyz
# => 2
```
In above example, `{ |prefix| puts prefix + "xyz" }` is assigned to explict block `block` when `test` is invoked. `block` now references a `Proc` object. `block` is then passed into `display` as an argument (notice we do not need an `&` prefix for block in `display` since the argument passed in is already a `Proc` object). We then invoke this `Proc` using `call` and supply it with `">>>"` as argument.

[Back to Top](#section-links)


### Using Closures
A powerful capability in Ruby and other languages is the ability to pass chunks of code i.e. `closures` which can be formed by blocks, `Proc` objects and `lamda`s. Closures **retain a memory** of their surrounding scope (binding) and **can use and update variables in those scope** even when they are executed elsewhere.
```ruby
def for_each_in(arr)
  arr.each { |element| yield element }
end

arr = [1, 2, 3, 4, 5]
results = [0]

for_each_in(arr) do |number|
  total = results[-1] + number
  results.push(total)
end

p results # => [0, 1, 3, 6, 10, 15]
```
Although `for_each_in` cannot access `results` array based on scoping rules, the implicit block it received enables `results` to be accessed and updated when yielded to within the method.

Methods and block can return `Proc` or `lambda` objects (note: a block cannot be a return value) that enhances the usefulness of closures.

**Proc as Return Value**
```ruby
def sequence
  counter = 0
  Proc.new { counter += 1 }
end

s1 = sequence
p s1.call           # => 1
p s1.call           # => 2
p s1.call           # => 3
puts

s2 = sequence
p s2.call           # => 1
p s1.call           # => 4 (note: this is s1)
p s2.call           # => 2
```
- `#sequence` method returns a `Proc` object that forms a closure with local variable `counter`.
- Each time we call the `Proc` object, it increments it own **private copy** of `counter`, returning `1`, `2`, `3` etc on each subsequent call.
- Every call to `sequence` returns a new `Proc` object, each with its **own copy** of `counter`. Hence the counter values of `s1` and `s2` are independent 

[Back to Top](#section-links)


## Blocks and Variable Scope
### Refresher on local variable scope
A block creates a new scope for local variables. Only outer local variables are accessible to inner blocks.
```ruby
level_1 = "outer-most variable"

[1, 2, 3].each do |n|                     # block creates a new scope
  level_2 = "inner variable"

  ['a', 'b', 'c'].each do |n2|            # nested block creates a nested scope
    level_3 = "inner-most variable"

    # all three level_X variables are accessible here
  end

  # level_1 is accessible here
  # level_2 is accessible here
  # level_3 is not accessible here

end

# level_1 is accessible here
# level_2 is not accessible here
# level_3 is not accessible here
```


### Closure and Binding
A closure **tracks its bindings** (i.e. local variables, method references, constants and other artifacts) to have all the information it needs for execution later. Whenever it is executed, it will drag all of it around. 
```ruby
def call_me(some_code)
  some_code.call
end

name = "Robert"
chunk_of_code = Proc.new {puts "hi #{name}"}
name = "Griffin III"        # re-assign name after Proc initialization

call_me(chunk_of_code)
```

The output is:
```none
hi Griffin III
=> nil
```
- In above example, local variable `name` that is otherwise out-of-scope in the method `call_me` is made available through closure binding when `some_code` was invoked within `call_me`. 
- Even though `name` was reassigned to a new value after the `Proc` object was created, it was still able to output the updated value when `chunk_of_code` was called. This suggest that the **value of bindings at `Proc` object creation** was **not stored for use during invocation**. Instead, the `Proc` object can **dynamically reference** and retrieve the up-to-date values at point of invocation.
- **Note**: If a **variable referenced in a closure is not in scope at point of closure instantiation, it will not be part of the binding** of the closure. For example, if we initialize `name = "Robert"` **after** the `Proc` object instantiation, `name` will not be part of the binding. Calling the `Proc` object will then raise a `NameError`.
```ruby
def call_me(some_code)
  some_code.call
end

chunk_of_code = Proc.new {puts "hi #{name}"}
name = "Griffin III"     # initialize name AFTER Proc instantiation

call_me(chunk_of_code)
```

New Output:
```ruby
block in <main>': undefined local variable or method `name' for main:Object (NameError)
```

[Back to Top](#section-links)


## Two Different and Opposite Usage of `&`
### Methods with an Explicit Block Parameter
```ruby
def test(&recipient)
  puts "What's &recipient? #{recipient}"
end

test { sleep(1) }

# What's &recipient? #<Proc:0x007f98e32b83c8@(irb):59>
# => nil
```
- In this scenario, `&` converts the block that is passed in into a named `Proc` object. This `Proc` object can be invoked within the method or passed to another method like any other object

### Convert A Method Argument To A Block
```ruby
# Syntax
caller.method(&object)
```

The argument can be **any object**, including `Proc` or Symbol objects. If the argument is already a `Proc` object, `&` will directly convert it to a block.

```ruby
squared = proc { |x| x**2 }
[1, 2, 3].map(&squared)
# => [1, 4, 9]
```

If the argument is not already a `Proc` object, Ruby will first try to convert it into a `Proc` by calling `#to_proc` on it. If successful, `&` will then convert the  `Proc` object into a block. This is essentially how **symbol to proc** works. Symbols have the [Symbol#to_proc](https://docs.ruby-lang.org/en/master/Symbol.html#method-i-to_proc) instance method to return a `Proc` object

```ruby
[1, 2, 3].map(&:to_s)
# => ["1", "2", "3"]
```

Is actually achieved in **two steps**:
```ruby
# Step 1: Convert Symbol to Proc
to_s_proc = :to_s.to_proc
puts to_s_proc              # <Proc:0x00007fed5917b120(&:to_s)>

# Step 2: & convert proc to block
[1, 2, 3].map(&to_s_proc)   # (&to_s_proc) => { |n| n.to_s }
# => ["1", "2", "3"]
```

Comparing the two code above, we see that `(&:to_s)` was converted to `{ |n| n.to_s }` so that both syntax have the same output.

This shortcut works with **any collection method that takes a block**, not only on `map`
```ruby
["hello", "world"].each(&:upcase!)              # => ["HELLO", "WORLD"]
[1, 2, 3, 4, 5].select(&:odd?)                  # => [1, 3, 5]
[1, 2, 3, 4, 5].select(&:odd?).any?(&:even?)    # => false
```

**Symbol-to-proc** shortcut works with any methods, including custom ones, as long as they don't have parameters
```ruby
def return_hi
  "hi"
end

[1, 2, 3].map(&:return_hi)
# => ["hi", "hi", "hi"]
```

**Methods with parameters don't work**
```ruby
# String#prepend(*other_string) takes at least 1 string argument

["1", "2"].map { |n| n.prepend("The number is: ") }  # => ["The number is: 1", "The number is: 2"]

["1", "2"].map(&:prepend("The number is: "))         # => syntax error, unexpected ')', expecting end-of-input ...ap(&:prepend "The number is: ")
```

Fortunately, there is a workaround using `Method` objects.
```ruby
def convert_to_base_8(n)
  n.to_s(8).to_i 
end

method_object = method(:convert_to_base_8)
puts method_object    # <Method: main.convert_to_base_8(n) (irb):189>

[8, 10, 12, 14, 16, 33].map(&method_object)
# => [10, 12, 14, 16, 20, 41]
```
- [Object#method(sym)](https://docs.ruby-lang.org/en/master/Object.html#method-i-method) takes a method name (without the parameters) in the form of symbol and returns a `Method` object
- `&method_object` first convert `method_object` to a `Proc` object, before `&` converts the `Proc` to a block that is equivalent to `{ |n| convert_to_base_8(n) }`. 
- This way, we have a indirect workaround for methods with parameters that cannot use the symbol-to-proc shortcut directly.

[Link to full example](../../../04_rb130_more_topics/03_exercises/04_medium_1/06_method_to_proc.rb)


Sometimes, we might have a situation where a method we call need to call another method that needs a block. We can do it by accepting the block as an explict parameter using `&parameter`, then convert `parameter` which is now a `Proc` object back to a block using `&` again for a method called within. 

**Example: Using `&` to Convert a Block to a Proc and then back to a Block**
```ruby
# any? returns true if any block return value is truthy
def any?(collection) 
  collection.each { |item| return true if yield(item) }  # expects a block
  false
end

# none? return true only if all block return value is falsey
def none?(collection)
  !any?(collection)
end

none?([1, 3, 5, 6]) { |value| value.even? }  # => # no block given (yield) (LocalJumpError)
```
While `#none?` is logically the negation of `#any?`, we cannot directly use the `#any?` code within `#none?` since the block passed to `#none?` will not be available to `#any?`, leading to `LocalJumpError`.

Solution:
```ruby
def none?(collection, &my_block)
  !any?(collection, &my_block)
end

none?([1, 3, 5, 6]) { |value| value.even? }  # => false
none?([1, 3, 5, 7]) { |value| value.even? }  # => true
```
- Use explicit parameter `&my_block`. This will convert the block passed in to a `Proc` object and assigned to `my_block`.
- When we call `#any?`, we cannot just pass in `my_block` as an argument since it is now a `Proc` object and not in block form that `#any?` is expecting. To convert the `Proc` object back into a block for `#any?` to use, we again prepend `&` to the `Proc` object, hence we call `any?` with a second argument `&my_block` even though it only accepts one based on its method definition.

[Back to Top](#section-links)


## Exploring Procs, Lambda and Blocks: Definition and Arity
### Proc
- A new `Proc` object can be created with `Kernel#proc { ... }` and is **equivalent** to those created using `Proc.new { ... }`
- **Proc** objects have **soft arity** (number of arguments taken): Mismatches between number of supplied arguments and parameters do not raise an ArgumentError. Missing arguments take on value of nil while extra arguments are ignored.
```Ruby
my_proc = proc { |thing| puts "This is a #{thing}." }
puts my_proc            # #<Proc:0x00007faec3095488 (irb):17>
puts my_proc.class      # output: Proc
	
my_proc.call			# output: "This is a ."
my_proc.call('cat')     # output: This is a cat.""
```

### Lambda
- A new `Lambda` object can be created with a call to `lambda` or `->`. We **cannot** create a new Lambda object with `Lambda.new` as Lambda class **does not exist**.
- While a `lambda` object is also a type of `Proc`, it **differs** from a regular `Proc`: this is seen when output using `puts`, with an extra `(lambda)` seen that is not present in regular `Proc` objects
- Lambda objects, like methods has **hard arity**: mismatch between supplied arguments and definition will raise an `ArgumentError`
```Ruby
my_lambda = lambda { |thing| puts "This is a #{thing}." }
my_second_lambda = -> (thing) { puts "This is a #{thing}." }

puts my_lambda         # output: #<Proc:0x00007faec68c6fa0 (irb):22 (lambda)>
puts my_second_lambda  # output: #<Proc:0x00007faec30802e0 (irb):23 (lambda)>
puts my_lambda.class   # output: Proc

my_lambda.call('dog')  # output: "This is a dog."
my_lambda.call         # ArgumentError (wrong number of arguments (given 0, expected 1))
```

### Blocks
- An **implicit block** is a grouping of code, a type of closure but **not an object**
- Like `Proc` objects, it has a **soft arity**
- Blocks will throw an error if a variable is referenced that doesn't exist in the block's scope
- A method yielding to a missing block will raise a `LocalJumpError`
```Ruby
def block_method(animal)
  yield(animal)
end

block_method('turtle') { |turtle| puts "This is a #{turtle}." }  # output: "This is a turtle."

block_method('turtle') do |turtle, seal|
  puts "This is a #{turtle} and a #{seal}."
end
 # output: "This is a turtle and a ."

block_method('turtle') { puts "This is a #{animal}." }
 # => NameError (undefined local variable or method `animal' for main:Object)

block_method('turtle')
 # => LocalJumpError (no block given (yield))
```

[Code Example for Proc, Lambda and Blocks](../../../04_rb130_more_topics/03_exercises/06_advanced_1/02_proc_lambda_blocks.rb)


## Use of Splat Operator `*` in Argument Passing
- At method invocation or block yielding, a splat operator converts an array (n elements) argument into a list i.e. n arguments

- On the receiving end i.e. method/block parameter definition, a splat operator allow a parameter receive a list of arguments i.e. match a variable number of arguments.

- The splat operator can be used at the start, middle, tail of the parameter list or not at all for a method definition or block

### Difference in Behavior Between Method Call and Yielding to Block
- In a **method call**, the splat operator `*` is **required** to convert an array into a list of arguments to match the parameters defined for that method. 
```ruby
def my_method(a, *b, c)
  p a
  p b
  p c
end

my_method(*[1, 2, 3])  # => a = 1, b = [2], c = 3
my_method(*[1, 2])     # => a = 1, b = [], c = 2
my_method([1, 2, 3])   # => Argument error, given 1, expected 2+
```

- When **yielding to a block**, the splat operator `*` is **not required** to convert an array argument to a list of arguments. This conversion happens **automatically** under the hood irrespective whether `*` is applied.
```ruby
def to_block(arr)
  yield(arr)
end

to_block([1, 2, 3, 4]) do |a, *b, c|
  p a
  p b
  p c
end
# argument assignment: a = 1, b = [2, 3], c = 4

def to_block(arr)
  yield(*arr)
end

to_block([1, 2, 3, 4]) do |a, *b, c|
  p a
  p b
  p c
end
# argument assignment: a = 1, b = [2, 3], c = 4
```
[Link to pass parameter example](../../../04_rb130_more_topics/03_exercises/04_medium_1/05_pass_parameter_3.rb)

[Back to Top](#section-links)