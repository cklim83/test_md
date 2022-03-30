# Steps to Modelling Problem In OOP
## Section Links
[Steps](#steps)\
[Other Considerations: OOP Relationships](#other-considerations-oop-relationships)\
[Rock, Paper, Scissor Game Example](#example-1-rock-paper-scissor-game)\
[Tic Tac Toe Game Example](#example-2-tic-tac-toe-game)

---

## Steps
1. Write a **textual description** of the problem 
2. Identify **major nouns** and **verbs** in the problem description
3. Organize and **associate the verbs within nouns**. Each noun represents a class and the associated verbs represent methods of that class
4. Construct a **spike** (throw away code) to explore the workability of the design
5. Think of how we could better organize constants, instance variables and methods amongst the classes to **eliminate unnecessary coupling**
	- If two classes share many common methods and attributes and can be represented in a hierarchical structure (super and child classes), it is best to place the common instance variables and methods in the superclass to avoid repeating code (DRY principle). Differences can then reside in the child classes either as new methods or to override the parent one. 
	- While "bad" design is more noticeable, it is difficult to come up with a "right" design
	- The tradeoff between designs is often between more flexible code and indirection (indirectness or not straightforward)
	- Tightly coupled dependencies are easier to understand, but offer less flexibility. 
	- Loosely coupled dependencies are more difficult to understand, but offer more long term flexibility.
6. **\[Optional\]** We could translate these classes and their inter-relations onto Class Responsibility Collaborator (CRC) cards to visualize inter-depencies

[Back to top](#section-links)

## Other Considerations: OOP Relationships
- Hierarchical **is-a** relationship: Use **class inheritance**
- Non-hierarchical **has-a** relationships: Use **modules** for interface inheritance
- Contains or Ownership: Use **collaborator objects**

[Back to top](#section-links)


## Example 1: Rock, Paper, Scissor Game
### Initial Design
```Text
Rock, Paper, Scissors is a two-player game where each player chooses one of three possible moves: rock, paper, or scissors. The chosen moves will then be compared to see who wins, according to the following rules:

- rock beats scissors
- scissors beats paper
- paper beats rock

If the players chose the same move, then it's a tie.
rock, paper and scissors are types of move


Nouns: player, move, rule
Verbs: choose, compare


Player
 - choose
Move
Rule

- compare
```

### Spike
```Ruby
class Player
  def initialize
    # maybe a "name"? what about a "move"?
  end

  def choose

  end
end

class Move
  def initialize
    # seems like we need something to keep track
    # of the choice... a move object can be "paper", "rock" or "scissors"
  end
end

class Rule
  def initialize
    # not sure what the "state" of a rule object should be
  end
end

# not sure where "compare" goes yet
def compare(move1, move2)

end

# Orchestration Engine
class RPSGame
  def initialize
    @human = Player.new
    @computer = Player.new
  end

  def play
    display_welcome_message
    human_choose_move
    computer_choose_move
    display_winner
    display_goodbye_message
  end
end

RPSGame.new.play
```

### Source Codes**
[Version 1](https://github.com/cklim83/launch_school/blob/main/03_rb120_oop/lesson_02_oop/08_oop_rock_paper_scissor/08a_oop_rps.rb) -> [Version 2a](https://github.com/cklim83/launch_school/blob/main/03_rb120_oop/lesson_02_oop/08_oop_rock_paper_scissor/08b_oop_rps_refactor_1.rb) -> [Version 2b](https://github.com/cklim83/launch_school/blob/main/03_rb120_oop/lesson_02_oop/08_oop_rock_paper_scissor/08c_oop_rps_refactor_2.rb) -> [Version 3a](https://github.com/cklim83/launch_school/blob/main/03_rb120_oop/lesson_02_oop/08_oop_rock_paper_scissor/08d_oop_rps_bonus_features.rb) -> [Version 3b](https://github.com/cklim83/launch_school/blob/main/03_rb120_oop/lesson_02_oop/08_oop_rock_paper_scissor/08g_oop_rps_bonus_refactored.rb)

[Back to top](#section-links)


## Example 2: Tic Tac Toe Game
### Initial Design
```Text
Tic Tac Toe is a 2-player board game played on a 3x3 grid. Players take turns
marking a square. The first player to mark 3 squares in a row wins.

Nouns: board, player, square, grid
Verbs: play, mark

Board
Square
Player
- mark
- play
```

### Spike
```ruby
class Board
  def initialize
    # we need some way to model the 3x3 grid. Maybe "squares"?
    # what data structure should we use?
    # - array/hash of Square objects?
    # - array/hash of strings or integers?
  end
end

class Square
  def initialize
    # maybe a "status" to keep track of this square's mark?
  end
end

class Player
  def initialize
    # maybe a "marker" to keep track of this player's symbol (ie, 'X' or 'O')
  end

  def mark

  end

  def play

  end
end

=begin
Lots of questions still remain about where responsibities 
lie, and how to cleanly organize the behaviors. There's still  even the basic question of whether all the classes above are  needed. For example, do we really need a Square or Player yet? It's not clear, and we really need to explore the problem a little to get a better feel for the code. One class we do need
that we don't have yet is some sort of orchestration engine.%%
=end

class TTTGame
  def play
    display_welcome_message
    loop do
      display_board
      first_player_moves
      break if someone_won? || board_full?

      second_player_moves
      break if someone_won? || board_full?
    end
    display_result
    display_goodbye_message
  end
end

TTTGame.new.play
```

### Source Codes
[Spike](https://github.com/cklim83/launch_school/blob/main/03_rb120_oop/lesson_05/01_tictactoe/02_oo_ttt_spike.rb) -> [Improvements](https://github.com/cklim83/launch_school/blob/main/03_rb120_oop/lesson_05/01_tictactoe/03_oo_ttt_improvements.rb) -> [Rubocop](https://github.com/cklim83/launch_school/blob/main/03_rb120_oop/lesson_05/01_tictactoe/04_oo_ttt_rubocop.rb) -> [Bonus Features](https://github.com/cklim83/launch_school/blob/main/03_rb120_oop/lesson_05/01_tictactoe/05_oo_ttt_bonus_features.rb) -> [Reorganize](https://github.com/cklim83/launch_school/blob/main/03_rb120_oop/lesson_05/01_tictactoe/06_oo_ttt_bonus_refactored.rb)

[Back to top](#section-links)


