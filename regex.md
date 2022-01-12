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
[Basic Matching - Control Character Escapes](#basic-matching---control-character-escapes)\
[Basic Matching - Case Insensitive Match](#basic-matching---case-insensitive-match)\
[Character Classes - Set of Characters](#character-classes---set-of-characters)\
[Character Classes - Range of Characters](#character-classes---range-of-characters)\
[Character Classes - Negated Classes](#character-classes---negated-classes)\
[Character Class Shortcuts - Any Character](#character-class-shortcuts---any-character)\
[Character Class Shortcuts - Whitespace](#character-class-shortcuts---whitespace)\
[Character Class Shortcuts - Digits and Hex Digits](#character-class-shortcuts---digits-and-hex-digits)\
[Character Class Shortcuts - Word Characters](#character-class-shortcuts---word-characters)\
[Anchors - Start/End of Line](#anchors---start/end-of-line)\
[Anchors - Lines vs String](#anchors---lines-vs-string)\
[Anchors - Start/End of String](#anchors---start/end-of-string)\
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
|`/\s/`, `/[\s/]`| Whitespace character (space, tab, newline etc)|
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

---

### Basic Matching - Alphanumerics
- In Ruby, regular expressions are delimited by `/` in the format `/pattern/`.
- Most basic regex is to match a specific alphanumeric character. 
	- For example, `/s/` matches `s`, `sand`, `cats`, `cast` and even `Mississippi` but does not match `S` or `KANSAS` as regex are case sensitive by default.
- The `match` method accepts a regex argument to find a pattern match in a string. `match` returns a `MatchData` object (which is truthy) if the pattern in found in the caller, `nil` otherwise
	```Ruby
	str = "cast"
	print "matched 's'" if str.match(/s/)
	print "matched 'x'" if str.match(/x/)

	# => matched 's' printed
	```

### Basic Matching - Special Characters**
- `$ ^ * + ? . ( ) [ ] { } | \ /` are _meta-characters_ and have a special meaning in Ruby or JavaScript regex.

- To match a meta-character **literally**, we need to escape it using a leading backslash `\`. 
	```Ruby
	# using /\?/ to match "?" prefixed with to !! to conver to boolean

	!!"?".match(/\?/)               => true
	!!"What's up, doc?".match(/\?/) => true
	!!"Silence!".match(/\?/)        => false
	!!"What's that?".match(/\?/)    => true
	```

- Remaining characters, including colons `:` and spaces ` ` are not meta-characters and do not need to be escaped when inside a pattern. Note: `/ /` and `/[ ]/` are equivalent and both match to spaces.  
	```Ruby
	!!"chris:x:300".match(/:/)				 	=> true
	!!"A thought; no, forget it.".match(/ /) 	=> true
	!!"::::".match(/:/)						 	=> true	
	!!"meta-characters".match(/-/)				=> true
	```

### Basic Matching - Concatenation
- We can **concatenate two or more patterns into a new pattern** that matches each of the original pattern in sequence: `/cat/` is a concatenation of `c`, `a` and `t` patterns and matches any strings that contains `c` followed by an `a` followed by a `t`
	```Ruby
	!!"copycat".match(/cat/)	=> true
	!!"cast".match(/cat/)		=> false as "t" not after "ca"
	!!"CAT".match(/cat/)		=> false as different case
	!!"CAT".match(/cat/i)		=> true as i flag asked for case-insensitive match
	```

Note: It is easy to write an unreadable and unmaintainable mess. Use regex only when required and refactor them to reduce complexity.

### Basic Matching - Alternation
- We can construct a regex that matches one of severall sub-patterns by separating each pattern using pipe `|` and enclosing them using `()`
	```Ruby
	pattern = /(cat|dog|rabbit)/ #match "cat" or "dog" or "rabbit"

	!!"The lazy cat".match(pattern)                     => true
	!!"The dog barks".match(pattern)                    => true
	!!"The cat ran from the barking dog".match(pattern) => true
	!!"dives down the rabbit hole".match(pattern)       => true
	!!"catalog".match(pattern)                          => true
	!!"The Yellow Dog".match(pattern)                   => false
	```

```Ruby
# Example to show how to escape mata-characters `()` and `|`
pattern = /\(cat\|dog\|rabbit\)/ #match "(cat|dog|rabbit)"

!!"(cat|dog)".match(pattern)                => false
!!"bird(cat|dog)zebra".match(pattern)       => false
!!"cat".match(pattern)                      => false
!!"dog".match(pattern)                      => false
!!"dn(cat|dog|rabbit)!!!".match(pattern)    => true
```

### Basic Matching - Control Character Escapes
- Control character escapes such as `\n`,`\r` and `\t` representing newline, carriage returns and tabs can also be matched using regex
	```Ruby
	!!"\thello".match(/\t/) 				=> true
	!!"Goodbye!\n".match(/\n/)				=> true
	puts "\\hello" if "\\hello".match(/\\/)	=> outputs \hello
	```

- Not everything with `\` are control character escapes
	- `\s` and `\d` are shortcuts (covered below)
	- `\A` and `\z` are anchors (covered below)
	- `\x` and `\u` are special character code markers (we won't cover these)
	- `\y` and `\q` have no special meaning at all

### Basic Matching - Case Insensitive Match
We can make a regex pattern case insensitive by appending `i` after the `/` of a regex. These options are called **flags** or **modifiers** and are language specific.
```Ruby
!!"Hello".match(/hello/) 	=> false
!!"Hello".match(/hello/i) 	=> true
```

---

### Character Classes - Set of Characters
- Character class patterns use a list of characters between square brackets e.g. `/[abc]/` to match **single occurrences** of **any** characters between the brackets.

- Character class patterns can be used to validate user inputs since user need to choose from a set of valid inputs: e.g. `/[ynYN]/` for `y/n` prompt responses `/[12345]/` for options between 1 to 5

- It is also useful if case insensitive flag is not suitable e.g. `/[Hh]oover/` to match `Hoover` or `hoover`

- When writing character classes, it's good practice to group characters by type: digits, uppercase letters, lowercase letters, whitespace, and non-alphanumeric characters. You can arrange the groups in any order, though typically the non-alphanumerics come first or last in the character class. This practice aids readability.

- We can also concatenate character classes e.g. `/[abc][12]/` will match any of `a1`, `a2`, `b1`, `b2`, `c1` or `c2`.

- Of the full set of meta-characters, only a subset `^ \ - [ ]` retain their special meaning inside a character class. Hence `/[*+]/` will match any literal `*` or `+` without need for backslash. We can add backlash even when it is not required: `/[\*\+]/` is equivalent to `/[*+]/` but the latter is preferred for better readability.

- Among this subset, some are meta_characters in specific situations. For example, `^` is only a meta-character if it is the **first character** in the class i.e. `/[^...]/`, otherwise it is just a literal. `-` becomes a literal if it is the first character in the class.

### Character Classes - Range of Characters
- For consecutive sequence of characters e.g. letters `a` through `z`, we can abbreviate it using `-`. Thus `/[a-z]/` matches any lower case alphabetic character, `/[0-9]/` matches any digits. We can also combine ranges e.g. `/[0-9A-Fa-f]/` matches any alphanumeric. 

- Note: 
	- It is advised not to use character range for non-alphanumeric even though this can be done
	- Do not combine lowercase and uppercase alphabetic cases in a single range `/[A-z]/`. Use `/[A-Za-z]/` instead to prevent inclusion of other non-alphabetic characters such as brackets (`[,]`), caret (`^`) and underscore (`_`) as matched characters unintentionally.

### Character Classes - Negated Classes
- Range negation is to match characters other than those in the brackets e.g. `/[^aeiou]/` will match any character except lowercase vowel characters. The caret `^` has to be the first character in the `[]` for it to be a meta_character (negation).

---

### Character Class Shortcuts - Any Character
- `/./` is used to match any character, including spaces except newline. To also match newlines, we need to include the `m` (multiline) option

- Note: Even though `.` is a shortcut for character class, it should not be inside square bracket. A `.` inside square bracket becomes a literal

### Character Class Shortcuts - Whitespace
- `/\s/` matches all whitespace characters which includes a) space `' '`, b) tab `\t`, c) vertical tab `\v`, d) carriage return `\r`, e) line feed `\n` and f) form feed `\f`. Thus `/\s/` is equivalent to `/[ \t\v\r\n\f]/`
	```ruby
	p 'matched' if 'Four score'.match(/\s/) 	=> matched
	p 'matched' if "Four\tscore".match(/\s/) 	=> matched
	p 'matched' if "Four-score\n".match(/\s/) 	=> matched
	p 'matched' if "Four-score".match(/\s/) 	=> nil
	```

- `/\S/` matches all non-whitespace characters and is equivalent to `/[^ \t\v\r\n\f]/`
	```ruby
	p 'matched' if 'a b'.match(/\S/)			=> matched
	p 'matched' if " \t\n\r\f\v".match(/\S/)	=> nil
	```

- `/\s/` and `/\S/` can both be used outside or inside square brackets: 
	- Outside square brackets e.g. `/\s/`, the regex match one of the whitespace characters, 
	- Inside the square brackets, it represent an **OR** relationship to other members of the class e.g. `/[a-z\s]/` will match either a lowercase letter or any whitespace characters.

### Character Class Shortcuts - Digits and Hex Digits

| Shortcut | Meaning |
|---|---|
| `\d` | Any decimal digit /`[0-9]`/ |
| `\D` | Any character other than a decimal digit `/[^0-9]/` |
| `\h` | Any hexadecimal digit `[0-9A-Fa-f]` (**Ruby**) |
| `\H` | Any character other than a hexadecmial digit (**Ruby**) |

Similar to `\s` and `\S`, `\d` and `\D` can be used in and other of square brackets

### Character Class Shortcuts - Word Characters
- `/\w/` match any word characters and is equivalent to `/[0-9a-zA-Z_]/`

- `/\W/` matches any non-word characters `/[^0-9a-zA-Z_]/`

- Similarly `\w` and `\W` can be used in and other of square brackets.

---

### Anchors - Start/End of Line
- `^` to match start of **line**
- `$` to match end of **line**

| string | `/^cat/` | `/cat$/` | `/^cat$/` |
| --- | --- | --- | --- |
|cat| match | match | match |
|catastrophe| match | no match | no match |
|wildcat| no match | match | no match |
|\<cat\>| no match | no match | no match|
	
- They serve as anchors when NOT inside square brackets (`[]`) but will have different meaning if inside.

| Regex | Meaning|
| --- | --- |
|`/[^cat]/`| match any character except `c`, `a` or `t` |
|`/^cat/`| match any **line** that starts with `cat`|
|`/[cat$]/`| match any character with `c`, `a`, `t` or `$` |
|`/cat$/`| match any line ending with `cat` |


### Anchors - Lines vs String
- `^` and `$` anchors lines (`\n`), NOT strings.
	```Ruby
	TEXT1 = "red fish\nblue fish"
	p "matched red" if TEXT1.match(/^red/) 		=> matched red
	p "matched blue" if TEXT1.match(/^blue/)	=> matched blue
	```
	The matching with regex `/^blue/` confirms that it anchor lines and not strings since `blue` is the start of a new line but not that of a string

	```ruby
	TEXT2 = "red fish\nred shirt"
	p "matched fish" if TEXT2.match(/fish$/)	=> matched fish
	p "matched shirt" if TEXT2.match(/shirt$/)	=> matched shirt
	```
	In Ruby, each line **starts after** `\n` and end either with a `\n` or end of string.
	Even though the first line in the string ends with a `\n`, `fish` is still said to occur at the end of the line. `$` doesn't care if there is a `\n` character at the end

### Anchors - Start/End of String
- `\A` to match start of **string**
- `\z` or `\Z` to match end of **string**
- `\z` always matches at the end of a string, while `\Z` matches up to, but not including, a newline at the end of the string. As a rule, use `\z` until you determine that you need `\Z`

	```Ruby
	TEXT3 = "red fish\nblue fish"
	TEXT4 = "red fish\nred shirt"

	p "matched red" if TEXT3.match(/\Ared/)		=> matched red
	p "matched blue" if TEXT3.match(/\Ablue/)	=> nil
	p "matched fish" if TEXT4.match(/fish\z/)	=> nil
	p "matched shirt" if TEXT4.match(/shirt\z/)	=> matched shirt

	TEXT5 = "red fish\ngreen shirt\n"
	p "matched shirt" if TEXT5.match(/shirt\z/)     => nil
	p "matched shirt" if TEXT5.match(/shirt\n\z/)   => matched shirt
	p "matched shirt" if TEXT5.match(/shirt\Z/)     => matched shirt
	```

### Anchors - Word Boundaries
- `\b` to anchor regex to word (`\w` i.e. `[0-9A-Za-z_]`) boundaries
- `\B` to anchor regex to non-word (`\W` i.e. `[^0-9A-Za-z_]`) boundaries

- A word boundary occurs when:
	- between any pair of characters, one a word character and the other a non-word character
	- At the start of a string **IF** first character is a word character
	- At the end of a string **IF** the last character is a word character

- A non-word boundary occurs:
	- Between any pair of characters where both are word or non-word characters
	- At the start of a string if first character is NOT a word character
	- At the end of a string if the last character is NOT a word character

	```Ruby 
	Eat some food.
	```
	Here word boundaries occur **before** `E` `s` and `f` and **after** `t`, `e` and `d`. Non-word boundaries occur elsewhere such as between `o` and `m` in `some` and between `.` and end of the string.

	**Word boundary `\b` Example**
	```Ruby
	pattern = /\b\w\w\w\b/

	One fish,           # Matches One since we have'\AOne '
	Two fish,           # Matches Two since we have '\nTwo '
	Red fish,           # Matches Red since we have '\nRed '
	Blue fish.          # No match
	123 456 7890        # Matches 123 and 456
	```

	**Non-word boundary `\B` Example
	```Ruby
	pattern = /\Bjohn/i

	John Silver        # No match
	Randy Johnson      # No match
	Duke Pettijohn     # Match john since i\Bjohn
	Joe_Johnson        # Match John since _\BJohn
	```

	Note: `\b` and `\B` do not work as word boundaries inside of character classes (between square brackets). In fact, `\b` means something else entirely when inside square brackets: it matches a backspace character.

---

### Quantifiers - Zero or More
- `*` matches zero or more occurrence of the pattern to its left
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

- The quantifier always applies to a single pattern on its left. We can use grouping parentheses to define the pattern to apply the `*`. For example `/1(234)*5/` matches all below as `(234)` is treated as a single pattern by the regex engine to apply the `*` on.
	```Ruby
	15
	12345
	12342345
	12342342345
	```

- Because `*` can match zero occurrence, it match any string as any string has an empty string as its subset
	```Ruby
	"hello".match(/x*/) => <MatchData ""> 
	```

- Note: The regex `*` quantifier is different from the `*` wildcard used in command line even though they looked the same: 
	- `*` wildcard is like `/.*/`. Hence the wildcard `blue*doc` in command line matches any files whose name begins with `blue` and ends with `doc`. 
	- `/blue*doc/` regex meant the `*` is apply on a single pattern `e` and matches any sequence of characters starting with `blu` and ending with `doc` with 0 or more consecutive `e`s in between.

### Quantifiers - One or More
- `+` quantifier to match 1 or more occurrence of a pattern to its left
- To match at least 3 digits, we could use `/\b\d\d\d+\b/` instead of `/\b\d\d\d\d*\b/`

### Quantifiers - Zero or One
- `?` quantifier to match 0 or 1 occurrence of a pattern to its left
	```Ruby
	pattern = /coo?t/ 

	cot     # matches pattern
	coot    # matches pattern
	```

- `?` can be useful to match dates with may or may not include `-` separator characters.
	```Ruby
	pattern = /\b\d\d\d\d-?\d\d-?\d\d\b/

	20170111     # matches pattern
	2017-01-11   # matches pattern
	2017-0111    # matches pattern
	201701-11    # matches pattern
	```

- Since `?` can match zero occurrence `/h?/` can match all these strings: 
	```Ruby
	pattern = `/h?/
	`
	his     # matches 'h'
	is      # matches ""
	ish     # matches 'h'
	```

- Note: `?` wildcard in command line shells is different from `?` in regex
	- `?` wildcard in command line matches 0 or 1 occurrence of **any** character
	- `?` quantifier in regex match 0 or 1 occurrence of a pattern (which could be multiple characters e.g. `(abc)?`) to its left.

### Quantifiers - Ranges
- Allows us to precisely specify the number of occurrences. Range quantifiers contains a pair of curly braces `{}`
	- `p{m}` matches **precisely `m`** occurrences of pattern `p`
	- `p{m,}` matches **m or more** occurrences of `p`
	- `p{m,n}` matches **at least `m` and up till `n`** occurrences of pattern `p`

	```Ruby
	/\b\d{10}\b/                                 # matches exactly 10 consecutive digits
	2225551212 1234567890 123456789 12345678900


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
- These quantifiers are **greedy** in that they try to match the longest possible string they can. For example `/a[abc]*c/` matching `xabcbcbacy` will match `abcbcbac`, not `abc` or `abcbc`.

- `?` is added after the main quantifier if we want to match the **least characters possible**, called a **lazy match**. For example, `a[abc]*?c/` matches `abc` and `ac` in `xabcbcbacy`

---

### Ruby Regex Application - Matching Strings
- `String#match` is used in conditionals and return `MatchData` if a match is found or `nil` otherwise
- The `MatchData` isn't an Array but responds to element referencing syntax e.g. `[0]`, `[1]` etc
	```Ruby
	fetch_url(text) if text.match(/\Ahttps?:\/\/\S+\z/)
	```

- `=~` is similar to `match` but returns the **index** within the string at which the regex matched or `nil` if there is no match. It is faster than `match`.
	```ruby
	fetch_url(text) if text =~ /\Ahttps?:\/\/\S+\z/
	```

- `String#scan` is a global form of `match` that returns an Array of all matching substrings

### Ruby Regex Application - Splitting Strings
- `String#split` accepts both string delimiter and regex to split a string and return an array containing the substrings as elements.
	```Ruby
	# simple well formatted input string
	record = "xyzzy\t3456\t334\tabc"
	fields = record.split("\t")
	# -> ['xyzzy', '3456', '334', 'abc']

	# less well formatted input string
	record = "xyzzy  3456  \t  334\t\t\tabc"
	fields = record.split(/\s+/)
	# -> ['xyzzy', '3456', '334', 'abc']
	```

- Note: Beware of regex like `/:*/` and `/\t?/` when using `split`. Recall that the `*` quantifier matches zero or more occurrences of the pattern it is modifying. In the case of `split`, the result may be totally unexpected:
	```Ruby
	'abc:xyz'.split(/:*/)
	# -> ['a', 'b', 'c', 'x', 'y', 'z']
	```
	A six element array instead of the two element array you may have expected. This result occurs because the regex matches the gaps between each letter; zero occurrences of `:` occurs between each pair of characters.

### Ruby Regex Application - Capture Groups
- **Capture groups** capture the matching characters that correspond to part of a regex using parentheses `()`. You can **reuse these matches later in the same regex**, and when constructing new values based on the matched string.

- For example, we may try using match single or double quotes
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

- Multiple capture groups: A regex can contain multiple capture groups, numbered from left to right as groups `1,2, ... up till 9` and the corresponding backreferences are `\1`, `\2`, ... `\9`

- Note: We may also explore using **named groups** and **named backreferences**

### Ruby Regex Application - Transformations in Ruby
- `String#sub` and `String#gsub` are used in Ruby to transform a string. `#sub` transforms the **first instance** of a string that matches a regex while `gsub` transforms **every instance** of string that matches.
	```Ruby
	text = 'Four score and seven'
	vowelless = text.gsub(/[aeiou]/, '*')
	# -> 'F**r sc*r* *nd s*v*n'
	```

- Using backreferences in replacement string
	```Ruby
	text = %(We read "War of the Worlds".)
	puts text.sub(/(['"]).+\1/, '\1The Time Machine\1')
	# prints: We read "The Time Machine".
	```
	Note: if we use double quotes in replacement strings, we need to use double backlashes:
	```Ruby
	puts text.sub(/(['"]).+\1/, "\\1The Time Machine\\1")
	```
	Where possible, try to use single quotes to avoid **leaning toothpick syndrome**

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


