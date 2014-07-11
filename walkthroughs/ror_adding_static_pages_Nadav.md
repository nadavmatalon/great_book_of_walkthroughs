
###Ruby on Rails: Adding Static Pages (Home, About, Contact, Help)


Please refer to the [New Project Setup walkthrough](https://github.com/nadavmatalon/great_book_of_walkthroughts/blob/master/walkthroughs/ror_new_project_setup_Nadav_and_Will.md) to ensure you have installed
Rspec properly for your RoR app before testing.


	(1)	To create a controller for static pages (homepage, about, contact, help) without the
		default rspec tests, use:

		Terminal> bin/rails generate controller StaticPages home about contact help --no-test-framework


	(2)	In “config/routes.rb” remove the following content:

		[name_of_project]::Application.routes.draw do		// line already exits in file

     			get 'static_pages/home'
      			get 'static_pages/about'
      			get 'static_pages/contact'
      			get 'static_pages/help'

		And replace it with the following content:

 			root 'static_pages#home'

		      	match '/home', to: 'static_pages#home', via: 'get'
      			match '/about', to: 'static_pages#about', via: 'get'
      			match '/contact', to: 'static_pages#contact', via: 'get'
      			match '/help', to: 'static_pages#help', via: 'get'


	(3)	Terminal>	bin/rake routes		// to see the currently available routes


	(4)	Open a new terminal tab and run:

		Terminal> 	bin/rails server

		(the server can run simultaneously with typing commands in terminal (although it may require 
		re-starts every time we run a major command like changing the database or running bundle install)


	(5)	Browser:	http://localhost:3000/			// this should show the "home" page

				http://localhost:3000/home			// this should show the "home" page

				http://localhost:3000/about			// this should show the "about" page

				http://localhost:3000/contact		// this should show the "contact" page

				http://localhost:3000/help			// this should show the "help" page


	(6)	(optional) To add a dynamic title page:

		Add the following line to the top of the <head> section in "app/views/layouts/application.html.erb":

			<title><%= full_title(yield(:page_title)) %></title>

		Add the following to the top of the "app/views/layout/static_pages/home.html.erb"

			<% provide(:page_title, "Home") %>

		Add the following to the top of the "app/views/layout/static_pages/about.html.erb"

			<% provide(:page_title, "About") %>

		Add the following to the top of the "app/views/layout/static_pages/contact.html.erb"

			<% provide(:page_title, "Contact") %>

		Add the following to the top of the "app/views/layout/static_pages/help.html.erb"

			<% provide(:page_title, "Help") %>

		In "app/helpers/application_helper.rb", add this content into the "module ApplicationHelper":

			def full_title page_title
				base_title = "Yelp"
				page_title.empty? ? base_title : "#{base_title} | #{page_title}"
			end


	(8)	To generate initial integration tests for the "static_pages" controller see: 

		“spec/features/static_pages_feature_spec.rb” at the end of this doc.

		Note that if the tests fail because ‘visit’ is not recognized, open “spec/spec_helper.rb",
		at the top of the file add:

			require ‘capybara' 

		And inside the config function:

			config.include Capybara::DSL
 

	(9)	(optional) To create links, in the “home" or any other page, add the following line:

 		<%= link_to “[“text_of_link]”, [route]_path, class: “[css_class_name]” %>

		To find [route] designators:

		Terminal>	bin/rake routes



STATIC PAGES: RSPEC TESTS

(1)		If the folder "spec/features" folder doesn't exist yet, create it.

(2)		In that folder create a new file "spec/features/static_pages_feature_spec.rb" and add the following content:

	
require 'rails_helper'

describe "Static pages: " do
	
	subject { page }

	describe "\'root\' page" do
		
		before (:each) { visit root_path }

		it "should have the title \'Web Templates | Home\'" do
			should have_title("Web Template | Home")
		end

		it "should have the content \'Web Templates\'" do
			should have_content("WEB TEMPLATE")
		end
	end

	describe "Home page" do
		
		before (:each) { visit home_path }

		it "should have the title \'Web Template | Home\'" do
			should have_title("Web Template | Home")
		end		

		it "should have the content 'Web Template'" do
			should have_content("WEB TEMPLATE")
		end		
	end

	describe "About Us page" do

		before (:each) { visit about_path }
		
		it "should have the title \'Web Template | About\'" do
			should have_title("Web Template | About")
		end

		it "should have the content \'About Us\'" do
			should have_content("About Us")
		end
	end

	describe "Contact Us page" do
	
		before (:each) { visit contact_path }

		it "should have the title \'Web Template | Contact\'" do
			should have_title("Web Template | Contact")
		end

		it "should have the content \'Contact Us\'" do
			should have_content("Contact Us")
		end
	end

	describe "Help page" do

		before (:each) { visit help_path }
		
		it "should have the content \'Web Template | Help\'" do
			should have_title("Web Template | Help")
		end

		it "should have the content \'Help\'" do
			should have_content("Help")
		end
	end
end

