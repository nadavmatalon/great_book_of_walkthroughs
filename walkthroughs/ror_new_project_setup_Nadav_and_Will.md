
###Ruby On Rails: Setting Up a New Project


Main Source:  http://www.railstutorial.org/book/   
(note that sometimes you need to refersh a couple of times before the chapters of the book are loaded properly)

#### General Notes

Note 1: Text in square brackets must be removed completely (including the brackets)
 		and replaced with the user's choice as appropriate (see example in step 1).

Note 2:	If a file's location isn't specified explicitly, that file is in the root directory
		of the project.

Note 3: Do not push to the remote github repo before adding "secrets.yml" to the .gitignore list (step 10)


#### Step-by-Step setting up a new Ruby on Rails project with postgresql databases:

	(*)	Check currently installed versions of Ruby and Rails:

		Terminal>	ruby -v

		Terminal>	rails -v

		To update installation of Rails:

		Terminal>	gem update rails


	(1)	Terminal>	rails new [name_of_project_lower_case] -T -B -d postgresql

		Alternatively, it is possible to create defualt settings for all new rails
		projects as follows:

		Terminal> cd ~

		Terminal> ls -a 	

		Check if there's a file called: ".railsrc", and if not:

			create it with: 	Terminal> 	touch .railsrc

			and open it with: 	Terminal> 	subl .railsrc	

		Put the following content into the ".railsrc" file:

			-d postgresql		// sets the project's database to postresql

			-T  				// doesn't include default testing units (to enable using Rspec)

			-B  				// skips immidiate bundle install right after project creation


		Save the ".railsrc" file and close it.

		From now on it's possible to simply use: 	

		Terminal> 	rails new [name_of_project_lower_case]


	(2)	Terminal>	cd [name_of_project_lower_case]


	(3)	Terminal> 	subl .


	(4)	Update the "Gemfile" file to include:

		(in tandem, go over the README of each Gem in the documentation - links above each 
		 gem below - and verify current versions and updated installation instructions)

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

		(Important: Make sure to remove Duplications, e.g. 'sdocs', 'spring')


	(5)	Terminal> 	bundle install

		(If the Bundler complains about a 'readline' error, try adding the gem 'rb-readline'
		 to the Gemfile and do a bundle install)	

		(optional) bundle install commands:

		Terminal>	bundle install --without production

		Terminal>	bundle update

		Terminal>	bundle install

		If using this alternative set of commands, the gems under the'production' group in the Gemfile will be 
		installed only by Heroku during deployment (in other words, gems in this group will not be installed locally - 
		note that this kind of install must be done on the FIRST bundle install command otherwise won't limit 
		the 'production' gem installation. Also, note that after the first time, you can use just 'buntle install' 
		as usual and 'production' gems will not be installed).


	(6)	Terminal> 	bin/rails generate rspec:install

		Alternative shorthand:

		Terminal>	bin/rails g rspec:install

		Also, check if the "spec/features" folder exists and if not - create it manually.	


	(7)	(optional) Update the “.rspec” file to have the following content:

			--color
			--format documentation
			--require spec_helper


	(8)	(optional) To install “rspec-collection_matchers” gem (extends syntax for rspec testing):

		Uncomment this line in the "Gemfile":

			gem 'rspec-collection_matchers'


		Add the following line at the top of “spec/helpers/rspec_helper.rb”:

			require 'rspec/collection_matchers'

		And then:

			Terminal> 	bundle install


	(9)	Rename the “README.rdoc” file to “README.md”


	(10)	In “/app/assets/javascripts/application.js”, remove fourth element ("turbo-links") and leave only the
		following three:

			//= require jquery
			//= require jquery_ujs
			//= require_tree .


			And in "app/views/layouts/application.html.erb":

			remove the references to ‘turbo links’ from the <head> section


	(11)	Update the “.gitignore” file to include:

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


	(12)	(optional) Add “.scss” to the following file: "app/assets/stylesheets/application.css”

			(end-result: "application.css.scss")


	(13)	Terminal> 	rspec  			// this is just to make sure rspec is running correctly


	(14)	Terminal> 	bin/rake db:create


	(15)	In "config/database.yml", add the following line immidiately after the "pool: 5" line:

				host: localhost


	(16)	(optional - but important!) 	

			On some (most?) systems, the previous command (bin/rake db:create) will only create a 'development' database.

			To create a 'test' database as well (needed for Rspec testing and possibly other things), run:

				Terminal> 	bin/rake db:create RAILS_ENV=test


	(17) 	Terminal> 	bin/rake routes			// just to see the different routes currently available			


	(18)	(optional) In “/app/assets/javascripts”, remove the “.coffee” file-type from all 
			file_names and remove the default content - unless you intend to use coffee scripts of course.


	(19)	(optional) In “app/views/layouts/application.html.erb” add this line:
			(this is just to have something to see in the browser to make sure it's working ok)

			<h1>[name_of_project]</h1><br/>


	(20)	Set up a local Github repo:

 			Terminal>	git init

 			Terminal>	touch README.md

			Terminal> 	git add .
			
			Terminal>	git commit -m "initial commit"


	(21)	Set up a Github remote repo and then link them together:

			Terminal>	git remote add origin [Github_ssh_address_of_repo]

			Terminal>	git push -u origin master	


	(22)	Update the “.gitignore” file to include:
			(the idea is to have this file in the remote Github repo for other developers to use,
			but to keep all the changes you might make locally)

			# Ignore the secrets files.
				/config/database.yml


	(23)	Terminal> 	bin/rails server 		// to enable viewing of project in the 
												// browser at url: localhost:3000)

			Alternative (shorthand) command:

			Terminal>	bin/rails s


	(24)	(optional) To test code and check the content of the development database:

				Terminal>	bin/rails console

			To test code and work with 'development' database content without affecting actual contents:

				Terminal>	bin/rails console --sandbox

			To test code and see content of 'test' database:

				Terminal>	bin/rails console test


	(25)	(optional) To add a "reset.css" file so as to reset the default css styling of browsers:

			Create a new file: “app/vendor/stylesheets/reset.css”
			(note that the new file should be located in the 'vendors' folder)

			Copy the content for this file from: http://meyerweb.com/eric/tools/css/reset/  (or any other provider you prefer)

			(updated content for the file: http://meyerweb.com/eric/thoughts/2011/01/03/reset-revisited/)

			In "app/assets/stylesheets/application.css.scss", add the following line ABOVE the other two existing lines as follows:
		
				*= require reset
				*= require_tree .
				*= require_self

			(if the '*=require reset' line is not located above all the other 'require' lines, it WILL mess up your css styling and browser rendering)


	(26)	(optional) To make sure webpages load fully before functionality becomes available, in “app/assets/javascripts” files, use:

			$(document).ready(function() {

				[js code]

			});


	(27)	(optional) HTML5 Shim

			To help tackle problems in redering older browser than IE9:

			Create a new file: “app/views/layouts/_shim.html.erb” 

			Add the following content (known as: “HTML5 shim”):

			<!--[if lt IE 9]>
			<script src="http://html5shim.googlecode.com/svn/trunk/html5.js"></script>
			<![endif]-->

			(content was taken from: https://code.google.com/p/html5shim/)

			Add the following line in “app/views/layouts/application.html.erb” to the end of the <head> section:
			(note that this is the <head> section and not the <header> section)

			<%= render 'layouts/shim' %>


	(28)    . . .



