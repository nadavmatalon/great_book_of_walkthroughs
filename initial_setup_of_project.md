
Ruby On Rails - Initial Setup of a Project

Main Source for Ruby on Rails Initial Setup:

http://www.railstutorial.org/book/beginning#cha-1_footnote-14


To set up a new project (with postgresql databases):

	(*)	To check currently installed versions of Ruby and Rails:

		Terminal>	ruby -v

		Terminal>	rails -v

		To update installation of Rails:

		Terminal>	gem update rails


	(1)	Terminal>	rails new [name_of_project_lower_case] -T -d postgresql


	(2)	cd [name_of_project_lower_case]


	(3)	Terminal> 	subl .


	(4)	Update the Gemfile to include:

		(go over the Github README in the documentation - links about each 
		 gem and verify current versions and updated installation instructions)

			ruby '2.1.1'   	# update to currently used Ruby version

			# https://github.com/plataformatec/devise
			gem 'devise'

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
			 	gem 'rails_12factor'		
			end


	(5)	Terminal> 	bundle install

		(If Bundler complains about a readline error, try adding gem ’rb-readline’ 
		 to the Gemfile)	


		Additional bundle install commands:

		// will be used only by Heroku to install gems related to the 'production group'
		// in the Gemfile (ie gems in this group will not be installed locally - note
		// that this install must be done on the FIRST bundle install otherwise won't work):

		Terminal>	bundle install --without production

		Terminal>	bundle update

		Terminal>	bundle install


	(6)	To install “rspec-collection_matchers” gem (extended syntax for rspec testing):

		Add the following line at the top of “spec/helpers/rspec_helper.rb”:

			require 'rspec/collection_matchers'

		Terminal> 	bundle install


	(7)	Terminal> 	bin/rails generate rspec:install	


	(8)	Update “.rspec” file:

			--color
			--format documentation
			--require spec_helper


	(9)	In “/app/assets/javascripts/application.js”, remove fourth element and leave only the
		following three:

			//= require jquery
			//= require jquery_ujs
			//= require_tree .


	(10)	Update the “.gitignore” file to include:

				# Ignore the secrets files.
					secrets.yml

				# Ignore other unneeded files.
					doc/
					*.swp
					*~
					.project
					.DS_Store
					.idea
					.secret

	(11)	Terminal> 	rspec


	(12)	Terminal> 	bin/rake db:create


	(13)	Terminal> 	bin/rake db:create RAILS_ENV=test


	(14) 	Terminal> 	bin/rake routes				


	(15)	In “/app/assets/javascripts”, remove the “.coffee” file-type from all 
			file_names and empty the files from default content


	(16)	Terminal>	bin/rails generate devise:install			


	(17)	In “config/environments/development.rb” add this line:

			config.action_mailer.default_url_options = { host: 'localhost:3000' }


	(18)	In “config/environments/test.rb” add this line:

  			config.action_mailer.default_url_options = { host: 'localhost:3000' }


	(19)	In “config/routes.rb” add the following line:

	       	root to: "home#index"


	(20)	Terminal> 	bin/rails generate devise:views


	(21)	(optional) In “app/views/layouts/application.html.erb” add this line:

			<h1>[name_of_project]</h1><br/>


	(22)	Set up Github repo:

 			Terminal>	git init

			Terminal> 	git add .
			
			Terminal>	git commit -m "Initial commit"


	(23)	Set up Github remote repo and then link them together:

			Terminal>	git remote add origin [Github_ssh_address_of_repo]

			Terminal>	git push -u origin master	


	(24)	Update the “.gitignore” file to include:

			# Ignore the secrets files.
				/config/database.yml


	(24)	To set up basic users interface (with just “name” and “email”):


		Terminal>	bin/rails generate scaffold User name:string email:string

		(The name of a scaffold follows the convention for models which is to use singular,

		while for names of resources and controllers the convention is to use plural)


		Terminal>	bin/rake db:migrate

		Alternative command:

			Terminal>	bundle exec rake db:migrate
			Terminal>	bundle exec bin/rake db:migrate


		Terminal>	bin/rake db:migrate RAILS_ENV=test

		Terminal>	rspec


	(25)	Terminal> 	bin/rails server

	  .

	  .

	  .

	  