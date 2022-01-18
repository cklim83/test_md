Topic: Common Ruby Methods\
Date: 18 Jan 2022\
Course: RB109

---

### Sections

---


### Integer Methods
#### Integer - Iteration
[`#downto`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-downto)\
downto(limit) {|i| ... } → self\
downto(limit) → enumerator

Calls the given block with each integer value from `self` down to `limit`; returns `self`:
```Ruby
a = []
10.downto(5) {|i| a << i }              # => 10
a                                       # => [10, 9, 8, 7, 6, 5]
a = []
0.downto(-5) {|i| a << i }              # => 0
a                                       # => [0, -1, -2, -3, -4, -5]
4.downto(5) {|i| fail 'Cannot happen' } # => 4
```

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
4.downto(5) {|i| fail 'Cannot happen' } # => 4
```

[`Integer#upto`](https://docs.ruby-lang.org/en/master/Integer.html#method-i-upto)

Calls the given block with each integer value from `self` up to `limit`; returns `self`:
```Ruby
Signatures 
upto(limit) {|i| ... } → self
upto(limit) → enumerator
---
	
a = []
5.upto(10) {|i| a << i }              # => 5
a                                     # => [5, 6, 7, 8, 9, 10]
a = []
-5.upto(0) {|i| a << i }              # => -5
a                                     # => [-5, -4, -3, -2, -1, 0]
5.upto(4) {|i| fail 'Cannot happen' } # => 5
```

`times`

#### Rounding
`#ceil`
`#abs`
`#round`
`#floor`


#### Division & Modulo
`#%`
`#div`
`#divmod`
`#fdiv`
`#modulo`
`#remainder`

#### Checking
`#even?`
`#integer?`
`#odd?`
`#zero?`

#### Arithemtic
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

#### Type Conversion
`#to_f`
`#to_i`
`#to_s`

#### Bitwise Operation
`#&`
`#|`

---

### String Methods

---

### Array Methods

---

### Enumerable Methods



