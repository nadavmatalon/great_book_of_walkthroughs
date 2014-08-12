
#Ruby On Rails: Setting Up a New Project with a PostgreSQL Database

__Written by:__ [Nadav](https://github.com/nadavmatalon) & [Will](https://github.com/painted)
(May 2014@[Makers Academy](http://www.makersacademy.com/))


__Main Source:__ [Michael Hartl, Ruby on Rails Tutorial: Learn Rails by Example](http://www.railstutorial.org/book/)


### General Notes

* These instructions have been written for projects using [Rails 4.0](http://rubyonrails.org/) 
or later.
* Text in ALL_CAPITALS_AND_UNDERSCORES indicated a __placeholder__ for your own text 
(see [Generating the Basic Rails Framework](#generating-the-basic-rails-framework) below)
* If a file's location within the file system isn't specified explicitly, that file is 
in the root director of the project.
* The following uses [Sublime Text](http://www.sublimetext.com/3) as the text editor of choice. However, you can use whichever
text editor you like.
* It is strongly recommended <strong>not to push the remote github repo</strong> before 
adding the `config/secrets.yml` file to the .gitignore list (See: [Tell Git what to Ignore](#tell-git-what-to-ignore) below).


### Creating a New Project

#### Preliminary Steps

It's always a good idea to check which versions of Ruby and Rails are currently installed   
(not to mention verify that they are indeed installed).

To do this, run:

```bash
$ ruby -v
=> ruby 2.1.1p76    (just an example of the output, yours might be different)
$ rails -v
=> Rails 4.1.2      (just an example of the output, yours might be different)
```

To update your installation of Rails:

```bash
$ gem update rails
```


#### Generating the Basic Rails Framework

To create a new directory with all the default Rails goodies, run:

```bash
$ rails new NAME_OF_YOUR_PROJECT -T -B -d postgresql
```

If you want to create defualt settings for __all__ your new Rails projects, do this:

```bash
$ cd ~
$ ls -a 	
```

In the list that comes up, check if there's a file called: `.railsrc`.

If there isn't, create it with:

```bash
$ touch .railsrc
```

And then - assuming you have Sublime Text installed - open it with:

```bash
$ subl .railsrc	
```

Next, put the following content into the `.railsrc` file:

```
-d postgresql		// sets the project's database to postresql
-T  				// doesn't include default testing units (to enable using Rspec)
-B  				// skips immidiate bundle install right after project creation
```

Save the file and close it.

From now on, you can simply use `$ rails new NAME_OF_YOUR_PROJECT` and it will have all 
the above settings in place: 	


#### Setting up the Gemfile

First, go into your project's directory and open it in Sublime Text:

```bash
$ cd NAME_OF_YOUR_PROJECT
$ subl .
```

Next, open the `Gemfile` (in the root directory of your project) and add the following content:

Btw, if you have time, go over the README's of these gems (see links above each 
gem below) and verify current versions and updated installation instructions.

```ruby
ruby '2.1.1'   	# update this line according to your own Ruby version

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
    gem 'rspec-collection_matchers'
end

group :development do
    #  https://github.com/rails/spring
    gem 'spring'
end

group :production do
    # https://github.com/heroku/rails_12factor	
    gem 'rails_12factor'	 #	not necessary for now (but will be needed for deployment on Heroku)
end
```

__Important:__ make sure to remove any __duplications__ (e.g. `sdocs`, `spring`) from your `Gemfile`.


Save the file and run:

```bash
$ bundle install
```

If Bundler complains about a 'readline' error, try adding the gem 'rb-readline'
to the Gemfile and re-run `bundle install`.

An alternative set of `bundle install` commands, includes:

```bash
$ bundle install --without production

$ bundle update

$ bundle install
```

When using this alternative set of commands, the gems under the __production group__ in 
the `Gemfile` will only be installed during deployment (e.g. in Heroku). That is to say, 
gems in this group will not be installed locally. However, note that this kind of selective 
install must be done on the __first__ bundle install command otherwise Bundler won't limit the 
'production' gems installation. Also note that after the first time you use the 
`$ bundle install --without production` command, you can use just use  `$ bundle install` 
as usual and the 'production' gems will not be installed until deployment to the 'production' 
environment.


#### Setting up Rspec

To set up [Rspec](http://rspec.info/) for testing your app, run:

```bash
$ bin/rails generate rspec:install
```

A useful alternative shorthand for Rails `generate` coammand is simply the letter `g`. 
For example, the above command could be run as follows:

```bash
$ bin/rails g rspec:install
```

If you want to update your Rspec settings to show better looking results in terminal
and to include the `spec/spec_helper.rb` file automatically (rather tha having to `require`
it in every individual `_spec` file, open the `.rspec` file, and change it's content to:

```
--color
--format documentation
--require spec_helper
```

Save the file and close it.

The next step is to add the following line at the top of `spec/helpers/rspec_helper.rb`:

```ruby
require 'rspec/collection_matchers'
```

Save that file too and close it.

Now, make sure rspec is running correctly:

```bash
$ rspec

```

You should see Rspec running with no tests except the pending ones created automatically by Rails
(these pending tests will show in yellow).



#### Modifying the README Extention

By defualt, Rails generate a README file with an `.rdoc` extension type.

If you want, you can rename this file to `README.md`


#### Removing Turbo-Links

Rummor has it that Turbo-Links doesn't play nice with other elements in the Rails framework 
and therefore it's sometimes recommended to remove it completely if it's not specifically needed.

To do this, in `app/assets/javascripts/application.js`, remove the third element from the 
top ("turbo-links") and leave only the following:

```js
//= require jquery
//= require jquery_ujs
//= require_tree .
```

And in `app/views/layouts/application.html.erb` remove the line referencing 
to __turbo-links__ from the `<head>` section.


#### Telling Git what to Ignore

Update the `.gitignore` file to include:

```yml
# Ignore the secrets files.
/config/secrets.yml               #this is important for security reasons (see details below)

# Ignore other unneeded files.
doc/
*.swp
*~
.project
.DS_Store
.idea
.secret
```

After implementing this step, it's a good idea to keep a backup of your `secrets.yml` file 
(e.g. on [Google Docs](https://docs.google.com/)) and to update that backup after any change.


#### Enabling SCSS

You can enable SCSS (an advanced HTML styling language built on top of CSS), by adding an
`.scss` extension type to the following file: `app/assets/stylesheets/application.css`

The end-result should look like this: `app/assets/stylesheets/application.css.scss`

Notice that the `.scss` extension comes __after__ the `.css` entension 
and __does not__ replace it.


#### Configuring the PostgreSQL Database

In `config/database.yml`, add the following line immidiately after the `pool: 5` line:

```yml
host: localhost
```

Then run:

```bash
$ bin/rake db:create
```

On some machines, the `bin/rake db:create` command will only create a __development database__. 
However, because our tests will need a __test` database__ we may need to create it manually 
if it wasn't create automatically by the previous command.

To check if the __test database__ already exists run:

```bash
$ psql
```

Note that the `command promot` should change to indicate that you've entered the __psql 
environment__.

If it has, run:

```
$# \list
```

The last command should give you a list of all the PostgreSQL databases on your machine.

First, make sure that `NAME_OF_YOUR_PROJECT_development` database is listed.

Then check if `NAME_OF_YOUR_PROJECT_test` is also on the list (if not, don't worry - we'll
create it in a sec).

To exit psql mode run:

```
$#	\q
```

If the __test database__ for your project wasn't on the list, create it by running:

```bash
$ bin/rake db:create RAILS_ENV=test
```

#### Creating a Local Github Repo

To set up a local Github repo run:

```bash
$ git init
$ git add .
$ git status
```

Next, go over the list given by Git in response to the last command and make sure 
that `config/secrets.yml` __is not__ on it.

If all is clear run:

```bash			
$ git commit -m "initial commit"
```

#### Creating a Remote Github Repo

After setting up a new __remote repo__ on [Github](http://github.com/), link it with 
your __local repo__ and with:

```bash
$ git remote add origin REMOTE_REPO_SSH_OR_HTML_URL
$ git push -u origin master	
```

#### Updating the Gitignore File

If you want, you can  update the `.gitignore` file at this point to include:

```yml
# Ignore the secrets files.
/config/database.yml
```

The idea is to have this file on your __remote repo__ in its original form so that 
other developers can to use it if they fork your repo, but at the same time to keep 
all the changes you might make in this file locally.

That said, if you do decide to use this option, make sure to keep a backup of your 
`database.yml` file (e.g. on [Google Docs](https://docs.google.com/)).


#### Using the Rails Server

To see your project in action in the browser, run:

```bash
$ bin/rails server
```

As with other Rails commands, you can also use the shorthand version: `$ bin/rails s`

With the server running, open your browser and type in the following url:

```
http://localhost:3000
```

Note that the Rails server will be linked by default to your __development database__ and
therefore anything that takes place in the browser side (e.g. registering new users to your app)
will involve updating that database.


#### Using the Rails Console

To test code snippets and check the content of the development database, you can use 
the Rails console:

```bash
$ bin/rails console
```

Alternatively, to test code and work with the `development` database without affecting 
its content run:

```bash
$ bin/rails console --sandbox
```

Another option is to test code and see the content of `test` database by running:

```bash
$ bin/rails console test
```

It's worth noting that the __Rails console__ can also serve as great tool for debugging. 
If you run into problems when trying to run the __Rails server__ due to some obscure bug 
in your code, try running the __Rails console__ instead. I won't work either, but chances 
are you'll get a more useful description of the problem.  


#### Adding a CSS RESET File

If you want to add a `reset.css` file so as to reset the default CSS styling of 
different browsers, follow these steps.

First, create a new file: `app/vendor/assets/stylesheets/reset.css`

(Note that the new file ought to be located in the `vendors` folder)

Next, copy the content for this file from: [CSS Tools: Reset CSS](http://meyerweb.com/eric/tools/css/reset/)

Alternatively use this updated version: [Reset Revisited](http://meyerweb.com/eric/thoughts/2011/01/03/reset-revisited/)

(You can, of course, use any other provider you like for the `reset.css` file content)


After that's done, in `app/assets/stylesheets/application.css.scss`, add the following 
line __above__ the other two existing lines as follows:

```css
*= require reset
*= require_tree .
*= require_self
```

Note that if the `*=require reset` line is not located __above__ all the other 'require' lines, 
it __will mess up__ your css styling and browser rendering.


Another tip for working with HTML/CSS is this:

To make sure webpages are fully loaded before JavaScript functionality becomes available, 
in `app/assets/javascripts/FILE_NAME.js` files, use:

```javascript
$(document).ready(function() {
    [your js code goes here]
});
```


#### Adding an HTML5 Shim

To help tackle redering problems in IE browsers older than IE9, you can create a new file called 
`app/views/layouts/_shim.html.erb` and add the following content (known as: “HTML5 shim”) 
inside:

```html
<!--[if lt IE 9]>
<script src="http://html5shim.googlecode.com/svn/trunk/html5.js"></script>
<![endif]-->
```

(__Source:__ [html5shim](https://code.google.com/p/html5shim/))

Next Add the following line in `app/views/layouts/application.html.erb` right at the end of 
the `<head>` section:

```ruby
<%= render 'layouts/shim' %>
```

Note that this is the `<head>` section and __not__ the `<header>` section of the `_shim.html` file.



#### Creating an Awesome Project

Well done! You're good to go :-)

