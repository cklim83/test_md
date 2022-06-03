# Truthiness

## Section Links
[Boolean](#boolean)\
[Expressions and Conditionals](#expressions-and-conditionals)\
[Logical Operators](#logical-operators)\
[Short Circuiting](#short-circuiting)\
[What is Truthiness](#what-is-truthiness)

---

## Boolean
**Boolean** datatypes are important in a programming language as they can express "true" or "false" values to build conditional logic. In Ruby, boolean datatypes are represented by `true` or `false` objects in Ruby.
```ruby
true.class          # => TrueClass
true.nil?           # => false
true.to_s           # => "true"
true.methods        # => list of methods you can call on the true object

false.class         # => FalseClass
false.nil?          # => false
false.to_s          # => "false"
false.methods       # => list of methods you can call on the false object
```

**Example: Using boolean in conditionals**
```ruby
if true
  puts 'hi'
else
  puts 'goodbye'
end
```

[Back to top](#section-links)


## Expressions and Conditionals
In actual code, we do not use `true` or `false` directly. Instead, we will likely have some expression or method call that evaluates to a `true` or `false` object in a conditional.

**Example: Expression as Conditional**
```ruby
num = 5

if (num < 10)
  puts "small number"
else
  puts "large number"
end
```
Above code outputs "small number" since the expression `num < 10` evaluates to `true`

**Example: Method Call As Conditional Guard**
```ruby
puts "it's true!" if some_method_call
```
Above code will output "its's true!" only if `some_method_call` returns `true`

[Back to top](#section-links)


## Logical Operators
Logical operators will return either a "truthy" or "falsey" value when evaluating two expressions
- **`&&`** logical "and" operator: returns `true` only when **both** expressions being evaluated are `true`
	```irb
	irb:001> true && true
	=> true
	irb:002> true && false
	=> false
	```

- **Note**: `&&` can also return non-boolean values, if the expressions do not return a boolean value. Nevertheless, the conditional can still work based on [truthiness](#what-is-truthiness)
	```ruby
	# && has to evaluate both operands
	# if 1st operand is false or nil, it will
	# return 1st operand value due to short 
	# ciruiting, else it return 2nd operand's value
	
	irb(main):001> nil && false
	=> nil

	irb(main):002> true && nil
	=> nil

	irb(main):003> true && 1
	=> 1

	irb(main):004> 0 && 1
    => 1
	```

- **`||`** the "or" operator: returns `true` as long as one expression is `true`.
	```irb
	irb:001> true || true
	=> true
	irb:002> true || false 
	=> true
	irb:003> false || false 
	=> false
	```

- **Note**: Like `&&`, `||` can also return non-boolean values.
	```ruby
	# short circuit behavior (see below) apply to ||
	# Will true first operand and stop checking if 
	# 1st operand is truthy. Else return second 
	# operand
	
	irb:001> true || nil
	=> true
	
	irb:002> 1 || false
	=> 1
	
	irb:003> false || nil
	=> nil
	
	irb:004> nil || false
	=> false
	
	irb:005> nil || 1
	=> 1
	```

[Back to top](#section-links)


## Short Circuiting
the `&&` and `||` operators exhibit a behavior called _short circuiting_, which means it will stop evaluating expressions once it can guarantee the return value.

**Example: `&&` short circuiting** 
```irb
irb:001> false && 3/0
=> false
```
`&&` will short circuit when it encounters the first `false` expression. Hence `3/0` never got executed and no `ZeroDivisionError` occurs.
	
```irb
irb:001> false || 3/0
ZeroDivisionError: divided by 0
```
Notice how `false || 3/0` will generate an error.

**Example: `||` short circuiting**
```irb
irb:001> true || 3/0
=> true
```
`||` will short circuit when it encounters the first `true` expression. Hence `3/0` is never executed, avoiding a `ZeroDivisionError`.

```irb
irb:001> true && 3/0
ZeroDivisionError: divided by 0
```
Similar to `||` above, `true && 3/0` will generate an error.

[Back to top](#section-links)


## What Is Truthiness
Truthiness is more than just `true`. In Ruby, everything that is not `false` or `nil` is considered `truthy`. Values such as `0`, `""` or `[]` are all considered truthy that **evaluates to true** when used in a conditional. 

Corresponding, `false` and `nil` are both considered falsey and evaluates to `false` in a conditional.

**Example: Conditional With Truthy Value**
```ruby
num = 5

if num
  puts "valid number"
else
  puts "error!"
end
```
Above code outputs "valid number" since `num` holds the value of `5` and evaluates to true in the if conditional.

A valid can be truthy but doesn't necessarily equate to `true`
```ruby
num = 5
num == true        # => false
```

**Tip:** We could convert a truthy or falsey value to a boolean by prepending it with `!!`
```ruby
!!""     # => true
!!0      # => true
!!true   # => true
!!nil    # => false
!!false  # => false
```

Truthiness from assignment could sometimes be used in a conditional. Presumably, the `find_name` method should return `nil` rather than `""` if no name was found for this to work. Nevertheless, doing this is dangerous as others can easily thought the developer meant to do an equality comparsion rather than an assignment.  
```ruby
if name = find_name
  puts "got a name"
else
  puts "couldn't find it"
end
```
 
Better practice:
- Assign variables **outside** of `if` statements to avoid confusion of intent
- Take advantage of short-circuit behavior in case `name` is falsey before checking validity.
```ruby
name = find_name

if name && name.valid?
  puts "great name!"
else
  puts "either couldn't find name or it's invalid"
end
```

[Back to top](#section-links)

