Deploying Ruby on Rails with Heroku

Written by: Julie


Preparing your app --> Adding 'rails_12factor' gem
   
We need to add 'rails_12factor' gem into our Gemfile to make our app available on Heroku.

We also need to add the 'Heroku_secrets' gem to bypass the security risk of pushing our secrets to Github before pushing them to Heroku:

group :production do
    # https://github.com/heroku/rails_12factor  
    gem 'rails_12factor'
	gem 'heroku_secrets', github: 'alexpeattie/heroku_secrets'
end


After adding these gems to the Gemfile, run bundle:

$ bundle


Next, we need to take care of the secret keys in the "config/secrets.yml" :

First, generate a new 'hash' key with:

$ bin/rake secret

Change the secret key for the 'test' environment to this key

Then follow the same operation twice more, replacing the secret keys for the 'development' and 'production' environments


(optional) To prevent the "devise secret key" (not to be confused with the above keys) from being pushed to Github, follow these steps:

Impotant note: these instructions assume that you have added the "secrets.yml" file to the list of ignored files in .gitignore before your first push to Github.

In "config/initializers/devise.rb": 

uncomment the following line:

config.secret_key = ...      <=  here there will be a long 'hash' number

After uncommenting this line, remove the current 'hash' number and replace it with:

config.secret_key = Rails.application.secrets.devise_secret_key


Now generate a new secret key in terminal:

$ bin/rake secret

Copy the result from terminal and in "config/secrets.yml":

Add the following line to each of the three environemnt (test, developmentm, production):

devise_secret_key: **the_new_key**


You can now commit the changes to your remote Github repository:

$ git --add all .
$ git commit -a -m "Added rails_12factor and heroku_secrets gems and updated Gemfile.lock"


Create a Heroku application and set up remote:

$ heroku create 				<= use this command for deault app name provided by Heroku

alternatively:

$ heroku create name_of_app 	<= use this command to give the app your own name


Time to deploy your code!

$ git push heroku master


Next we should push all the secret keys directly to heroku:

$ bin/rake heroku:secrets RAILS_ENV=production


Next we need manually migrate our database by running:

$ heroku run rake db:migrate


When that command is finished type heroku open in the terminal to visit the page:

$ heroku open


Note: a useful way to debug (even if the build is said to be successfull but app 
doesn't work is to run:

	Terminal>	heroku run rails console




