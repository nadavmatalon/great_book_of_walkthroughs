
###Ruby On Rails: Setting Thin Webserver

By default, Rails 4 (and Heroku) use WEBrick as the webserver.

However, according to Heroku documentation:

While WEBrick should be fine for development, it was not designed to handle a high concurrent workload that a Ruby app must serve in production. A production web server should be used instead.

(https://devcenter.heroku.com/articles/ruby-default-web-server)

Thin webserver is a common alternative on Rails and Heroku.

Thin webserver setup:

(1) Add the 'thin' gem to the Gemfile:

		# https://github.com/macournoyer/thin/
		gem 'thin'

	(also, make sure the 'capybara' gem is included in the gem file)


(2)	Terminal>	bundle


(3)	Create a new file in the main root of the app (same level as the Gemfile" and name it: "Procfile" (this file doesn't have a file type/extension defined, so in terminal you can just type: touch Procfile)


(4) Add the following line to the Procfile and save it:

	web: bundle exec thin start -p $PORT


(5) In "spec/rails_helper.rb" add the following lines 
		
		Capybara.server do |app, port|
  			require 'rack/handler/thin'
  			Rack::Handler::Thin.run(app, :Port => port)
		end


(6) If the local Rails server is working, stop it with CTL+C

	The activate the new thin server with this command:

	Terminal>	thin start

	This server will also listen on localhost:3000 (just like the original WEBbrick server).


