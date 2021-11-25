#### Summary
- `loop` is a method and associate `do ... end` generates its own scope
- `while` is a control statement (similar to `if`, `unless` or `break`) and do not generate its own scope
- An if conditional placed **after** an expression is known as an if modifier
- Conditional placed at the **end** of a loop is equivalent to a do-while loop since content loop at least once
- Conditional placed at the **start** of a loop is equivalent to a while loop

#### Loop
- `loop` is a **method** from the Ruby Kernel module and is autoloaded and available anywhere
- As `loop` is a method, the do ... end that follows is a `block` and has its own scope
- `while` is not a method but a [control expression](https://docs.ruby-lang.org/en/2.6.0/doc/syntax_rdoc.html) similar to `if`, `unless` or `break`. Hence it does not create own scope and local variables initialized within is still accessible outside

```ruby
loop do
  a = 10
  break
end

a	# => NameError (undefined local variable or method `b' for main:Object)

while true do
  b = 10
  break
end

b 	# => 10, accessible
```

#### If Modifier
- `if` becomes a modifier when the conditional is appended after a statement.
- Although readable, if modifiers cannot support multiple expressions unlike an if statement
```ruby
num = 0

loop do
  puts "Hello!"
  num += 1
  break if num > 10  # if modifier example
end
```

#### Break Placement
- `break`, when placed on the **end** of a loop is 	equivalent to a `"do/while"` effects as loop contents are guaranteed to execute at least once
- `break`, when placed at the **start** of a loop is equivalent to a `while` loop that checks the conditional before executing any contents in a loop

#### Next
- `next` keyword skips the rest of current iteration and is often paired with an if modifier 
```ruby
counter = 0

loop do
  counter += 1
  next if counter.odd?	# skip all odd numbers, i.e. loop only print even numbers
  puts counter
  break if counter > 5  # has to be > 5 since 5 is also an odd number and will be skipped
end
```


#### Looping a String
```ruby
alphabet = 'abcdefghijklmnopqrstuvwxyz'
counter = 0

loop do
  break if counter == alphabet.size
  puts alphabet[counter]
  counter += 1
end
```
- Note the following edge case when looping through strings
	- Placing `counter += 1` before if modifier could result in an **infinite loop** if string is empty as counter > string length 
	```ruby
	alphabet = ""
	counter = 0
	
	loop do
	  puts alphabet[counter]
	  counter += 1
	  break if counter == alphabet.size
	end
	```

	- Use  "counter **>=** alphabet.size" as a conditional to ensure we can `break` out of the loop regardless of whether `counter` is exactly equal to `alphabet.size` or not.
	```ruby
	alphabet = "abc"
	counter = 3
	
	loop do
	  puts alphabet[counter]
	  counter += 1
	  break if counter >= alphabet.size
	end
	```
	
#### Looping an Array
- Similar to looping Strings
```ruby
colors = ['green', 'blue', 'purple', 'orange']
counter = 0

loop do
  break if counter == colors.size
  puts "I'm the color #{colors[counter]}!"
  counter += 1
end
```
	
#### Looping a Hash
- More complicated to loop as hashes are not integer indexed and keys can be any Ruby object.
- To loop, we first get keys of a hash as an array using Hash#keys, then retrieve the corresponding values.

```ruby
number_of_pets = {
  'dogs' => 2,
  'cats' => 4,
  'fish' => 1
}
pets = number_of_pets.keys # => ['dogs', 'cats', 'fish']
counter = 0

loop do
  break if counter == number_of_pets.size
  current_pet = pets[counter]
  current_pet_number = number_of_pets[current_pet]
  puts "I have #{current_pet_number} #{current_pet}!"
  counter += 1
end
```