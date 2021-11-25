## Truthiness and Logical Operations

#### Summary
- Truthiness: All values except `false` and `nil`
- Falsey: `false` and `nil`
- Logical operators: `&&` and `||`
- Short-circuiting behavior of logical operators allows certain operations to be skipped if the truthiness of the expression is not longer in doubt
<br>

#### Boolean
- `Boolean` datatypes represented by `true` or `false` values are used in Ruby for conditional logic expressions.
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
<br>

#### Truthiness
- In Ruby, "falsey" is a superset of `false` and contains both the values `false` and `nil`
- "Truthy" is a superset of `true` and encompasses all values that are **not "falsely"**, including the integer `0` which is interpretated as false in some programming languages (e.g. C++)
- A "truthy" object does not necessarily equates to `true`. For example the number 5 is truthy but returns false when compared to `true`
```ruby
5 == true        # => false
```
<br>

#### Logical Operators
Logical operators return either "truthy" or "falsey" values after evaluating expressions
<br>
- **`&&`** logical "and" operator 
	- returns a `truthy` value only when **both** operands are `truthy`. It returns a `falsey` value otherwise
	```irb
	irb:001> true && true
	=> true
	irb:002> true && false
	=> false
	```

	- NOTE: `&&` does NOT always return a boolean value but the logical conditional still work based on truthiness
	```ruby
	# && has to evaluate both operands
	# if 1st operand is false or nil, it will
	# return 1st operand value due to short 
	# ciruiting, else it return 2nd operand's value
	
	irb(main):001> nil && false
	=> nil

	irb(main):002> false && nil
	=> false
	
	irb(main):003> true && nil
	=> nil
      
	irb(main):004> true && false
	=> false

	irb(main):005> 1 && 21
	=> 21
	```

- **`||`** the "or" operator 
	- returns a `truthy` value as long as one of its operands is `truthy`. It only returns false when all its operands are false
	```irb
	irb:001> true || true
	=> true
	irb:002> true || false 
	=> true
	irb:003> false || false 
	=> false
	```

- NOTE: `||` does NOT always return a boolean value but the logical conditional still work based on truthiness
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
	
	irb:006> false || true
	=> true
	```

- Comparison operators such as `>,<,>=,<=, ==` has a higher [[precedence]] than `&&` so  `num < 10 && num.odd?` is automatically interpreted as  `(num < 10) && (num.odd?)` rather than  `num < (10 && num.odd?)` without the need for parenthesis
	```ruby
	irb:001> num = 5
	=> 5
	irb:002> num < 10 && num.odd?
	=> true
	irb:003> num > 10 && num.odd?
	=> false
	```

- We can chain expressions and it will be evaluated **left to right**
	```irb
	irb:002> 5 < 10 && 5.odd? && 5 > 0
	=> true
	irb:003> 5 < 10 && 5.odd? && 5 > 0 && false
	=> false
	irb:004> 5 > 10 || false || 4
	=> 4
	``` 

- `&&` and `||` operators both exhibit [[short-circuiting]] behavior, expressions need not be fully evaluated once return value is guaranteed

```irb
irb:001> false && 3/0
=> false
```
- No `ZeroDivisionError` as 3/0 is not evaluated as return value will be false after processing first operand to `&&` 
<br>

```irb
irb:001> false || 3/0
ZeroDivisionError: divided by 0
```
- A `ZeroDivisionError` will be generated since `||` still need to process the second operand
<br>

```irb
irb:001> true || 3/0
=> true
```
- `||` can skip 2nd operand due to short circuiting as 1st operand already true. No `ZeroDivisionError` results
<br>

```irb
irb:001> true && 3/0
ZeroDivisionError: divided by 0
```
- `ZeroDivisionError` results as no short circuiting applicable
<br>

#### Expressions and Conditionals
In real code, we often use expressions that evaluates to either `true` or `false` objects in conditionals
```ruby
num = 5

if (num < 10)
  puts "small number"
else
  puts "large number"
end
```

- Truthiness can be used to combine variable assignment within a conditional by letting `find_name` method will return either a valid object or a falsey value (`nil` or `false`). Even though this works, this is NOT preferred as others might assume the developer had intended equality comparison `==` rather than assignment.  
```ruby
if name = find_name
  puts "got a name"
else
  puts "couldn't find it"
end
```
 
- Instead, the following is **preferred**:
	- Assign variables **outside** of `if` statements to avoid confusion of intent
	- Take advantage of short-circuit behavior in case `name` is falsey before checking validity. Note: valid? is not a built-in method but a custom implementation
```ruby
name = find_name

if name && name.valid?
  puts "great name!"
else
  puts "either couldn't find name or it's invalid"
end
```
<br>

#### Tags
#truthiness, #precedence, #logical_operators, #short_circuiting
