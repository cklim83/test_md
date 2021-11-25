#### Source
Link to [video](https://launchschool.com/lessons/de05b300/assignments/f9bd863d)
#### Outline
- What is debugging?
- Debugging steps
- Debugging tools / pry
- What is pry?
- Using pry
- Invoking pry at runtime
- Accessing variables and scope
- Debugging with pry
- Stepping through code with pry-byebug


#### What is debugging?
- Bugs are human errors in code. Debugging is the process of identifying and fixing those errors

#### How do we debug code?
1. Identify the problem: where in the program
2. Understand the problem: why the lines of code are causing problem. Context, data input and output
3. Implement a solution

#### Types of error
- Syntax errors
	- Written code does not conform to grammar of programming language used
	- Generally stops code from functioning
- Logical errors
	- Errors in logic of code
	- Code runs but produces unexpected results

#### What is pry?
- pry is a Rubygem. To use it, we need to install it using `gem install pry`
- pry is a REPL (Read-Evaluate-Print Loop) i.e. an interactive environment that
	- Takes user input
	- Evaluates the input
	- Returns the result to the user
	- Loops back to the start

#### Using Pry
- To use pry in terminal, we can just type `pry`
	- `cd` can be used to change scope in pry e.g. from main scope to that of an object
	```ruby
	[1] pry(main)> arr = [1, 2, 3]
	=> [1, 2, 3]
	[2] pry(main)> cd arr
	[3] pry(#<Array>):1>
	```
	- `cd ..` to go up 1 level in scope
	- `cd -`  to reverse to previous scope
	- `cd /` to go the top level scope i.e. main
	- `ls` will list all the methods available to the new scope
	- we can invoke the method on the current scope by type the method name
	- `show-doc method_nam` will print the documentation in pry repl

#### Invoking pry at runtime
-  To use pry in ruby files, we need to include `require 'pry'` before using
- using `binding.pry`
	- A `binding` is something that contains references to any variables that were in scope at the point where it was created
	- `pry` interrupts program execution and **pries open** the binding so that we can have a look around
	- `whereami` can give context of execution after we clear screen. `whereami n` will list n lines above and below binding.pry statement for more context. Default value of n is 5

#### Stepping through and into code
- `pry-byebug` library extends `pry` with some additional commands we can use in repl environment
	- `next`: execute next expression below binding.pry
	- `step`: step into a method call
	- `continue`: execute next iteration
- `require "pry"` and `require "pry-byebug"` in ruby file
- Similar gems exist such as `pry-nav` and `pry-debugger`
- Concept of stepping through and into code is not limited to `pry` or Ruby
- There are equivalents in other languages such as `Chrome Dev Tools Debugger` for javascript

#### Take-aways
- Debugging is an important skill to learn and practice
- Tools such as pry and pry-byebug make debugging easier
- Using these tools also helps to learn more about code
- These debugging concepts are not limited to Ruby