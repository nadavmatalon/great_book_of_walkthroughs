
Ruby On Rails: Creating a New Project


Main Source:  http://www.railstutorial.org/book/beginning#cha-1_footnote-14


Note 1: Text in square brackets must be removed completely (including the brackets)
 		and replaced with the user's choice as appropriate (see example in step 1).

Note 2:	If a file's location isn't specified explicitly, that file is in the root directory
		of the project.

Note 3: Do not push to the remote github repo before adding "secrets.yml" to the .gitignore list (step 10)


To set up a new Runy on Rails project with postgresql databases:

	(*)	Check currently installed versions of Ruby and Rails:

		Terminal>	ruby -v

		Terminal>	rails -v

		To update installation of Rails:

		Terminal>	gem update rails


	(1)	Terminal>	rails new [name_of_project_lower_case] -T -d postgresql


	(2)	Terminal>	cd [name_of_project_lower_case]


	(3)	Terminal> 	subl .


	(4)	Update the "Gemfile" file to include:

		(go over the Github README in the documentation - links above each 
		 gem - and verify current versions and updated installation instructions)

			ruby '2.1.1'   	# update this according to currently used Ruby version

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

		(If the Bundler complains about a readline error, try adding gem 'rb-readline'
		 to the Gemfile)	

		(optional) Additional bundle install commands:

		// will be used only by Heroku to install gems related to the 'production group'
		// in the Gemfile (ie gems in this group will not be installed locally - note
		// that this install must be done on the FIRST bundle install otherwise won't work):

		Terminal>	bundle install --without production

		Terminal>	bundle update

		Terminal>	bundle install


	(6)	To install “rspec-collection_matchers” gem (extends syntax for rspec testing):

		Add the following line at the top of “spec/helpers/rspec_helper.rb”:

			require 'rspec/collection_matchers'

		And then:

		Terminal> 	bundle install


	(7)	Terminal> 	bin/rails generate rspec:install	


	(8)	(optional) Update the “.rspec” file as follows:

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

	(11)	Terminal> 	rspec  			// this is just to make sure rspec is running correctly


	(12)	Terminal> 	bin/rake db:create


	(13)	Terminal> 	bin/rake db:create RAILS_ENV=test


	(14) 	Terminal> 	bin/rake routes				


	(15)	(optional) In “/app/assets/javascripts”, remove the “.coffee” file-type from all 
			file_names and remove the default content


	(16)	(optional) In “app/views/layouts/application.html.erb” add this line:

			<h1>[name_of_project]</h1><br/>


	(17)	Set up a local Github repo:

 			Terminal>	git init

 			Terminal>	touch README.md

			Terminal> 	git add .
			
			Terminal>	git commit -m "initial commit"


	(18)	Set up a Github remote repo and then link them together:

			Terminal>	git remote add origin [Github_ssh_address_of_repo]

			Terminal>	git push -u origin master	


	(19)	Update the “.gitignore” file to include:

			# Ignore the secrets files.
				/config/database.yml


	(20)	Terminal> 	bin/rails server 		// to enable viewing of project in the 
												//browser (url: localhost:3000)

	  .

	  .

	  .

	  