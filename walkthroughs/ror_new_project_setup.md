
#Ruby On Rails: Setting Up a New Project

Written by: Nadav & Will

Main Source:  http://www.railstutorial.org/book/   
(note that sometimes you need to refersh a couple of times before the chapters of the 
book are loaded properly)

## General Notes

* Text in square brackets must be removed completely (including the brackets) and 
replaced with the user's choice as appropriate (see example in step 1).

* If a file's location isn't specified explicitly, that file is in the root director 
of the project.

* You are strongly advised __not to push the remote github repo__ before adding the 
"secrets.yml" to the .gitignore list__ (See: step 10)


## Setting up a new Ruby on Rails project with PostgreSQL databases

(optional) Check currently installed versions of Ruby and Rails:

$ ruby -v

$ rails -v

To update installation of Rails:

$ gem update rails


$ rails new [name_of_project_lower_case] -T -B -d postgresql

Alternatively, it is possible to create defualt settings for all new rails
projects as follows:

$ cd ~

$ ls -a 	

Check if there's a file called: ".railsrc", and if not:

create it with:

$ touch .railsrc

and open it with:

$ subl .railsrc	

Put the following content into the ".railsrc" file:

-d postgresql		// sets the project's database to postresql
-T  				// doesn't include default testing units (to enable using Rspec)
-B  				// skips immidiate bundle install right after project creation


Save the ".railsrc" file and close it.

From now on it's possible to simply use: 	

rails new [name_of_project_lower_case]


$ cd [name_of_project_lower_case]

$ subl .

Update the "Gemfile" file to include:

(in tandem, go over the README of each Gem in the documentation - links above each 
gem below - and verify current versions and updated installation instructions)

```ruby
ruby '2.1.1'   	# update this according to currently used Ruby version

group :doc do
# https://github.com/voloko/sdoc
gem 'sdoc', '~> 0.4.0'
end

group :test do
# https://github.com/vertis/selenium-webdriver
# gem 'selenium-webdriver'  # not installed for now
end

group :test, :development do
# https://github.com/rspec/rspec-rails
gem 'rspec-rails'
# https://github.com/jnicklas/capybara
gem 'capybara'
# https://github.com/cldwalker/debugger
gem 'debugger'
# https://github.com/copiousfreetime/launchy
gem 'launchy'
# https://github.com/rweng/pry-rails
gem 'pry-rails'
# https://github.com/rspec/rspec-collection_matchers
# gem 'rspec-collection_matchers'
end

group :development do
#  https://github.com/rails/spring
gem 'spring'
end

group :production do
# https://github.com/heroku/rails_12factor	
# gem 'rails_12factor'	 #	not necessary for now (will only be needed for deployment)
end
```

(Important: Make sure to remove Duplications, e.g. 'sdocs', 'spring')

After saving the file, run:

$ bundle install

(If the Bundler complains about a 'readline' error, try adding the gem 'rb-readline'
to the Gemfile and do a bundle install)	

(optional) bundle install commands:

$ bundle install --without production

$ bundle update

$ bundle install

If using this alternative set of commands, the gems under the'production' group in 
the Gemfile will be installed only by Heroku during deployment (in other words, 
gems in this group will not be installed locally - note that this kind of install 
must be done on the FIRST bundle install command otherwise won't limit the 'production' 
gem installation. Also, note that after the first time, you can use just 'buntle install' 
as usual and 'production' gems will not be installed).

$ bin/rails generate rspec:install

Alternative shorthand:

$ bin/rails g rspec:install


Also, check if the "spec/features" folder exists and if not - create it manually.	

(optional) Update the “.rspec” file to have the following content:

--color
--format documentation
--require spec_helper

(optional) To install “rspec-collection_matchers” gem (extends syntax for rspec testing):

Uncomment this line in the "Gemfile":

gem 'rspec-collection_matchers'


Add the following line at the top of “spec/helpers/rspec_helper.rb”:

require 'rspec/collection_matchers'

And now time to bundle again:

$ bundle install


(optional) Rename the “README.rdoc” file to “README.md”

In “/app/assets/javascripts/application.js”, remove the third element from the 
top ("turbo-links") and leave only the following three:

```
//= require jquery
//= require jquery_ujs
//= require_tree .
```

And in "app/views/layouts/application.html.erb":

remove the references to ‘turbo links’ from the `<head>` section

Update the “.gitignore” file to include:

```ruby
# Ignore the secrets files.
/config/secrets.yml

# Ignore other unneeded files.
doc/
*.swp
*~
.project
.DS_Store
.idea
.secret
```

(optional) Add “.scss” to the following file: "app/assets/stylesheets/application.css”

(end-result: "application.css.scss")


Make sure rspec is running correctly:

$ rspec


In "config/database.yml", add the following line immidiately after the "pool: 5" line:

host: localhost


$ bin/rake db:create


(optional - but important!) 	

Sometimes the previous command (bin/rake db:create) will only create a 'development' database.

To check if the 'test' database already exists:

$ psql

$# \list

check if '[project_name_lower_case]_test' is already on the list and exit psql mode:

$#	\q

If not, to create the 'test' database (needed for Rspec testing and possibly other things):

$ bin/rake db:create RAILS_ENV=test


Set up a local Github repo:

$ git init

$ git add .

$ git status

go over the list and make sure "config/secrets.yml" is NOT on it
			
$ git commit -m "initial commit"


Create a Github remote repo and then link them together:

$ git remote add origin [Github_ssh_address_of_repo]

$ git push -u origin master	


(optional) Update the “.gitignore” file to include:
(the idea is to have this file in the remote Github repo for other developers to use, 
but to keep all the changes you might make locally)

# Ignore the secrets files.
/config/database.yml


(optional) To test code and check the content of the development database:

$ bin/rails console

To test code and work with 'development' database without affecting its contents:

$ bin/rails console --sandbox

To test code and see content of 'test' database:

$ bin/rails console test


(optional) To add a "reset.css" file so as to reset the default css styling of browsers:

Create a new file: “app/vendor/assets/stylesheets/reset.css”
(note that the new file should be located in the 'vendors' folder)

Copy the content for this file from: http://meyerweb.com/eric/tools/css/reset/  (or any other provider you prefer)

(updated content for the file: http://meyerweb.com/eric/thoughts/2011/01/03/reset-revisited/)

In "app/assets/stylesheets/application.css.scss", add the following line ABOVE the other two existing lines as follows:

```ruby	
*= require reset
*= require_tree .
*= require_self
```

(if the '*=require reset' line is not located above all the other 'require' lines, 
it __WILL MESS UP__ your css styling and browser rendering)


(optional) To make sure webpages load fully before functionality becomes available, 
in “app/assets/javascripts” files, use:

```javascript
$(document).ready(function() {
[js code]
});
```

(optional) HTML5 Shim

To help tackle redering problems in browser older than IE9:

Create a new file: “app/views/layouts/_shim.html.erb” 
Add the following content (known as: “HTML5 shim”):

```html
<!--[if lt IE 9]>
<script src="http://html5shim.googlecode.com/svn/trunk/html5.js"></script>
<![endif]-->
```

(Source: https://code.google.com/p/html5shim/)

Add the following line in “app/views/layouts/application.html.erb” 
at the end of the `<head>` section:

note that this is the `<head>` section and not the `<header>` section of the _shim.html file.

<%= render 'layouts/shim' %>


And you're good to go :-)

