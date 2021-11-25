## Precedence

#### Summary
- Use parentheses to make clear order of execution. Else it is easy to forget precedence order (both ourself and other developers) and introduce a bug
- Evaluation order depends **not just** on operator precedence and can be influenced by left-to-right, right-to-left or short-circuiting (`?:`, `&&`, `||`) considerations
- short-circuiting of logical operators can meant higher precedence operations (e.g. arithmetic) are skipped
<br>

#### Operator Precedence
- A set of [rules](https://ruby-doc.org/core-3.0.2/doc/syntax/precedence_rdoc.html) that dictate how Ruby determines what operands each operator will take
- Most operators take 2 operands, but there are some which takes 1 (unary) or 3 (ternary)
```ruby
2 + 5             # Two operands (2 and 5)
!true             # Unary: One operand (true)
value ? 1 : 2     # Ternary: Three operands (value, 1, 2)
```
- An operand can be the result of evaluating some other operator and their operands
```irb
> 3 + 5 * 7		  # The second operand of + is the result of 5 * 7
```
- We can use parentheses to override the default evaluation order or improve clarity over execution order
```ruby
(3 + 5) * 7 # => 56
```
- When two operations involve operators of the same precedence, operations will occur left-to-right (or right-to-left in some cases e.g. assignment)
- An operator with higher precedence than another is said to **bind** more tightly to its operands
<br>

#### Evaluation Order
- Precedence do not fully determine evaluation order. Other considerations include left-to-right or right-to-left evaluation, [[short-circuiting]] of logical operators (`&&` and `||`) and ternary expressions.

```ruby
def value(n)
  puts n
  n
end

puts value(3) + value(5) * value(7)
```

```plaintext
3
5
7
38
```
- In above arithmetic expression above, Ruby will need the values from the method calls first before calIing any operators. 
- `value(3)`, `value(5)` and `value(7)` are thus evaluated in a `left-to-right` order to return the operands first before we follow operator precedence to compute the results 
- Hence, actual execution order depends on multiple considerations and is complicated 

##### Right-to-left Evaluation
```ruby
a = b = c = 3
puts a if a == b if a == c
```
- `right-to-left` evaluation happens in assignments or multiple modifiers
- However, these are **BAD** practices and should be avoided

##### Ternary (`?:`) and Logical  (`&&`, `||`) Operators
- `&&` have **higher** precedence than `||`
- [[short-circuiting]] behavior meant Ruby treats `?:`, `&&` and `||` differently and may skip seemingly higher precedence operators unless required

```ruby
3 ? 1 / 0 : 1 + 2  # raises error ZeroDivisionError
5 && 1 / 0         # raises error ZeroDivisionError
nil || 1 / 0       # raises error ZeroDivisionError
```

```ruby
nil ? 1 / 0 : 1 + 2  # 3, short-circuiting
nil && 1 / 0         # nil, short-circuiting
5 || 1 / 0           # 5, short-circuiting
```
- Thus, even though precedence rules suggest `1 / 0` to be evaluated first, [[short-circuiting]] of logical operators may mean these never get executed

##### Block Precedence
```ruby
array = [1, 2, 3]

array.map { |num| num + 1 }     # => [2, 3, 4]
```
```ruby
p array.map { |num| num + 1 }      #  outputs [2, 3, 4]
                                   #  => [2, 3, 4]
```
- In above example, return value of `map` then gets passed into `p` as an argument, which outputs `[2, 3, 4]`

```ruby
array.map do |num|
  num + 1
end                                 #   => [2, 3, 4]
```
```ruby
p array.map do |num|
  num + 1                   #  outputs #<Enumerator: [1, 2, 3]:map>
end                           #  => #<Enumerator: [1, 2, 3]:map>
```
- Even though the code here is exactly the same, by changing block format from `{ ... }` to `do ... end`, we got a totally different result 


```ruby
p array.map                   # outputs <Enumerator: [1, 2, 3]:map>

p array.map do |num|
  num + 1
end                           # outputs <Enumerator: [1, 2, 3]:map>
```
- The reason is `do ... end` has the lowest precedence of all operators, even slightly lower than fellow block form `{ ... }`
- Hence `p` and `array.map` are binded more tightly then `array.map` and accompanying `do ... end`. array.map thus got executed without the block, returning an Enumerator object. Both the Enumerator object and block are then passed as arguments to `p`. Since `p` doesn't accept a block, it merely prints the Enumerator, and disregards the block
- A `{}` block, having higher priority, will bind more tightly to `array.map` and get passed in as an argument to the `map` method call. The result is then pass in to `p`.

```ruby
# Using parentheses to show code equivalent to above example 
array = [1, 2, 3]

p(array.map) do |num|
  num + 1                           #  <Enumerator: [1, 2, 3]:map>
end                                 #  => <Enumerator: [1, 2, 3]:map>

p(array.map { |num| num + 1 })      # [2, 3, 4]
                                    # => [2, 3, 4]
```

##### Ruby's **tap** Method
- [`tap`](https://ruby-doc.org/core-2.7.0/Object.html#method-i-tap), an Object instance method, will pass the calling object as the block argument, then returns that same object.
-  This property make it easy to print the calling object in the block passed to tap for debugging purposes

```ruby
array = [1, 2, 3]
mapped_array = array.map { |num| num + 1 }

mapped_array.tap { |value| p value }              # => [2, 3, 4]
```
- We can also use it debug intermediate objects by tapping method chains. 
```ruby
(1..10)                 .tap { |x| p x }   # 1..10
 .to_a                  .tap { |x| p x }   # [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
 .select {|x| x.even? } .tap { |x| p x }   # [2, 4, 6, 8, 10]
 .map {|x| x*x }        .tap { |x| p x }   # [4, 16, 36, 64, 100]
```
<br>

#### Tags
#precedence, #left-to-right, #right-to-left, #short_circuiting, #logical_operators