#### Naming Things
- Variables could be named according to their purpose and not the actual values they can be set
	- e.g. naming a user response variable as `yes_no` may be inadequate if more options arise in future. `play_again` could be a better name.
- Naming conventions:
	- UPPERCASE for constants
	- CamelCase for classes
	- snake_case for everything else, including file names


#### Mutating Constants
- We **should not** change values of constant variables even though Ruby doesn't raise an error. Constants should be immutable.
```ruby
CARDS = [1, 2, 3]
```


#### Methods
- Method should be short (<15 lines) and perform a **single** function: 
	- perform side effects i.e. print to console or mutate a caller or its arguments without returning any meaningful value OR
	- return a value without side effects 
- Their names should provide clarity on what they do and how to use them
	- e.g. `update_total(total, cards)` will be expected to mutate total and we should not expect to use it like `total = update_total(total, cards)`
	- methods that prints an output should be prefixed with `display_`, `print_`
- Methods should be implemented at the same level of abstraction
- Methods should specify what it does, not how it does it
  
  
#### `If` Conditional
- It is good practice to only perform equality comparison (`==`) and avoid assignment (`=`) in an if statement
```ruby
if operator == '1'  # ok
...
if operator = '1'   # truthy, others will not know if you intended it to be == or =
```

-   Pay attention to data types when making comparison
```ruby
if operator == 1
# vs
if operator == '1'
```
    
-   `if` is part of ruby syntax for control expressions. Unlike a method, it does not create a new scope. Hence local variables initialized within an `if` is still accessible outside of the `if` statement.
    
-   `if` expressions return the value from the executed branch in Ruby
```ruby
answer = if true
           'yes'
         else
           'no'
         end
Kernel.puts(answer)       # => yes
```


#### Miscellaneous Tips
##### Truthiness
- We do not have to compare a control expression to `true` or `false` but can rely on it's truthiness directly
```ruby
if user_input == true

# could be just

if user_input
```

- The truthiness of a variable can be used directly as a method return value. Note however return value based on truthiness may not necessarily be boolean
```ruby
def method_name
	...
	if user_input
		true
	else
		false
	end
end

# could be just

def method_name
	...
	user_input
end
```


##### Learning and Retention
- Problem areas that gave us the biggest pain tend to be the ones we retain the longest. The time spent fixing problems we encountered is not time wasted but will pay dividends in future
