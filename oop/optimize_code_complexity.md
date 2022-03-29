# Optimizing Code Complexity
## Section Links
[What is Rubocop's AbcSize Complaint?](#what-is-rubocops-abcsize-complaint)\
[Why We Should Keep AbcSize Low?](#why-we-should-keep-abcsize-low)\
[Counting Assignments](#counting-assignments)\
[Counting Conditions](#counting-conditions)\
[Counting branches](#counting-branches)

---


## What is Rubocop's AbcSize Complaint?
AbcSize stands for **assignment**, **branch** and **conditional** size and is a measure code complexity.
```ruby
abc_size = Math.sqrt(a**2 + b**2 + c**2)

# a: assignments
# b: branches i.e. method calls
# c: conditionals
```
- If `abc_size` exceeds **18**, rubocop flags a method as too complex.
- The most effective way to reduce `abc_size` is to **focus on reducing the biggest component** (due to the square effect)
	- Example: A method with `6` assignments, `16` branches and `7` conditions will have a `abc_size` of `18.47` i.e. `Math.sqrt(6**2 + 16**2 + 7**2)`. Just by reducing the branch count, the biggest contributor by `1`, we bring the `abc_size` to `17.6`, below the threshold.

[Back to top](#section-links)


## Why We Should Keep AbcSize Low?
- Complex code is usually **hard to read, maintain and more prone to bugs.** Hence, whenever Rubocop flags a method as complex, we should endeavour to refactor it to make it simpler
- If refactoring to meet the `AbcSize` metric actually worsen code readability or make it unworkable, we can choose to disable this measure (use it as a guide) since refactoring becomes counterproductive.

[Back to top](#section-links)


## Counting Assignments
- Expressions of the form `variable = some_expression` or its variations (e.g. `variable += some_expression`) is an assignment. 
- We can reduce assignments in our method by **removing repetitive assignments or unnecessary intermediaries** although we should be mindful this could reduce readability

[Back to top](#section-links)


## Counting Conditions
A condition is a point in the code where execution can take 2 or more paths
```ruby
if a == 1
  do_this
elsif b == 2 || c.empty?
  do_that
end
puts final_result
```
Above code has 3 conditions: `a==1`, `b==2` and `c.empty?`. A simple way to refactor complicated conditional expressions involves **giving a condition a name and putting it in a method**. For instance, rewrite:
```ruby
if move1 == "rock" && (move2 == "scissors" || move2 == "lizard")...
```
as
```ruby
def rock_wins?(move1, move2)
  move1 == "rock" && (move2 == "scissors" || move2 == "lizard")
end

if rock_wins?(move1, move2)...
```
This replaces 3 conditions with 1 condition and 1 branch

[Back to top](#section-links)


## Counting branches
- A lot of Ruby operators are actually method calls (remember [fake operators](fake_operators.md)). Example include
	- `+`, `-`, `*`, `/`, `%`
	- `==`, `>`, `<`
	- `[]`, `[]=` (Note: this is not assignment)
	- `puts`, `loop`, `Array#each`
	- `getter` and `setter` 

```ruby
class Example
  attr_reader :something
  def initialize
    @something = [...]
  end
  def print_something
    if something.size == 0
      puts "something has just 1 item: #{something[0]}"
    elsif something.size == 1
      puts "something has 2 items: #{something[0]} #{something[1]}"
    else
      puts "something has #{something.size} items: #{something.join(' ')}"
    end
  end
end
```
In above example, `print_something` has 19 branches:
- 7 `something` getter calls
- 3 `[]` calls on `something`
- 2 `==` calls
- 3 `puts` calls
- 3 `size` calls
- 1 `join` calls
Together with 2 conditions, its has an `abc_size` of `19.1`

A simple fix would be reduce the number of `something` getter calls using a variable as an intermediary. This would **replace 7 branches with 1 assignment**. Using `@something` directly, if appropriate, would remove 7 branches.
```ruby
value = something
if value.size == 0
  ...
```

[Back to top](#section-links)



