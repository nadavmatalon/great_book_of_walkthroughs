Deploying Ruby on Rails with Heroku


Preparing your app --> Adding rails_12factor
   
   We need to add rails_12factor into our Gemfile to make our app available on Heroku.

   group :production do
	gem ‘rails_12factor’
end


Run bundle then commit the changes to your repository:


$ bundle
$ git --add all .
$ git commit -a -m "Added rails_12factor gem and updated Gemfile.lock"


Add Heroku-secrets gem to automate the process of storing our secrets in environment variables:

group :production do 
	gem 'heroku_secrets', github: 'alexpeattie/heroku_secrets'


Run bundle:

$ bundle

$ git add .
$ git commit -m 'Add heroku secrets'



Create a Heroku application and set up remote:

$ heroku create 


Time to deploy your code!

$ git push heroku master


Next we need manually migrate our database by running:

$ heroku run rake db:migrate


When that command is finished type heroku open in the terminal to visit the page:

$ heroku open


Tests should say 'Missing 'secret_base-key' for production environment.

Set this value in config/secrets.yml

--> generate a new secret key to replace existing one

$ bin/rake secret


Set up devise secret key in devise initialiser (devise.rb)

---> generate another secret key

$ bin/rake secret

---> activate the secret key by replacing existing one.




	






















