Paperclip:

What is it?

	- Easy file attachment library for Active Record

	- Tool for allowing the use of photos/imges in your application and database (used in our Instagram app)

	- Keeps info like file size and date the photo/image was uploaded in the database

	- Stores the url of the photo/image in your database (not the photo/image itself given space and latency issues=> images stored under public folder or on S3)

	- Website for overview and installation: https://github.com/thoughtbot/paperclip


How do we start?

(1) We create a features test in our spec directory

		it 'should be able to create posts and add photos' do
			visit '/posts/new'

			fill_in 'Comment',  with: "Test"
			fill_in 'Description', with: "Test"

			We included the below in our test 
			(rails.root is a capybara method to look for the file) => 

			attach_file 'Image', Rails.root.join('spec/images/image.jpg')
			click_button 'create'

			expect(current_path).to eq '/posts'
			expect(page).to have_content "Test"
			
			Our expectation for images=> 

			expect(page).to have_css 'img.uploaded-pic'
		end

(2) Need to install imagemagick (an image library for paperclip to work):
	
	Terminal command: brew install imagemagick

(3) Install gem in gemfile: gem "paperclip", github: 'thoughtbot/paperclip'
	
	NB! There is a bug in main rails gem, therefore we need to get it from github/thoughtbot/paperclip
	
	(remember to bundle)

(4) NB! on https://github.com/thoughtbot/paperclip we need to copy from the Quick 
	
	Start, Models, and Rails 4 section: 

	class User < ActiveRecord::Base
	  has_attached_file :avatar
	  validates_attachment_content_type :avatar, :content_type => /\Aimage\/.*\Z/
	end

	Paste in your model.rb file (in Instagram app it was our Post.rb file)

(5) Update/migrate database with image file name:
	command line: bin/rails g 'model_name' 'avatar_name'
	(avatar here is what we wanted to upload in instagram app => image)

(6) Migrate the database:
	
	bin/rake db:migrate & bin/rake db:migrate RAILS_ENV=test

(7) Rspec test still failing: can't find field 'attach_file'

(8) Update the new.html.erb form with missing field:

	<%=	f.label :image %>
	<%= f.file_field :image%>

(9) Additional step in case we want to upload images larger than 4 mb.
	(largest size for post requests) updated form with:

	<%= form_for @post, html: {multipart: true} do |f| %>

(10) Rspec test still failing: expected to find css 'img.uploaded-pic'

(11) Permit image in model controller create method (example below):
	
	def create
		
	    @post = Post.new(params[:post].permit(:comment, :description, :image, :tag_names))
	    @post.user = current_user
	    @post.save!

    	redirect_to posts_path
  	end

(12) Display the image in form post.html.erb

	 <%= image_tag post.image.url, class: 'uploaded-pic' %>

	 image.url => 	a helper method from paperclip to dynamically grab image
	 class =>  		uploaded-pic is used to ensure actual class we specified is used

(13) Restart server to view output

(14) Size uploaded image. There is no need to use .css file
	 paperclip comes with styling functionality. 
	 in model.rb file can specify style:

	has_attached_file :image, styles: { thumb: '300x300>' }

	now update our model.html.erb file with style (thumb example provided below):

	<%= image_tag post.image.url(:thumb), class: 'uploaded-pic' %>

(15) Now create test to see ensure if user does not attach an image 
	 there is no broken image shown page. Same test as before but add:
	
	expect(page).not_to have_css'img.uploaded-pic'

(16) Update model.html.erb:

 	<% if post.image.present? %>
        <%= image_tag post.image.url(:thumb), class: 'uploaded-pic' %>
   <% end %>
    

