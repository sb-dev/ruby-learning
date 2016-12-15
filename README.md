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

* Constants:
```ruby
MY_CONSTANT = 'I am available throughout your app.'
```

* Global variables:
```ruby
$var = 'I am also available throughout your app.'
```

* Class variables:
```ruby
@@instances = 0
```

* Instance variables:
```ruby
@var = 'I am available throughout the current instance of this class.'
```

* Local variables:
```ruby
var = 'I must be passed around to cross scope boundaries.'
```

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

## Loops & Iterators

### Loops
**loop** takes a block, which is denoted by **{ ... }** or **do ... end**

**next** jump to the next loop iteration.

**break** exit the loop immediately.

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

```ruby
array = [1, 'Bob', 4.33]
array.push("another string")
```

```bash
=> [1, "Bob", 4.33, "another string"]
```

```ruby
array.pop
```

```bash
=> [1, "Bob", 4.33]
```

```ruby
array << "another string"
```

```bash
=> [1, "Bob", 4.33, "another string"]
```

**delete_at** delete the value at a certain index from an array.

**delete** delete all items that are equal to a certain value.
