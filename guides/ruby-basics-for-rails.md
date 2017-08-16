# Ruby Basics for Rails

Behold: some basics of Ruby that you should be familiar with so that dealing with Rails (or any Ruby project for that matter) won't feel so mysterious. Don't expect to grok everything from reading this short guide, but this should give you a good overview so you'll be able to take note of these Ruby features when you see them while working on your Rails project or on any code katas.

<!-- vim-markdown-toc GFM -->
* [Hash syntax and Symbols](#hash-syntax-and-symbols)
  * [Symbols vs Strings](#symbols-vs-strings)
  * [Okay, back to hashes](#okay-back-to-hashes)
* [Functions](#functions)
* [Blocks](#blocks)
* [Class-based Object Oriented Programming (OOP)](#class-based-object-oriented-programming-oop)
  * [Method Visibility](#method-visibility)
* [String Interpolation and `inspect`](#string-interpolation-and-inspect)

<!-- vim-markdown-toc -->

## Hash syntax and Symbols
Let's talk about two common data structures you'll meet in Ruby: the hash and the symbol.

The hash is used for storing key-value pairs. Other languages may also refer to it as a 'map' or 'dictionary' (if you've heard those terms before). So maybe for storing something like information in a phonebook, where a person's name is the key, and the value is the person's phone number. We can actually write the hash literal two ways:

```ruby
# the {} braces denote a hash literal
# Here we use strings as our keys (as well as values)
phonebook = {
  'Daniel' => '01234567',
  'Jun Qi' => '12345678',
  'Wei-Liang' => '23456789',
}

# Here we use symbols as our keys (the values are still strings though)
phonebook = {
  Daniel: '01234567',
  'Jun-Qi': '12345678',
  'Wei-Liang': '23456789',
}
```

Notice the difference in syntax. In the first case we used *strings* as the keys of our hash, and string literals are denoted with the single-quotes or double-quotes, as in for eg. `'Jun Qi'`. In the second case, we used **symbols** as the keys of our hash, and symbol literals are denoted with the colon in front, possibly combined with single quotes (or double quotes) if Ruby can't parse the symbol name normally. Try this out in `irb` (note: when you try out lines 2, 5, and 6, you may have to add a `)` after you press `Enter` the first time, or `irb` will keep waiting to evaluate a full statement):

```ruby
irb> :Daniel
irb> :Jun-Qi
irb> :"Jun-Qi"
irb> :underscores_are_fine
irb> :spaces are not
irb> :neither-are-hyphens
irb> :"so you need to use double-quotes"
```
If the symbol name is valid, `irb` will just return its value to you straightaway (so for eg., if you typed `:Daniel`, `irb` will parrot `:Daniel` back to you)

Now I must detract a little to talk a little bit more about symbols.

### Symbols vs Strings
What's the difference between symbols and strings? The key thing is that strings are *mutable*, meaning they can change over their lifetime. Consider this:

```ruby
x = "HELLO"
x.downcase!
puts x
```

This short script should print `hello` to screen. The thing to note here is that the string `"HELLO"` that was originally stored in `x` was mutated by `downcase!`. The value of that same string, in that same memory address, changed (as opposed to a new string with a new value having been created). This is what *mutability* means. Strings are mutable, which is why methods like `downcase!` can exist. You won't see such methods used on symbols, however, because symbols are *immutable*. If you do something like:

```ruby
x = :hello
y = :hello
```

The variables `x` and `y` point to the *same symbol* `:hello` in memory, not two copies of `:hello`. Whereas if you do:

```ruby
x = "hello"
y = "hello"
```
The variables `x` and `y` point to *different strings* in different places in memory, even if they have the same value `"hello"`.

Ok, so

> Strings are *mutable* while symbols are *immutable*

So *why use symbols instead of strings*? It comes down to that difference: because symbols are immutable, they are more efficient to reference, as efficient as integers in fact. And so they make 'better' hash keys, better in the sense that we can index a symbol key in the hash more quickly than string keys. So you will see hashes with symbol keys much more commmonly than string keys.

### Okay, back to hashes
So recall the phonebook example where we could write the hash with either string keys or symbol keys:

```ruby
# string keys
phonebook = {
  "Daniel" => "01234567",
  'Jun Qi' => "12345678",
  "Wei-Liang" => "23456789",
}

# symbol keys
phonebook = {
  Daniel: "01234567",
  "Jun-Qi": "12345678",
  "Wei-Liang": "23456789",
}
```

Notice previously when we were playing around in `irb` we typed `:Daniel` and not `Daniel:` (the colon is in front, not at the back). When we use symbol keys in a hash, however, we can put the colon at the back, instead of  using the clunky `=>` (called the rocket-hash syntax).

Here are some other examples of hashes with symbol keys:

```ruby
colors = {
  red: '0xFF0000',
  green: '0x00FF00',
  blue: '0x0000FF'
}

grades = {
  A: 95,
  B: 85,
  C: 75
}
```
## Functions
Functions are like a bunch of statements grouped together that can be referenced later on by a name:

```ruby
def greet(name)
  puts "Hello #{name}!"
end
```

By defining this function `greet`, I can *call* this function in other places without having to duplicate all the code inside it - in other words I am able to reuse this logic with minimal duplication.

Controller actions are just functions that you define. The statements you write in the `config/routes.rb` file are just calling some functions provided by Rails (such as `get` or `resources`). Model validations and associations are also function calls. The point of this section is to get you familiar with the Ruby function calls as you will see them in Rails (and elsewhere) look like. Or, in other words, to get you to understand the function call syntax.

For the `greet` function we defined just now, we can call it simply like this:

```ruby
greet('Jun Qi') # prints Hello Jun Qi to screen
```

Here we pass in the string argument `'Jun Qi'` in the parenthesis. But, in Ruby, unlike in some other languages (eg. Java, Python, C), the parentheses are entirely optional -- the following is equally valid:

```ruby
greet 'Jun Qi'
```

You will in fact very often see function calls without the parentheses. For example, to specify the root route in your `config/routes.rb` file, you would do something like this:

```ruby
root 'pages#home'
```

You pass the string argument `'pages#home'` to the `root` method (which, if you are curious, is defined in the `ActionDispatch::Routing::Mapper::Resources` module in Rails).

But of course, you will also see routes like this:

```ruby
get '/contact', to: 'pages#contact'
```

The `to: 'pages#contact'` part should seem familiar to you from the previous section: it is just a hash. In fact, it may seem clearer to you what exactly is going on if I write the line like this instead:

```ruby
get('/contact', {to: 'pages#contact'})
```

Essentially, we are passing two arguments to the `get` argument, the first a string `'/contact'` and the second a hash `{to: 'pages#contact'}`. That's all there is to it. It's just that this way of writing things feels a little too cluttered and un-Ruby-like, so we drop the parentheses to get the nicer and cleaner:

```ruby
get '/contact', to: 'pages#contact'
```
You will see this pattern of a function call pretty much everywhere. Hashes are used very commonly as arguments to functions, as a so-called *options hash*. Meaning, each key of the hash can be used to specify some option that can change the behaviour of the function. Here are some other common function calls you will see in Rails:

```ruby
# model association
belongs_to :author, class_name: 'User'

# model validation
validates :content, presence: true

# before action hook in a controller
before_action :find_post, only: [:show, :new, :edit]

# specifying the layout for a controller
layout 'admin'

# an RSpec describe block
describe 'validations' do
  # your specs here...
end

# link tag in your HTML ERB template
<%= link_to 'Contact Us', contact_us_path %>
# In this case we are calling two functions
# 1) the `link_to` helper
# 2) the `contact_us_path` helper whose value we pass in (the url path string) as the second argument of `link_to`
```

and so on and so forth. Hopefully all these lines of code no longer look so mysterious. Except maybe the RSpec one - that there uses a *block*. We'll take a look at blocks next.

## Blocks
Blocks are sorta like functions: a bunch of statements grouped together, BUT - they don't have a name, so you can't call a block again like you can call a function.

We pass blocks to functions for the function to execute those bunch of statements (in addition to the bunch of statements that make up the function itself). For example, a very common thing you want to do is iterate over an array. For that, you use the `each` method (a method is basically a function; more on this later):

```ruby
coaches = ['Daniel', 'Jun Qi', 'Wei-Liang']

coaches.each do |coach|
  puts "Your coach is #{coach}!"
end
```

Everything within the `do` and `end` keywords is part of the block that is passed to the `each` method. There is actually an alternate syntax that does the exact same thing:

```ruby
coaches.each { |coach| puts "Your coach is #{coach}!" }
```

In essence, instead of the `do...end` thing, we enclose the block in curly braces `{}`. With curly braces, the code in our block can only be on one line. If you need several lines of code in your block, use the `do...end` syntax.

Notice that blocks, like functions, can take arguments. In the example above the block to `each` takes an argument (which I named `coach`) which can then be referenced inside the block. In the case of `each`, the argument passed to the block is the element in the array currently being accessed.

You will see blocks everywhere in Ruby and Rails as well. In fact, if you've noticed, your `config/routes.rb` file is actually just a giant block:

```ruby
Rails.application.routes.draw do
  # ...your routes in this block...
end
```

And so is your `db/schema.rb`:

```ruby
ActiveRecord::Schema.define(version: 0) do

  # These are extensions that must be enabled in order to support this database
  enable_extension "plpgsql"

  # ...rest of the schema...

end
```

And so is your application environment config (any of the config files in `config/environments`):

```ruby

Rails.application.configure do
  # ...config here...
end
```

And so are your RSpec specs:

```ruby
require 'rails_helper'

RSpec.describe User, type: :model do

  describe 'validations' do
    it { expect(subject).to validate_presence_of(:name) }
  end

  describe 'associations' do
    it { expect(subject).to have_many(:posts) }
  end

end
```

## Class-based Object Oriented Programming (OOP)
This is a gargantuan topic. I will only talk about it enough so that you can understand what is going on in a Rails project (where OOP is everywhere, as in most Ruby projects).

OOP is a paradigm for modelling our problem and structuring our code. The central concept is the *object*, which contains 1) attributes, and 2) methods. Think of attributes as properties, sort of like adjectives which describe the object, while methods are verbs, things that the object can *do*. The example I always use is a dog: a dog has properties like fur color, weight, age and so on, while it can do things like bark and eat. Here is how I would specify a dog object, by writing a Dog *class*:

```ruby
class Dog
  def initialize(fur_color, age, weight)
    @fur_color = fur_color
    @age = age
    @weight = weight
  end

  def bark
    puts "Woof woof!"
  end

  def eat(amount)
    @weight += amount
  end
end
```

There's a fair bit going on here. Let's talk about the big `class Dog...end` thing first. This syntax is the way you define a class in Ruby. Note that class names must always be *camel-cased* (eg. like `Dog`, or `ApplicantsController`). Classes are categories, the 'general specification' of an object. By creating this `Dog` class, I'm specifying that all dogs are going to have fur color, age, weight, and can bark as well as eat. Defining the class doesn't create any *instances* of the `Dog` object, however. To do so, we would need to use the `new` method:

```ruby
# Create a new instance of the `Dog` class
# and assign it to a variable named `doge`
doge = Dog.new('yellow', 5, 30)

# call the `bark` method on `doge`
doge.bark # should print Woof woof!
```

Does `new` look familiar? Well, that's exactly how you create a new model in your Rails console. And hey, Rails models are basically classes too, aren't they:

```ruby
class User < ApplicationRecord
end
```

So hopefully you understand now the distinction between a *class* and an *instance* of a class. Let's look at our `Dog` class again:

```ruby
class Dog
  def initialize(fur_color, age, weight)
    @fur_color = fur_color
    @age = age
    @weight = weight
  end

  def bark
    puts "Woof woof!"
  end

  def eat(amount)
    @weight += amount
  end
end
```

What's inside the class body? It's actually a series of function definitions. Or, you will also hear people calling them *method* definitions. What is a method exactly? *A method is a function*, but it is specifically *a function associated with, or bound to, an object*. So, a method is a specific kind of function. Not all functions are methods (it's a subset and superset thing)

> A method is a function associated with/bound to an object

The way you call normal functions is different from the way you call a method. Namely, with methods, you use something called the *dot syntax*, like how we called the `bark` method of `doge`:

```ruby
doge.bark
```

Compare this to how we would call a normal function like `greet` which you saw in the previous section:

```ruby
# define a normal function
# ie., not inside a class body
def greet(name)
  puts "Hello #{name}!"
end

# calling a normal function - no dot syntax here
greet('Jun Qi')
```

So all the functions that you see defined inside the `class...end` part, ie. the class body, are methods. These are methods *bound to instances of that class*. So, the `eat` and `bark` methods are bound to instances of `Dog`, such as `doge`. Thus you also hear people refering to them as **instance methods**.

> Methods bound to instances of a class are called **instance methods**

Ok, back to the `Dog` class again:

```ruby
class Dog
  def initialize(fur_color, age, weight)
    @fur_color = fur_color
    @age = age
    @weight = weight
  end

  def bark
    puts "Woof woof!"
  end

  def eat(amount)
    @weight += amount
  end
end
```

We've defined 3 methods here: `initialize`, `bark`, and `eat`. The first method, `initialize`, is a very special method. It is termed the **constructor**, becuase this method tells Ruby how instances of our class should be constructed (or initialized, or instantiated; you will hear these terms being used interchangeably). You will never call the `initialize` method the same you call other normal methods like `bark` and `eat`:

```ruby
# DON'T do this; it will result in an error
doge.initialize

# normal methods are fine
doge.bark
```

When is `initialize` being used then? Basically when you create a new instance of the class, ie. when you call the `new` method! Recall:

```ruby
doge = Dog.new('yellow', 5, 30)
```

Notice how the arguments we pass to `new` actually correspond to the arguments passed to `initialize`. If you don't pass the correct number of arguments (eg. you specified that `initialize` should take 3 arguments but you only passed 2 arguments to `new`), an error will be thrown. Just like any other function, if you call the function with the wrong number of arguments, Ruby will complain:

```ruby
# try this and see what happens
doge = Dog.new('yellow')
```

> `initialize` is a special function called the **constructor**. It is called everytime you create a new instance of a class with `new`

Now let's look more carefully at what's in `initialize` itself:

```ruby
def initialize(fur_color, age, weight)
  @fur_color = fur_color
  @age = age
  @weight = weight
end
```

Notice the funny-looking variables with the `@` sign in front. You should be familiar with that from writing your controller actions, but maybe you didn't really understand what the whole `@` thing is about. Well, the `@` naming is a special way to tell Ruby that this variable here is an **instance variables**. Just like how instance methods are functions bound to an instance of an object, instance variables are variables bound to an instance of an object. So, every instance of `Dog` will have it's own fur color, age, and weight. These variables are specific to that instance.

And you see that we are setting the instance variables in `initialize`, through those 3 assignment statements. So a new instance of `Dog` will be created with a fur color that corresponds to the first argument passed in to `new`:

```ruby
doge = Dog.new('yellow', 30, 10)
# doge's @fur_color variable will store the value 'yellow'
# doge's @age variable will store the value 30
# doge's @weight variable will store the value 10
```

> Instance variables are named with a `@` in front. We typically initialize them in the `initialize` method (but not always! We can assign them in other instance methods too)

Why do we want to store instance variables? What's the difference between instance variables and arguments that are passed into the `initialize` function? That is, what is the difference between `@fur_color` and `fur_color`? Well, the key thing is that, `fur_color` only exists within the `initialize` function - *there is no way to reference it anywhere outside the function*. Whereas, instance variables are persisted on the instance object itself, so we can actually *reference instance variables inside other instance methods*, like how we are referencing `@weight` inside `eat`:

```ruby
def eat(amount)
  @weight += amount
end
```

Notice how we seem to be able to reference `@weight` even though with have never defined it anywhere before in the `eat` function. That's because the `@weight` variable was defined and assigned in the `initialize` function and our `Dog` instance carries it around forever after. If you tried to write the `eat` function with a normal old variable `weight` you wouldn't be able to get it to work:

```ruby
# if you defined it like this,
# and tried to call the method with something like
# `dog.eat(0.5)`
# Ruby will throw an error
def eat(amount)
  weight += amount
end
```

> Instance variables can be referenced in any instance method

So hopefully you understand instance variables a little better now, and you understand what those funny `@` things you were writing in your controller meant. Now another thing, which is Rails-specific, that you have the understand about your instance variables in your controller, is that, part of the magic which is Rails actually makes your controller instance variables available as instance variables in your view.

In my controller file:
```ruby
class BootcampsController < ApplicationController

  def index
    @bootcamps = Bootcamp.all
  end

end
```

Now in my index view:
```html
<% @bootcamps.each do |bootcamp| %>
  <%= bootcamp.name %>
<% end %>
```

> Controller instance variables defined in an action are made available to the corresponding view in Rails


### Method Visibility
When we define methods on our class, we are specifying an interface for people to work with objects of that class. We are telling them, "Hey, this is how you interact with my `Dog` instance - you can tell it `bark`, or to `eat` some amount of food". And so people using your class will call these methods on instances of your class:

```ruby
doge.bark
doge.eat(10)
```

This is all fine and dandy, but sometimes, you want to define methods that not any old person can use. Sometimes, you want to define methods that you only want to use *within the class itself*. You want to restrict the **visibility** of your method, to within the `class...end` part, the class body, alone. To do this, you would define a **private method**:

```ruby
class Dog
  def initialize(fur_color, age, weight)
    @fur_color = fur_color
    @age = age
    @weight = weight
  end

  def bark
    puts "Woof woof!"
  end

  def eat(amount)
    # we can call our private method inside any other instance method
    digest_food(amount)
    @weight += amount
  end

  # any methods you define below a `private`
  # statement will be private methods
  private

  def digest_food(amount)
    puts "Churn churn churning #{amount} kilos of food"
  end
end
```

Here I've defined a `digest_food` private method. Because it is private, I *cannot* do:

```ruby
doge.digest_food(10)
```

You can, however, call this private method from within any other instance method in your class. In the example above, I actually called the `digest_food` private instance method from the `eat` instance method.

Where do you see this in Rails? When you write the private method for processing the `params` coming into your controller:

```ruby
class BootcampsController < ApplicationController
  def create
    # call the `bootcamp_params` private method from
    # within my `create` action, which is an instance method
    # of the `BootcampsController` class
    @bootcamp = Bootcamp.new(bootcamp_params)
    if @bootcamp.save
      # do something
    else
      # do something else
    end
  end

  private

  def bootcamp_params
    params.require(:bootcamp).permit(:name, :start_date)
  end
end
```

Why do we make `bootcamp_params` private? Because, if you think about it, this shouldn't be used anywhere outside of my `BootcampsController` class.

Yay, now you should have a better understanding about what that `private` stuff is all about.

## String Interpolation and `inspect`

By now you should have seen something like this around alot:

```ruby
x = 10
puts "some string with an interpolated value #{x}"
```

The `"#{some_variable}"` syntax is basically a way to do **string interpolation**, to construct a string with some parts of the string coming from a value stored in a variable. It should be pretty self-evident how this can be useful.

One thing to take note of is that, *string interpolation only works if you use **double-quotes** around your string*.

> One big difference between single-quoted strings and double-quoted strings is that only double-quoted strings allow you to do **string interpolation**

Another thing, you may have come across a method called `inspect`:

```ruby
puts params.inspect
```

`inspect` basically constructs a more detailed string representation of whatever object you are calling it on. This is particularly useful for debugging. Now try this, in any old controller that you have, say in the show action:

```ruby
# for example, in the BootcampsController
class BootcampsController < ApplicationController

  def show
    puts params
  end

end
```

Make a request to the `show` action of the `BootcampsController` (eg. by visiting the route `/bootcamps/1` in your browser), and check your server logs. Try to find the line where it is printing the `params`. You may see something like this:

```ruby
#<ActionController::Parameters:0x00000004f34c48>
```

All this tells us is that `params` is actually an instance of a class called `ActionController::Parameters` at a certain memory address specified by that long string of numbers (remember, we call it a params hash, but it really isn't an ordinary hash. It's actually an instance of this class). But we can't actually see what's inside the params hash. What keys are there? This is where `inspect` comes in handy. Now change the `puts` statement to add in `inspect`:

```ruby
# for example, in the BootcampsController
class BootcampsController < ApplicationController

  def show
    puts params.inspect
  end

end
```

Make a request to that action again, and check your server logs. You should see a line that looks something like:

```ruby
<ActionController::Parameters {"controller"=>"bootcamps", "action"=>"sh
ow", "id"=>"1"} permitted: false>
```

Much better. Now you can actually see what's inside `params`.
