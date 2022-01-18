Topic: RB109 Interview Assessment Preparation
Date: 03 Jan 2022
Course: RB109

---

### Sections

---

### Integer Methods
#### downto, upto
**downto(limit) {|i| block } → self
downto(limit) → an_enumerator**

Iterates the given block, passing in decreasing values from `int` down to and including `limit`.

If no block is given, an Enumerator is returned instead.
```Ruby
5.downto(1) { |n| print n, ".. " }
puts "Liftoff!"
#=> "5.. 4.. 3.. 2.. 1.. Liftoff!"
```

**upto(limit) {|i| block } → self
upto(limit) → an_enumerator**

Iterates the given block, passing in integer values from `int` up to and including `limit`.
If no block is given, an [`Enumerator`](https://docs.ruby-lang.org/en/2.6.0/Enumerator.html) is returned instead.
```Ruby
5.upto(10) {|i| print i, " " }   #=> 5 6 7 8 9 10
```

#### times
**times {|i| block } → self
times → an_enumerator**

Iterates the given block `int` times, passing in values from zero to `int - 1`.
If no block is given, an `Enumerator` is returned instead.
```Ruby
5.times {|i| print i, " " }   #=> 0 1 2 3 4
```
---

#### %, remainder and divmod
int % other → real

Returns `int` modulo `other`.

See [`Numeric#divmod`](https://docs.ruby-lang.org/en/2.6.0/Numeric.html#method-i-divmod) for more information.

loat % other → float

Returns the modulo after division of `float` by `other`.

6543.21.modulo(137)      #=> 104.21000000000004
6543.21.modulo(137.24)   #=> 92.92999999999961


**numeric % other_numeric -> real**
Returns `numeric` modulo `other_numeric`

**numeric.remainder(numeric) → real**
Returns the remainder: `x.remainder(y)` returns `x-y*(x/y).truncate`

**numeric.divmod(numeric) → array**
Returns an **array** containing the **quotient** and **modulus** obtained by dividing `num` by `numeric`.


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

#### odd?, even?, prime?
**odd? → true or false**
Returns `true` if `int` is an odd number.

**even? → true or false**
Returns `true` if `int` is an even number.

**prime?()**
Returns `true` if `self` is a prime number, else returns `false`.

#### round
**round([ndigits] [, half: mode]) → integer or float**

Returns `int` rounded to the nearest value with a precision of `ndigits` decimal digits (default: 0).
When the precision is negative, the returned value is an integer with at least `ndigits.abs` trailing zeros.
Returns `self` when `ndigits` is zero or positive.
```Ruby
1.round           #=> 1
1.round(2)        #=> 1
15.round(-1)      #=> 20
(-15).round(-1)   #=> -20
``
#### chr
**chr([encoding]) → string**
Returns a string containing the character represented by the `int`'s value according to `encoding`.
```Ruby
65.chr    #=> "A"
230.chr   #=> "\xE6"
255.chr(Encoding::UTF_8)   #=> "\u00FF"
```

#### abs
**abs → integer**
Returns the absolute value of `int`.
```Ruby
(-12345).abs   #=> 12345
-12345.abs     #=> 12345
12345.abs      #=> 12345
```

#### gcd, lcm
**gcd(other_int) → integer**
Returns the greatest common divisor of the two integers. The result is always positive. 0.gcd(x) and x.gcd(0) return x.abs.
```Ruby
36.gcd(60)                  #=> 12
2.gcd(2)                    #=> 2
3.gcd(-7)                   #=> 1
```

**lcm(other_int) → integer**
Returns the least common multiple of the two integers. The result is always positive. 0.lcm(x) and x.lcm(0) return zero.
```Ruby
36.lcm(60)                  #=> 180
2.lcm(2)                    #=> 2
3.lcm(-7)                   #=> 21
```


### String Methods
#### `String#%`
**str % arg → new_str**

Format - Uses _str_ as a format specification, and returns the result of applying it to _arg_. If the format specification contains more than one substitution, then _arg_ must be an `Array` or `Hash` containing the values to be substituted. See `Kernel::sprintf` for details of the format string.
```Ruby
"%05d" % 123                              #=> "00123"
"%-5s: %016x" % [ "ID", self.object_id ]  #=> "ID   : 00002b054ec93168"
"foo = %{foo}" % { :foo => 'bar' }        #=> "foo = bar"
```

#### `String#*`
**str * integer → new_str**

Copy: Returns a **new `String`** containing `integer` copies of the receiver. `integer` must be greater than or equal to 0.
```Ruby
"Ho! " * 3   #=> "Ho! Ho! Ho! "
"Ho! " * 0   #=> ""
```

#### `String#+`
**str + other_str → new_str**

Concatenation: Returns a **new `String`** containing _other_str_ concatenated to _str_.
```Ruby
"Hello from " + self.to_s   #=> "Hello from main"
```

#### `String#<<`
**str << obj → str
str << integer → str**

Destructively appends the given object to _str_. If the object is an `Integer`, it is considered a codepoint and converted to a character from ascii table before being appended.
```Ruby
a = "hello "
a << "world"   #=> "hello world"
a << 33        #=> "hello world!"
```

#### `String#concat`
**concat(obj1, obj2, ...) → str**

Destructively concatenates one or more object(s) in given order to _str_. If an object is an `Integer`, it is considered a codepoint and converted to a character from ascii table before concatenation.
```Ruby
a = "hello "
a.concat("world", 33)      #=> "hello world!"
a                          #=> "hello world!"

b = "sn"
b.concat("_", b, "_", b)   #=> "sn_sn_sn"
```

#### `String#=~`
**str =~ obj → integer or nil**
Match: If _obj_ is a `Regexp`, use it as a pattern to match against _str_, and returns the index where the match starts, or `nil` if there is no match. Otherwise, invokes _obj.=~_, passing _str_ as an argument. The default `=~` in `Object` returns `nil`.

Note: `str =~ regexp` is not the same as `regexp =~ str`. Strings captured from named capture groups are assigned to local variables only in the second case.
```Ruby
"cat o' 9 tails" =~ /\d/   #=> 7
"cat o' 9 tails" =~ 9      #=> nil
```

#### `String#capitalize`, `String#capitalize!`
**capitalize → new_str**
Returns a copy of _str_ with the first character converted to uppercase and the remainder to lowercase.
```Ruby
"hello".capitalize    #=> "Hello"
"HELLO".capitalize    #=> "Hello"
"123ABC".capitalize   #=> "123abc"
```

**capitalize! → str or nil**
Modifies _str_ by converting the first character to uppercase and the remainder to lowercase. Returns `nil` if no changes are made. 
```Ruby
a = "hello"
a.capitalize!   #=> "Hello"
a               #=> "Hello"
a.capitalize!   #=> nil
```

#### `String#center`
**center(width, padstr=' ') → new_str**
Centers `str` in `width`. If `width` is greater than the length of `str`, returns a new [`String`](https://docs.ruby-lang.org/en/2.6.0/String.html) of length `width` with `str` centered and padded with `padstr`; otherwise, returns `str`.
```Ruby
"hello".center(4)         #=> "hello"
"hello".center(20)        #=> "       hello        "
"hello".center(20, '123') #=> "1231231hello12312312"
```

#### `String#chars`
**chars → an_array**
Returns an array of characters in _str_. This is a shorthand for `str.each_char.to_a`.

#### `String#chomp` and `String#chomp!`
**chomp(separator=$/) → new_str**
Returns a new `String` with the given record separator removed from the end of _str_ (if present). If `$/` has not been changed from the default Ruby record separator, then `chomp` also removes carriage return characters (that is it will remove `\n`, `\r`, and `\r\n`). If `$/` is an empty string, it will remove all trailing newlines from the string.
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

**chomp!(separator=$/) → str or nil**
Modifies _str_ in place as described for `String#chomp`, returning _str_, or `nil` if no modifications were made.

#### `String#chop` and `String#chop!`
**chop → new_str**
Returns a new `String` with the last character removed. If the string ends with `\r\n`, both characters are removed. Applying `chop` to an empty string returns an empty string. `String#chomp` is often a safer alternative, as it leaves the string unchanged if it doesn't end in a record separator.
```Ruby
"string\r\n".chop   #=> "string"
"string\n\r".chop   #=> "string\n"
"string\n".chop     #=> "string"
"string".chop       #=> "strin"
"x".chop.chop       #=> ""
```

**chop! → str or nil**
Processes _str_ as for `String#chop`, returning _str_, or `nil` if _str_ is the empty string.

#### `String#chr`
**chr → new string**
Returns the **first character** of the caller string
```Ruby
a = "abcde"
a.chr    #=> "a"
```

#### `String#count`
**count([other_str]+) → integer**
- Does not accept regex
- Accepts one or more strings, the resultant intersection of string will be the character sets that it find and count
	- count("hello", "^l") == count("heo")
- Uses some regex notation in the string argument
	- ^ for negation
	- "Am-z" for range
- String count is case sensitive.

Accepts **1 or more String arguments** represented by `[`other_str`]+`. Each String defines **a set of characters** to count. The **intersection of these sets defines the characters to count in `str`. Any `other_str` that starts with a caret `^` is negated. The sequence `c1-c2` means all characters between c1 and c2. The backslash character `\` can be used to escape `^` or `-` and is otherwise ignored unless it appears at the end of a sequence or the end of a `other_str`.
```Ruby
a = "hello world"
a.count "lo"                   #=> 5 count of l and o
a.count "lo", "o"              #=> 2 count of o
a.count "hello", "^l"          #=> 4 count of h, e and o
a.count "ej-m"                 #=> 4 count of e, j, k, l, m

"hello^world".count "\\^aeiou" #=> 4 count of ^, and vowels
"hello-world".count "a\\-eo"   #=> 4 count of a, -, e and o

c = "hello world\\r\\n"
c.count "\\"                   #=> 2 count of \\
c.count "\\A"                  #=> 0 count of A
c.count "X-\\w"                #=> 3
```

#### `String#delete` and `String#delete!`
**delete([other_str]+) → new_str**
Accepts 1 or more String arguments. Returns a copy of _str_ with all characters in the **intersection of its arguments** deleted. Uses the same rules for building the set of characters as `String#count`.
```Ruby
"hello".delete "l","lo"        #=> "heo" 
"hello".delete "lo"            #=> "he"
"hello".delete "aeiou", "^e"   #=> "hell"
"hello".delete "ej-m"          #=> "ho"
```

**delete!([other_str]+) → str or nil**
Performs a `delete` operation in place, returning _str_, or `nil` if _str_ was not modified

#### `String#downcase`, `String#downcase!`
**downcase → new_str
downcase([options]) → new_str**
Returns a copy of _str_ with all uppercase letters replaced with their lowercase counterparts. Which letters exactly are replaced, and by which other letters, depends on the presence or absence of [options](https://docs.ruby-lang.org/en/2.6.0/String.html#method-i-downcase), and on the `encoding` of the string.

**downcase! → str or nil
downcase!([options]) → str or nil**
Downcases the contents of _str_, returning `nil` if no changes were made.

#### `String#empty?`
**empty? → true or false**
Returns `true` if _str_ has a length of zero.
```Ruby
"hello".empty?   #=> false
" ".empty?       #=> false
"".empty?        #=> true
```

#### `String#end_with?`
**end_with?([suffixes]+) → true or false**
Returns true if `str` ends with one of the `suffixes` given.
```Ruby
"hello".end_with?("ello")               #=> true

# returns true if one of the +suffixes+ matches.
"hello".end_with?("heaven", "ello")     #=> true
"hello".end_with?("heaven", "paradise") #=> false
```

#### `String#gsub` and `String#gsub!`
**gsub(pattern, replacement) → new_str
gsub(pattern, hash) → new_str
gsub(pattern) {|match| block } → new_str
gsub(pattern) → enumerator**

Returns a copy of _str_ with **_all_** occurrences of _pattern_ substituted for the second argument. The _pattern_ is typically a `Regexp`; if given as a `String`, any regular expression metacharacters it contains will be interpreted literally, e.g. `'\\d'` will match a backslash followed by 'd', instead of a digit.

If _replacement_ is a `String` it will be substituted for the matched text. It may contain back-references to the pattern's capture groups of the form `\\d`, where _d_ is a group number, or `\\k<n>`, where _n_ is a group name. If it is a double-quoted string, both back-references must be preceded by an additional backslash. However, within _replacement_ the special match variables, such as `$&`, will not refer to the current match.

If the second argument is a `Hash`, and the matched text is one of its keys, the corresponding value is the replacement string.

In the block form, the current match string is passed in as a parameter, and variables such as `$1`, `$2`, ``$` ``, `$&`, and `$'` will be set appropriately. The value returned by the block will be substituted for the match on each call.

The result inherits any tainting in the original string or any supplied replacement string.

When neither a block nor a second argument is supplied, an `Enumerator` is returned.
```Ruby
"hello".gsub(/[aeiou]/, '*')                  #=> "h*ll*"

#group number example
"hello".gsub(/([aeiou])/, '<\1>')             #=> "h<e>ll<o>"

# Block example
"hello".gsub(/./) {|s| s.ord.to_s + ' '}      #=> "104 101 108 108 111 "

#group name example
"hello".gsub(/(?<foo>[aeiou])/, '{\k<foo>}')  #=> "h{e}ll{o}"

#hash example
'hello'.gsub(/[eo]/, 'e' => 3, 'o' => '*')    #=> "h3ll*"		
```

#### `String#include?`
**include? other_str → true or false**
Returns `true` if _str_ contains the given string or character.
```Ruby
"hello".include? "lo"   #=> true	string example
"hello".include? "ol"   #=> false	string example
"hello".include? ?h     #=> true  	single_char example
```

#### `String#index`
**index(substring [, offset]) → integer or nil
index(regexp [, offset]) → integer or nil**
Returns the **index of the first occurrence** of the given _substring_ or pattern (_regexp_) in _str_. Returns `nil` if not found. If the second parameter is present, it specifies the position in the string to begin the search.
```Ruby
"hello".index('e')             #=> 1
"hello".index('lo')            #=> 3
"hello".index('a')             #=> nil

# Single character example using ?char
"hello".index(?e)              #=> 1

# offset example, start search from index -3, 
# matched 'o', return its index in 'hello'
"hello".index(/[aeiou]/, -3)   #=> 4
```

#### `String#insert`
**insert(index, other_str) → str**
Destructively inserts _other_str_ before the character at the given _index_, modifying _str_. Negative indices count from the end of the string, and insert _after_ the given character. The intent is insert _aString_ so that it starts at the given _index_.
```Ruby
# positive index, insert AT that index
"abcd".insert(0, 'X')    #=> "Xabcd"
"abcd".insert(3, 'X')    #=> "abcXd"
"abcd".insert(4, 'X')    #=> "abcdX"

# negative index, insert AFTER that index
"abcd".insert(-3, 'X')   #=> "abXcd"
"abcd".insert(-1, 'X')   #=> "abcdX"
```

#### `String#intern` or `String#to_sym`
**intern → symbol
to_sym → symbol**
Returns the `Symbol` corresponding to _str_, creating the symbol if it did not previously exist.
```Ruby
"Koala".intern         #=> :Koala
s = 'cat'.to_sym       #=> :cat
s == :cat              #=> true
'cat and dog'.to_sym   #=> :"cat and dog"
```

#### `String#length` or `String#size`
**length → integer
size → integer**
Returns the character length of _str_.

#### `String#ljust`
**ljust(integer, padstr=' ') → new_str**
If _integer_ is greater than the length of _str_, returns a new `String` of length _integer_ with _str_ left justified and padded with _padstr_; otherwise, returns _str_.
```Ruby
# Example of integer < str.length
"hello".ljust(4)            #=> "hello"

# Example of integer > str.length, default ' ' padding
"hello".ljust(20)           #=> "hello               "

# Example of different padding
"hello".ljust(20, '1234')   #=> "hello123412341234123"
```

#### `String#lstrip` or `String#lstrip!`
**lstrip → new_str**
Returns a copy of the receiver with leading whitespace removed. See also [`String#rstrip`](https://docs.ruby-lang.org/en/2.6.0/String.html#method-i-rstrip) and [`String#strip`](https://docs.ruby-lang.org/en/2.6.0/String.html#method-i-strip).
```Ruby
"  hello  ".lstrip   #=> "hello  "
"hello".lstrip       #=> "hello"
```

**lstrip! → self or nil**
Removes leading whitespace from the receiver. Returns the altered receiver, or `nil` if no change was made.

#### `String#match`
**match(pattern) → matchdata or nil
match(pattern, pos) → matchdata or nil**
Converts _pattern_ to a `Regexp` (if it isn't already one), then invokes its `match` method on _str_. If the second parameter is present, it specifies the position in the string to begin the search.
```Ruby
'hello'.match('(.)\1')      #=> #<MatchData "ll" 1:"l">
'hello'.match('(.)\1')[0]   #=> "ll"
'hello'.match(/(.)\1/)[0]   #=> "ll"
'hello'.match(/(.)\1/, 3)   #=> nil
'hello'.match('xx')         #=> nil
```

If a block is given, invoke the block with [`MatchData`](https://docs.ruby-lang.org/en/2.6.0/MatchData.html) if match succeed. The return value is a value from block execution in this case.

#### `String#match?`
**match?(pattern) → true or false
match?(pattern, pos) → true or false**
Converts _pattern_ to a `Regexp` (if it isn't already one), then returns a `true` or `false` indicates whether the regexp is matched _str_ or not without updating `$~` and other related variables. If the second parameter is present, it specifies the position in the string to begin the search.
```Ruby
"Ruby".match?(/R.../)    #=> true

# Index offset in string before match
"Ruby".match?(/R.../, 1) #=> false

"Ruby".match?(/P.../)    #=> false
```

#### `String#ord`
**ord → integer**
Returns the `Integer` ordinal of a one-character string.
```Ruby
"a".ord         #=> 97
```


#### `String#replace`
**replace(other_str) → str**
Replaces the contents and taintedness of _str_ with the corresponding values in _other_str_.
```Ruby
s = "hello"         #=> "hello"
s.replace "world"   #=> "world"
```

#### `String#reverse` or `String#reverse!`
**reverse → new_str**
Returns a new string with the characters from _str_ in reverse order.
```Ruby
"stressed".reverse   #=> "desserts"
```

**reverse! → str**
Reverses _str_ in place.

#### `String#slice` or `String#[]` or `String#slice!`
**slice(index) → new_str or nil
slice(start, length) → new_str or nil
slice(range) → new_str or nil
slice(regexp) → new_str or nil
slice(regexp, capture) → new_str or nil
slice(match_str) → new_str or nil**

Element Reference — If passed a single `index`, returns a substring of one character at that index. If passed a `start` index and a `length`, returns a substring containing `length` characters starting at the `start` index. If passed a `range`, its beginning and end are interpreted as offsets delimiting the substring to be returned.

In these three cases, if an index is negative, it is counted from the end of the string. For the `start` and `range` cases the starting index is just before a character and an index matching the string's size. Additionally, an empty string is returned when the starting index for a character range is at the end of the string.

Returns `nil` if the initial index falls outside the string or the length is negative.

If a `Regexp` is supplied, the matching portion of the string is returned. If a `capture` follows the regular expression, which may be a capture group index or name, follows the regular expression that component of the [`MatchData`](https://docs.ruby-lang.org/en/2.6.0/MatchData.html) is returned instead.

If a `match_str` is given, that string is returned if it occurs in the string.

Returns `nil` if the regular expression does not match or the match string cannot be found.
```Ruby
a = "hello there"

a[1]                   #=> "e"
a[2, 3]                #=> "llo"
a[2..3]                #=> "ll"

a[-3, 2]               #=> "er"
a[7..-2]               #=> "her"
a[-4..-2]              #=> "her"
a[-2..-4]              #=> ""

a[11, 0]               #=> ""
a[11]                  #=> nil
a[12, 0]               #=> nil
a[12..-1]              #=> nil

a[/[aeiou](.)\1/]      #=> "ell"
a[/[aeiou](.)\1/, 0]   #=> "ell"
a[/[aeiou](.)\1/, 1]   #=> "l"
a[/[aeiou](.)\1/, 2]   #=> nil

a[/(?<vowel>[aeiou])(?<non_vowel>[^aeiou])/, "non_vowel"] #=> "l"
a[/(?<vowel>[aeiou])(?<non_vowel>[^aeiou])/, "vowel"]     #=> "e"

a["lo"]                #=> "lo"
a["bye"]               #=> nil
```

#### `String#split`
**split(pattern=nil, [limit]) → an_array
split(pattern=nil, [limit]) {|sub| block } → str**

Divides _str_ into substrings based on a delimiter, returning an array of these substrings.

If _pattern_ is a `String`, then its contents are used as the delimiter when splitting _str_. If _pattern_ is a single space, _str_ is split on whitespace, with leading and trailing whitespace and runs of contiguous whitespace characters ignored.

If _pattern_ is a `Regexp`, _str_ is divided where the pattern matches. Whenever the pattern matches a zero-length string, _str_ is split into individual characters. If _pattern_ contains groups, the respective matches will be returned in the array as well.

If _pattern_ is `nil`, the value of `$;` is used. If `$;` is `nil` (which is the default), _str_ is split on whitespace as if ' ' were specified.

If the _limit_ parameter is omitted, trailing null fields are suppressed. If _limit_ is a positive number, at most that number of split substrings will be returned (captured groups will be returned as well, but are not counted towards the limit). If _limit_ is `1`, the entire string is returned as the only entry in an array. If negative, there is no limit to the number of fields returned, and trailing null fields are not suppressed.

When the input `str` is empty an empty [`Array`](https://docs.ruby-lang.org/en/2.6.0/Array.html) is returned as the string is considered to have no fields to split.
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

#### `String#squeeze`
**squeeze([other_str]*) → new_str**

Builds a set of characters from the _other_str_ parameter(s) using the procedure described for `String#count`. Returns a new string where runs of the same character that occur in this set are replaced by a single character. If no arguments are given, all runs of identical characters are replaced by a single character.
```Ruby
"yellow moon".squeeze                  #=> "yelow mon"
"  now   is  the".squeeze(" ")         #=> " now is the"
"putters shoot balls".squeeze("m-z")   #=> "puters shot balls"
```

#### `String#start_with?`
**start_with?([prefixes]+) → true or false**

Returns true if `str` starts with one of the `prefixes` given. Each of the `prefixes` should be a [`String`](https://docs.ruby-lang.org/en/2.6.0/String.html) or a [`Regexp`](https://docs.ruby-lang.org/en/2.6.0/Regexp.html).
```Ruby
"hello".start_with?("hell")               #=> true
"hello".start_with?(/H/i)                 #=> true

# returns true if one of the prefixes matches.
"hello".start_with?("heaven", "hell")     #=> true
"hello".start_with?("heaven", "paradise") #=> false
```

#### `String#strip`
**strip → new_str**
Returns a copy of the receiver with leading and trailing whitespace removed.

Whitespace is defined as any of the following characters: null, horizontal tab, line feed, vertical tab, form feed, carriage return, space.
```Ruby
"    hello    ".strip   #=> "hello"
"\tgoodbye\r\n".strip   #=> "goodbye"
"\x00\t\n\v\f\r ".strip #=> ""
"hello".strip           #=> "hello"
```

#### `String#sub`
**sub(pattern, replacement) → new_str
sub(pattern, hash) → new_str
sub(pattern) {|match| block } → new_str**

Returns a copy of `str` with the **_first_** occurrence of `pattern` replaced by the second argument. The `pattern` is typically a [`Regexp`](https://docs.ruby-lang.org/en/2.6.0/Regexp.html); if given as a [`String`](https://docs.ruby-lang.org/en/2.6.0/String.html), any regular expression metacharacters it contains will be interpreted literally, e.g. `'\\d'` will match a backslash followed by 'd', instead of a digit.

If `replacement` is a [`String`](https://docs.ruby-lang.org/en/2.6.0/String.html) it will be substituted for the matched text. It may contain back-references to the pattern's capture groups of the form `"\d"`, where _d_ is a group number, or `"\k<n>"`, where _n_ is a group name. If it is a double-quoted string, both back-references must be preceded by an additional backslash. However, within `replacement` the special match variables, such as `$&`, will not refer to the current match. If `replacement` is a [`String`](https://docs.ruby-lang.org/en/2.6.0/String.html) that looks like a pattern's capture group but is actually not a pattern capture group e.g. `"\'"`, then it will have to be preceded by two backslashes like so `"\\'"`.

If the second argument is a [`Hash`](https://docs.ruby-lang.org/en/2.6.0/Hash.html), and the matched text is one of its keys, the corresponding value is the replacement string.

In the block form, the current match string is passed in as a parameter, and variables such as `$1`, `$2`, ``$` ``, `$&`, and `$'` will be set appropriately. The value returned by the block will be substituted for the match on each call.

The result inherits any tainting in the original string or any supplied replacement string.
```Ruby
"hello".sub(/[aeiou]/, '*')                  #=> "h*llo"
"hello".sub(/([aeiou])/, '<\1>')             #=> "h<e>llo"
"hello".sub(/./) {|s| s.ord.to_s + ' ' }     #=> "104 ello"
"hello".sub(/(?<foo>[aeiou])/, '*\k<foo>*')  #=> "h*e*llo"
'Is SHELL your preferred shell?'.sub(/[[:upper:]]{2,}/, ENV)
 #=> "Is /bin/bash your preferred shell?"
```

#### `String#swapcase` or `String#swapcase!`
**swapcase → new_str
swapcase([options]) → new_str**

Returns a copy of _str_ with uppercase alphabetic characters converted to lowercase and lowercase characters converted to uppercase.

See [`String#downcase`](https://docs.ruby-lang.org/en/2.6.0/String.html#method-i-downcase) for meaning of `options` and use with different encodings.
```Ruby
"Hello".swapcase          #=> "hELLO"
"cYbEr_PuNk11".swapcase   #=> "CyBeR_pUnK11"
```

#### `String#upcase` or `String#upcase!`
**upcase → new_str
upcase([options]) → new_str**

Returns a copy of _str_ with all lowercase letters replaced with their uppercase counterparts.

See [`String#downcase`](https://docs.ruby-lang.org/en/2.6.0/String.html#method-i-downcase) for meaning of `options` and use with different encodings.
```Ruby
"hEllO".upcase   #=> "HELLO"
```

#### `String#upto`
**upto(other_str, exclusive=false) {|s| block } → str
upto(other_str, exclusive=false) → an_enumerator**

Iterates through successive values, starting at _str_ and ending at _other_str_ inclusive, passing each value in turn to the block. The `String#succ` method is used to generate each value. If optional second argument exclusive is omitted or is false, the last value will be included; otherwise it will be excluded.

If no block is given, an enumerator is returned instead.
```Ruby
"a8".upto("b6") {|s| print s, ' ' }
for s in "a8".."b6"
  print s, ' '
end
```
_produces:_
```Ruby
a8 a9 b0 b1 b2 b3 b4 b5 b6
a8 a9 b0 b1 b2 b3 b4 b5 b6
```
If _str_ and _other_str_ contains only ascii numeric characters, both are recognized as decimal numbers. In addition, the width of string (e.g. leading zeros) is handled appropriately.
```Ruby
"9".upto("11").to_a   #=> ["9", "10", "11"]
"25".upto("5").to_a   #=> []
"07".upto("11").to_a  #=> ["07", "08", "09", "10", "11"]
```

### Common Array Methods


### Common Enumerable Methods

---

### Notes

---

### Questions/Cues



