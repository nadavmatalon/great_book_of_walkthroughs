##Ruby On Rails 
#Devise Setup

__Written by:__ [Nadav](https://github.com/nadavmatalon)
(May 2014@[Makers Academy](http://www.makersacademy.com/))

###General Notes

* For a basic Ruby on Rails project setup please refer to the 
  [New Project Setup](./ror_new_project_setup.md) in this folder.
* The following instructions have been written for projects using 
  [Rails 4.0](http://rubyonrails.org/) or later &amp; 
  [Devise 3.2.4](https://github.com/plataformatec/devise) or later. 
* Text in ALL_CAPITALS_AND_UNDERSCORES indicated a __placeholder__ for your own text 
* If a file's location within the file system isn't specified explicitly, that file is 
  in the root directory of the project.


###Disclaimer

Please note that the the following is meant only to offer guidance, advice, and helpful 
tips written by software development students as part of their learning process. 
As such, the author/s shall have responsibility whatsoever for any loss of data 
or any other damage caused to users and/or their code as a result of following the 
the steps presented in this walkthrough.


###Adding 'Devise to the Gemfile

Add the following gem to the `Gemfile`:

```ruby
# https://github.com/plataformatec/devise
gem 'devise', '~> 3.2.4'
```

__Note:__ you can change the version of `Devise` from `3.2.4` to the 
version you have installed if you prefer.

Save the file and in terminal run:

```bash
$> bundle install
```

And then:

```bash
$> bin/rails generate devise:install

```

###Configuring Devise

In accordance with the on-screen instructions that will appear after the previous 
command:

In `config/environments/development.rb`, add this line:

```ruby
config.action_mailer.default_url_options = { host: 'localhost:3000' }
```

In `config/environments/test.rb`, add this line:

```ruby
config.action_mailer.default_url_options = { host: 'localhost:3000' }
```

In `config/routes.rb`, make sure that the `root_url` is defined, for example:

```ruby
root to: "home#index"
```

In `app/views/layouts/application.html.erb` add the following lines in 
the `<body>` section:

```
<p class="notice"><%= notice %></p>
<p class="alert"><%= alert %></p>
```

To have access to the various `devise` views, in terminal run:

```bash	
$> bin/rails generate devise:views
```


###Generating a Devise 'User' Model

To generate the basic devise `User` model (just `email` and `password`), 
in terminal run:	

```bash	
$> bin/rails generate devise user
$> bin/rake db:migrate
$> bin/rake db:migrate RAILS_ENV=test
```

Open a new tab of terminal and run (or re-start) the Rails server:

```bash	
$> bin/rails server
```

Then open the browser of your choice and go to:

```
http://localhost:3000/users/sign_up
	
http://localhost:3000/users/sign_in
```

### Adding a 'username' to the 'User' Model

To add a "username" attribute to users, in terminal run:

```bash
$> bin/rails generate migration add_name_to_users username:string
$> bin/rake db:migrate
$> bin/rake db:migrate RAILS_ENV=test
```

### Updating Premitted Parameters

In `app/controllers/application_controller.rb`, update the content to the following:

```ruby
class ApplicationController < ActionController::Base
  # Prevent CSRF attacks by raising an exception.
  # For APIs, you may want to use :null_session instead.
  protect_from_forgery with: :null_session

  before_filter :configure_permitted_parameters, if: :devise_controller?

  protected

  def configure_permitted_parameters
    devise_parameter_sanitizer.for(:sign_in) { |u| u.permit(:username, :email, :password, :remember_me) }
    devise_parameter_sanitizer.for(:sign_up) { |u| u.permit(:username, :email, :password, :password_confirmation) }
    devise_parameter_sanitizer.for(:account_update) { |u| u.permit(:username, :email, :password, :password_confirmation, :current_password) }
  end

end
```

### Changing Authentication from 'Email' to 'Username'

If you want to change the way `devise` authenticates users from being based on 
__email__to being based on __username__:

In `app/models/user.rb`, change the content to the following:

```ruby
class User < ActiveRecord::Base
  # Include default devise modules. Others available are:
  # :lockable, :timeoutable and :omniauthable
  devise :database_authenticatable, :registerable, :confirmable, 
         :recoverable, :rememberable, :trackable, :validatable
         authentication_keys: [ :username ]
end
```


### Setting the Devise Secret

In `config/initializers/devise.rb`, uncomment the following line:

```ruby
config.secret_key = …
```

To this:

```ruby
config.secret_key = Rails.application.secrets.devise_secret_key
```

Next, generate a new secret key in terminal:

```bash
$> bin/rake secret
```

Copy the new secret key and in `config/secrets.yml`, change the secret key 
for the 'test' environment to this new key:

```yml
test:
	devise_secret_key: NEW_SECRET_KEY
```

Repeat this process twice more for the `development` and `production` environments.


When done, push to Github.
