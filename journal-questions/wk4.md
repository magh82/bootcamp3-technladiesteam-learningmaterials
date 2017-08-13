# Journal Questions
1. The following is valid Ruby code, but please tell me what is, stylistically speaking, wrong with it:

```ruby
my_awesome_hash = { "awesome" => "bro", cool_dude: 999, chillax: "person", "super" => "callifragillistic" }
```

2. Look at the following Ruby class:
```ruby
class Person
  def initialize(name, age, occupation)
    @name = name
    @age = age
    @occupation = occupation
  end

  def introduce
    puts "Hello, my name is #{@name}. I am #{age} years old and work as a #{@occupation}"
  end
end
```

List down all the *instance variables* and *instance methods*. Add an instance method to this class so I can do something like:

```ruby
jun_qi = Person.new('Jun Qi', 22, 'almost-not-a-student')
puts jun_qi.age # => this should print out `22`
```

That is, write a *getter* method that will return the age of a `Person`. Please add it to the template below:

```ruby
class Person
  def initialize(name, age, occupation)
    @name = name
    @age = age
    @occupation = occupation
  end

  def introduce
    puts "Hello, my name is #{@name}. I am #{age} years old and work as a #{@occupation}"
  end

  # add your new method here
end
```

(note: you can test out this code in any random old Ruby file and run it with the `ruby` command)

3. Please explain it like I'm 5: what is a *session*? (Hint: your ActionController Rails Guides reading!)

4. What is the difference between adding a flash message with `flash.now[:error]` vs `flash[:error]`?

5. How would you a) write a routing spec to check that the route `GET /bootcamps` exists and routes to the `index` action of `BootcampsController`? b) write a routing spec to check that the route `DELETE /bootcamps/:id` does NOT exist? Add your code into the template below:

```ruby
require 'rails_helper'

RSpec.describe BootcampsController, type: :routing do
  # add your code here
end
```
