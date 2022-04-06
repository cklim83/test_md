# Coding Tips

## Section Links
[Explore Problem before Design](#explore-problem-before-design)\
[Lookout for Repetitive Nouns](#lookout-for-repetitive-nouns)\
[Naming Methods](#naming-methods)\
[Avoid Long Method Invocation Chains](#avoid-long-method-invocation-chains)\
[Avoid Design Patterns For Now](#avoid-design-patterns-for-now)

---


## Explore Problem before Design
Use **spike (exploratory code)** to play around with problem and **gain understanding**: they can help validate hunches and hypotheses. We do not need to worry about code quality at this stage since these are throwaway code to crystalize our inital idea dump. Once we have a better understanding of the problem, we can use it to **organize our code** into coherent classes and methods

[Back to top](#section-links)


## Lookout for Repetitive Nouns
Repetitive nouns in method names is a sign we are missing a class
```Ruby
human.make_move
computer.make_move

puts "Human move was #{format_move(human.move)}."
puts "Computer move was #{format_move(computer.move)}."

if compare_moves(human.move, computer.move) > 1
  puts "Human won!"
elsif compare_moves(human.move, computer.move) < 1
  puts "Computer won!"
else
  puts "It's a tie!"
end
```
In our Rock, Paper, Scissor game example, we kept referencing a "move". Assuming `move` is a string or integer and not yet a custom object. We might create a `Move` class with a string value and having methods to compare between different move values

[Back to top](#section-links)


## Naming Methods
When naming methods, do not include the class name
```Ruby
class Player
  def player_info
    # returns player's name, move and other data
  end
end
```

Since the method name includes the class name, it leads to awkward looking code when we use it.
```Ruby
player1 = Player.new
player2 = Player.new

puts player1.player_info
puts player2.player_info
```
If we had just named the method info, the code would look more natural and intuitive
```Ruby
puts player1.info
puts player2.info
```

[Back to top](#section-links)


## Avoid Long Method Invocation Chains
The longer the chain, the more fragile the code becomes. It could be difficult to debug especially if somethings breaks and returns `nil` somewhere in the chain
```Ruby
human.move.display.size  # 3 chain method invocation
```

If we identify areas where the code can break (e.g. `human.move` could return `nil`), we can **use guard expressions**
```Ruby
move = human.move
puts move.display.size if move
```

[Back to top](#section-links)


## Avoid Design Patterns For Now
- Will likely lead to premature optimisation for beginner programmers
- As we spend time mastering design patterns and best practices, we should focus on understanding **when** to use them

[Back to top](#section-links)


