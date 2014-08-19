##Ruby On Rails 
#Adding Static Pages (Home, About, Contact, Help)

__Written by:__ [Nadav](https://github.com/nadavmatalon)
(May 2014@[Makers Academy](http://www.makersacademy.com/))


__Main Source:__ [Michael Hartl, Ruby on Rails Tutorial: Learn Rails by Example](http://www.railstutorial.org/book/)


###General Notes

* For a basic Ruby on Rails project setup please refer to the [New Project Setup](./ror_new_project_setup.md) 
in this folder.
* The following instructions have been written for projects using 
[Rails 4.0](http://rubyonrails.org/) or later.
* Text in ALL_CAPITALS_AND_UNDERSCORES indicated a __placeholder__ for your own text 
(see [Generating the Basic Rails Framework](#generating-the-basic-rails-framework) below)
* If a file's location within the file system isn't specified explicitly, that file is 
in the root director of the project.
* The following uses [Sublime Text](http://www.sublimetext.com/3) as the text editor of choice. However, you can use whichever
text editor you like.


###Creating the Controller

To create a controller for static pages (homepage, about, contact, help) without the 
default rspec tests (we'll create our own), run:

```
$ bin/rails generate controller StaticPages home about contact help --no-test-framework
```

Then, in `config/routes.rb` remove the following content:

```ruby
[name_of_project]::Application.routes.draw do
    get 'static_pages/home'
    get 'static_pages/about'
	get 'static_pages/contact'
	get 'static_pages/help'
    .
    .
    .
end
```

And replace it with the following content:

```ruby
root 'static_pages#home'
match '/home', to: 'static_pages#home', via: 'get'
match '/about', to: 'static_pages#about', via: 'get'
match '/contact', to: 'static_pages#contact', via: 'get'
match '/help', to: 'static_pages#help', via: 'get'
```

Switch to the terminal and run:

```bash
$ bin/rake routes
```

This comman should list all the currently available routes in your project.


Now, open a new terminal tab and run:

```bash
$ bin/rails server
```

Note that the server can run simultaneously with typing commands in terminal 
(although it will require e-starts every time we run a major command like 
changing the database or running bundle install)

Turn to your browser and enter the following Url's:

```
http://localhost:3000/				// this should show the "home" page

http://localhost:3000/home			// this should show the "home" page

http://localhost:3000/about			// this should show the "about" page

http://localhost:3000/contact		// this should show the "contact" page

http://localhost:3000/help			// this should show the "help" page
```


####Adding Dynamic Page Titles

If you want to create dynamic page titles, in `app/views/layouts/application.html.erb`, 
add the following line to the top of the `<head>` section:

```html
<title><%= full_title(yield(:page_title)) %></title>
```

In `app/views/layout/static_pages/home.html.erb`, add the following at the top:

```html
<% provide(:page_title, "Home") %>
```
		
In `app/views/layout/static_pages/about.html.erb`, add the following at the top:

```html
<% provide(:page_title, "About") %>
```

In `app/views/layout/static_pages/contact.html.erb`, add the following at the top:

```html
<% provide(:page_title, "Contact") %>
```

And in `app/views/layout/static_pages/help.html.erb`, add the following at the top:

```html
<% provide(:page_title, "Help") %>
```

Now, in `app/helpers/application_helper.rb`, add this new method inside `module ApplicationHelper`:

```ruby
def full_title page_title
    base_title = "NAME_OF_YOUR_APPLICATION"
    page_title.empty? ? base_title : "#{base_title} | #{page_title}"
end
```

####Controller Integration Testing

To generate initial integration tests for our `static_pages controller` follow these steps:

If the folder `spec/features` doesn't exist, create it.

Them inside that folder, create a new file `spec/features/static_pages_feature_spec.rb` 
and add the following content:

```ruby	
require 'rails_helper'

describe "Static pages: " do

    subject { page }

    context "\'root\' page" do

        before (:each) { visit root_path }

        it "should have the title \'Web Templates | Home\'" do
            should have_title "Web Template | Home"
        end

        it "should have the content \'Web Templates\'" do
            should have_content "WEB TEMPLATE"
        end
    end

	context "Home page" do

	    before (:each) { visit home_path }

        it "should have the title \'Web Template | Home\'" do
            should have_title "Web Template | Home"
            end

        it "should have the content 'Web Template'" do
            should have_content "WEB TEMPLATE"
        end
	end

    context "About Us page" do

        before (:each) { visit about_path }

        it "should have the title \'Web Template | About\'" do
           should have_title "Web Template | About"
        end

        it "should have the content \'About Us\'" do
            should have_content "About Us"
        end
    end

    context "Contact Us page" do

        before (:each) { visit contact_path }

        it "should have the title \'Web Template | Contact\'" do
            should have_title "Web Template | Contact"
        end

        it "should have the content \'Contact Us\'" do
            should have_content "Contact Us"
        end
    end

    context "Help page" do

        before (:each) { visit help_path }

        it "should have the content \'Web Template | Help\'" do
            should have_title "Web Template | Help"
        end

        it "should have the content \'Help\'" do
            should have_content "Help"
        end
    end
end
```

Note that if the tests fail because ‘visit’ is not recognized, you can try this:

In `spec/spec_helper.rb`, at the top of the file add:

```ruby
require 'capybara' 
```

And inside the config method add:

```ruby
RSpec.configure do |config|
    .
    .
    .
    config.include Capybara::DSL
    .
    .
    .
end
```

####Adding Links to Static Pages

To create links to the static pages, either in the `homepage` or in any other page, 
add the following line to the `app/views/*.html.erb` file of the relevant page:

```erb
<%= link_to "[“TEXT_OF_LINK]", [ROUTE]_path, class: "[CSS_CLASS_NAME/S]" %>
```

Note that you need to replace the capitalized text above with your own for the links to work.

Now, to To find the relevant [ROUTE] designators, run:

```bash
$ bin/rake routes
```

On the list, you can see all the relevant routes to the static pages (ending with *_path)

