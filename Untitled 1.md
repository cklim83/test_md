Topic: Common Ruby Methods\
Date: 18 Jan 2022\
Course: RB109

---

### Sections

---


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

**Summary Comparison**

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
**`Integer#*`**
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

`#**`, `#pow`
`#+`
`#-`
`#/`

#### Integer - Type Conversion
`#to_f`
`#to_i`
`#to_s`

#### Integer - Bitwise Operation
`#&`
`#|`

#### Integer - Others
`#abs`

---

### String Methods

---

### Array Methods

---

### Enumerable Methods



