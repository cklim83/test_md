# Ruby Style
[Full Ruby Style Guide](https://rubystyle.guide/)

## Indentation
- Use **2 spaces** for *indentation*. 
- Ensure tab key is converted to 2 spaces in chosen editor

## Commenting
A single line comment in Ruby is starts with `#`. Multi-line comments are bounded by `=begin` ... `=end`.

## Formatting
Use **snake_case** for naming files, assigning variables and defining methods
```ruby
# Naming a file
my_program.rb

# Assigning a variable
forty_two = 42

# Defining a method
def my_method
  # do stuff
end
```

Use **CamelCase** for naming a **Class**. CamelCase has no spaces and capitalizes every word.
```ruby
# Class naming
class MyFirstClass
end

class MySecondClass
end
```

Use **all uppercase letters** when defining **CONSTANTS**
```ruby
# Constant declaration
FOUR = 'four'
FIVE = 5
```

For **blocks**, use `{}` braces for single line blocks and `do ... end` for multi-line ones.
```ruby
# Single line block
[1, 2, 3].each { |i| # do stuff }

# Multi-line block
[1, 2, 3].each do |i|
  # do stuff
  # ...
end
```

**Parentheses** are **optional** when calling methods. Although this improves convenience during coding, it can make it difficult to differentiate between local variables and method invocations.
```ruby
gets.chomp # usual way to get input

Kernel.gets().chomp() # equivalent way but parentheses are usually omitted.
```

Even the interpreter shares this confusion. An undefined name `a` will raise a `NameError` but the error message would express this uncertainty by indicating that `a` could be a local variable or a method. However, if we try to execute `a()` instead, the ambiguity is resolved, with the error message referring to an undefined method.
```ruby
irb(main):002:0> a 
NameError (undefined local variable or method `a' for 	main:Object)
	
irb(main):003:0> a() 
NoMethodError (undefined method `a' for main:Object)
```