##Ruby On Rails 
#Deploying a Ruby on Rails Project with Heroku

__Written by:__ [Julie](https://github.com/julieannwalker)
(May 2014@[Makers Academy](http://www.makersacademy.com/))


###General Notes

* For a basic Ruby on Rails project setup please refer to the [New Project Setup](./ror_new_project_setup.md) 
in this folder.
* The following instructions have been written for projects using 
[Rails 4.0](http://rubyonrails.org/) or later.
* Text in ALL_CAPITALS_AND_UNDERSCORES indicated a __placeholder__ for your own text 
* If a file's location within the file system isn't specified explicitly, that file is 
in the root directory of the project.


### Updating the Gemfile
   
We need to add the `rails_12factor gem` into our Gemfile to make our app available on Heroku.

We also need to add the `Heroku_secrets gem` to bypass the security risk of pushing our 
secrets to Github before pushing them to Heroku.

In your `Gemfile`, add the following content:

```ruby
group :production do
    # https://github.com/heroku/rails_12factor  
    gem 'rails_12factor'
	gem 'heroku_secrets', github: 'alexpeattie/heroku_secrets'
end
```

After adding these gems to the Gemfile, run `Bundler` in terminal:

```bash
$ bundle install
```

###Configuring the App's Secret Keys

Next, we need to take care of the secret keys in `config/secrets.yml`.

First, generate a new `hash` key in terminal with:

```bash
$ bin/rake secret
```

This should generate a long string of random characters similar to this one:

```
8efab2631f8bea27502ce2fabbd86a1a0cb06e3cc39d06cb8bbb80f34b9bf9cd2b0d13be9396cab13c62b3b5239bdda6e82b8f9e494fba1c5fc3f687c1c6d2ba
```

Copy this key and paste it in `config/secrets.yml` instead of the exiting one 
for the `test` environment.

Then follow the same procedure twice more, replacing the secret keys for the `development` 
and `production` environments.

__Impotant note:__ these instructions assume that you have added the `secrets.yml` 
file to the list of ignored files in `.gitignore` before your first push to Github.


###Pushing to Github

You can now commit the changes to your remote Github repository:

```
$ git add .
$ git commit -m "Added rails_12factor and heroku_secrets gems and updated Gemfile.lock"
$ git push -u origin master
```

###Hiding Devise's Secret Key

If you're using `Devise` and would like to prevent the "devise secret key" (not to be 
confused with the above keys) from being pushed to Github, follow these steps:

In `config/initializers/devise.rb`, uncomment the following line:

```ruby
config.secret_key = ...      <=  here there will be a long 'hash' number
```

After uncommenting this line, remove the current `hash key` (ie the long string of 
random characters) and replace it with this:

```ruby
config.secret_key = Rails.application.secrets.devise_secret_key
```

Now generate a new secret key in terminal:

```bash
$ bin/rake secret
```

Copy the result from terminal and in `config/secrets.yml` add the following line to each 
of the three environemnt (`test`, `development`, `production`):

```ruby
devise_secret_key: REPLACE_THIS_WITH_THE_NEW_DEVISE_SECRECT_KEY_GENERATED_IN_TERMINAL
```

###Pushing to Github Again

You can now commit the changes to your remote Github repository:

```
$ git add .
$ git commit -m "changed and hid Devise secret key"
$ git push -u origin master
```

###Creating the Heroku App

To create a new Heroku application and set up a `remote` for it (similar to Github's), follow
these steps.

To create a new app with an Heroku generated name:

```bash
$ heroku create 				
```

Alternatively you can give the app your own name:

$ heroku create NAME_OF_YOUR_APP


###Time to deploy your app!

```bash
$ git push heroku master
```

but we're not done yet.


###Pushing the Secret Keys to Heroku

We need to push all of the app's secret keys __directly__ to Heroku with:

```bash
$ bin/rake heroku:secrets RAILS_ENV=production
```

Next, if we have a database, we need manually migrate it by running:

```bash
$ heroku run rake db:migrate
```

###Openning the App from the Terminal

To visit the app's page after deployment to Heroku, run:

```bash
$ heroku open
```

This should open your default browser in the app's Url.


###Debugging Tip

A useful way to debug (even if the build is said to be successfull but the app 
still doesn't work for some reason), is to run:

```bash
$ heroku run rails console
```
