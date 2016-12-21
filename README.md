# Ruby Learning


## Basics

### String interpolation

```ruby
"My favourite number is #{a}!"
```

### Symbols

A symbol is used to reference something like a string with no intend to print it to the screen or change it

```ruby
:name
:a_symbol
```

### nil

A way to express "nothing", check for nil:

```ruby
"Hello, World".nil?
```

*Treated as false, but not equivalent to false (nil != false)*

### Operations

```bash
15 / 4
=> 3
```

```bash
15.0 / 4
=> 3.75
```

## Variables

### Variable scope

Variable scope is defined by a *block*. A block is a piece of code following a method invocation, usually delimited by either curly braces **{}** or **do/end**.

*Not all do/end pairs imply a block*.

### Types of Variables

* **Constants**:
If declared within a class or module, available anywhere within the context of that class or module. Otherwise, available everywhere within the Ruby script.
```ruby
MY_CONSTANT = 'I am available throughout your app.'
```

* **Global variables**:
Available everywhere within the Ruby script.
```ruby
$var = 'I am also available throughout your app.'
```

* **Class variables**:
Available from the class definition and any sub-classes. Not available from anywhere outside.
```ruby
@@instances = 0
```

* **Instance variables**:
Available only within a specific object, across all methods in a class instance. Not available directly from class definitions.
```ruby
@var = 'I am available throughout the current instance of this class.'
```

* **Local variables**:
```ruby
var = 'I must be passed around to cross scope boundaries.'
```

### Pointers

```ruby
a = "hello"
b = a
a = "not hello"
```

**=** reassigned the variable *a* to a different address in memory.

```ruby
a = "hello"
b = a
a << " there"
```

**<<** mutated the caller by changing the actual address space in memory, thereby affecting all variables that point to that address space.

## Methods

### Default Parameters
```ruby
def say(words='hello')
  puts words + '.'
end

say()
say("hi")
```

### Optional Parentheses
```ruby
say("how are you")
say "I'm fine"
```

### Chaining Methods
Every method returns the evaluated result of the last line that is executed.

```ruby
def add_three(n)
  n + 3
end
add_three(5).times { puts 'this should print 8 times'}
```

## Blocks and Procs

**Blocks**

```ruby
def take_block(number, &block)
  block.call(number)
end

number = 42   
take_block(number) do |num|
  puts "Block being called in the method! #{num}"
end
```

**Procs** blocks wrapped in a proc object and stored in a variable

```ruby
def take_proc(proc)
  [1, 2, 3, 4, 5].each do |number|
    proc.call number
  end
end

proc = Proc.new do |number|
  puts "#{number}. Proc being called in the method!"
end

take_proc(proc)
```

## Flow Control

### Conditionals
```ruby
puts "Put in a number"
a = gets.chomp.to_i

if a == 3
  puts "a is 3"
elsif a == 4
  puts "a is 4"
else
  puts "a is neither 3, nor 4"
end
```

*1-line syntax:*
```ruby
if x == 3 then puts "x is 3" end

puts "x is 3" if x == 3

puts "x is NOT 3" unless x == 3
```

### Comparisons

Order of precedence:

1. <=, <, >, >= - Comparison

2. ==, != - Equality

3. && - Logical AND

4. || - Logical OR

### Case Statement
```ruby
num = 5

answer = case num
    when 5
        "a is 5"
    when 6
        "a is 6"
    else
        "a is neither 5, nor 6"
    end
```
```ruby
answer = case
    when num < 0
        puts "You can't enter a negative num!"
    when num <= 50
        puts "#{num} is between 0 and 50"
    when num <= 100
        puts "#{num} is between 51 and 100"
    else
        puts "#{num} is above 100"
    end
```
```ruby
answer = case num
    when 0..50
        puts "#{num} is between 0 and 50"
    when 51..100
        puts "#{num} is between 51 and 100"
    else
        if num < 0
            puts "You can't enter a negative num!"
        else
            puts "#{num} is above 100"
        end
    end    

puts answer
```

### True and False
Every expression evaluates to true when used in flow control, except for **false** and **nil**.

```ruby
a = 5
if a
  puts "how can this be true?"
else
  puts "it is not true"
end
```

### Regex
```ruby
"powerball" =~ /b/

/b/.match("powerball")
```

*Return 5, as the first match took place at the 5th index*

## Loops & Iterators

### Loops
* **loop** takes a block, which is denoted by **{ ... }** or **do ... end**

* **next** jump to the next loop iteration.

* **break** exit the loop immediately.

```ruby
i = 0
loop do
  i += 2
  puts i
  if i == 10
    break
  end
end
```

### While Loops
```ruby
while x >= 0
  puts x
  x -= 1
end
```

### Until Loops
```ruby
until x < 0
  puts x
  x -= 1
end
```

### Do/While Loops
```ruby
loop do
  puts "Do you want to do that again?"
  answer = gets.chomp
  if answer != 'Y'
    break
  end
end
```

### For Loops
```ruby
for i in 1..x do
  puts i
end
```

### Iterators
By convention;

Use the curly braces **{}** when everything can be contained in one line.
```ruby
names = ['Bob', 'Joe', 'Steve', 'Janice', 'Susan', 'Helen']

names.each { |name| puts name }
```

Use the words **do** and **end** when we are performing multi-line operations.
```ruby
names = ['Bob', 'Joe', 'Steve', 'Janice', 'Susan', 'Helen']
x = 1

names.each do |name|
  puts "#{x}. #{name}"
  x += 1
end
```

### Recursion
```ruby
def fibonacci(number)
  if number < 2
    number
  else
    fibonacci(number - 1) + fibonacci(number - 2)
  end
end

puts fibonacci(6)
```

## Arrays
An ordered list of elements that can be of any type (indexed lists).

### Modifying Arrays

* **push** add value at the end of an array

```ruby
array = [1, 'Bob', 4.33]
array.push("another string")
```

```bash
=> [1, "Bob", 4.33, "another string"]
```

* **pop** delete value at the end of an array

```ruby
array.pop
```

```bash
=> [1, "Bob", 4.33]
```

* **Shovel operator** push value to an array

```ruby
array << "another string"
```

```bash
=> [1, "Bob", 4.33, "another string"]
```

* **unshift** add value at the beginning of an array

* **delete_at** delete the value at a certain index from an array.

* **delete** delete all items that are equal to a certain value.

* **uniq** delete any duplicate values that exist, then return the result as a new array. With bang operator it becomes destructive.

### Iterating Over an Array

* **select**

```ruby
numbers.select { |number| number > 4 }
```

* **each_index**

```ruby
a.each_index { |idx| puts "This is index #{idx}" }
```

* **each_with_index**

```ruby
a.each_with_index { |val, idx| puts "#{idx+1}. #{val}" }
```

* **each** if given a block, *each* runs the code in the block once for each element in the collection and returns the collection it was invoked on. If no block is given, it returns an *Enumerator*.

* **map** when given a block, create and return a new array containing the values returned by the block. If no block is given, map returns an *Enumerator*.

### Common Array Methods

* **include?** checks to see if the argument given is included in the array.

* **flatten** take an array that contains nested arrays and create a one-dimensional array.

* **sort** order an array.

* **product** combine two arrays.

* **to_s** create a string representation of an array.

## Hashes
A data structure that stores items by associated keys.

The older syntax
```ruby
old_syntax_hash = {:name => 'bob'}
```

* String as key
```ruby
{"height" => "6 ft"}
```

* Array as key
```ruby
{["height"] => "6 ft"}
```

* Integer as key
```ruby
{1 => "one"}
```

* Float as key
```ruby
{45.324 => "forty-five point something"}
```

* Hash as key
```ruby
{{key: "key"} => "hash as a key"}
```

The newer syntax
```ruby
new_hash = {name: 'bob'}
```

Retrieve an element from an existing hash.
```ruby
person[:name]
```

* **delete** remove an element from an existing hash.
```ruby
person.delete(:name)
```

* **merge** merge two hashes together.
```ruby
person.merge!(new_hash)
```

*Notice that the bang operator (!) makes this change destructive*

# Iterating Over Hashes

```ruby
person = {name: 'bob', height: '6 ft', weight: '160 lbs', hair: 'brown'}

person.each do |key, value|
  puts "Bob's #{key} is #{value}"
end
```

# Hashes as Optional Parameters

```ruby
def greeting(name, options = {})
  if options.empty?
    puts "Hi, my name is #{name}"
  else
    puts "Hi, my name is #{name} and I'm #{options[:age]}" +
         " years old and I live in #{options[:city]}."
  end
end
```

```ruby
greeting("Bob")
greeting("Bob", {age: 62, city: "New York City"})
greeting("Bob", age: 62, city: "New York City")
```

*Curly braces, { }, are not required when a hash is the last argument*

### Common Hash Methods

* **has_key?** check if a hash contains a specific key.

* **has_value?** check if a hash contains a specific value.

* **select** return any key-value pairs that evaluate to true when ran through the passed block.
```ruby
immediate_family = family.select do |k, v|
  k == :sisters || k == :brothers
end
```

* **fetch** return the value for a given key if it exists. Specify an option for return if that key is not present
```ruby
name_and_age.fetch("Steve")
name_and_age.fetch("Larry", "Larry isn't in this hash")
```

* **to_a** return an array version of a hash.

* **keys** and **values** retrieve all the keys or all the values out of a hash in an array.
```ruby
name_and_age.keys
name_and_age.values
```

## Files

### Creating a file
```ruby
my_file = File.new("simple_file.txt", "w+")
my_file.close
```

### Opening Files

* r : read-only
* w : write-only (if the file exists, overwrites everything in the file).
* w+ : read and write (if the file exists, overwrites everything in the file).
* a+ : read and write (if file exists, starts at end of file. Otherwise creates a new file).

#### For Reading;

* Retrieve entire contents of the file.
```ruby
File.read("file_name")
```

* Read the entire file based on individual lines and returns those lines in an array.
```ruby
File.readlines("file_name")
```

#### For Writing;

Puts adds a line break to the end of strings, while write does not.

```ruby
File.open("simple_file.txt",  "a+") do |file|
    file.write "Writing to files in Ruby is simple."
end

File.readlines("simple_file.txt").each_with_index do |line, line_num|
    puts "#{line_num}: #{line}"
end
```

The file closes at the end of the block.
```ruby
File.open("simple_file.txt", "w") { |file| file.write("adding first line of text") }
```

Alternatively; open the file, write to it and finally close it.

```ruby
sample = File.open("simple_file.txt",  "w+")
sample.puts("another example of writing to a file.")
sample.close

File.read("simple_file.txt")
```

```ruby
File.open("simple_file.txt", "a+") do |file|
    file << "Here we are with a new line of text"
end

File.readlines("simple_file.txt").each do |line|
    puts line
end
```

### Deleting a file

```ruby
File.delete("dummy_file.txt")
```

### File classes

* **Dir** interface for manipulating directories and their contents
```ruby
d = Dir.new(".")
while file = d.read do
    puts "#{file} has extension .txt" if File.extname(file) == ".txt"
end
```

* **Pathname** class that exposes all of the methods of *File* and *Dir*
```ruby
pn = Pathname.new(".")
pn.entries.each { |f| puts "#{f} has extension .txt" if f.extname == ".txt" }
```

### Working with file formats

* **xml**

```ruby
require 'nokogiri'

slashdot_articles = []
File.open("slashdot.xml", "r") do |f|
  doc = Nokogiri::XML(f)
  slashdot_articles = doc.css('item').map { |i|
    {
      title: i.at_css('title').content,
      link: i.at_css('link').content,
      summary: i.at_css('description').content
    }
  }
end
```

* **json**

```ruby
require 'json'

feedzilla_articles =[]
File.open("feedzilla.json", "r") do |f|
  items = JSON.parse(f.read)
  feedzilla_articles= items['articles'].map { |a|
    {
      title: a['title'],
      link: a['url'],
      summary: a['summary']
    }
  }
end
```

* **csv**

```ruby
require 'csv'

CSV.open("article.csv", "wb") do |csv|
    sorted_articles.each { |a| csv << [ a[:title], a[:link], a[:summary] ]  }
end
```
* **xlsx**

```ruby
require 'axlsx'

pkg = Axlsx::Package.new
pkg.workbook.add_worksheet(:name => "Articles") do |sheet|
    sorted_articles.each { |a| sheet.add_row [a[:title], a[:link], a[:summary]] }
end
pkg.serialize("articles.xlsx")
```

## Exception Handling

**Block**

```ruby
begin
    answer = number / divisor
rescue ZeroDivisionError => e
    puts e.message
end
```

**Inline**

```ruby
zero.each { |element| puts element } rescue puts "Can't do that!"
```
