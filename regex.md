Topic: Introduction to Regular Expressions in Ruby\
Date: 06 Jan 2022\
Course: Launch School Open Bookshelf

---

### Sections
[Summary](#summary)\
[Basic Matching - Alphanumerics](#basic-matching---alphanumerics)\
[Basic Matching - Special Characters](#basic-matching---special-characters)\
[Basic Matching - Concatenation](#basic-matching---concatenation)\
[Basic Matching - Alternation](#basic-matching---alternation)\
[Basic Matching - Control Characters](#basic-matching---control-characters)\
[Basic Matching - Case Insensitive Match](#basic-matching---case-insensitive-match)\
[Character Classes - Set of Characters](#character-classes---set-of-characters)\
[Character Classes - Range of Characters](#character-classes---range-of-characters)\
[Character Classes - Negated Classes](#character-classes---negated-classes)\
[Character Class Shortcuts - Any Character](#character-class-shortcuts---any-character)\
[Character Class Shortcuts - Whitespace](#character-class-shortcuts---whitespace)\
[Character Class Shortcuts - Digits and Hex Digits](#character-class-shortcuts---digits-and-hex-digits)\
[Character Class Shortcuts - Word Characters](#character-class-shortcuts---word-characters)\
[Anchors - Start/End of Line](#anchors---startend-of-line)\
[Anchors - Lines vs String](#anchors---lines-vs-string)\
[Anchors - Start/End of String](#anchors---startend-of-string)\
[Anchors - Word Boundaries](#anchors---word-boundaries)\
[Quantifiers - Zero or More](#quantifiers---zero-or-more)\
[Quantifiers - One or More](#quantifiers---one-or-more)\
[Quantifiers - Zero or One](#quantifiers---zero-or-one)\
[Quantifiers - Ranges](#quantifiers---ranges)\
[Quantifiers - Greedy vs Lazy Match](#quantifiers---greedy-vs-lazy-match)\
[Ruby Application - Matching Strings](#ruby-regex-application---matching-strings)\
[Ruby Application - Splitting Strings](#ruby-regex-application---splitting-strings)\
[Ruby Application - Capture Groups](#ruby-regex-application---capture-groups)\
[Ruby Application - Transformations in Ruby](#ruby-regex-application---transformations-in-ruby)

---

### Summary
- Use [rubular](https://rubular.com/) to test out Regex in Ruby.

- In the following tables, unescaped `a`, `b`, and `z` characters denote regular characters (letters, digits, punctuations) while unescaped `p` and `q` characters represent patterns (each pattern may be arbitrarily complex). Other characters are literals.

#### Basic Matching

| Pattern | Meaning |
| --- | --- |
|`/a/`|Match the character `a`|
|`/\?/`, `/\./`|Match a meta-character literally|
|`/\n/`, `/\t/`|Match a control character (newline, tab etc)|
|`/pq/`| Concatenation (`p` followed by `q`)|
|`/(p)/`| Capture group |
|`/(p \| q)/`| Alternation (`p` or `q`)|
|`/p/i`| Case insensitive match|

#### Character Classes and Shortcuts

|Pattern|Meaning|
|---|---|
|`/[ab]/`| `a` or `b`|
|`/[a-z]`| `a` through `z`, inclusive|
|`/[^ab]/`| Not(`a` or `b` )|
|`/[^a-z]/`| Not(`a` through `z`)|
|`/./`| Any character except newline|
|`/\s/`, `/[\s]/`| Whitespace character (space, tab, newline etc)|
|`/\S/`, `/[\S]/`| Not whitespace character|
|`/\d/`, `/[\d]/`| Decimal digit (0-9)|
|`/\D/`, `/[\D]/`| Not a decimal digit|
|`/\w/`, `/[\w]/`| Word character (`0-9`, `a-z`, `A-Z`, `_`)|
|`/\W/`, `/[\W]/`| Not a word character|

#### Anchors

|Pattern|Meaning|
|---|---|
|`/^p/`|Pattern at start of **line**|
|`/p$/`|Pattern at end of **line**|
|`/\Ap/`|Pattern at start of **string**|
|`/p\z/`|Pattern at end of string (after newline)|
|`/p\Z/`|Pattern at end of string (before newline)|
|`/\bp/`|Pattern begins at word boundary|
|`/p\b/`|Pattern ends at word boundary|
|`/\Bp/`|Pattern begins at non-word boundary|
|`/p\B/`|Pattern ends at non-word boundary|

#### Quantifiers

|Pattern|Meaning|
|---|---|
|`/p*/`|0 or more occurrences of pattern|
|`/p+/`|1 or more occurrences of pattern|
|`/p?/`|0 or 1 occurrence of pattern|
|`/p{m}/`|`m` occurrences of pattern|
|`/p{m,}/`|`m` or more occurrences of pattern|
|`/p{m,n}/`|`m` through `n` occurrences of pattern|
|`/p*?/`|0 or more occurrences (lazy)|
|`/p+?/`|1 or more occurrences (lazy)|
|`/p??/`|0 or 1 occurrence (lazy)|
|`/p{m,}?/`|`m` or more occurrences (lazy)|
|`/p{m,n}?/`|`m` through `n` occurrences (lazy)|

#### Meta-Characters

|Outside Character Classes|Inside Character Classes|
|---|---|
|`$ ^ * + ? . ( ) [ ] { }`|`^ - \ /`|

#### Common Ruby Methods for Regex

|Method|Use|
|---|---|
|`String#match`|Determine if regex matches a string, returns `MatchData` or `nil`|
|`string =~ regex`|Determine if regex matches a string, return index of match or `nil`|
|`String#split`|Split string by regex|
|`String#sub`|Replace regex match **one** time|
|`String#gsub`|Replace regex match globally|

[Back to top](#sections)

---

### Basic Matching - Alphanumerics
- In Ruby, regular expressions are delimited by `/` in the format `/pattern/`.
- At the most basic, we delimit a literal alphanumeric character between `/` to form a regex 
	- e.g. `/s/` will match `s`, `sand`, `cats`, `cast` and even `Mississippi`. However it will not match `S` or `KANSAS` as regex are case sensitive by default
- `String#match` accepts a regex argument to find a pattern match in the caller string. It returns a `MatchData` object (truthy) if the pattern is found or `nil` otherwise
	```Ruby
	str = "cast"
	print "matched 's'" if str.match(/s/)
	print "matched 'x'" if str.match(/x/)

	# => matched 's' printed
	```


### Basic Matching - Special Characters
- `$ ^ * + ? . ( ) [ ] { } | \ /` are _meta-characters_ and can have special meanings in Ruby or JavaScript regex, depending on the context

- To use a meta-character as a **literal**, it has to be escaped using a leading backslash `\`
	```Ruby
	# using /\?/ to match "?" instead of the special meaning of ? as a quantifier or to indicate lazy match. We prepend the match with !! to convert result to boolean

	!!"?".match(/\?/)               # => true
	!!"What's up, doc?".match(/\?/) # => true
	!!"Silence!".match(/\?/)        # => false
	!!"What's that?".match(/\?/)    # => true
	```

- Remaining characters, including colons `:` and spaces ` ` are not meta-characters and do not have to be escaped when used to construct a pattern. Note: `/ /` and `/[ ]/` are equivalent and both match to spaces.  
	```Ruby
	!!"chris:x:300".match(/:/)                  # => true
	!!"A thought; no, forget it.".match(/ /)    # => true
	!!"::::".match(/:/)                         # => true	
	!!"meta-characters".match(/-/)              # => true
	```


### Basic Matching - Concatenation
- A new pattern can be formed by **concatenating 2 or more patterns**. For example, the pattern `/cat/` is formed by concatenating `c`, `a` and `t` in that order and will match any `cat` substrings
	```Ruby
	!!"copycat".match(/cat/)	# => true
	!!"cast".match(/cat/)		# => false as "t" not after "ca"
	!!"CAT".match(/cat/)		# => false as different case
	!!"CAT".match(/cat/i)		# => true as i flag asked for case-insensitive match
	```

- While concatenation allows us to construct complex regex from simpler ones, it is easy to go overboard and build something overly complex and unreadable. We should use regex judiciously and try to keep their complexity at the lowest.


### Basic Matching - Alternation
- Alternation involves building a regex that matches one of several sub-patterns using the syntax: `(pattern_a|...| pattern_n)`
	```Ruby
	pattern = /(cat|dog|rabbit)/ #match "cat" or "dog" or "rabbit"

	!!"The lazy cat".match(pattern)                     # => true
	!!"The dog barks".match(pattern)                    # => true
	!!"The cat ran from the barking dog".match(pattern) # => true
	!!"dives down the rabbit hole".match(pattern)       # => true
	!!"catalog".match(pattern)                          # => true
	!!"The Yellow Dog".match(pattern)                   # => false
	```

- Meta-characters such as `()` and `|` can also be escaped to yield their literals as patterns to be matched
```Ruby
# Example to show how to escape mata-characters `()` and `|`
pattern = /\(cat\|dog\|rabbit\)/ #match "(cat|dog|rabbit)"

!!"(cat|dog)".match(pattern)                # => false
!!"bird(cat|dog)zebra".match(pattern)       # => false
!!"cat".match(pattern)                      # => false
!!"dog".match(pattern)                      # => false
!!"dn(cat|dog|rabbit)!!!".match(pattern)    # => true
```


### Basic Matching - Control Characters
- Control characters such as `\n` (newline), `\r` (carriage return) and `\t` (tabs) can also be matched using regex
	```Ruby
	!!"\thello".match(/\t/)                 # => true
	!!"Goodbye!\n".match(/\n/)              # => true
	puts "\\hello" if "\\hello".match(/\\/) # => outputs \hello
	```

- Not all characters with leading `\` are control characters
	- `\s` and `\d` are shortcuts
	- `\A` and `\z` are anchors
	- `\x` and `\u` are special character code markers (not covered in this introduction)
	- `\y` and `\q` have no special meaning at all

### Basic Matching - Case Insensitive Match
- A regex pattern is case insensitive if it has a `i` appended after the closing `/`. These options are called **flags** or **modifiers** and are language specific
	```Ruby
	!!"Hello".match(/hello/)    # => false
	!!"Hello".match(/hello/i)   # => true
	```

[Back to top](#sections)

---

### Character Classes - Set of Characters
- Character class patterns use a list of characters between square brackets e.g. `/[abc]/`. They match **single occurrence** of **any** of the characters between the brackets

- We could use these patterns if only a **subset of characters need to be case insensitive** e.g. `/[Hh]oover/` to match `Hoover` or `hoover`

- When listing characters in `[]`, we should **order them by type** e.g. digits followed by upper and lower case letters `[0-9A-Za-z]` to aid readability

- Character classes can also be concatenated e.g. `/[abc][12]/` will match any of `a1`, `a2`, `b1`, `b2`, `c1` or `c2`.

- Among the full set of meta-characters, only `^, \, -, [ ]` retain their special meaning **inside** a character class
	- `^`, `\`, `-` are only meta_characters in specific situations. `^` only represent negation if it is the **first character** in the class i.e. `/[^...]/`, otherwise it is just a literal. `-` will represent an inclusive character range in `/[c-z]/` but becomes a literal if it is the first character in the square bracket i.e. `/[-z]/`
	- As `*` and `+` do not have special meaning in `[]`, `/[*+]/` will match any literal `*` or `+` without the need for backslash. Although adding backslash `/[\*\+]/` do not change the meaning, `/[*+]/` is preferred for readability
 

### Character Classes - Range of Characters
- `-` is used for consecutive and inclusive sequence of characters in a character class. For example, `/[a-z]/` matches any lower case alphabetic character, `/[0-9]/` matches any digits. We can also combine ranges to create more patterns e.g. `/[0-9A-Fa-f]/` to match any alphanumeric. 

- Best practices:
	- It is not advisable to use range for non-alphanumeric characters
	- Do not combine lowercase and uppercase alphabetic cases in a single range i.e. `/[A-z]/` as several non-alphabetic characters such as such as brackets (`[`, `]`), caret (`^`) and underscore (`_`) lies between `Z` to `a`. Use `/[A-Za-z]/` instead if we only want to include letters.

### Character Classes - Negated Classes
- Negation is to match characters **other than** those listed in the brackets e.g. `/[^aeiou]/` will match all characters except lowercase vowels. The caret `^` has to be the first character in `[]` to represent negation.

[Back to top](#sections)

---

### Character Class Shortcuts - Any Character
- `/./` is used to match a single occurrence of **any character except newline (`\n`)**. To also match newlines, we need to include the `m` (multiline) option

- **Note:** Even though `.` is a shortcut for a character class, it **literally** just matches a **dot** inside square brackets

### Character Class Shortcuts - Whitespace
- `/\s/` is a shortcut for **all whitespace characters** which encompasses 
	- space `' '`, 
	- tab `\t`,
	- vertical tab `\v`,
	- carriage return `\r`, 
	- line feed `\n` and 
	- form feed `\f`. 
- Thus `/\s/` is equivalent to `/[ \t\v\r\n\f]/`
	```ruby
	p 'matched' if 'Four score'.match(/\s/) 	# => matched
	p 'matched' if "Four\tscore".match(/\s/) 	# => matched
	p 'matched' if "Four-score\n".match(/\s/) 	# => matched
	p 'matched' if "Four-score".match(/\s/) 	# => nil
	```

- `/\S/` is a shortcut for **all non-whitespace characters** and is equivalent to `/[^ \t\v\r\n\f]/`
	```ruby
	p 'matched' if 'a b'.match(/\S/)            # => matched
	p 'matched' if " \t\n\r\f\v".match(/\S/)    # => nil
	```

- `/\s/` and `/\S/` can be used outside or inside square brackets 
	- Outside square brackets e.g. `/\s/`, the regex matches any of the whitespace characters, 
	- Inside the square brackets, it represent an **OR** relationship to other members of the class e.g. `/[a-z\s]/` will match either a lowercase letter or any whitespace characters.

### Character Class Shortcuts - Digits and Hex Digits

| Shortcut | Meaning |
|---|---|
| `\d` | Any decimal digit `/[0-9]/` |
| `\D` | Any character other than a decimal digit `/[^0-9]/` |
| `\h` | Any hexadecimal digit `[0-9A-Fa-f]` (**Ruby**) |
| `\H` | Any character other than a hexadecmial digit (**Ruby**) |

Like `\s` and `\S`, `\d` and `\D` can be used both inside and outside square brackets. `/[a-z\d]/` matches any lower case letters or decimal digits. 

### Character Class Shortcuts - Word Characters
- `/\w/` is a shortcut for any **word characters** and is equivalent to **`/[0-9a-zA-Z_]/`**

- `/\W/` is a shortcut for any **non-word characters** **`/[^0-9a-zA-Z_]/`**

- `\w` and `\W` can be used inside and outside of square brackets.

[Back to top](#sections)

---

### Anchors - Start/End of Line
- `^` matches the start of **line**
- `$` matches the end of **line**

| string | `/^cat/` | `/cat$/` | `/^cat$/` |
| --- | --- | --- | --- |
|"cat"| match | match | match |
|"catastrophe"| match | no match | no match |
|"wildcat"| no match | match | no match |
|"\<cat\>""| no match | no match | no match|
	
	
- `^` and `$` serve as anchors when outside square brackets `[]` but have different meaning inside.

| Regex | Meaning|
| --- | --- |
|`/[^cat]/`| match any character except `c`, `a` or `t` |
|`/^cat/`| match any **line** that starts with `cat`|
|`/[cat$]/`| match any character with `c`, `a`, `t` or `$` |
|`/cat$/`| match any line ending with `cat` |


### Anchors - Lines vs String
- `^` and `$` anchor lines NOT strings.
	```Ruby
	TEXT1 = "red fish\nblue fish"
	p "matched red" if TEXT1.match(/^red/)      # => matched red
	p "matched blue" if TEXT1.match(/^blue/)    # => matched blue
	```
	Regex `/^blue/` matching `TEXT1` confirms `^` anchor lines and not strings since `blue` is the start of a new line but is in the middle of the `TEXT1` string

	```Ruby
	TEXT2 = "red fish\nred shirt"
	p "matched fish" if TEXT2.match(/fish$/)    # => matched fish
	p "matched shirt" if TEXT2.match(/shirt$/)  # => matched shirt
	```
	In Ruby, each line ends with either with a `\n` or string end. A line starts with either the start of a string or immediately after `\n`.
	Even though the first line in the string contains `\n`, `fish` is still positioned at end of the line. `$` doesn't care if there is a `\n` character at the end.

### Anchors - Start/End of String
- `\A` matches the start of a **string**
- `\z` matches the end of a **string**
	```Ruby
	TEXT3 = "red fish\nblue fish"
	TEXT4 = "red fish\nred shirt"

	p "matched red" if TEXT3.match(/\Ared/)         # => matched red
	p "matched blue" if TEXT3.match(/\Ablue/)       # => nil
	p "matched fish" if TEXT4.match(/fish\z/)       # => nil
	p "matched shirt" if TEXT4.match(/shirt\z/)     # => matched shirt
	```

- `\Z` matches the end of a string unless the string ends with a `\n`, in which case it matches up to but excluding `\n`.
```Ruby
# No newline at string end
"hello world".match(/world\z/)      # => #<MatchData "world">
"hello world".match(/world\Z/)      # => #<MatchData "world">

# With newline at string end
"hello world\n".match(/world\z/)    # => nil
"hello world\n".match(/world\n\z/)  # => #<MatchData "world\n">
"hello world\n".match(/world\Z/)     # => #<MatchData "world">

# With 2 newline at string end
"hello world\n\n".match(/world\Z/)   # => nil
```
- So use lowercase `\z` for exact character match at string end. `\Z` would still return a match by ignoring a `\n` at string end which may cause unexpected behavior.


### Anchors - Word Boundaries
- `\b` matches word boundaries (`\w` i.e. `[0-9A-Za-z_]`)
- `\B` matches non-word boundaries (`\W` i.e. `[^0-9A-Za-z_]`) boundaries

- A word boundary occurs:
	- Between any pair of characters, where one is a word character and the other a non-word character
	- At the **start of a string** provided first character is a word character
	- At the **end of a string** provided the last character is a word character

- A non-word boundary occurs:
	- Between any pair of characters, where both are word or both are non-word characters
	- At the **start of a string** if first character is NOT a word character
	- At the **end of a string** if the last character is NOT a word character

	
- In the string `Eat some food`, word boundaries occur **before** `E` `s` and `f` and **after** `t`, `e` and `d`. Non-word boundaries occur elsewhere such as between `o` and `m` in `some` and between `.` and end of the string.

**Example: Word boundary (`\b`) Match**
```Ruby
pattern = /\b\w\w\w\b/

One fish,           # Matches One since we have'\AOne '
Two fish,           # Matches Two since we have '\nTwo '
Red fish,           # Matches Red since we have '\nRed '
Blue fish.          # No match
123 456 7890        # Matches 123 and 456
```

**Example: Non-word boundary (`\B`) Match**
```Ruby
pattern = /\Bjohn/i

John Silver        # No match
Randy Johnson      # No match
Duke Pettijohn     # Match john since i\Bjohn
Joe_Johnson        # Match John since _\BJohn
```
**Note:** `\b` and `\B` do not work as word boundaries inside of character classes (between square brackets). In fact, `\b` inside square brackets matches a backspace character instead.

[Back to top](#sections)

---

### Quantifiers - Zero or More
- `*` matches **0 or more** occurrence of the pattern to its left
	```Ruby
	pattern = /\b\d\d\d\d*\b/  # match 3 or more digits surrounded by word boundaries

	Four and 20 black birds
	365 days in a year, 100 years in a century.
	My phone number is 222-555-1212.
	My serial number is 345678912.

	# matches 365, 100, 222, 555, 1212 and 345678912 but not 20
	```
	| Pattern | Explanation |
	| --- | --- |
	| `\b` | Start at word boundary |
	| `\d` | A single digit ... |
	| `\d` | followed by a single digit ... |
	| `\d` | followed by a single digit ... |
	| `\d*` | followed by 0 or more digits |
	| `\b` | ending with a word boundary |

- The quantifier always applies to a single pattern on its left. We can use parentheses to define group pattern to apply the `*`. For example `/1(234)*5/` matches all the below as `(234)` is treated as a single pattern by the regex engine to apply the `*`.
	```Ruby
	15
	12345
	12342345
	12342342345
	```

- Since `*` will match zero occurrence, it will match all strings since they all contain empty strings `""` by default
	```Ruby
	"hello".match(/x*/)    # => <MatchData ""> 
	```

- **Note:** The regex `*` quantifier has a different meaning to the `*` wildcard used in command line even though they both looked the same 
	- The `*` wildcard is equivalent to `/.*/` in regex. For example, `blue*doc` in command line matches any files whose name begins with `blue` and ends with `doc`. 
	- The regex `/blue*doc/` has the `*` applied on the `e` character and matches any sequence of characters starting with `blu` and ending with `doc` with 0 or more consecutive `e`s in between.

### Quantifiers - One or More
- `+` quantifier matches **1 or more** occurrence of a pattern to its left
- To match 3 or more digits, we could use `/\b\d\d\d+\b/` instead of `/\b\d\d\d\d*\b/`

### Quantifiers - Zero or One
- `?` quantifier matches **0 or 1** occurrence of a pattern to its left
	```Ruby
	pattern = /coo?t/ 

	cot     # matches pattern
	coot    # matches pattern
	```

- `?` is useful in matching optional characters e.g. `-` separator in dates
	```Ruby
	pattern = /\b\d\d\d\d-?\d\d-?\d\d\b/

	20170111     # matches pattern
	2017-01-11   # matches pattern
	2017-0111    # matches pattern
	201701-11    # matches pattern
	```

- Since `?` can match zero occurrence `/h?/` will match any string since it will also match `""` 
	```Ruby
	pattern = `/h?/
	`
	his     # matches 'h'
	is      # matches ""
	ish     # matches 'h'
	```

- **Note:** The `?` wildcard used in command line is different from the `?` used in regex
	- `?` wildcard in command line matches 0 or 1 occurrence of **any** character
	- `?` quantifier in regex match 0 or 1 occurrence of a pattern (which could be multiple characters e.g. `(abc)?`) to its left.

### Quantifiers - Ranges
- Range quantifiers allow us to specify the number of occurrences more precisely. The quantities are enclosed within curly braces `{}`
	- `p{m}` matches **precisely `m`** occurrences of pattern `p`
	- `p{m,}` matches **m or more** occurrences of `p`
	- `p{m,n}` matches **at least `m` and up till `n`** occurrences of pattern `p`

	```Ruby
	/\b\d{10}\b/                                 # matches exactly 10 consecutive digits


	/\b\d{3,}\b/                                 # matches 3 or more digits
	Four and 20 black birds                      # no match
	365 days in a year, 100 years in a century.  # match 365, 100
	My phone number is 222-555-1212.             # match 222, 555, 1212
	My serial number is 345678912.               # match 345678912


	/\b[a-z]{5,8}\b/i                            # matches 5-8 letter words, case insensitive
	Bizarre                                      # matches Bizarre
	a                                            # no match
	one two three four five six seven eight nine # match three, seven, eight
	sensitive                                    # no match as word exceeds match length
	dropouts                                     # match dropouts
	```


### Quantifiers - Greedy vs Lazy Match
- Quantifier matches are **greedy** by nature: strive to match the longest string that meets the pattern. For example using regex `/a[abc]*c/` on `xabcbcbacy` will match `abcbcbac`, not `abc` or `abcbc`.

- `?` is added after the main quantifier if we want to match the **least characters possible**, called a **lazy match**. For example, `a[abc]*?c/` matches `abc` and `ac` in `xabcbcbacy`

[Back to top](#sections)

---

### Ruby Regex Application - Matching Strings
- `String#match` are often used in conditionals. They return `MatchData`, a truthy value if a match is found or `nil` otherwise
	```Ruby
	fetch_url(text) if text.match(/\Ahttps?:\/\/\S+\z/)
	```

- `MatchData` is not an Array but responds to element referencing syntax such as `[0]`, `[1]` etc
	
- `=~` is similar to `String#index` and returns the **lowest index** within the string at which the regex matched or `nil` if there is no match. It is faster than `match`.
	```ruby
	str = "hello world"

	str.match(/\bw.*?l\B/)		# => #<MatchData "worl">
	str =~ /\bw.*?l\B/          # => 6
	str.index(/\bw.*?l\B/)		# => 6
	```

- `String#scan` is a global form of `match` that returns an Array of all matching substrings
	```ruby
	str = "hello world"
	str.scan(/\b.*?l\B/)		# => ["hel", " worl"]
	```

### Ruby Regex Application - Splitting Strings
- `String#split` can both string delimiter or regex as arguments to split a string, returning an array containing the substrings as elements.
	```Ruby
	# use str to split for well formatted inputs
	record = "xyzzy\t3456\t334\tabc"
	fields = record.split("\t")
	# -> ['xyzzy', '3456', '334', 'abc']

	# use regex to split less well formatted inputs
	record = "xyzzy  3456  \t  334\t\t\tabc"
	fields = record.split(/\s+/)
	# -> ['xyzzy', '3456', '334', 'abc']
	```

- **Beware** of `*` or `?` quantifiers in single character regex e.g. `/:*/` and `/\t?/` as they can produce unexpected results when using `split`. Since the `*` quantifier matches 0 or more occurrences of a pattern, they can split a string on `""`
	```Ruby
	'abc:xyz'.split(/:*/)
	# -> ['a', 'b', 'c', 'x', 'y', 'z']
	```
	A six element array instead of the two element array you may have expected. This result occurs because the regex matches the gaps between each letter; zero occurrences of `:` occurs between each pair of characters.

### Ruby Regex Application - Capture Groups
- **Capture groups** uses parentheses `()` to demarcate subsegments of a regex and stores the captured matches in variables e.g. `\1, \2`. These variables can then be **used as backreferences in the same regex or replacement strings in `String#gsub`/`String#sub`** (see transformation section below)

**Example: Matching Quotes**
```Ruby
# Initial attempt lazy match single and double quotes but 
# also includes mixed quotes ('..." or "...') unintendedly
/['"].+?['"]/ 

# using two regex
if text.match(/".+?"/) || text.match(/'.+?'/)
	puts "Got a quoted string"
end

# using captured groups
# \1 is a backreference and ensure the closing quote matches 
# the opening one
/(['"]).+?\1/
```

- A regex can contain multiple capture groups, up till 9. They are numbered from left to right as groups `1, 2, ... 9` and the corresponding backreferences are `\1`, `\2`, ... `\9`

- Note: We may also explore using **named groups** and **named backreferences**

### Ruby Regex Application - Transformations in Ruby
- `String#sub` and `String#gsub` are used in Ruby to transform a string. `#sub` finds and replaces the **first instance** of a string that matches a regex while `gsub` does that for **every matched substring** in that string.
	```Ruby
	text = 'Four score and seven'
	vowelless = text.gsub(/[aeiou]/, '*')
	# -> 'F**r sc*r* *nd s*v*n'
	```

- Using **backreferences in replacement string**
	```Ruby
	text = %(We read "War of the Worlds".)
	puts text.sub(/(['"]).+\1/, '\1The Time Machine\1')
	# prints: We read "The Time Machine".
	```
	**Note:** if we use double quotes in replacement strings, we need to use double backlashes:
	```Ruby
	puts text.sub(/(['"]).+\1/, "\\1The Time Machine\\1")
	```
	Where possible, try to use single quotes to avoid **leaning toothpick syndrome**

[Back to top](#sections)

---

### Questions/Cues
Challenge: Write a regex that matches blueberry or blackberry, but **write berry precisely once**. Test it with these strings:

blueberry
blackberry
black berry
strawberry

There should be two matches.

Hint: you need both concatenation and alternation.

```Ruby
pattern = /(blue|black)berry/

puts "blueberry" if "blueberry".match(pattern)  # printed
puts "blackberry" if "blackberry".match(pattern) # printed
puts "black berry" if "black berry".match(pattern) #not printed
puts "strawberry" if "strawberry".match(pattern) #not printed
```


