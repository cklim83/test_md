## Collections

#### Summary
- Strings and Arrays are ordered, zero-index collections of characters and objects respectively while Hashes are collections of key-value pairs
<br>

- `String/Array/Hash#[]` will reference an element from the collection but return `nil` when index is out-of-bound or key is absent in hash
<br>

- `String/Array#fetch(index)` or `Hash.fetch(key)` are preferred to reference elements as they give explicit errors (`IndexError` or `KeyError`) when invalid inputs are given
<br>

- `String/Array#slice(start, length)` or syntactic sugar form `String/Array#[start, length]` returns a new String or Array containing its subset, even when length is 1
<br>

- Negative indices for Strings and Arrays start from the last element
<br>

- Conversions
	- `String#chars` converts String to Array of characters
	- `Array#join` converts Array to String
	- `Array#to_h` converts a nested array of key, value pairs to hash
	- `Hash#to_a` converts a hash to nested array of key, value pairs
<br>

- `[]=` mutates the String/Array/Hash
	- `String/Array/Hash[index/key] += value` is shorthand for String/Array/Hash[index/key] = String/Array/Hash[index/key] + value and combines reference and reassignment. It mutates the collection through element reassignment  
<br>

#### Element Reference (String)
- String are ordered, zero-indexed collection of characters
- We can reference single or multiple characters in a string

```ruby
str = 'abcdefghi'

str[2] # => "c"

str[2, 3] # => "cde"
str.slice(2, 3)
```
- str[2, 3] is actually calling [String#slice(start, length)](https://docs.ruby-lang.org/en/2.6.0/String.html#method-i-slice). This short form is Ruby's syntatic sugar. If given length is longer than the remaining substring, only the substring from start to end of string is returned
- Methods can be chained to further reference the returned substring
```ruby
str = 'abcdefghi'
str[2, 3][0] # => "c"
```
<br>

#### Element Reference (Array)
- Like Strings, Arrays are ordered, zero-indexed collections of any object
- Single or multiple elements can be referenced at any time

```ruby
arr = ['a', 'b', 'c', 'd', 'e', 'f', 'g']

arr[2] # => "c"

arr[2, 3] # => ['c', 'd', 'e']
```
- Similar to Strings, multiple array elements referencing is achieved using [Array#slice](https://docs.ruby-lang.org/en/2.6.0/Array.html#method-i-slice). Note that although `Array#slice` and `String#slice` behave similarly, they are different methods. One is for arrays and the other is for strings.

```ruby
arr = [1, 'two', :three, '4']
arr.slice(3, 1) # => ["4"]
arr.slice(3..3) # => ["4"]
arr.slice(3)    # => "4"
```
- Note: `Array.slice(start_index, length)` and `Array.slice(range)` will both return a **new array**, if it is only for one element while Array.slice(index) will return an **element**. 
<br>

#### Element Reference (Hash)
- Rather than integer based indexing, Hashes are associative collections storing key-value pairs
```ruby
hsh = { 'fruit' => 'apple', 'vegetable' => 'carrot' }

hsh['fruit']    # => "apple"
hsh['fruit'][0] # => "a"
```

- When initializing hashes, keys have to be **unique**. Else, the latter duplicate key will overwrite the earlier one. 
```ruby
hsh = { 'fruit' => 'apple', 'vegetable' => 'carrot', 'fruit' => 'pear' }

(irb):1: warning: key :fruit is duplicated and overwritten on line 1
=> {"fruit"=>"pear", "vegetable"=>"carrot"}
```

- `Hash#keys` and `Hash#values` will return all the keys and values in **arrays** respectively
```ruby
country_capitals = { uk: 'London', france: 'Paris', germany: 'Berlin' }
country_capitals.keys      # => [:uk, :france, :germany]
country_capitals.values    # => ["London", "Paris", "Berlin"]
country_capitals.values[0] # => "London"
```

- Note: Although both hash keys and values can be any object in Ruby, it is common practice to use symbols as the keys. Symbols in Ruby can be thought of as immutable strings. There's a number of advantages to using symbols for hash keys and are preferred.
<br>

#### Element Reference Gotchas (Out of Bound Indices)
- Strings and Arrays return **`nil`** when supplied indices are out-of-bound. While a nil return will indicate an invalid return for a string, it is unclear for an array since nil could be a valid element
```ruby
str = 'abcde'
arr = ['a', 'b', 'c', 'd', 'e']

str[2] # => "c"
arr[2] # => "c"

str[5] # => nil
arr[5] # => nil
```
```ruby
arr = [3, 'd', nil]
arr[2] # => nil
arr[3] # => nil
```

- A better way to get elements for an Array is to use **[`Array#fetch`]**(https://docs.ruby-lang.org/en/2.6.0/Array.html#method-i-fetch) since they will raise an **`IndexError`** exception if given index is **out of bound**
```ruby
arr = [3, 'd', nil]
arr.fetch(2) # => nil
arr.fetch(3) # => IndexError: index 3 outside of array bounds: -3...3
             #        from (irb):3:in `fetch'
             #        from (irb):3
             #        from /usr/bin/irb:11:in `<main>'
```

#### Element Reference Gotchas (Negative Indices)
![String Negative Indices](https://d1b1wr57ag5rdp.cloudfront.net/images/collections/string-negative-index-diagram.png)
![Array Negative Indices](https://d1b1wr57ag5rdp.cloudfront.net/images/collections/array-negative-index-diagram.png)
- Strings and Arrays can be referenced in reverse order using negative indices, starting from -1 pointing to the last element
- Similar out-of-bound behavior also applies for negative indices
```ruby
str = 'ghijk'
arr = ['g', 'h', 'i', 'j', 'k']

str[-6] # => nil
arr[-6] # => nil


arr.fetch(-6) # => IndexError: index -6 outside of array bounds: -5...5
              #        from (irb):2:in `fetch'
              #        from (irb):2
              #        from /usr/bin/irb:11:in `<main>'
```

#### Element Reference Gotchas (Invalid Hash Keys)
- `Hash` also has a `#fetch` method which can be useful when trying to disambiguate valid hash keys with a `nil` value from invalid hash keys.
```ruby
hsh = { :a => 1, 'b' => 'two', :c => nil }

hsh['b']       # => "two"
hsh[:c]        # => nil
hsh['c']       # => nil
hsh[:d]        # => nil

hsh.fetch(:c)  # => nil
hsh.fetch('c') # => KeyError: key not found: "c"
               #        from (irb):2:in `fetch'
               #        from (irb):2
               #        from /usr/bin/irb:11:in `<main>'
hsh.fetch(:d)  # => KeyError: key not found: :d
               #        from (irb):3:in `fetch'
               #        from (irb):3
               #        from /usr/bin/irb:11:in `<main>'
```


#### Conversions (Array, String, Hash)
- `String#chars` converts a string to an array
- `Array#join` merges elements in an array to a string
```ruby
str = 'Practice'

arr = str.chars # => ["P", "r", "a", "c", "t", "i", "c", "e"]

arr.join # => "Practice"
```

- Hash has a [`#to_a`](https://docs.ruby-lang.org/en/2.6.0/Hash.html#method-i-to_a) method, which returns an array
- `Array` has a [`#to_h`](https://docs.ruby-lang.org/en/2.6.0/Array.html#method-i-to_h) method to convert an array of inner key, value pair arrays to a hash

```ruby
hsh = { sky: "blue", grass: "green" }
hsh.to_a # => [[:sky, "blue"], [:grass, "green"]]


arr = [[:name, 'Joe'], [:age, 10], [:favorite_color, 'blue']]
arr.to_h # => { :name => "Joe", :age => 10, :favorite_color => "blue" }
```


#### Element Assignment (String)
- `String#[]=` is used to mutate elements within a string. This is a destructive method and will change the string permanently

```ruby
str = "joe's favorite color is blue"
str[0] = 'J'
str # => "Joe's favorite color is blue"
```

#### Element Assignment (Array)
- `Array#[]=` can be used to permanently mutate an array
```ruby
arr = [1, 2, 3, 4, 5]
arr[0] += 1 # => 2
arr         # => [2, 2, 3, 4, 5]
```
- arr[0] += 1 is a shorthand for arr[0] = arr[0] + 1 and combines array element reference i.e. arr[0] and array element assignment i.e. Array#[]= which is destructive to the array, not the element the array is pointing

#### Element Assignment (Hash)
- Similar to Strings and Arrays, hash element assignment is achieved using the destructive `Hash#[]=` method by providing a hash key and associated value

```ruby
hsh = { apple: 'Produce', carrot: 'Produce', pear: 'Produce', broccoli: 'Produce' }
hsh[:apple] = 'Fruit'
hsh # => { :apple => "Fruit", :carrot => "Produce", :pear => "Produce", :broccoli => "Produce" }
```


#### Tags
#reference, #assignment, #slice, #fetch