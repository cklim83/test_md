# Exceptions

## Section Links
[What is an Exception?](#what-is-an-exception)\
[The Exception Class Hierarchy](#the-exception-class-hierarchy)\
[How to Handle an Exceptional State](#how-to-handle-an-exceptional-state)\
[Exception Objects and Built-in Methods](#exception-objects-and-built-in-methods)\
[Raising Exceptions Manually](#raising-exceptions-manually)\
[Raising Custom Exceptions](#raising-custom-exceptions)

---

## What is an Exception?
An exception is way for Ruby to inform the developer the code is **behaving unexpectedly**. If an exception is raised and not handled properly, the program will crash. Ruby will then generate a message informing about the type of error encountered.

[Back to top](#section-links)


## The Exception Class Hierarchy
```text
Exception  
	NoMemoryError  
  	ScriptError  
    	LoadError  
    	NotImplementedError  
    	SyntaxError  
  	SecurityError  
  	SignalException  
    	Interrupt  
  	StandardError  
    	ArgumentError  
      		UncaughtThrowError  
    	EncodingError  
    	FiberError  
    	IOError  
      		EOFError  
    	IndexError  
      		KeyError  
      		StopIteration  
    	LocalJumpError  
    	NameError  
      		NoMethodError  
    	RangeError  
      		FloatDomainError  
    	RegexpError  
    	RuntimeError  
    	SystemCallError  
      		Errno::*  
    	ThreadError  
    	TypeError  
    	ZeroDivisionError  
  	SystemExit  
  	SystemStackError  
  	fatal
```
The list above shows the hierarchy of exception classes. All the top is the overall Exception class and it gets more specific as the indentation increases. Some examples of the exception classes are:
- Using `ctr-c` to terminate a program raises an exception via the `Interrupt` class
- `SyntaxError` arose when the code contains syntatic error according to Ruby interpreter.
- `SystemStackError` arose when a stack overflow e.g. due to infinite loop or infinite recursion.
- `StandardError` consists of many commonly encountered errors such as `ArgumentError` (e.g. non-matching number of arguments in method call), `TypeError`, `ZeroDivisionError` and `NoMethodError`.

[Back to top](#section-links)


## When Should An Exception Be Handled?
We should mostly **focus on StandardError and its descendents** as they often arises due to unexpected user inputs, faulty type conversions, division by zero and are relative **safe** to handle these and continue to run the program.

We should **not handle all exceptions** as certain failures can be dangerous. When they occur, we should let the program crash rather than handle as **masking it can be dangerous and also make debugging difficult**. Error such as `NoMemoryError`, `SyntaxError` and `LoadError` must be addressed for program to operate appropriately.

To avoid causing unwanted behaviour through exception handling, we should be **intentional and very specific which exception to handle** and what action to take (e.g. logging error, displaying error to user, email administrator).

[Back to top](#section-links)


## How To Handle An Exceptional State
Use a `begin` / `rescue` block
```ruby
begin
  # code that may generate exception
rescue TypeError
  # action to handle TypeError
rescue ArgumentError
  # action to handle argumentError
end
```
If the specified exceptions are not raised, the respective rescue clause are not executed. 

If no exception type is specified, all `StandardError` will be rescued and handled. 

Do **NOT** rescue the `Exception` class as it will cause **ALL** exceptions (including systemic ones) to be rescued, making it unspecific and dangerous as critical errors may be improperly handled and masked.

We can also take the same action for multiple exception types using the following syntax:
```ruby
begin
  # some code
rescue ZeroDivisionError, TypeError
  # common action
end
```

[Back to top](#section-links)


## Exception Objects and Built-in Methods
We can call useful build-in instance methods such as `Exception#message` and `Exception#backtrace` to either return the error message or backtrace associated with the exception. Refer to [exception documentation](https://docs.ruby-lang.org/en/3.0/Exception.html) for more information.
```ruby
begin
  # code at risk of failing
rescue StandardError => e   # store exception object in e
  puts e.message            # output error message
end
```

### `ensure`
An `ensure` clause that always execute, regardless whether an exception was raised or not can be useful in resource management e.g. closing of file, ports. We should be careful the `ensure` block does not raise any exception, else any except raised earlier will be masked and debugging can be difficult.
```ruby
file = open(file_name, 'w')begin  
  # do something with file  
rescue  
  # handle exception  
rescue  
  # handle a different exception  
ensure  
  file.close  
  # executes every time  
end
```

### `retry`
`retry` in `begin`/`rescue` block redirects program back to `begin` statement. This feature may be useful if we want to make repeated attempts e.g. to connect to remote server. To avoid infinite loop, we should set a retry limit. `retry` must be called within the `rescue` block, placed elsewhere it raises a `SyntaxError`.
```ruby
RETRY_LIMIT = 5
begin  
  attempts = attempts || 0  # set attempts to 0 if uninitialized
  # do something  
rescue  
  attempts += 1  
  retry if attempts < RETRY_LIMIT  
end
```

[Back to top](#section-links)


## Raising Exceptions Manually
Ruby allows us to manually raise exceptions by calling `Kernel#raise`. We can select the type of exception to raise and even set the error message. 
```ruby
raise TypeError.new("Something went wrong!")

raise TypeError, "Something went wrong!"
```

If the exception type is not specified, it defaults to `RuntimeError`, a subclass of `StandardError`. A `RuntimeError` with "invalid age" error message is raised below.
```ruby
def validate_age(age)  
  raise("invalid age") unless (0..105).include?(age)  
end
```

We can handle the error as such:
```ruby
begin  
  validate_age(age)  
rescue RuntimeError => e  
  puts e.message              #=> "invalid age"  
end
```

[Back to top](#section-links)


## Raising Custom Exceptions
Ruby allow us to create our own custom exception classes. The custom exception classes will have full access to all built-in exception instance methods such as `Exception#message` and `Exception#backtrace`.


A good practice is for a library to create a single “generic” exception class (typically a subclass of [`StandardError`](https://docs.ruby-lang.org/en/3.0/StandardError.html) or [`RuntimeError`](https://docs.ruby-lang.org/en/3.0/RuntimeError.html)) and have its other exception classes derive from that class. This allows the user to rescue the generic exception, thus catching all exceptions the library may raise even if future versions of the library add new exception subclasses.

For example:
```ruby
class MyLibrary
  class Error < ::StandardError
  end

  class WidgetError < Error
  end

  class FrobError < Error
  end

end
```
To handle both `MyLibrary::WidgetError` and `MyLibrary::FrobError`, we can just rescue `MyLibrary::Error`

[Back to top](#section-links)