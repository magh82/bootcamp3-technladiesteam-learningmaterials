# Techladies Bootcamp #3
#### thm journal

## Week 1 - 29 July 2017

1. What were some of the tasks that you do not feel comfortable executing by yourself?
  - set up and authentication
  - pushing code into cloud/ repo


2. Compare your app to the one you commonly use on the web. Rate your app on a scale of 1/10. Justify
your rating. What are some of the concepts/skills that you need to learn/improve on?
  - I rate my app 3/10
  - some confidence the method/way I structure my code - aim for code to be flexible, elegant and scalable
  - some confidence in basic UI design such pages are easy and intuitive to navigate
  - to learn how to use relative alignment/bootstrap for app to be desktop and friendly

## Week 2 - 05 August 2017

1. Write a route (/user/:id) for a get request to a UserController show action
 - get '/user/:id', to: 'user#show'


2. Describe the stages that code has to go through in order to become part of the Git history
 - to save files/folder in local machine
 - git init > to initialise git into a local repo in the local machine
 - git add . or git add -A > to add files from the local repo to the staging area
 - git commit -m "" > to commit the files/folders into the remote repo
 - git push > to push the files/folders into the remote repo


What view template file will be rendered by UsersController#index? State the filepath
 - app/view/users/index.html.erb

## Week 3 - 12 August 2017

1. If I have the following CSS code:
body.applicants.index {
/* some css code here*/
}
which controller and which action in that controller am I scoping the CSS within that selector to?
 - applicants#index


2. Ideally, we should try to group our CSS/SCSS files based on whether they are for views for a
particular controller or apply to a layout instead. Suppose I generate a PagesController with the
Rails generator; by default I will get something like this in my app/assets/stylesheets:
app/assets/stylesheets/
|____application.scss
|____pages.scss
The Rails generator default is telling you to put your styling for views handled by the pages
controller inside pages.scss. That's probably acceptable for quick and dirty projects, but for a
proper project we'd like to structure our stylesheets more nicely. So, to repeat again, we'd like to
separate the stylesheet that is meant to apply to the views handled by the PagesController, and the
stylesheet that is meant to apply to the app/views/layouts/application.html.erb application layout.
Please draw out a modified directory structure of app/assets/stylesheets to achieve this (hint:
you will need to add two new directories; and you will need to add one new scss file for the
stylesheet meant for app/views/layouts/application.html.erb)
 - app/assets/stylesheets/layouts/<applications>.scss
 - app/assets/stylesheets/controllers/<pages>.scss
 - app/assets/stylesheets/applications.scss (manifest file)


3. a) Name the rake command to create a database. b) Suppose you pull the latest code from master
and your teammate has added some new tables to the database schema. What new files did your teammate
add and where were they added (name the directory)? Which file would have been modified? Name the rake
command you should run to make sure those tables are added to the database on your machine as well.
 - a) rake db:create or bin/rake db:create
 - b) new files would have been added in db/migrate. db/schema.rb would have been modified to include
 details of the new database. To check that tables have been added to the database, run rake db:migrate:status. To ensure that tables have been added, run rake db:migrate


4. You have previously created a users table in your database with email and password columns. Write
a migration file for adding the following new columns: first_name, last_name, date_of_birth. (Hint:
which method should I use for adding columns? What should be the new column types?
 - bin/rails generate migration AddDetailstoUsers first_name:string last_name:string date_of_birth:string
 ```
 class User < ApplicationRecord
    def change
      add_column :users, :first_name, :string
      add_column :users, :last_name, :string
      add_column :users, :date_of_birth, :date
    end
   end
```

5. Again, say you have a users table in your database with email and password columns. a) Write a
model file to represent that database table (the User model). Where would that file be located? b)
Write the code that would create a new User object with only an email, without saving it to the
database. Now write the code that would create a new User object with only an email, but which would
also save it to the database.
```
class User < ApplicationRecord
end
```
 - app/models/user.rb (models > convention is singular)
 - User.new(email:"hmintoh@gmail.com") > create in memory but not save
 - User.create(email:"hmintoh@gmail.com") > effectively .new + .save in database


6. The fact that you can save a User without a password to the database is not good at all. To fix
this, you should use v_ (fill in the blanks). Please modify your User model code to ensure that you
can only save a User object to the database if it has both an email and a password. (If at any point
you are not sure about anything, I suggest you actually create the users table and User model in
your sandbox app so you can actually play around with this in the Rails console)
 - Model Validation
 - validates :name, presence: true
 - validates :password, presence: true


7. Write the code to fetch all Users from the database ordered by their email in descending order.
Write the code to fetch only User(s) from the database whose email is john@doe.com.
 - User.order(id: :desc)
 - User.where(email: "john@doe.com") (.find only returns the first hit in database, whereas .where returns an array with all matching records)


8. What does the t.timestamps method do inside a migration file? Why do we use it?
 - creates 2 columns: t.datetime "created_at" and t.datetime "updated_at"
 - timestamps is good for debugging database logs


9. Given the users table and User model, write a migration file for adding a posts table, with title
and content columns, as well as a column for referencing the user who wrote the post. Ensure that
that column has a foreign key constraint on it. (Hint: if you need a column to reference another
table, the method you need is t.ref____ (fill in the blanks)) Other than the three columns you
explicitly wrote in the migration, what is the other column that Rails automatically generates for you?

```
class CreatePosts < ActiveRecord::Migration
  def change
    create_table :posts do |t|
      t.string :title
      t.text :content
      t.references :user, foreign_key: true, index: true

      t.timestamps
    end
  end
end
```

10. Suppose I want to be able to do something like user.posts to fetch all posts associated with user, as well as something like post.user to fetch the author of post. Please add on the User model you've written so far and write a Post model to add these associations. What is the type of the
association from User to Post? What is the type of the association from Post to User?
 - in post model file, belongs_to :user
 - in user model file, has_one :post
BONUS: Now, suppose instead of using code like post.user, I want to be able to write post.author, because that reads better. How should I modify the line of code for creating the association in the Post model?
(hint: there are :class_name and foreign_key: options for the association method)

# Week 4 - 19 August 2017

1. The following is valid Ruby code, but please tell me what is, stylistically speaking, wrong with it:
```my_awesome_hash = { "awesome" => "bro", cool_dude: 999, chillax: "person", "super" => "callifragillistic" }
```
  - "cool dude" => "999" or cool dude: "999"
  - "chillax" => "person" or chillax: "person"
  - the former hash key is a string i.e. "cool dude" whereas the latter is a symbol i.e. cool dude. good practice is to use symbols


2. Look at the following Ruby class:
```
class Person
  def initialize(name, age, occupation)
    @name = name
    @age = age
    @occupation = occupation
  end
```
```
  def introduce
    puts "Hello, my name is #{@name}. I am #{age} years old and work as a #{@occupation}"
  end
end
```

List down all the instance variables and instance methods. Add an instance method to this class so I can do something like:
```
jun_qi = Person.new('Jun Qi', 22, 'almost-not-a-student')
puts jun_qi.age # => this should print out `22`
```
That is, write a getter method that will return the age of a Person. Please add it to the template below:
```
class Person
  def initialize(name, age, occupation)
    @name = name
    @age = age
    @occupation = occupation
  end
```
```
  def introduce
    puts "Hello, my name is #{@name}. I am #{@age} years old and work as a #{@occupation}"
  end

  def age
    @age
  end
end
```
(note: you can test out this code in any random old Ruby file and run it with the ruby command)

  - instance variables = @name, @age, @occupation
  - instance methods = initialize (), introduce

3. Please explain it like I'm 5: what is a session? (Hint: your ActionController Rails Guides reading!)
  - HTTP is stateless i.e. there is no way for the server to differentiate one user from another. When a client connects for the first time to a server it will generate a session ID, which will be given to the client as a cookie value. From now on this session id will identify the client connection, and is used to access session data stored in the server.
  - For each session ID, the server keeps a data structure known as sessions. A session stores small amounts of data specific to each user that persists between requests.
  - In the controller you can access the session data using the session instance method. Session values are stored in key/value pairs like a hash


4. What is the difference between adding a flash message with flash.now[:error] vs flash[:error]?
  - the flash is a special part of the session which is cleared with each request, thus it is useful for passing error messages as it will only be available in the next request
  - a flash message will appear in the resulting page and persist to the next action i.e. if we use it before we do anything but redirect, for instance before render, it would appear in the rendered page but is still waiting for a "redirect" request and so will appear again if you click a link.
  - a flash.now message will be displayed in the view you are rendering i.e. available in the same request


5. How would you a) write a routing spec to check that the route GET /bootcamps exists and routes
to the index action of BootcampsController? b) write a routing spec to check that the route
 DELETE /bootcamps/:id does NOT exist? Add your code into the template below:
```
require 'rails_helper'
RSpec.describe BootcampsController, type: :routing do
    it { expect(get: "/bootcamps").to route_to(controller: subject.contoller_path, action: "index") }
    end
end
```
