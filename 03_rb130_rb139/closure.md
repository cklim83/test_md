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
- [When To Use Blocks In Own Methods](#when-to-use-blocks-in-own-methods)
- [Methods With Explicit Block Parameter](#methods-with-explicit-block-parameter)
- [Using Closures](#using-closures)

[Blocks and Variable Scope](#blocks-and-variable-scope)\
[Symbol to Proc](#symbol-to-proc)

---

## Summary
- Blocks are one way that Ruby implements closures. Closures are a way to pass around an unnamed "chunk of code" to be executed later.
- Blocks can take arguments, just like normal methods. But unlike normal methods, it won't complain about wrong number of arguments passed to it.
- Blocks return a value, just like normal methods.
- Blocks are a way to defer some implementation decisions to method invocation time. It allows method callers to refine a method at invocation time for a specific use case. It allows method implementors to build generic methods that can be used in a variety of ways.
- Blocks are a good use case for "sandwich code" scenarios, like closing a `File` automatically.
- Methods and blocks can return a chunk of code by returning a `Proc` or `lambda`.

## What Is A Closure
- A **closure** is a general programming concept that allows programmers to **save a chunk of code** to be executed at a later time. This construct is useful to pass code into methods. 

- It is called a closure as it **binds surrounding artifacts** (i.e. variables and constants the code chunk references and the methods it calls) and builds an "enclosure" around them so that these artifacts can be referenced when the closure is executed later.

- In Ruby, a closure can be implemented in one of 3 ways:
	- Instantiating a **`Proc`** object 
	- Using **lambda**
	- Using **blocks**

We will focus on blocks in this lesson.

[Back to Top](#section-links)


## Calling Methods With Blocks
`do ... end` or `{ ... }` following a method invocation represents a block passed as an **argument** to the method call.

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

Whether a block will affect the return value of the method invocation depends on how the method is implemented. The method implementation determines whether if execute or ignores the block and how it treats the block's return values.

[Back to Top](#section-links)


## Writing Methods That Take Blocks
In Ruby, **every method, regardless of its definition, takes an implict block**. It may ignore the implicit block but it still accepts it and does not cause an error. 
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
A method will only execute the passed-in block if it contains the `yield` keyword. When `yield` is evaluated, execution is passed from the method to the block. After the block completes execution and returns (a value), execution resumes after the yield keyword.
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


We encounter a `LocalJumpError` when a method expects to yield to a block but no block was passed in.
```ruby
echo_with_yield("hello!")                          # => LocalJumpError: no block given (yield)
```


To provide a method the flexibility to be invoked with or without a block, we use `block_given?` as a conditional guard for `yield`. `block_given` is a **Kernel** method and returns `true` only if a block is passed into a method invocation. 

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

1.  Execution starts at method invocation, on line 8. The `say` method is invoked with two arguments: a `"hi there"` string and a block (the block is an implicit parameter and not part of the method definition).
2.  Execution goes to line 2, where the method local variable `words` is assigned the string `"hi there"`. The block is passed in implicitly, without being assigned to a variable.
3.  Execution continues into the first line of the method implementation, line 3, which immediately yields to the block.
4.  The block, line 9, is now executed, which clears the screen.
5.  After the block is done executing, execution continues in the method implementation on line 4. Executing line 4 results in output being displayed.
6.  The method ends, which means the last expression's value is returned by this method. The last expression in the method invokes the `puts` method, so the return value for the method is `nil`.

[Back to Top](#section-links)


### Yielding With Argument
Sometimes, the block passed to a method requires an argument. In the example below, the block passed to `#times` has a single parameter `num` and takes 1 argument when yielded.
```ruby
3.times do |num|
  puts num
end
```

In Ruby, the **number of arguments** that we must pass to a `block`, `proc` or `lambda` is called its **arity**. `Method` and `lambda` has **strict arity** i.e. the number of argument **have to match** what is expected, else an error is raise. `Blocks` and `procs` have lenient arity: passing wrong number of arguments **would not** generate an error.
1. Passing more arguments than block require: Extra arguments simply ignored.
```ruby
# method implementation
def test
  yield(1, 2)                           # passing 2 block arguments at block invocation time
end

# method invocation
test { |num| puts num }                 # outputs "1" and second argument is ignored
```

2. Passing less arguments than block expects. The local variable with missing argument will be assigned `nil` value
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
Similar to methods, the last evaluated statement from a block will return a value, which can be used within the method. In above example, the return value is assigned to `after` for subsequent output.

[Back to Top](#section-links)


### When To Use Blocks In Own Methods
1. To defer implementation code to method invocation time. For any method, we have a **method implementator** and a **method caller**. There are times when the method implementator will just setup the processes (e.g. iterate through elements of a collection) but provide flexibility for the method caller to supply their processing logic at invocation time. `#each`, `#map`, `#select` are good examples of such methods. In general, in situations where we are **calling a method multiple times, each time with a minor tweak**, we could explore implementing a generic method that yields to a block. 

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

Typically, we use `File::open` to open a file and have to **manually close** it to release system resources used by `my_file`.
```ruby
my_file = File.open("some_file.txt", "w+")          # creates a file called "some_file.txt" with write/read permissions
# write to this file using my_file.write
my_file.close
```

`File::open` can also take a block and **automatically close** the file after block is executed. This happens because the method implementator of `File::open` will open the file, yields to the block, then close the file.
```ruby
File.open("some_file.txt", "w+") do |file|
  # write to this file using file.write
end
```

[Back to Top](#section-links)


### Methods With Explicit Block Parameter
An explicit block is a block that gets assigned to a method parameter and is treated as a named object. Like any object, it can be reassigned, passed to toher methods and invoked multiple times. 

To define an explicit block, we add a parameter to the method definition whose name begins with a `&` character. The `&` signify to Ruby to convert the block into a `Proc` object.
```ruby
def test(&block)
  puts "What's &block? #{block}"
end

test { sleep(1) }

# What's &block? #<Proc:0x007f98e32b83c8@(irb):59>
# => nil
```
In above example, `{ sleep(1) }` is assigned to `block` which when interpolated within the string with `to_s`, reveals that it is referencing a `Proc` object.

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
In above example, `{ |prefix| puts prefix + "xyz" }` is assigned to explict block `block` when `test` is invoked. `block` now references a `Proc` object. `block` is then passed into `display` as an argument (notice we do not need an `&` prefix for block in `display` since we argument passed in is already a `Proc` object). We then invoke this `Proc` using `call` and supply it with `">>>"` as argument.

[Back to Top](#section-links)


### Using Closures
A powerful capability in Ruby and other languages is the ability to pass chunks of code i.e. `closures` which can be formed by blocks, `Proc` objects and `lamda`s. They retain a memory of their surrounding scope (binding) and can use and update variables in that scope when they are executed, even if the block, `Proc` or `lambda` is called from somewhere else.
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
Though the block passed to `for_each_in` is invoked inside that method, the block still retained access to `results` array through closure. 


Where closures is really useful is when a method or block returns a closure in the form of `Proc` or `lamda` (note: we cannot return a block). Below is an example of a method returning a `Proc`
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
In above example, `#sequence` method returns a `Proc` that forms a closure with local variable `counter`. Subsequently, we can call the returned `Proc` repeatedly and each time we call, it increments it own **private copy** of `counter`. Hence it returns `1`, `2`, and `3` on the first, second and third call.

We can create multiple `Proc` objects from sequence, each with its own independent copy of `counter`. Thus when `s2` is assigned `#sequence` and called, it outputs its own copy of `counter`.

[Back to Top](#section-links)


## Blocks and Variable Scope
### Refresher on local variable scope
A block creates a new scope for local variables and only outer local variables are accessible to inner blocks.
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
A closure keeps track its binding (i.e. local variables, method references, constants and other artifacts) to have all the information it needs for execution later. Whenever it is executed, it will drag all of it around. 
```ruby
def call_me(some_code)
  some_code.call
end

#name = "Robert"
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
- Even though `name` was reassigned to a new value after the `Proc` was created, invocation of `chunk_of_code` still outputs the updated value. This shows that when the `Proc` was first created, the current value of name was not preprocessed and stored for later use. Instead, when a `Proc` is called, it can **dynamically reference** the current values of binded artifacts.
- Note: removing `name = "Robert"` would mean that `name` is not part of the binding of the `Proc` object since `name` is only initialized after the `Proc` is instantiated and is thus not in scope. Calling the `Proc` now will generate a NameError.
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


## Symbol to Proc
```ruby
[1, 2, 3, 4, 5].map do |num|
  num.to_s
end

# => ["1", "2", "3", "4", "5"]
```

Equivalent shortcut:
```ruby
[1, 2, 3, 4, 5].map(&:to_s)                     # => ["1", "2", "3", "4", "5
```

**When applied to an argument object for a method, a lone `&` causes ruby to
try to convert that object to a block. If that object is a proc, the
conversion happens automatically, just as shown above. If the object is
not a proc, then & attempts to call the #to_proc method on the object first.
Used with symbols, e.g., &:to_s, Ruby creates a proc that calls the #to_s
method on a passed object, and then converts that proc to a block. This is
the "symbol to proc" operation (though perhaps it should be called "symbol
to block").

Note that &, when applied to an argument object is not the same as an &
applied to a method parameter, as in this code:
```ruby
def foo(&block)
  block.call
end
```

While & applied to an argument object causes the object to be converted to
a block, & applied to a method parameter causes the associated object to be
converted to a proc. In essence, the two uses of & are opposites.**

Comparing the two code above, we see that `(&:to_s)` is equivalent to `{ |n| n.to_s }`. This is achieved in a two step process:
1. Ruby first calls `Symbol#to_proc` to convert `:to_s` to a `Proc` object
2. The `&` operator will then convert the `Proc` object into a block

```ruby
def my_method
  yield(2)
end

# turns the symbol into a Proc, then & turns the Proc into a block
my_method(&:to_s)               # => "2"
```

Is equivalent to:
```ruby
def my_method
  yield(2)
end

a_proc = :to_s.to_proc          # explicitly call to_proc on the symbol
my_method(&a_proc)              # convert Proc into block, then pass block in. Returns "2"
```

This shortcut works with **any collection method that takes a block**, not only on `map`
```ruby
["hello", "world"].each(&:upcase!)              # => ["HELLO", "WORLD"]
[1, 2, 3, 4, 5].select(&:odd?)                  # => [1, 3, 5]
[1, 2, 3, 4, 5].select(&:odd?).any?(&:even?)    # => false
```

Note: this shortcut **does not work for methods with parameters** e.g. `String#prepend(*other_string)`. So we cannot do something like 
```ruby     
[1, 2].map(&:to_s).map { |n| n.prepend("The number is: ") }  # => ["The number is: 1", "The number is: 2"]

[1, 2].map(&:to_s).map(&:prepend("The number is: "))         # => syntax error, unexpected ')', expecting end-of-input ...ap(&:prepend "The number is: ")
```


**Example: Using `&` to Convert a Block to a Proc and then back to a Block**
```ruby
# any? returns true if any block return value is truthy
def any?(collection)
  collection.each { |item| return true if yield(item) }
  false
end

# none? return true only if all block return value is falsey
def none?(collection)
  !any?(collection)
end

none?([1, 3, 5, 6]) { |value| value.even? }  # => # no block given (yield) (LocalJumpError)
```
While `#none?` is the negation of `#any?`, we cannot directly use the `#any?` code within `#none?` since The block passed to `#none?` will not be available to `#any?`, leading to `LocalJumpError`.

Solution:
```ruby
def none?(collection, &my_block)
  !any?(collection, &my_block)
end

none?([1, 3, 5, 6]) { |value| value.even? }  # => false
none?([1, 3, 5, 7]) { |value| value.even? }  # => true
```
- Store the block as an explicit parameter using `&my_block`. This will convert the block passed in to a `Proc` object named `my_block`
- When we call `#any?`, we DO NOT just pass in `my_block` as an argument it is now a `Proc` object while `#any?` is expecting a block. To convert back to a block for `#any?` to use, we need to prepend `&` to convert back to a block, hence `&my_block` again.

[Back to Top](#section-links)