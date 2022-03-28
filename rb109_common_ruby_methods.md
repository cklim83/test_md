Topic: Common Ruby Methods\
Date: 18 Jan 2022\
Course: RB109

---

### Sections
[Kernel Methods](#Kernel-methods)\
[Integer Methods](#integer-methods)\
[String Methods](#string-methods)\
[Array Methods](#array-methods)\

---

### Kernel Methods
#### Kernel - Formatting
`#format`
```Ruby
# Use "%0.nf" to round decimals, 
# Use "%0nd" for leading 0s

format("%04d, and %0.3f", 2, 34.53145)    # => "0002, and 34.531"
format("%04d, and %0.3f" % [2, 34.53145]) # => "0002, and 34.531"

#For String#%, each % in string corresponds to 1 argument in array
puts "The scores are %04d, %0.2f and %0.2f" % [78, 68.9, 70.156]
# Ouput:
The scores are 0078, 68.90 and 70.16
```

### Integer Methods
#### Integer - Iteration
[`#downto`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-downto)\
**downto(limit) {|i| ... } → self\
downto(limit) → enumerator**

Calls the given block with each integer value from `self` down to `limit`; returns `self`:
```Ruby
a = []
10.downto(5) {|i| a << i }              # => 10
a                                       # => [10, 9, 8, 7, 6, 5]
a = []
0.downto(-5) {|i| a << i }              # => 0
a                                       # => [0, -1, -2, -3, -4, -5]
4.downto(4) {|i| puts 'executed' }      # output "executed", returns 4
4.downto(5) {|i| puts 'executed' }      # block skipped, returns 4
```

[`#upto`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-upto)\
**upto(limit) {|i| ... } → self\
upto(limit) → enumerator**

Calls the given block with each integer value from `self` up to `limit`; returns `self`:
```Ruby	
a = []
5.upto(10) {|i| a << i }              # => 5
a                                     # => [5, 6, 7, 8, 9, 10]
a = []
-5.upto(0) {|i| a << i }              # => -5
a                                     # => [-5, -4, -3, -2, -1, 0]
5.upto(5) {|i| puts 'executed' } # output "executed", returns 5
5.upto(4) {|i| puts 'executed' } # block skipped, returns 5
```

[`times`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-times)\
**times {|i| ... } → self\
times → enumerator**

Calls the given block `self` times with each integer in `(0..self-1)`; returns `self`:
```Ruby
a = []
5.times {|i| a.push(i) } # => 5
a                        # => [0, 1, 2, 3, 4]
```
<br>

#### Integer - Rounding
[`#ceil`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-ceil)\
**ceil(ndigits = 0) → integer**

Returns the smallest number greater than or equal to `self` with a precision of `ndigits` decimal digits.

When the precision is negative, the returned value is an integer with at least `ndigits.abs` trailing zeros:
```Ruby
555.ceil(-1)  # => 560
555.ceil(-2)  # => 600
-555.ceil(-2) # => -500
555.ceil(-3)  # => 1000

# Returns `self` when `ndigits` is zero or positive.

555.ceil     # => 555
555.ceil(50) # => 555
```


[`#floor`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-floor)\
**floor(ndigits = 0) → integer**

Returns the largest number less than or equal to `self` with a precision of `ndigits` decimal digits.

When `ndigits` is negative, the returned value has at least `ndigits.abs` trailing zeros:
```Ruby
555.floor(-1)  # => 550
555.floor(-2)  # => 500
-555.floor(-2) # => -600
555.floor(-3)  # => 0

Returns `self` when `ndigits` is zero or positive.

555.floor     # => 555
555.floor(50) # => 555
```


[`#round`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-round)\
**round(ndigits= 0, [half: :up]) → integer**

Returns `self` rounded to the nearest value with a precision of `ndigits` decimal digits.

When `ndigits` is negative, the returned value has at least `ndigits.abs` trailing zeros:
```Ruby
555.round(-1)      # => 560
555.round(-2)      # => 600
555.round(-3)      # => 1000
-555.round(-2)     # => -600
555.round(-4)      # => 0

# Returns `self` when `ndigits` is zero or positive.

555.round     # => 555
555.round(1)  # => 555
555.round(50) # => 555
```

If keyword argument `half` is given, and `self` is equidistant from the two candidate values, the rounding is according to the given `half` value:
- `:up` or `nil`: round away from zero:
	```Ruby    
	25.round(-1, half: :up)      # => 30
	(-25).round(-1, half: :up)   # => -30
	```
- `:down`: round toward zero:
	```Ruby  
	25.round(-1, half: :down)    # => 20
	(-25).round(-1, half: :down) # => -20
	```    
- `:even`: round toward the candidate whose last nonzero digit is even:
	```Ruby
	25.round(-1, half: :even)    # => 20
	15.round(-1, half: :even)    # => 20
	(-25).round(-1, half: :even) # => -20
	```

#### Integer - Division & Modulo
[`#%`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-25)\
**self % other → real_number**

Returns `self` modulo `other` as a real number. Result follows the **sign of `other`** (Alias for `Integer#modulo`)
```Ruby
10 % 2              # => 0
10 % 3              # => 1
-10 % 3				# => 2
10 % 4              # => 2

10 % -2             # => 0
10 % -3             # => -2
10 % -4             # => -2

10 % 3.0            # => 1.0
10 % Rational(3, 1) # => (1/1)
```


[`#modulo`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-modulo)\
**modulo(p1)**

Returns `self` modulo `other` as a real number. (Alias for `Integer#%`)


[`#div`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-div)\
**div(numeric) → integer**

Performs integer division; returns the integer result of dividing `self` by `numeric`:
```Ruby
  4.div(3)      # => 1
  4.div(-3)      # => -2
  -4.div(3)      # => -2
  -4.div(-3)      # => 1
  4.div(3.0)      # => 1
  4.div(Rational(3, 1))      # => 1

Raises an exception if +numeric+ does not have method +div+.
```

[`#divmod`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-divmod)\
**divmod(other) → array**

Returns a 2-element array `[q, r]`, where
```Ruby
q = (self/other).floor    # Quotient
r = self % other          # Remainder

# Examples:
11.divmod(4)              # => [2, 3]
11.divmod(-4)             # => [-3, -1]
-11.divmod(4)             # => [-3, 1]
-11.divmod(-4)            # => [2, -3]

12.divmod(4)              # => [3, 0]
12.divmod(-4)             # => [-3, 0]
-12.divmod(4)             # => [-3, 0]
-12.divmod(-4)            # => [3, 0]

13.divmod(4.0)            # => [3, 1.0]
13.divmod(Rational(4, 1)) # => [3, (1/1)]
```


[`#fdiv`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-fdiv)\
**fdiv(numeric) → float**

Returns the `Float` result of dividing `self` by `numeric`:
```Ruby
4.fdiv(2)      				# => 2.0
4.fdiv(-2)      			# => -2.0
-4.fdiv(2)      			# => -2.0
4.fdiv(2.0)      			# => 2.0
4.fdiv(Rational(3, 4))      # => 5.333333333333333
```


[`#remainder`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-remainder)\
**remainder(other) → real_number**

Returns the remainder after dividing `self` by `other`. Result follows the **sign of the caller**.
```Ruby
11.remainder(4)              # => 3
11.remainder(-4)             # => 3
-11.remainder(4)             # => -3
-11.remainder(-4)            # => -3

12.remainder(4)              # => 0
12.remainder(-4)             # => 0
-12.remainder(4)             # => 0
-12.remainder(-4)            # => 0

13.remainder(4.0)            # => 1.0
13.remainder(Rational(4, 1)) # => (1/1)
```

**Comparison Summary**

| a | b | a.divmod(b) | a/b | a.modulo(b) | a.remainder(b) |
|---|---|---|---|---|---|
| 13 | 4 | `[3, 1]` | 3 | 1 | 1 |
| 13 | -4 | `[-4, -3]` | -4 | -3 | 1 |
| -13 | 4 | `[-4, 3]` | -4 | 3 | -1 |
| -13 | -4 | `[3, -1]` | 3 | -1 | -1 |
| 11.5 | 4 | `[2, 3.5]` | 2.875 | 3.5 | 3.5 |
| 11.5 | -4 | `[-3, -0.5]` | -2.875 | -0.5 | 3.5 |
| -11.5 | 4 | `[-3, 0.5]` | -2.875 | 0.5 | -3.5 |
| -11.5 | -4 | `[2, -3.5]` | 2.875 | -3.5 | -3.5 |

- The quotient returned by `divmod` is equal to the **floor of a/b** and always an **integer**
- The modulus portion of `divmod` is equal to `a.modulo(b)`. The sign of this result follows the **sign of the argument** while the result of a `remainder` call follows the **sign of the caller**.
<br>

#### Integer - Type Checks
[`#even?`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-even-3F)\
**even? → true or false**

Returns `true` if `int` is an even number.
```Ruby
3.even?		# => false
4.even?		# => true
(4.0).even?	# undefined method 'even?' for 4.0:float
```


[`#integer?`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-integer-3F)\
**integer? → true**

Since `int` is already an `Integer`, this always returns `true`. 
```Ruby
5.integer?	    # => true
(5.3).integer?  # => false
'5'.integer?    # No method error, no integer? for string
```


[`#odd?`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-odd-3F)\
**odd? → true or false**

Returns `true` if `int` is an odd number.
```Ruby
3.odd?		# => true
4.odd?		# => false
(4.0).odd?	# undefined method 'odd?' for 4.0:float
```


[`#zero?`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-zero-3F)\
**zero? → true or false**

Returns `true` if `int` has a zero value.
```Ruby
0.zero?      # => true
(0.0).zero?  # => true
20.zero?     # => false
```
<br>

#### Integer - Arithmetic
[`#+`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-2B)\
**self + numeric → numeric_result**

Performs addition:
```Ruby
2 + 2              # => 4
-2 + 2             # => 0
-2 + -2            # => -4
2 + 2.0            # => 4.0
2 + Rational(2, 1) # => (4/1)
2 + Complex(2, 0)  # => (4+0i)
```


[`#-`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-2D)\
**self - numeric → numeric_result**

Performs subtraction:
```Ruby
4 - 2              # => 2
-4 - 2             # => -6
-4 - -2            # => -2
4 - 2.0            # => 2.0
4 - Rational(2, 1) # => (2/1)
4 - Complex(2, 0)  # => (2+0i)
```


[`#*`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-2A)\
**self * numeric → numeric_result**
Performs multiplication
```Ruby
4 * 2              # => 8
4 * -2             # => -8
-4 * 2             # => -8
4 * 2.0            # => 8.0
4 * Rational(1, 3) # => (4/3)
4 * Complex(2, 0)  # => (8+0i)
```


[`#/`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-2F)\
**self / numeric → numeric_result**

Performs division; for integer `numeric`, truncates the result to an integer:
```Ruby
 4 / 3              # => 1
 4 / -3             # => -2
 -4 / 3             # => -2
 -4 / -3            # => 1

# For other +numeric+, returns non-integer result:

 4 / 3.0            # => 1.3333333333333333
 4 / Rational(3, 1) # => (4/3)
 4 / Complex(3, 0)  # => ((4/3)+0i)
 ```


[`#**`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-2A-2A)\
**self ** numeric → numeric_result**

Raises `self` to the power of `numeric`:
```Ruby
2 ** 3              # => 8
2 ** -3             # => (1/8)
-2 ** 3             # => -8
-2 ** -3            # => (-1/8)
2 ** 3.3            # => 9.849155306759329
2 ** Rational(3, 1) # => (8/1)
2 ** Complex(3, 0)  # => (8+0i)
```


[`#pow`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-pow)\
**pow(numeric) → numeric\
pow(integer, integer) → integer**

Returns (modular) exponentiation as:
```Ruby
a.pow(b)     #=> same as a**b
a.pow(b, m)  #=> same as (a**b) % m, but avoids huge temporary values
```
<br>

#### Integer - Type Conversion
[`#to_f`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-to_f)\
**to_f → float**

Converts `self` to a Float:
```Ruby
1.to_f          # => 1.0
-1.to_f         # => -1.0

# If the value of `self` does not fit in a Float, the result is # infinity:

(10**400).to_f  # => Infinity
(-10**400).to_f # => -Infinity
```


[`#to_i`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-to_i)\
**to_i → integer**

Since `int` is already an `Integer`, returns `self`.


[`#to_s`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-to_s)\
**to_s(base = 10) → string**

Returns a string containing the place-value representation of `self` in radix `base` (in 2..36). Also aliased as `Integer#inspect`
```Ruby
12345.to_s               # => "12345"
12345.to_s(2)            # => "11000000111001"
12345.to_s(8)            # => "30071"
12345.to_s(10)           # => "12345"
12345.to_s(16)           # => "3039"
12345.to_s(36)           # => "9ix"
78546939656932.to_s(36)  # => "rubyrules"
```
Raises an exception if `base` is out of range.
<br>

#### Integer - Bitwise Operation
[`#&`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-26)\
**self & other → integer**

Convert `self` and `other` to binary, then perform bitwise AND; each bit in the result is 1 if both corresponding bits in `self` and `other` are 1, 0 otherwise:
```Ruby
"%04b" % (0b0101 & 0b0110) # => "0100"

7 & 3 					   # => 3 as 0b0111 & 0b0011 == 0b0011
```
Raises an exception if `other` is not an Integer.


[`#|`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-7C)\
**self | other → integer**

Convert `self` and `other` to binary, then perform **bitwise OR**; each bit in the result is 1 if either corresponding bit in `self` or `other` is 1, 0 otherwise:
```Ruby
"%04b" % (0b0101 | 0b0110) # => "0111"
7 | 3 					   # => 7 as 0b0111 | 0b0011 == 0b0111
```
Raises an exception if `other` is not an Integer.


[`#^`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-5E)\
**self ^ other → integer**

Convert `'self` and `other` to binary, then perform **bitwise EXCLUSIVE OR**; each bit in the result is 1 if the corresponding bits in `self` and `other` are different, 0 otherwise:
```Ruby
"%04b" % (0b0101 ^ 0b0110) # => "0011"
7 ^ 3 					   # => 4 as 0b0111 ^ 0b0011 == 0b0100
```
Raises an exception if `other` is not an Integer.
<br>

#### Integer - Others
[`#abs`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-abs)\
**abs → integer**

Returns the absolute value of `int`. `Integer#magnitude` ia an alias.
```Ruby
(-12345).abs   #=> 12345
-12345.abs     #=> 12345
12345.abs      #=> 12345
```

---

### String Methods
#### String - Concatenation
[`#*`](https://docs.ruby-lang.org/en/master/String.html#method-i-2A)\
**self * integer -> new string**

Returns a new string containing `integer` copies of `self`
```Ruby
"Ho! " * 3 # => "Ho! Ho! Ho! "
"Ho! " * 0 # => ""
```


[`#+`](https://docs.ruby-lang.org/en/master/String.html#method-i-2B)\
**self + other_string -> new string**

Returns new string containing `other_string` concatenated to `self`
```Ruby
"Hello from " + self.to_s # => "Hello from main"
```


[`#<<`](https://docs.ruby-lang.org/en/master/String.html#method-i-3C-3C)\
**self << object -> string**

Concatenate `object` to `self` and return `self`
```Ruby
s = 'foo'
s << 'bar' # => "foobar"
s          # => "foobar"
```

If `object` is an Integer, the value is treated as a codepoint and converted to a character (based on ascii table) before concatenation:
```Ruby
s = 'foo'
s << 33 # => "foo!"
```

[`#concat`](https://docs.ruby-lang.org/en/master/String.html#method-i-concat)\
**concat(\*objects) -> string**

Multiple argument version of `#<<`. Concatenate each object in `objects` to `self` and return `self`
```Ruby
s = 'foo'
s.concat(32, 'bar', 32, 'baz') # => "foo bar baz" 

# Note: 32 corresponds to ' ' in ascii table
```
<br>

#### String - Cleaning
[`#chomp`](https://docs.ruby-lang.org/en/master/String.html#method-i-chomp)\
**chomp(separator=$/) → new_str**

Return new string with given separator removed from end of string if present. `$/` default value will remove trailing `\n`, `\r` or `\r\n`.
```Ruby
"hello".chomp                #=> "hello"
"hello\n".chomp              #=> "hello"
"hello\r\n".chomp            #=> "hello"
"hello\n\r".chomp            #=> "hello\n"
"hello\r".chomp              #=> "hello"
"hello \n there".chomp       #=> "hello \n there"
"hello".chomp("llo")         #=> "he"
"hello\r\n\r\n".chomp('')    #=> "hello"
"hello\r\n\r\r\n".chomp('')  #=> "hello\r\n\r"
```

`#chomp!` version exist to modify string in-place and return `str` or `nil` if no modifications made


[`#chop`](https://docs.ruby-lang.org/en/master/String.html#method-i-chop)\
**chop -> new_str**

Return new string with last character removed. The removed character could be `\n'`, `\r`, `\r\n` or any other character. `#chomp` is safer as it won't truncate the string unintentionally
```Ruby
"string\r\n".chop   #=> "string"
"string\n\r".chop   #=> "string\n"
"string\n".chop     #=> "string"
"string".chop       #=> "strin"
"x".chop.chop       #=> ""
```

`#chop!` version exist to modify string in-place. Returns `str` or `nil` if no modification made


[`#lstrip`](https://docs.ruby-lang.org/en/master/String.html#method-i-lstrip)\
**lstrip -> new_str**

Return new string with leading whitespace (`"\x00\t\n\v\f\r "` i.e. `/\A\s*/`) removed
```Ruby
"  hello  ".lstrip   #=> "hello  "
"hello".lstrip       #=> "hello"
```

`#lstrip!` version exists to modify string in-place and return `str` or `nil` if no modification made


[`#rstrip`](https://docs.ruby-lang.org/en/master/String.html#method-i-rstrip)\
**rstrip -> new_str**

Return new string with trailing whitespaces i.e. `/\s*\z/` removed
```Ruby
"  hello  ".rstrip   #=> "  hello"
"hello".rstrip       #=> "hello"
```

`#rstrip!` version exists to modify string in-place and return `str` or `nil` if no modification made


[`#strip`](https://docs.ruby-lang.org/en/master/String.html#method-i-strip)\
**strip -> new_str**

Return new string with both leading and trailing whitespaces i.e. `/\A\s*/` and `/\s*\z/` removed
```Ruby
"    hello    ".strip   #=> "hello"
"\tgoodbye\r\n".strip   #=> "goodbye"
"\x00\t\n\v\f\r ".strip #=> ""
"hello".strip           #=> "hello"
```

`#strip!` version exists to modify string in-place and return `str` or `nil` if no modification made
<br>

#### String - Change Case
[`#capitalize`](https://docs.ruby-lang.org/en/master/String.html#method-i-capitalize)\
**capitalize(\*options) -> new_str**

Return new string with first character upcased and the rest downcased
```Ruby
s = 'hello World!' # => "hello World!"
s.capitalize       # => "Hello world!"
```

`#capitalize!` version exists to modify string in-place and return `str` or `nil` if no modification made.


[`#downcase`](https://docs.ruby-lang.org/en/master/String.html#method-i-downcase)\
**downcase(\*options) -> new_str**

Return new string with all characters downcased
```Ruby
s = 'Hello World!' # => "Hello World!"
s.downcase         # => "hello world!"
```

`#downcase!` version exists to modify string in-place and return `str` or `nil` if no modification made.


[`#swapcase`](https://docs.ruby-lang.org/en/master/String.html#method-i-swapcase)\
**swapcase(\*options) -> new_str**

Return new string with cases of original characters inverted: upcase becomes downcase and vice versa
```Ruby
s = 'Hello World!' # => "Hello World!"
s.swapcase         # => "hELLO wORLD!"
```

`#swapcase!` version exists to modify string in-place and return `str` or `nil` if no modification made.


[`#upcase`](https://docs.ruby-lang.org/en/master/String.html#method-i-upcase)\
**upcase(\*options) -> new_str**

Return new string with all characters upcased.
```Ruby
s = 'Hello World!' # => "Hello World!"
s.upcase         # => "HELLO WORLD!"
```

`#upcase!` version exists to modify string in-place and return `str` or `nil` if no modification made.
<br>

#### String - Comparison
[`#<=>`](https://docs.ruby-lang.org/en/master/String.html#method-i-3C-3D-3E)\
**string <=> other_string -> -1, 0, 1 or nil**

Compares `string` and `other_string`, returning:
- -1 if `other_string` is larger.
- 0 if the two are equal.
- 1 if `other_string` is smaller.
- `nil` if the two are incomparable. 

In general, `0-9` < `A-Z` < `a-z`. If both strings share the same preceding characters, the longer string is larger.
```Ruby
'foo' <=> 'foo' # => 0
'foo' <=> 'food' # => -1
'food' <=> 'foo' # => 1
'FOO' <=> 'foo' # => -1
'foo' <=> 'FOO' # => 1
'foo' <=> 1 # => nil
```


[`#==`](https://docs.ruby-lang.org/en/master/String.html#method-i-3D-3D)\
**string == object -> boolean**

Returns `true` if `object` has the same value as `string`. Returns `false` otherwise.
```Ruby
s = 'foo'
s == 'foo' # => true
s == 'food' # => false
s == 'FOO' # => false
```


[`#casecmp`](https://docs.ruby-lang.org/en/master/String.html#method-i-casecmp)\
**casecmp(other_string) -> -1, 0, 1, or nil**

Case insensitive comparison i.e. `self.downcase <=> other_string.downcase` and returns:
- -1 if `other_string.downcase` is larger.
- 0 if the two are equal.
- 1 if `other_string.downcase` is smaller.
- `nil` if the two are incomparable.
```Ruby
'foo'.casecmp('foo') # => 0
'foo'.casecmp('food') # => -1
'food'.casecmp('foo') # => 1
'FOO'.casecmp('foo') # => 0
'foo'.casecmp('FOO') # => 0
'foo'.casecmp(1) # => nil
```


[`#casecmp?`](https://docs.ruby-lang.org/en/master/String.html#method-i-casecmp-3F)\
**casecmp?(other_string) -> true, false or nil**

Returns `true` if `self` and `other_string` are equal after Unicode case folding, otherwise `false`:
```Ruby
'foo'.casecmp?('foo') # => true
'foo'.casecmp?('food') # => false
'food'.casecmp?('foo') # => false
'FOO'.casecmp?('foo') # => true
'foo'.casecmp?('FOO') # => true
```

Returns `nil` if the two values are incomparable:
```Ruby
'foo'.casecmp?(1) # => nil
```


[`#count`](https://docs.ruby-lang.org/en/master/String.html#method-i-count)\
**count([other_str]+) → integer**

Accepts **one or more strings (not regex)** and return the number of occurrences of the character set, which are determined by **intersection** of characters from each string argument
- e.g. count("hello", "^l") == count("heo")
- Uses some regex like notation in the string are 
	- ^ for negation
	- "Am-z" for range
	- `\\` can be used to escape special characters `^` and `-`
```Ruby
a = "hello world"
a.count "lo"                   #=> 5 count of l or o
a.count "lo", "o"              #=> 2 count of o
a.count "hello", "^l"          #=> 4 count of h, e or o
a.count "ej-m"                 #=> 4 count of e, j, k, l or m

"hello^world".count "\\^aeiou" #=> 4 count of ^, or vowels
"hello-world".count "a\\-eo"   #=> 4 count of a, -, e or o

c = "hello world\\r\\n"
c.count "\\"                   #=> 2 count of \\
c.count "\\A"                  #=> 0 count of A
c.count "X-\\w"                #=> 3
```
<br>

#### String - Inspect
[`#empty?`](https://docs.ruby-lang.org/en/master/String.html#method-i-empty-3F)\
**empty? -> boolean**

Returns `true` if the length of `self` is zero, `false` otherwise
```Ruby
"hello".empty? # => false
" ".empty? # => false
"".empty? # => true
```


[`#end_with?`](https://docs.ruby-lang.org/en/master/String.html#method-i-end_with-3F)\
**end_with?(\[suffixes\]+) -> boolean**

Returns `true` if `str` ends with one of the suffixes given
```Ruby
"hello".end_with?("ello")               #=> true

# returns true if one of the +suffixes+ matches.
"hello".end_with?("heaven", "ello")     #=> true
"hello".end_with?("heaven", "paradise") #=> false
```


[`#include?`](https://docs.ruby-lang.org/en/master/String.html#method-i-include-3F)\
**include? other_string -> boolean**

Return `true` if other_string is a contiguous substring of `self`. Return false otherwise.
```Ruby
"abc".include?("a") => true
"abc".include?("ab") => true
"abc".include?("ac") => false
"abc".include?("abc") => true
"abc".include?("Abc") => false
"abc".include?("abcd") => false
```

[`#start_with?`](https://docs.ruby-lang.org/en/master/String.html#method-i-start_with-3F)\
**start_with?(\[prefixes\]+) -> boolean**

Returns true if `str` starts with one of the `prefixes` given. Each of the `prefixes` should be a `String` or a `Regexp`
```Ruby
"hello".start_with?("hell")               #=> true
"hello".start_with?(/H/i)                 #=> true

# returns true if one of the prefixes matches.
"hello".start_with?("heaven", "hell")     #=> true
"hello".start_with?("heaven", "paradise") #=> false
```
<br>

#### String - Iteration
[`#chars`](https://docs.ruby-lang.org/en/master/String.html#method-i-chars)\
**chars -> array**

Returns an array whose elements are individual characters of `self`. Equivalent to `str.each_char.to_a`.


[`#each_char`](https://docs.ruby-lang.org/en/master/String.html#method-i-each_char)\
**each_char { |char| ... } -> self\
**each_char -> enumerator**

When called with a block, pass each character in sequence as a **new string** as arguments to the block and return `self` 
```Ruby
"hello".each_char {|c| print c, ' ' }

# outputs
h e l l o
```


[`#length`](https://docs.ruby-lang.org/en/master/String.html#method-i-length)\
**length -> integer**

Returns the count of characters in `self`. Alias for `String#size`
```Ruby
"\x80\u3042".length # => 2
"hello".length # => 5
```

[`#size`](https://docs.ruby-lang.org/en/master/String.html#method-i-size)\
**size -> integer**

Returns the count of characters in `self`. Alias for `String#length`
```Ruby
"\x80\u3042".length # => 2
"hello".length # => 5
```


[`#upto`](https://docs.ruby-lang.org/en/master/String.html#method-i-upto)\
**upto(other_str, exclusive=false) { |string| ... } -> self\
upto(other_str, exclusive=false) -> enumerator**

When called with a block, pass strings of consecutive increments given by `String#succ` from `self` to `other_str` as arguments to the block and return `self`
```Ruby
'a8'.upto('b6') {|s| print s, ' ' } # => "a8"

# outputs
# a8 a9 b0 b1 b2 b3 b4 b5 b6
```
If exclusive is assigned a truthy value, the last value is ommitted, similar to `(a...b)` 

Block is not called if other_string cannot be reached.
<br>

#### String - Formatting
[`#%`](https://docs.ruby-lang.org/en/master/String.html#method-i-25)\
**string % object -> new_string**

Returns the result of formatting `object` into the format specification `self`
```Ruby
"%05d" % 123 # => "00123"
```

If `self` specified >1 substitutions, `object` must be an Array or Hash containing the values to be substituted.
```Ruby
"%-5s: %016x" % [ "ID", self.object_id ] # => "ID   : 00002b054ec93168"
"foo = %{foo}" % {foo: 'bar'} # => "foo = bar"
"foo = %{foo}, baz = %{baz}" % {foo: 'bar', baz: 'bat'} # => "foo = bar, baz = bat"
```


[`#center`](https://docs.ruby-lang.org/en/master/String.html#method-i-center)\
**center(integer, padstr=" ") -> new_string**

If integer > `self.length`, centers `self` and padd its left and right with `padstr` such that `new_string.length == integer`. Else return a new string with same value as `self`. 
```Ruby
"hello".center(4)         #=> "hello"
"hello".center(20)        #=> "       hello        "
"hello".center(20, '123') #=> "1231231hello12312312"
```


[`ljust`](https://docs.ruby-lang.org/en/master/String.html#method-i-ljust)\
**ljust(integer, padstr=" ") -> new_string**

If integer > `self.length`, left justify `self` and padd its right with `padstr` such that `new_string.length == integer`. Else return a new string with same value as `self`.
```Ruby
"hello".ljust(4)            #=> "hello"
"hello".ljust(20)           #=> "hello               "
"hello".ljust(20, '1234')   #=> "hello123412341234123"
```
<br>

#### String - Matching
[`#=~`](https://docs.ruby-lang.org/en/master/String.html#method-i-3D~)\
**str =~ regex -> integer or nil\
str =~ object -> integer or nil**

Returns the index of the first character of the first substring match in string or `nil` if no match found. 
```Ruby
'foo' =~ /f/ # => 0
'foo' =~ /o/ # => 1
'foo' =~ /x/ # => nil
```


[`#index`](https://docs.ruby-lang.org/en/master/String.html#method-i-index)\
**index(substring, offset=0) -> integer or nil\
index(regex, offset=0) -> integer or nil**

Returns the index of the first character of the first substring match in string or `nil` if no match found. 
```Ruby
#string
'foo'.index('f') # => 0
'foo'.index('o') # => 1
'foo'.index('oo') # => 1
'foo'.index('ooo') # => nil

# regex
'foo'.index(/f/) # => 0
'foo'.index(/o/) # => 1
'foo'.index(/oo/) # => 1
'foo'.index(/ooo/) # => nil
```

Integer argument `offset`, if given, specifies the position in the string to begin the search:
```Ruby
'foo'.index('o', 1) # => 1
'foo'.index('o', 2) # => 2
'foo'.index('o', 3) # => nil
```

If `offset` is negative, counts backward from the end of `self`:
```Ruby
'foo'.index('o', -1) # => 2
'foo'.index('o', -2) # => 1
'foo'.index('o', -3) # => 1
'foo'.index('o', -4) # => nil
```


[`#match?`](https://docs.ruby-lang.org/en/master/String.html#method-i-match-3F)\
**match?(pattern, offset=0) -> boolean**

Returns `true` if the pattern (which can be a string or regex) is found in the string. Returns `false` otherwise. 
```Ruby
'foo'.match?(/o/) # => true
'foo'.match?('o') # => true
'foo'.match?(/x/) # => false
```

If Integer argument `offset` is given, the search begins at index `offset`:
```Ruby
'foo'.match?('f', 1) # => false
'foo'.match?('o', 1) # => true
```


[`#scan`](https://docs.ruby-lang.org/en/master/String.html#method-i-scan)\
**scan(pattern) → array\
scan(pattern) {|match, ...| block } → str**

Returns an array containing all the substrings that matches given pattern. Pattern can be a string or regex. If pattern contains groups, the matched substring is an array, nested in the return result array.

```Ruby
a = "cruel world"
a.scan(/\w+/)        #=> ["cruel", "world"]
a.scan(/.../)        #=> ["cru", "el ", "wor"]
a.scan(/(...)/)      #=> [["cru"], ["el "], ["wor"]]
a.scan(/(..)(..)/)   #=> [["cr", "ue"], ["l ", "wo"]]
```

Block form
```Ruby
a = "cruel world"
a.scan(/\w+/) {|w| print "<<#{w}>> " }
print "\n"
a.scan(/(.)(.)/) {|x,y| print y, x }
print "\n"

# outputs
<<cruel>> <<world>>
rceu lowlr
```
<br>

#### String - Substring
[`#[]`](https://docs.ruby-lang.org/en/master/String.html#method-i-5B-5D)\
**string[index] → new_string or nil\
string[start, length] → new_string or nil\
string[range] → new_string or nil\
string[regexp, capture = 0] → new_string or nil\
string[substring] → new_string or nil**

Returns the substring of `self` specified by the arguments and `nil` if not found. Indices can be positive or negative (count from end of `self`) `String#[]` is an alias for `String#slice`. 


[`#slice`](https://docs.ruby-lang.org/en/master/String.html#method-i-slice)\
**slice(*args)**

Returns the substring of `self` specified by the arguments. `String#slice` is an alias for `String#[]`.
<br>

#### String - Transformation
[`#delete`](https://docs.ruby-lang.org/en/master/String.html#method-i-delete)\
**delete([other_str]+) → new_str**

Returns a new string with characters matching the intersections of string arguments deleted. Uses the same rules for building the set of characters as `String#count`
```Ruby
"hello".delete "l","lo"        #=> "heo" as delete "l"
"hello".delete "lo"            #=> "he" as delete "lo"
"hello".delete "aeiou", "^e"   #=> "hell" as delet "aiou"
"hello".delete "ej-m"          #=> "ho" as delete "e" and "j" to "m"
```

Has `#delete!` version exists which deletes the string in-place and returns `self` or `nil` if no changes made.


[`#gsub`](https://docs.ruby-lang.org/en/master/String.html#method-i-gsub)\
**gsub(pattern, replacement) → new_string\
gsub(pattern) {|match| ... } → new_string\
gsub(pattern) → enumerator**

Returns a copy of `self` with **all occurrences** of the given `pattern` replaced.
```Ruby
"zz22zz12zz12zz".gsub("12", "00")         # => "zz22zz00zz00zz"
"zz22zz12zz12zz".gsub(/12/, "00")         # => "zz22zz00zz00zz"
      
"zz22zz12zz12zz".gsub(/12/) { |str| "00" }  # => "zz22zz00zz00zz"
"zz22zz12zz12zz".gsub(/12/) { |str| nil }   # => "zz22zzzzzz"
```

[`#insert`](https://docs.ruby-lang.org/en/master/String.html#method-i-insert)\
**insert(index, other_string) → self**

Inserts the given `other_string` into `self`; returns `self`.

If the Integer `index` is positive, inserts `other_string` at offset `index`:
```Ruby
'foo'.insert(1, 'bar') # => "fbaroo"
```

If the Integer `index` is **negative**, counts backward from the end of `self` and inserts `other_string` at offset `index+1` (that is, **_after_** `self[index]`):
```Ruby
'foo'.insert(-2, 'bar') # => "fobaro"
```


[`#split`](https://docs.ruby-lang.org/en/master/String.html#method-i-split)\
**split(pattern=nil, [limit]) → an_array\
split(pattern=nil, [limit]) {|sub| block } → str**

Divides `str` into substrings based on pattern, which can be a `String` or a `Regex`
```Ruby
" now's  the time ".split       #=> ["now's", "the", "time"]
" now's  the time ".split(' ')  #=> ["now's", "the", "time"]
" now's  the time".split(/ /)   #=> ["", "now's", "", "the", "time"]
"1, 2.34,56, 7".split(%r{,\s*}) #=> ["1", "2.34", "56", "7"]
"hello".split(//)               #=> ["h", "e", "l", "l", "o"]
"hello".split(//, 3)            #=> ["h", "e", "llo"]
"hi mom".split(%r{\s*})         #=> ["h", "i", "m", "o", "m"]

"mellow yellow".split("ello")   #=> ["m", "w y", "w"]
"1,2,,3,4,,".split(',')         #=> ["1", "2", "", "3", "4"]
"1,2,,3,4,,".split(',', 4)      #=> ["1", "2", "", "3,4,,"]
"1,2,,3,4,,".split(',', -4)     #=> ["1", "2", "", "3", "4", "", ""]

"1:2:3".split(/(:)()()/, 2)     #=> ["1", ":", "", "", "2:3"]

"".split(',', -1)               #=> []
```
If a block is given, invoke the block with each split substring.


[`#sub`](https://docs.ruby-lang.org/en/master/String.html#method-i-sub)\
**sub(pattern, replacement) → new_string\
sub(pattern) {|match| ... } → new_string**

- Returns a copy of `self` with only the **first** occurrence (not all occurrences) of the given `pattern` replaced
- Can use "" in replacement to delete a pattern
- Pattern can be `String` (exactly match) or `Regex`

```Ruby
"zz22zz12zz12zz".sub("12", "00")          # => "zz22zz00zz12zz"
"zz22zz12zz12zz".sub(/12/, "00")          # => "zz22zz00zz12zz"
      
"zz22zz12zz2zz".sub(/12/) { |str| "00" }  # => "zz22zz00zz12zz"
"zz22zz12zz12zz".sub(/12/) { |str| nil }  # => "zz22zzzz12zz"
```

Has `#sub!` version which modifies the string in-place and return `self` or `nil` if no changes made.
<br>

#### String - Type Conversion
[`#to_i`](https://docs.ruby-lang.org/en/master/String.html#method-i-to_i)\
**to_i(base = 10) → integer**

Returns the result of interpreting leading characters in `self` as an integer in the given `base` (which must be in (2..36)):
```Ruby
'123456'.to_i     # => 123456
'123def'.to_i(16) # => 1195503
```


[`#to_sym`](https://docs.ruby-lang.org/en/master/String.html#method-i-to_sym)\
**to_sym -> symbol**

Returns the `Symbol` corresponding to `self`, creating the symbol if it did not previously exist. Alias for `String#intern`
```Ruby
"Koala".intern         #=> :Koala
s = 'cat'.to_sym       #=> :cat
s == :cat              #=> true
s = '@cat'.to_sym      #=> :@cat
s == :@cat             #=> true
```
<br>

#### String - Others
[`#squeeze`](https://docs.ruby-lang.org/en/master/String.html#method-i-squeeze)\
**squeeze([other_str]*) → new_str**

Builds a set of characters from the `other_str` parameter(s) using the procedure described for `String#count`. Returns a new string where **contiguous runs of the same character** that occur in this set are **replaced by a single character**. If no arguments are given, all runs of identical characters are replaced by a single character.
```Ruby
"yellow moon".squeeze                  #=> "yelow mon"
"helloo ooo".squeeze				   #=> "helo o"
"  now   is  the".squeeze(" ")         #=> " now is the"
"putters shoot balls".squeeze("m-z")   #=> "puters shot balls"
```

Has `String#squeeze!` version which modifies `self` in-place and return `self` or `nil` if no modifications are made.


[`#reverse`](https://docs.ruby-lang.org/en/master/String.html#method-i-reverse)\
**reverse → new_string**

Returns a new string with the characters from `self` in reverse order.
```Ruby
'stressed'.reverse # => "desserts"
```

Has `String#reverse!` version which modifies `self` in-place and return `self` or `nil` if no modifications are made.
<br>

---

### Array Methods
#### Array - Querying

[`length`](https://docs.ruby-lang.org/en/master/Array.html#method-i-length), [`size`](https://docs.ruby-lang.org/en/master/Array.html#method-i-size)\
**size -> integer\
length -> integer**

Returns the count of elements. `length` and `size` are aliases.


[`include?`](https://docs.ruby-lang.org/en/master/Array.html#method-i-include-3F)\
**include?(an_object) -> boolean**

Returns whether any element `==` a given object. Object could be any type e.g. a String, Integer, Array etc.


[`empty?`](https://docs.ruby-lang.org/en/master/Array.html#method-i-empty-3F)\
**empty? -> boolean**

Returns `true` if there are no elements. Else `false`.


[`all?`](https://docs.ruby-lang.org/en/master/Array.html#method-i-all-3F)\
**all? -> boolean\
all? { |element| block } -> boolean\
all?(obj) → boolean**

Returns `true` only when **all** elements/block return values are truthy or `==` given object. Else return `false`.
```Ruby
# array elements
[1, 3, 5].all?	                     # => true
[1, 3, 5, nil].all?                  # => false

# block return values
[1 ,3, 5].all? { |num| num.odd? }    # => true
[1, 4, 7, 9].all? { |num| num.odd? } # => false

# == object argument
['food', 'fool', 'foot'].all?(/foo/) # => true
['food', 'drink'].all?(/bar/) # => false
[].all?(/foo/) # => true
[0, 0, 0].all?(0) # => true
[0, 1, 2].all?(1) # => false
```


[`any?`](https://docs.ruby-lang.org/en/master/Array.html#method-i-any-3F)
**any? -> boolean\
any? { |element| block } -> boolean\
any?(obj) → boolean**

Returns `true` when **at least 1** element/block return value is truthy or `==` given object. Else return `false`.
```Ruby
# array elements
[nil, 3, false].any?	                # => true
[false, nil, nil, nil].any?             # => false
[].any?							        # => false

# block return values
[1 ,3, 5].any? { |num| num.even? }      # => false
[1, 4, 7, 9].any? { |num| num.even? }   # => true

# == object argument
['food', 'drink'].any?(/foo/)           # => true
['food', 'drink'].any?(/bar/)           # => false
[0, 1, 2].any?(1)                       # => true
[0, 1, 2].any?(3)                       # => false
```


[`none?`](https://docs.ruby-lang.org/en/master/Array.html#method-i-none-3F)\
**none? -> boolean\
none? { |element| ... } -> boolean\
none?(obj) -> boolean**

Returns `true` when none of the elements/block return values are truthy or `==` given object. Else return `false`

[`one?`](https://docs.ruby-lang.org/en/master/Array.html#method-i-one-3F)\
**one? -> boolean\
one? { |element| ... } -> boolean\
one?(obj) -> boolean**

Returns `true` when exactly one element/block return value is truthy or `==` given object.
```Ruby
# array elements
[nil, 0].one? # => true
[0, 0].one? # => false
[nil, nil].one? # => false
[].one? # => false

# block return values
[0, 1, 2].one? {|element| element > 0 } # => false
[0, 1, 2].one? {|element| element > 1 } # => true
[0, 1, 2].one?(0) # => true
[0, 0, 1].one?(0) # => false

# == object argument
['food', 'drink'].one?(/bar/) # => false
['food', 'drink'].one?(/foo/) # => true
```


[`count`](https://docs.ruby-lang.org/en/master/Array.html#method-i-count)\
**count → an_integer\
count(obj) → an_integer
count {|element| ... } → an_integer**

Returns a count of either:
- number of elements in array
- number of elements in array `==` object
- number of elements whose block return value is truthy
```Ruby
# size of array
[0, 1, 2].count # => 3
[].count # => 0

# == object
[0, 1, 2, 0.0].count(0) # => 2
[0, 1, 2].count(3) # => 0

# block return values
[0, 1, 2, 3].count {|element| element > 1} # => 2
```


[`find_index`](https://docs.ruby-lang.org/en/master/Array.html#method-i-find_index), [`index`](https://docs.ruby-lang.org/en/master/Array.html#method-i-index)\
**index(object) → integer or nil\
index {|element| ... } → integer or nil\
index → new_enumerator**

Returns the index of the first element in array that
- `==` object
- whose block return value is truthy
```Ruby
# == object
a = [:foo, 'bar', 2, 'bar']
a.index('bar') # => 1

# first block return value is truthy
a = [:foo, 'bar', 2, 'bar']
a.index {|element| element == 'bar' } # => 1
```
Or `nil` if not found.


[`rindex`](https://docs.ruby-lang.org/en/master/Array.html#method-i-rindex)\
**index(object) → integer or nil\
index {|element| ... } → integer or nil\
index → new_enumerator**

Returns the index of the last element in array that
- `==` object
- whose block return value is truthy
```Ruby
# == object
a = [:foo, 'bar', 2, 'bar']
a.rindex('bar') # => 3

# first block return value is truthy
a = [:foo, 'bar', 2, 'bar']
a.rindex {|element| element == 'bar' } # => 3
```
Or `nil` if not found.


[`hash`](https://docs.ruby-lang.org/en/master/Array.html#method-i-hash)

Returns the integer hash code.
<br>

#### Array - Comparing

[`#<=>`](https://docs.ruby-lang.org/en/master/Array.html#method-i-3C-3D-3E)

Returns -1, 0, or 1 as `self` is less than, equal to, or greater than a given object.

[`#==`](https://docs.ruby-lang.org/en/master/Array.html#method-i-3D-3D)

Returns whether each element in `self` is `==` to the corresponding element in a given object.

[`eql?`](https://docs.ruby-lang.org/en/master/Array.html#method-i-eql-3F)

Returns whether each element in `self` is `eql?` to the corresponding element in a given object.
<br>

#### Array - Fetching

These methods do not modify `self`.

[`[]`](https://docs.ruby-lang.org/en/master/Array.html#method-i-5B-5D)\
**\[index\] -> element\
\[start_index, size\] -> new_array\
\[range\] -> new_array\

Returns one or more elements.

[`fetch`](https://docs.ruby-lang.org/en/master/Array.html#method-i-fetch)

Returns the element at a given offset.

[`first`](https://docs.ruby-lang.org/en/master/Array.html#method-i-first)

Returns one or more leading elements.

[`last`](https://docs.ruby-lang.org/en/master/Array.html#method-i-last)

Returns one or more trailing elements.

[`max`](https://docs.ruby-lang.org/en/master/Array.html#method-i-max)

Returns one or more maximum-valued elements, as determined by `<=>` or a given block.

[`max`](https://docs.ruby-lang.org/en/master/Array.html#method-i-max)

Returns one or more minimum-valued elements, as determined by `<=>` or a given block.

[`minmax`](https://docs.ruby-lang.org/en/master/Array.html#method-i-minmax)

Returns the minimum-valued and maximum-valued elements, as determined by `<=>` or a given block.

[`assoc`](https://docs.ruby-lang.org/en/master/Array.html#method-i-assoc)

Returns the first element that is an array whose first element `==` a given object.

[`rassoc`](https://docs.ruby-lang.org/en/master/Array.html#method-i-rassoc)

Returns the first element that is an array whose second element `==` a given object.

[`at`](https://docs.ruby-lang.org/en/master/Array.html#method-i-at)

Returns the element at a given offset.

[`values_at`](https://docs.ruby-lang.org/en/master/Array.html#method-i-values_at)

Returns the elements at given offsets.

[`dig`](https://docs.ruby-lang.org/en/master/Array.html#method-i-dig)

Returns the object in nested objects that is specified by a given index and additional arguments.

[`drop`](https://docs.ruby-lang.org/en/master/Array.html#method-i-drop)

Returns trailing elements as determined by a given index.

[`take`](https://docs.ruby-lang.org/en/master/Array.html#method-i-take)

Returns leading elements as determined by a given index.

[`drop_while`](https://docs.ruby-lang.org/en/master/Array.html#method-i-drop_while)

Returns trailing elements as determined by a given block.

[`take_while`](https://docs.ruby-lang.org/en/master/Array.html#method-i-take_while)

Returns leading elements as determined by a given block.

[`slice`](https://docs.ruby-lang.org/en/master/Array.html#method-i-slice)

Returns consecutive elements as determined by a given argument.

[`sort`](https://docs.ruby-lang.org/en/master/Array.html#method-i-sort)

Returns all elements in an order determined by `<=>` or a given block.

[`reverse`](https://docs.ruby-lang.org/en/master/Array.html#method-i-reverse)

Returns all elements in reverse order.

[`compact`](https://docs.ruby-lang.org/en/master/Array.html#method-i-compact)

Returns an array containing all non-`nil` elements.

[`select`](https://docs.ruby-lang.org/en/master/Array.html#method-i-select), [`filter`](https://docs.ruby-lang.org/en/master/Array.html#method-i-filter)

Returns an array containing elements selected by a given block.

[`uniq`](https://docs.ruby-lang.org/en/master/Array.html#method-i-uniq)

Returns an array containing non-duplicate elements.

[`rotate`](https://docs.ruby-lang.org/en/master/Array.html#method-i-rotate)

Returns all elements with some rotated from one end to the other.

[`bsearch`](https://docs.ruby-lang.org/en/master/Array.html#method-i-bsearch)

Returns an element selected via a binary search as determined by a given block.

[`bsearch_index`](https://docs.ruby-lang.org/en/master/Array.html#method-i-bsearch_index)

Returns the index of an element selected via a binary search as determined by a given block.

[`sample`](https://docs.ruby-lang.org/en/master/Array.html#method-i-sample)

Returns one or more random elements.

[`shuffle`](https://docs.ruby-lang.org/en/master/Array.html#method-i-shuffle)

Returns elements in a random order.

#### Array - Assigning

These methods add, replace, or reorder elements in `self`.

[`[]=`](https://docs.ruby-lang.org/en/master/Array.html#method-i-5B-5D-3D)

Assigns specified elements with a given object.

[`push`](https://docs.ruby-lang.org/en/master/Array.html#method-i-push), [`append`](https://docs.ruby-lang.org/en/master/Array.html#method-i-append), [`<<`](https://docs.ruby-lang.org/en/master/Array.html#method-i-3C-3C)

Appends trailing elements.

[`unshift`](https://docs.ruby-lang.org/en/master/Array.html#method-i-unshift), [`prepend`](https://docs.ruby-lang.org/en/master/Array.html#method-i-prepend)

Prepends leading elements.

[`insert`](https://docs.ruby-lang.org/en/master/Array.html#method-i-insert)\
**insert(index, \*objects) -> self**

Inserts given objects at a given index; does not replace elements.
```Ruby
# positive index
a = [:foo, 'bar', 2]
a.insert(1, :bat, :bam) # => [:foo, :bat, :bam, "bar", 2]

# negative index
a = [:foo, 'bar', 2]
a.insert(-2, :bat, :bam)
a # => [:foo, "bar", :bat, :bam, 2]

# index beyond self.size
a = [:foo, 'bar', 2]
a.insert(5, :bat, :bam)
a # => [:foo, "bar", 2, nil, nil, :bat, :bam]
```


[`concat`](https://docs.ruby-lang.org/en/master/Array.html#method-i-concat)\
**concat(\*other_arrays) -> self**

Appends all elements from `other_arrays` and return `self`.
```Ruby
a = [0, 1]
a.concat([2, 3], [4, 5]) # => [0, 1, 2, 3, 4, 5]
```


[`fill`](https://docs.ruby-lang.org/en/master/Array.html#method-i-fill)

Replaces specified elements with specified objects.

[`replace`](https://docs.ruby-lang.org/en/master/Array.html#method-i-replace)\
**replace(other_array) -> self**

Replaces the content of `self` with the content of `other_array`.

[`reverse!`](https://docs.ruby-lang.org/en/master/Array.html#method-i-reverse-21)

Replaces `self` with its elements reversed.

[`rotate!`](https://docs.ruby-lang.org/en/master/Array.html#method-i-rotate-21)

Replaces `self` with its elements rotated.

[`shuffle!`](https://docs.ruby-lang.org/en/master/Array.html#method-i-shuffle-21)

Replaces `self` with its elements in random order.

[`sort!`](https://docs.ruby-lang.org/en/master/Array.html#method-i-sort-21)

Replaces `self` with its elements sorted, as determined by `<=>` or a given block.

[`sort_by!`](https://docs.ruby-lang.org/en/master/Array.html#method-i-sort_by-21)

Replaces `self` with its elements sorted, as determined by a given block.

#### Array - Deleting
Each of these methods removes elements from `self`:

[`pop`](https://docs.ruby-lang.org/en/master/Array.html#method-i-pop)

Removes and returns the last element.

[`shift`](https://docs.ruby-lang.org/en/master/Array.html#method-i-shift)

Removes and returns the first element.

[`compact!`](https://docs.ruby-lang.org/en/master/Array.html#method-i-compact-21)

Removes all non-`nil` elements.

[`delete`](https://docs.ruby-lang.org/en/master/Array.html#method-i-delete)\
**delete(object) -> object or nil**

Removes all elements equal to a given object from `self` and return object or `nil` if object not present.
```Ruby
a = [5, 1, 2, 3, 5]
a.delete(5)  # => 5
a			 # => [1, 2, 3]
a.delete(10) # => nil
a			 # => [1, 2, 3]
```


[`delete_at`](https://docs.ruby-lang.org/en/master/Array.html#method-i-delete_at)\
**delete_at(index) -> element or nil**

Removes the element at a given index and return the element or `nil` if out of bound. Index can be positive or negative (starting at end of array).

[`delete_if`](https://docs.ruby-lang.org/en/master/Array.html#method-i-delete_if)\
**delete_if { |element| block } -> self**

Remove elements that have truthy block return value. Return `self`.
```Ruby
a = [1, 2, 3]
a.delete_if { |num| num == 2 } # => [1, 3]
a							   # => [1, 3]
```

[`keep_if`](https://docs.ruby-lang.org/en/master/Array.html#method-i-keep_if)

Removes elements whose block return value are falsy. Return `self`. Alias for `Array#select!`

[`reject!`](https://docs.ruby-lang.org/en/master/Array.html#method-i-reject-21)

Removes elements that have truthy block return value. Return `self`. Alias to `delete_if`.

[`select!`](https://docs.ruby-lang.org/en/master/Array.html#method-i-select-21), [`filter!`](https://docs.ruby-lang.org/en/master/Array.html#method-i-filter-21)

Removes elements not specified by a given block.

[`slice!`](https://docs.ruby-lang.org/en/master/Array.html#method-i-slice-21)

Removes and returns a sequence of elements.

[`uniq!`](https://docs.ruby-lang.org/en/master/Array.html#method-i-uniq-21)

Removes duplicates.
<br>

#### Array - Combining

[#&](https://docs.ruby-lang.org/en/master/Array.html#method-i-26)\
**array & other_array -> new_array**

Returns an new array containing elements found both in `self` and other array, duplicates omitted. Order of `self` is preserved.
```Ruby
[3, 1, 2, 3, 2] & [2, 2, 3, 5, 3] # => [3, 2]
```


[`intersection`](https://docs.ruby-lang.org/en/master/Array.html#method-i-intersection)\
**intersection(\*other_arrays) -> new_array**

Returns an array containing common elements found in `self` and in **all** given arrays, duplicates ommitted.
```Ruby
[0, 1, 2, 3].intersection([0, 1, 2], [0, 1, 3]) # => [0, 1]
[0, 0, 1, 1, 2, 3].intersection([0, 1, 2, 0], [0, 1, 3, 0]) # => [0, 1]
```


[`+`](https://docs.ruby-lang.org/en/master/Array.html#method-i-2B)\
**self + other_array -> new_array**

Returns a new array containing all elements of `self` followed by all elements of `other_array`.


[`-`](https://docs.ruby-lang.org/en/master/Array.html#method-i-2D)\
**self - other_array -> new_array**

Returns a new array containing all elements of `self` that are not found in `other_array`.
```Ruby
a = [1, 4, 2, 3, 4, 5, 2]
a - [5, 2, 5, 7]       # => [1, 4, 3, 4]
a                      # => [1, 4, 2, 3, 4, 5, 2]
```

[#|](https://docs.ruby-lang.org/en/master/Array.html#method-i-7C)\
**self | other_array -> new_array**

Returns a new array containing all elements of `self` and all elements of `other_array`. Elements of `new_array` follow the order of `self` then `other_array`, duplicates removed.
```Ruby
a = [1, 2, 2, 3]
b = [7, 7, 3, 2, 6]
a | b  # => [1, 2, 3, 7, 6] 

```


[`union`](https://docs.ruby-lang.org/en/master/Array.html#method-i-union)\
**union(\*other_arrays) -> new_array**

Returns a new array containing all elements of `self` and all elements of **one or more arrays**, duplicates removed.
```Ruby
a = [1, 2, 2, 3]
a.union([7, 7, 3, 2, 6], [11, 3, "3"]) # => [1, 2, 3, 7, 6, 11, "3"]
a  # => [1, 2, 2, 3]
```

[`difference`](https://docs.ruby-lang.org/en/master/Array.html#method-i-difference)\
**difference(\*other_arrays) -> new_array**

Returns a new array containing all elements of `self` that are not found in any of the given arrays.
```Ruby
a = [1, 2, 2, 3]
a.difference([5, 3], [1, 1, "2"]) # => [2, 2]
a # => [1, 2, 2, 3]
```

[`product`](https://docs.ruby-lang.org/en/master/Array.html#method-i-product)\
**product(\*other_arrays) -> new_array**

Returns or yields all combinations of elements from `self` and given arrays.
```Ruby
a = [1, 2, 3]
a.product([4, 5]) # => [[1, 4], [1, 5], [2, 4], [2, 5], [3, 4], [3, 5]]
a  # => [1, 2, 3]

[1, 2].product([3, 4], [5, 6]) # => [[1, 3, 5], [1, 3, 6], [1, 4, 5], [1, 4, 6], [2, 3, 5], [2, 3, 6], [2, 4, 5], [2, 4, 6]] 
```
<br>

#### Array - Iterating

[`each`](https://docs.ruby-lang.org/en/master/Array.html#method-i-each)

Passes each element to a given block.

[`reverse_each`](https://docs.ruby-lang.org/en/master/Array.html#method-i-reverse_each)

Passes each element, in reverse order, to a given block.

[`each_index`](https://docs.ruby-lang.org/en/master/Array.html#method-i-each_index)

Passes each element index to a given block.

[`cycle`](https://docs.ruby-lang.org/en/master/Array.html#method-i-cycle)

Calls a given block with each element, then does so again, for a specified number of times, or forever.


[`combination`](https://docs.ruby-lang.org/en/master/Array.html#method-i-combination)\
**combination(n) {|element| ... } → self\
combination(n) → new_enumerator**

Takes an integer parameter `n` and calls a given block with combinations of elements of `self`; a combination does not use the same element more than once.
```Ruby
a = [0, 1, 2]
a.combination(2) {|combination| p combination }
# Output:
[0, 1]
[0, 2]
[1, 2]


#Another example:
a = [0, 1, 2]
a.combination(3) {|combination| p combination }
# Output:
[0, 1, 2]
```


[`permutation`](https://docs.ruby-lang.org/en/master/Array.html#method-i-permutation)\
**permutation {|element| ... } → self\
permutation(n) {|element| ... } → self\
permutation → new_enumerator\
permutation(n) → new_enumerator**

Calls a given block with permutations of elements of `self`. The order of permutations is indeterminate. When `n` is not given, it permutates with `self.size`.
```Ruby
a = [0, 1, 2]
a.permutation(2) {|permutation| p permutation }
# output
[0, 1]
[0, 2]
[1, 0]
[1, 2]
[2, 0]
[2, 1]

# n not given, permutates with n = self.size
a.permutation {|permutation| p permutation }
# output
[0, 1, 2]
[0, 2, 1]
[1, 0, 2]
[1, 2, 0]
[2, 0, 1]
[2, 1, 0]
```


[`repeated_combination`](https://docs.ruby-lang.org/en/master/Array.html#method-i-repeated_combination)\
**repeated_combination(n) {|combination| ... } → self\
repeated_combination(n) → new_enumerator**

Calls a given block with combinations of elements of `self`; a combination may use the same element more than once.
```Ruby
a = [0, 1, 2]
a.repeated_combination(1) {|combination| p combination }
# output
[0]
[1]
[2]


# n = 2 
a.repeated_combination(2) {|combination| p combination }
# output
[0, 0]
[0, 1]
[0, 2]
[1, 1]
[1, 2]
[2, 2]
```


[`repeated_permutation`](https://docs.ruby-lang.org/en/master/Array.html#method-i-repeated_permutation)\
**repeated_permutation(n) {|permutation| ... } → self
repeated_permutation(n) → new_enumerator**

Calls a given block with permutations of elements of `self`; a permutation may use the same element more than once.
```Ruby
# n = 1:
a = [0, 1, 2]
a.repeated_permutation(1) {|permutation| p permutation }
# Output:
[0]
[1]
[2]


# n = 2:
a.repeated_permutation(2) {|permutation| p permutation }
# Output:
[0, 0]
[0, 1]
[0, 2]
[1, 0]
[1, 1]
[1, 2]
[2, 0]
[2, 1]
[2, 2]
```
<br>

#### Array - Converting

[`map`](https://docs.ruby-lang.org/en/master/Array.html#method-i-map), [`collect`](https://docs.ruby-lang.org/en/master/Array.html#method-i-collect)

Returns an array containing the block return-value for each element.

[`map!`](https://docs.ruby-lang.org/en/master/Array.html#method-i-map-21), [`collect!`](https://docs.ruby-lang.org/en/master/Array.html#method-i-collect-21)

Replaces each element with a block return-value.

[`flatten`](https://docs.ruby-lang.org/en/master/Array.html#method-i-flatten)

Returns an array that is a recursive flattening of `self`.

[`flatten!`](https://docs.ruby-lang.org/en/master/Array.html#method-i-flatten-21)

Replaces each nested array in `self` with the elements from that array.

[`inspect`](https://docs.ruby-lang.org/en/master/Array.html#method-i-inspect), [`to_s`](https://docs.ruby-lang.org/en/master/Array.html#method-i-to_s)

Returns a new [`String`](https://docs.ruby-lang.org/en/master/String.html) containing the elements.

[`join`](https://docs.ruby-lang.org/en/master/Array.html#method-i-join)

Returns a newsString containing the elements joined by the field separator.

[`to_a`](https://docs.ruby-lang.org/en/master/Array.html#method-i-to_a)

Returns `self` or a new array containing all elements.

[`to_ary`](https://docs.ruby-lang.org/en/master/Array.html#method-i-to_ary)

Returns `self`.

[`to_h`](https://docs.ruby-lang.org/en/master/Array.html#method-i-to_h)

Returns a new hash formed from the elements.

[`transpose`](https://docs.ruby-lang.org/en/master/Array.html#method-i-transpose)

Transposes `self`, which must be an array of arrays.

[`zip`](https://docs.ruby-lang.org/en/master/Array.html#method-i-zip)

Returns a new array of arrays containing `self` and given arrays; follow the link for details.

#### Array - Others

[`*`](https://docs.ruby-lang.org/en/master/Array.html#method-i-2A)

Returns one of the following:

-   With integer argument `n`, a new array that is the concatenation of `n` copies of `self`.
    
-   With string argument `field_separator`, a new string that is equivalent to `join(field_separator)`.
    

[`abbrev`](https://docs.ruby-lang.org/en/master/Array.html#method-i-abbrev)

Returns a hash of unambiguous abbreviations for elements.

[`pack`](https://docs.ruby-lang.org/en/master/Array.html#method-i-pack)

Packs the elements into a binary sequence.

[`sum`](https://docs.ruby-lang.org/en/master/Array.html#method-i-sum)

Returns a sum of elements according to either `+` or a given block.
### Hash Methods
#### Hash - Querying

[`any?`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-any-3F)

Returns whether any element satisfies a given criterion.

[`compare_by_identity?`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-compare_by_identity-3F)

Returns whether the hash considers only identity when comparing keys.

[`default`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-default)

Returns the default value, or the default value for a given key.

[`default_proc`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-default_proc)

Returns the default proc.

[`empty?`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-empty-3F)

Returns whether there are no entries.

[`eql?`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-eql-3F)

Returns whether a given object is equal to `self`.

[`hash`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-hash)

Returns the integer hash code.

[`has_value?`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-has_value-3F)

Returns whether a given object is a value in `self`.

[`include?`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-include-3F), [`has_key?`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-has_key-3F), [`member?`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-member-3F), [`key?`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-key-3F)

Returns whether a given object is a key in `self`.

[`length`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-length), [`size`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-size)

Returns the count of entries.

[`value?`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-value-3F)

Returns whether a given object is a value in `self`.

#### Hash - Comparing

[#<](https://docs.ruby-lang.org/en/master/Hash.html#method-i-3C)

Returns whether `self` is a proper subset of a given object.

[#<=](https://docs.ruby-lang.org/en/master/Hash.html#method-i-3C-3D)

Returns whether `self` is a subset of a given object.

[#==](https://docs.ruby-lang.org/en/master/Hash.html#method-i-3D-3D)

Returns whether a given object is equal to `self`.

[#>](https://docs.ruby-lang.org/en/master/Hash.html#method-i-3E)

Returns whether `self` is a proper superset of a given object

[#>=](https://docs.ruby-lang.org/en/master/Hash.html#method-i-3E-3D)

Returns whether `self` is a proper superset of a given object.

#### Hash - Fetching

[`[]`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-5B-5D)

Returns the value associated with a given key.

[`assoc`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-assoc)

Returns a 2-element array containing a given key and its value.

[`dig`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-dig)

Returns the object in nested objects that is specified by a given key and additional arguments.

[`fetch`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-fetch)

Returns the value for a given key.

[`fetch_values`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-fetch_values)

Returns array containing the values associated with given keys.

[`key`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-key)

Returns the key for the first-found entry with a given value.

[`keys`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-keys)

Returns an array containing all keys in `self`.

[`rassoc`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-rassoc)

Returns a 2-element array consisting of the key and value of the first-found entry having a given value.

[`values`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-values)

Returns an array containing all values in `self`/

[`values_at`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-values_at)

Returns an array containing values for given keys.

#### Hash - Assigning

[`[]=`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-5B-5D-3D), [`store`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-store)

Associates a given key with a given value.

[`merge`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-merge)

Returns the hash formed by merging each given hash into a copy of `self`.

[`merge!`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-merge-21), [`update`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-update)

Merges each given hash into `self`.

[`replace`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-replace)

Replaces the entire contents of `self` with the contents of a givan hash.

#### Hash - Deleting

These methods remove entries from `self`:

[`clear`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-clear)

Removes all entries from `self`.

[`compact!`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-compact-21)

Removes all `nil`-valued entries from `self`.

[`delete`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-delete)

Removes the entry for a given key.

[`delete_if`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-delete_if)

Removes entries selected by a given block.

[`filter!`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-filter-21), [`select!`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-select-21)

Keep only those entries selected by a given block.

[`keep_if`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-keep_if)

Keep only those entries selected by a given block.

[`reject!`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-reject-21)

Removes entries selected by a given block.

[`shift`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-shift)

Removes and returns the first entry.

These methods return a copy of `self` with some entries removed:

[`compact`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-compact)

Returns a copy of `self` with all `nil`-valued entries removed.

[`except`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-except)

Returns a copy of `self` with entries removed for specified keys.

[`filter`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-filter), [`select`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-select)

Returns a copy of `self` with only those entries selected by a given block.

[`reject`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-reject)

Returns a copy of `self` with entries removed as specified by a given block.

[`slice`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-slice)

Returns a hash containing the entries for given keys.

#### Hash - Iterating

[`each`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-each), [`each_pair`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-each_pair)

Calls a given block with each key-value pair.

[`each_key`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-each_key)

Calls a given block with each key.

[`each_value`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-each_value)

Calls a given block with each value.

#### Hash - Converting

[`inspect`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-inspect), [`to_s`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-to_s)

Returns a new [`String`](https://docs.ruby-lang.org/en/master/String.html) containing the hash entries.

[`to_a`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-to_a)

Returns a new array of 2-element arrays; each nested array contains a key-value pair from `self`.

[`to_h`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-to_h)

Returns `self` if a Hash; if a subclass of Hash, returns a Hash containing the entries from `self`.

[`to_hash`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-to_hash)

Returns `self`.

[`to_proc`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-to_proc)

Returns a proc that maps a given key to its value.

#### Hash - Transforming Keys and Values

[`transform_keys`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-transform_keys)

Returns a copy of `self` with modified keys.

[`transform_keys!`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-transform_keys-21)

Modifies keys in `self`

[`transform_values`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-transform_values)

Returns a copy of `self` with modified values.

[`transform_values!`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-transform_values-21)

Modifies values in `self`.

#### Hash - Others

[`flatten`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-flatten)

Returns an array that is a 1-dimensional flattening of `self`.

[`invert`](https://docs.ruby-lang.org/en/master/Hash.html#method-i-invert)

Returns a hash with the each key-value pair inverted.
### Enumerable Methods



