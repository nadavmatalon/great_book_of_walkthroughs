##Ruby On Rails 
#Basic Form Creation

__Written by:__ [Will](https://github.com/painted)
(May 2014@[Makers Academy](http://www.makersacademy.com/))

###General Notes

* The following instructions have been written for projects using 
[Rails 4.0](http://rubyonrails.org/) or later.
* These instructions assume that you have a basic Ruby on Rails project setup 
with PostrgeSQL database (including the relevant model/s for the form you want to
create. For guidance on how to set up a basic Ruby on Rails project please refer to 
the [New Project Setup](./ror_new_project_setup.md) in this folder.
* Text in ALL_CAPITALS_AND_UNDERSCORES indicated a __placeholder__ for your own text 
* If a file's location within the file system isn't specified explicitly, that file is 
in the root directory of the project.


###Preparing the Database

All fields that are being used in the new form must already be included as columns 
in the table of the relevant model within the database. 

For our example, let's say we want to generate a form that allows users to create 
new `posts`. 

This requires that we have a `Post` model in our project and a corresponding table 
in the database called: `posts` which has the relevant columns (say, `id` `title` 
and `content`).

If you want to add an additional field to the table (that is, one which was not included 
when the model was created), you can run a `database migration` by using:

```bash
$ bin/rails generate migration AddColumnsToModelname fieldName:fieldType

$ bin/rake db:migrate
```

For our example, if the `posts` table only includes `id` and nothing else we can run the 
following to add the necessary columns:

```bash
$ bin/rails generate migration AddColumnsToPosts title:string content:text

$ bin/rake db:migrate 

$ bin/rake db:migrate RAILS_ENV=test    // This will make sure the 'test' database is updated as well
```

You can add more than one field at a time just by adding more FIELD_NAME:FIELD_TYPE 
with a space between them.

In the rest of this walkthrough, we'll use the `posts` example. Make the necessary 
changes in your own code.


###Writing a Test for the New Form

After the project's initial setup has been completed, create a new `spec` file called: 
`spec/features/posts_controller_feature_spec.rb`, and add the following test:

```ruby
describe 'form for creating Posts' do
 
    it 'can create a new post' do
        visit 'posts/new'
        fill_in 'Title', with: 'Test_Post'
        fill_in 'Content', with: 'Test_Content'
        click_button 'Create'
        expect(current_path).to eq posts_path
        expect(page).to have_content 'Test_Post'
        expect(page).to have_content 'Test_Content'
    end
end
```

Run the test to see it fail:

```bash
$> rspec
=> No route matches [GET] "/posts/new"
```

###Confirguring the Controller and the Routes

Create the file `app/controllers/posts_controller.rb`, 
and inside define a the new controller class and action:

```ruby
class PostsController < ApplicationController
	def new
		@post = Post.new
	end
end
```

And inside the `config/routes.rb` file make sure you have this line:

```ruby
Rails.application.routes.draw do

	resources :posts

end
```

Now run the test to see that the error message has changed:

```bash
$> rspec
=> Missing template posts/new
```

###Creating the View for the New Form

Create a new file (and folder if needed) called: `app/views/posts/new.html.erb`

Run the test to see that the error message has changed once again:

```bash
$> rspec
=> Unable to find field "title"
```

Go to the `new.html.erb` file and add:

```
<%= form_for [@post] do |f| %>
	<%= f.label :title %><br/>
	<%= f.text_field :title %><br/>
	<%= f.label :content %><br/>
	<%= f.text_area :content %><br/>
	<%= f.submit 'Create' %>
<% end %>
```

Note that there are many options for the FIELD_TYPE, for example: 

`check_box`, `color_field`, `date_field`, `datetime_field`, `datetime_local_field`, 
`email_field`, `file_field`, `hidden_field`, `month_field`, `week_field`, 
`number_field`, `password_field`, `phone_field`, `range_field`, `radio_button`, 
`search_field`, `telphone_field`, `text_area`, `text_field`, `time_field`, `url_field`. 


As before, run the test to see that the error message has changed:

```bash
$> rspec
=> The action 'create' could not be found for PostsController
```

###Going back to the Controller

In `app/controllers/posts_controller.rb`, define the missing `create` action and
'whitelinging' the params with a private method:

```ruby
	def create
		@post = Post.new post_params
		@post.save!
		redirect_to posts_path
	end

	private

	def post_params
		params[:post].permit(:id, :title, :content)
	end
```

The test fails again, this time because there is no `index` action:

```bash
$> rspec
=> The action 'index' could not be found for PostsController
```

So in `app/controllers/posts_controller.rb`, define the missing `index` action:

```ruby
	def index
		@posts = Post.all
	end
```

And as before, the test fails as there is a missing template for this action:

```bash
$> rspec
=> Missing template posts/index 
```

So let's create it.

Create a new file (and folder if needed) called: `app/views/posts/index.html.erb`,
and add the following content inside:

```
<h2>List of Posts</h2>
<% @posts.each do |post| %>
	<p>Post ID: <%= post.id %></p>
	<p>Post Title: <%= post.title %></p>
	<p>Post Content: <%= post.content %></p><br/>
<% end %>
```

The test should now pass:

```bash
$> rspec
=> 1 example, 0 failures
```

###Work with the New Form in the Browser

To see the form in the browser, run:

```bash
$ bin/rails server
```

In browser go to the page in which the form is located: 

```
localhost/3000/posts/new
```

Fill-in the form and click 'Create'.

This should redirect you to the `posts/index` page where you can see a list of all the 
posts you've created.


