# Introduction to Testing
[Documentation](https://docs.seattlerb.org/minitest/)

## Section Links
[Summary](#summary)\
[Why Write Tests?](#why-write-tests)\
[Setting up Minitest](#setting-up-minitest)\
[Minitest Summary](#minitest-summary)\
[Important Notes on Testing](#important-notes-on-testing)\
[Assertions](#assertions)\
[SEAT Approach](#seat-approach)\
[Testing Equality](#testing-equality)\
[Code Coverage](#code-coverage)

## Summary
- Minitest is Ruby's default testing library. It comes installed with Ruby.
- Minitest tests come in 2 flavors: assert-style and spec-style. Unless you really like RSpec, use assert-style.
- A test suite contains many tests. A test can contain many assertions.
- Use `assert_equal` liberally, but don't be afraid to look up other assertions when necessary. Remember that `assert_equal` is for asserting value equality as determined by the `==` method.
- Use the SEAT approach to writing tests.
- Use code coverage as a metric to gauge test quality. (But not the only metric.)
- Practice writing tests -- it's easy!

[Back to Top](#section-links)


## Why Write Tests?
- We write test to prevent regression. 

- Regression tests check for bugs that occur in formerly working code after you make changes somewhere in the codebase. Using tests to identify these bugs means we don't have to verify that everything works manually after each change.

[Back to Top](#section-links)


## Setting up Minitest
### Installing Minitest
```ruby
$ gem install minitest     # install latest version

$ gem list minitest        # Should see 5.4.3 or higher

*** LOCAL GEMS ***
minitest(5.4.3)
```

### Verify the Installation
Create a temporary ruby file to test Minitest was installed properly.
```ruby
# in temp.rb
require 'minitest/autorun'

class TempTest < Minitest::Test
  def test_my_test
    assert true
  end
end
```

Run the file
```plaintext
$ ruby temp.rb
Run options: --seed 30741

# Running:

MyFirstTest .

Finished in 0.001034s, 967.2303 runs/s, 967.2303 assertions/s.

1 runs, 1 assertions, 0 failures, 0 errors, 0 skips
```


## Using Minitest
### Steps Overview
- Create a test file
- Create a test class by subclassing `MiniTest::Test`.
-  Create a test by creating an instance method that starts with `test_`.
-  Create assertions with `assert_equal`, and pass it the expected value and the actual value.
-  Colorize Minitest output with `minitest-reporters`
-  You can skip tests with `skip`.
-  Minitest comes in two syntax flavors: assertion style and expectation style. The latter is to appease RSpec users, but the former is far more intuitive for beginning Ruby developers.

### Minitest vs RSpec
- Minitest is the default testing library and comes installed with Ruby while RSpec doesn't

- Minitest lets us use normal Ruby syntax but RSpec is a Domain Specific Language (DSL) and uses code that reads like natural English

### Terminology
- **Test Suite**: The **entire set of tests** that accompanies the program or application.
- **Test**: A situation or context in which verification checks are made. For example, making sure we get an error messafe after trying to log in with the wrong password. A test may contain 1 or more assertions.
- **Assertion**: Actual verification step to confirm that results returned by a program or application match the expected results. 

### Components of a Test file?
```ruby
# car.rb
class Car
  attr_accessor :wheels

  def initialize
    @wheels = 4
  end
end
```

```ruby
# car_test.rb
require 'minitest/autorun'
require_relative 'car'

class CarTest < MiniTest::Test
  def test_wheels
    car = Car.new
    assert_equal(4, car.wheels)
  end
end
```
- `require minitest/autorun` is needed to load all necessary files from the `minitest` gem.
- `require_relative 'car'` is needed to load the `car.rb` file so that object of the class can be instantiated in the test suite. `require_relative` indicates that `car.rb` is in the same directory as this test file `car_test.rb`
- Sometimes, the class to be tested have custom collaborator objects. In that case, we also need to include the file of the collaborator object class using `require_relative`.
- We create our test class. By convention, it is usually named **ClassnameTest** in CamelCase, where Classname denoted the class being tested.
- This class should subclass `Minitest::Test` to inherit all the methods necessary for writing tests e.g. `assert, assert_equal.
- Tests are instance methods within the test class. They **must start with `test_`** so that Minitest will know these methods are individual tests that needs to be run.
- Before making assertions, we need to setup the appriopriate data/context to make assertions against. Here, we instantiate a `Car` object to test that newly created `Car` objects indeed have 4 wheels (based on the test method name)
- We use `assert_equal`, an inherited instance method for the assertion. It takes two parameters, the first is the expected value, the second is the test or actual value. If the two values are equal, the assertion pass. Otherwise `assert_equal` will save the error and `Minitest` will report the error as a failure at the end of test run. Assertion methods generally also take an optional string parameter to display failure message.

### Reading Test Output
```terminal
$ ruby car_test.rb

Run options: --seed 62238

# Running:

CarTest .

Finished in 0.001097s, 911.3428 runs/s, 911.3428 assertions/s.

1 runs, 1 assertions, 0 failures, 0 errors, 0 skips
```
- The "seed" tells Minitest what order the tests were run in. Most test suite have many tests run in random order. We can use the seed to tell Minitest to run the test in a particular order but this is rarely used.
- Every dot `.` meant a test was run and nothing went wrong. If we skip a test with `skip` keyword, it will print "S". If we have a failure, it print "F"

### Failures
```ruby
require 'minitest/autorun'
require_relative 'car'

class CarTest < MiniTest::Test
  def test_wheels
    car = Car.new
    assert_equal(4, car.wheels)
  end

  def test_bad_wheels
    car = Car.new
    assert_equal(3, car.wheels)  # Deliberate to fail
  end
end
```

```terminal
$ ruby car_test.rb

Run options: --seed 8732

# Running:

CarTest F.

Finished in 0.001222s, 1636.7965 runs/s, 1636.7965 assertions/s.

  1) Failure:
CarTest#test_bad_wheels [car_test.rb:13]:
Expected: 3
  Actual: 4

2 runs, 2 assertions, 1 failures, 0 errors, 0 skips
```
- `F.` indicates one test failed while another passed.
- The error message give more information on the fail test.

### Adding Color With minitest-reporter
We need to install the `minitest-reporters` gem
```terminal
$ gem install minitest-reporters
```

Then add these lines to the top of the file
```ruby
require "minitest/reporters"
Minitest::Reporters.use!
```

Revised test file
```ruby
require 'minitest/autorun'
require "minitest/reporters"
Minitest::Reporters.use!

require_relative 'car'

class CarTest < MiniTest::Test
  def test_wheels
    car = Car.new
    assert_equal(4, car.wheels)
  end

  def test_bad_wheels
    car = Car.new
    assert_equal(3, car.wheels)
  end
end
```
Failed test colorized output:
![failed test image](https://d1b1wr57ag5rdp.cloudfront.net/images/failed_test_output.png)

Sample successful run:
![success test image](https://d1b1wr57ag5rdp.cloudfront.net/images/success_test_run.png)

### Skipping Tests
- We can skip tests (e.g. those not ready to be test) using the `skip` keyword at the **top** of the test instance method
```ruby
require 'minitest/autorun'
require "minitest/reporters"
Minitest::Reporters.use!

require_relative 'car'

class CarTest < MiniTest::Test
  def test_wheels
    car = Car.new
    assert_equal(4, car.wheels)
  end

  def test_bad_wheels
    skip
    car = Car.new
    assert_equal(3, car.wheels)
  end
end
```

Skipped test output:
![skipped test image](https://d1b1wr57ag5rdp.cloudfront.net/images/skip_test_run.png)


### Expectation Syntax
- Besides _assert-style_ syntax shown above, Minitest also has an _expectation_ or _spec-style_ syntax. 
- Under expectation style, 
	- tests are grouped into describe blocks
	- individual tests are written with the `it` method
	- use _expectation matchers_ instead of assertions
	- This Domain Specific Language (DSL) mimics RSpec's syntax but doesn't look like normal Ruby code.
```ruby
require 'minitest/autorun'
require_relative 'car'

describe 'Car#wheels' do
  it 'has 4 wheels' do
    car = Car.new
    _(car.wheels).must_equal 4           # this is the expectation
  end
end
```

Output:
```terminal
Run options: --seed 51670

# Running:

Car#wheels .

Finished in 0.001067s, 937.0051 runs/s, 937.0051 assertions/s.

1 runs, 1 assertions, 0 failures, 0 errors, 0 skips
```

[Back to Top](#section-links)


## Important Notes on Testing
- When writing tests, try not to rely on dependencies (e.g. other methods beyond the one being tested) so that we can isolate any failure to the method being tested. See [example](../../../04_rb130_more_topics/03_exercises/05_medium_2_testing/03_test_accept_money.rb) for details.

- To automate testing involving inputs, we can use `StringIO` to avoid the need for manual entry via keyboard. e.g. `StringIO.new('10\n').gets.chomp` will return `'10'`, simulating entering `10` and pressing enter on keyboard. Each `StringIO` object can only be `gets` once, subsequent `gets` will return `nil`. See [input retrieval](../../../04_rb130_more_topics/03_exercises/05_medium_2_testing/06_test_prompt_payment.rb) and [output suppression](../../../04_rb130_more_topics/03_exercises/05_medium_2_testing/07_test_prompt_payment2.rb) examples.

- Within a test, once an assertion failure occurs, subsequent assertions below the failed one are not executed. Subsequent tests (i.e. runs) are still executed however.

[Back to Top](#section-links)


## Assertions
### Common assertions
|Assertion|Description|
|---|---|
|assert(test)|Fails unless `test` is truthy|
|assert_equal(exp, act)|Fails unless `exp == act`|
|assert_nil(obj)|Fails unless obj is `nil`|
|assert_raises(`*exp`) {...}|Fails unless block raises one of `*exp`|
|assert_instance_of(cls, obj)|Fails unless `obj` is an instance of `cls`|
|assert_includes(collection, obj)|Fails unless collection includes `obj`|

Full list can be found in the [Minitest documentation](https://docs.seattlerb.org/minitest/Minitest/Assertions.html)

**Examples**
```ruby
class Car
  attr_accessor :wheels, :name

  def initialize
    @wheels = 4
  end
end
```

1. `assert`
```ruby
def test_car_exists
  car = Car.new
  assert(car)
end
```

2. assert_equal
```ruby
def test_wheels
  car = Car.new
  assert_equal(4, car.wheels)
end
```

3. assert_nil
```ruby
def test_name_is_nil
  car = Car.new
  assert_nil(car.name)
end
```

4. assert_raises
```ruby
def test_raise_initialize_with_arg
  assert_raises(ArgumentError) do
    car = Car.new(name: "Joey")         # this code raises ArgumentError, so this assertion passes
  end
end
```

5. assert_instance_of
```ruby
def test_instance_of_car
  car = Car.new
  assert_instance_of(Car, car)
end
```
This test is more useful when dealing with inheritance. This example is a little contrived.

6. assert_includes
```ruby
def test_includes_car
  car = Car.new
  arr = [1, 2, 3]
  arr << car

  assert_includes(arr, car)
end

# assert_includes calls assert_equal in its implementation, and Minitest counts that call
# as a separate assertion. For each assert_includes call, you will get 2 assertions, not 1.
```

### Refutations
- Refutations are the opposite of assertions. 
- Every assertion has a corresponding refutation (e.g. `refute`, `refute_equal`)
- `refute` passes if the object you pass to it is falsey.

[Back to Top](#section-links)


## SEAT Approach
SEAT is an acroynm for 
1. **Setup**: Instantiate any objects that will be used in the tests
2. **Execute**: Run code against the object being tested
3. **Assert**: Affirm the results of code execution
4. **Tear down**: Clean up any lingering artifacts

```ruby
require 'minitest/autorun'

require_relative 'car'

class CarTest < MiniTest::Test

  def test_car_exists
    car = Car.new
    assert(car)
  end

  def test_wheels
    car = Car.new
    assert_equal(4, car.wheels)
  end

  def test_name_is_nil
    car = Car.new
    assert_nil(car.name)
  end

  def test_raise_initialize_with_arg
    assert_raises(ArgumentError) do
      car = Car.new(name: "Joey")
    end
  end

  def test_instance_of_car
    car = Car.new
    assert_instance_of(Car, car)
  end

  def test_includes_car
    car = Car.new
    arr = [1, 2, 3]
    arr << car

    assert_includes(arr, car)
  end
end
```
In the test file above, we repeatedly create test objects for use in each test instance method. 

```ruby
require 'minitest/autorun'

require_relative 'car'

class CarTest < MiniTest::Test
  def setup
    @car = Car.new
  end

  def test_car_exists
    assert(@car)
  end

  def test_wheels
    assert_equal(4, @car.wheels)
  end

  def test_name_is_nil
    assert_nil(@car.name)
  end

  def test_raise_initialize_with_arg
    assert_raises(ArgumentError) do
      Car.new(name: "Joey")
    end
  end

  def test_instance_of_car
    assert_instance_of(Car, @car)
  end

  def test_includes_car
    arr = [1, 2, 3]
    arr << @car

    assert_includes(arr, @car)
  end
end
```
- Including Set up and Tear Down steps reduces redundancy in the Test Suite code.
- `setup` can hold common objects that are needed in most tests. `teardown` are often used to clean up resources e.g. close files, connections to database or log information. They are optional and not needed in simple cases.
- The `setup` method runs before each test and the `teardown` method after each test. 
- We have to use instance variables rather than local variables for setup objects to make them available all test methods

[Back to Top](#section-links)


## Testing Equality
`assert_equal` compares objects using the `#==` method. For object equality, we have to use `assert_same`.
```ruby
require 'minitest/autorun'

class EqualityTest < Minitest::Test
  def test_value_equality
    str1 = "hi there"
    str2 = "hi there"

    assert_equal(str1, str2)
    assert_same(str1, str2)
  end
end
```

Output:
```none
1) Failure:
EqualityTest#test_value_equality [temp.rb:9]:
Expected "hi there" (oid=70321861410720) to be the same as "hi there" (oid=70321861410740).
```

### Equality with Custom Class
For standard ruby classes such as strings, arrays and hashes, `==` is already implemented so that `assert_equal` will work as expected i.e. comparing values.

For custom classes, for `assert_equal` to work as expected, we will need to implement `==` for the custom class. Otherwise, assert_equal will utilise `BasicObject#==` made available through inheritance, which will fail the test if both objects are not the same. Nonetheless, the test will indicate a failed run and remind us to implement `==` for our custom class.
```ruby
class Car
  attr_accessor :wheels, :name

  def initialize
    @wheels = 4
  end
end

class CarTest < MiniTest::Test
  def test_value_equality
    car1 = Car.new
    car2 = Car.new

    car1.name = "Kim"
    car2.name = "Kim"

    assert_equal(car1, car2)
  end
end
```

Output:
```plaintext
# Running:

CarTest F

Finished in 0.021080s, 47.4375 runs/s, 47.4375 assertions/s.

  1) Failure:
CarTest#test_value_equality [car_test.rb:48]:
No visible difference in the Car#inspect output.
You should look at the implementation of #== on Car or its members.
#<Car:0xXXXXXX @wheels=4, @name="Kim">

1 runs, 1 assertions, 1 failures, 0 errors, 0 skips
```

```ruby
class Car
  attr_accessor :wheels, :name

  def initialize
    @wheels = 4
  end

  def ==(other)                       # assert_equal would fail without this method
    other.is_a?(Car) && name == other.name
  end
end
```

```ruby
class CarTest < MiniTest::Test
  def test_value_equality
    car1 = Car.new
    car2 = Car.new

    car1.name = "Kim"
    car2.name = "Kim"

    assert_equal(car1, car2)          # this will pass
    assert_same(car1, car2)           # this will fail
  end
end
```

[Back to Top](#section-links)


## Code Coverage
Code coverage is a measure of **how much** of a program is tested by a test suite. It is based on all code, covering **both public and private methods** and can be used as a **metric** to assess **code quality**.


Note: even if coverage is 100%, it just means we have some tests for every method and doesn't mean every edge case is considered or our program is working correctly.

### Coverage using simplecov gem 
```terminal
$ gem install simplecov
```

Then add, the following to the top of the test file, before `require 'minitest/autorun'`
```ruby
require 'simplecov'
SimpleCov.start
```

After running the test file, a coverage directory will be generated. Open the `index.html` file and the coverage metric will be shown.

Sample Output:
![simplecov output image](https://d1b1wr57ag5rdp.cloudfront.net/images/simplecov_output_92.jpg)

[Example Code](../../../04_rb130_more_topics/02_testing)

[Back to Top](#section-links)


## Exercises
[Practices](../../../04_rb130_more_topics/02_testing)\
[Assertions](../../../04_rb130_more_topics/03_exercises/03_easy_testing)\
[Medium Testing](../../../04_rb130_more_topics/03_exercises/05_medium_2_testing)
[Challenges](../../../04_rb130_more_topics/04_ruby_challenges/01_easy_challenges)