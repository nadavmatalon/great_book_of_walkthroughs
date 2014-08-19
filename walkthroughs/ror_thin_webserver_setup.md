##Ruby On Rails 
#Thin Webserver Setup

__Written by:__ [Nadav](https://github.com/nadavmatalon)
(May 2014@[Makers Academy](http://www.makersacademy.com/))

###General Notes

* For a basic Ruby on Rails project setup please refer to the [New Project Setup](./ror_new_project_setup.md) 
in this folder.
* The following instructions have been written for projects using 
[Rails 4.0](http://rubyonrails.org/) or later.
* Text in ALL_CAPITALS_AND_UNDERSCORES indicated a __placeholder__ for your own text 
* If a file's location within the file system isn't specified explicitly, that file is 
in the root directory of the project.


###Why Switch to Thin Webserver?

By default, Rails 4 (and Heroku) use WEBrick as their webserver.

However, according to Heroku documentation:

```
While WEBrick should be fine for development, it was not designed to handle a high concurrent 
workload that a Ruby app must serve in production. A production web server should be used 
instead. ([source](https://devcenter.heroku.com/articles/ruby-default-web-server))
```

`Thin webserver` is a common alternative on Rails and Heroku.

Follow these steps to configure `Thin` as the server for your Rails app.


###Updating the Gemfile

First add the 'thin' gem into your `Gemfile`:

```ruby
# https://github.com/macournoyer/thin/
gem 'thin'
```

Also, if you plan on using `Capybara` for testing, make sure that the `capybara gem` is included in the gem file


###Running Bundler

Next, open the terminal and run:

```bash
$ bundle
```

###Running Bundler

In the root of your project directory (at the same level as the `Gemfile`), create a new
file called `Procfile`.

Notice that this file doesn't have a file type/extension defined, so in terminal 
you can create it just by typing: 

```bash
$ touch Procfile
```

Then add the following line to the Procfile and save it:

```ruby
	web: bundle exec thin start -p $PORT
```

###Configuring Capybara

If you're using `Capybara`, in `app/spec/rails_helper.rb`, add the following lines: 

```ruby		
Capybara.server do |app, port|
    require 'rack/handler/thin'
    Rack::Handler::Thin.run(app, Port: port)
end
```

###(Re)Starting the Rails Server

If the local Rails server is working, stop it with CTL+C

To activate the new thin server you can use the familiar:

```
$ bin/rails s
```

Alternatively, you can use this command:

```
$ thin start
```

This server will listen on `localhost:3000` just like the original `WEBbrick server`.

