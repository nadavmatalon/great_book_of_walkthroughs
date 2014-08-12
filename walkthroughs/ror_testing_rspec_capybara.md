Testing Ruby on Rails with Rspec and Capybara
=============================================

Written by: Julia

Please refer to the [New Project Setup walkthrough](https://github.com/nadavmatalon/great_book_of_walkthroughts/blob/master/walkthroughs/ror_new_project_setup_Nadav_and_Will.md) to ensure you have installed
Rspec properly for your RoR app.

As it will probably be easier to understand this in the context of an example, I have
chosen the Instagram project.

Initial setup for testing with Rspec
------------------------------------

1.  Within **spec/rails\_helper.rb**, and under the section of code with:

        RSpec.configure do |config|

    paste in the following code:

        config.include Warden::Test::Helpers
        Warden.test_mode!

        config.after(:each) do
          Warden.test_reset!
        end

Setting up feature specs with Capybara
--------------------------------------

1.  Create a **features** folder within the automatically generated **spec** folder
1.  Create a **post\_feature\_spec.rb** file
    *  require 'rails_helper'
    *  describe 'Displaying posts'
        *  context 'No posts'
            *  it 'shows no posts'
        *  context 'has posts'
            *  it 'displays posts'
    *  describe 'Creating posts'
        *  context 'When logged out'
            *  it 'cannot add a post'
        *  context 'When logged in' (need to have Devise set up)
            *  it 'can add a post'
            *  it 'can upload a photo to the post' (need to have Amazon S3 set up)
    
    **NOTE: When testing the uploading of photos, be sure to stub out the act of
    saving the photo to S3 (see below)**
1.  Create a **user\_feature\_spec.rb** file (only done after installing Devise)
    *  require 'rails_helper'
    *  describe 'User registration and login'
        *  it 'can sign up'
        *  it 'can sign in'
1.  Create a **tag\_feature\_spec.rb** file
    *  require 'rails_helper'
    *  describe 'Tagging posts'
        * context 'When logged in'
            *  it 'displays tag when post is added'
        * context 'Existing posts'
            *  it 'should filter posts by selected tag'
            *  it 'uses the tag name in the url'

    **NOTE: To test post tagging in detail, you would now move into unit testing
    with Rspec (see below)**
1.  Create a **maps\_feature\_spec.rb** file (only done after installing Poltergeist)
    *  require 'rails_helper'
    *  describe 'Maps'
        *  context 'Post with addresses'
          *  it 'displays map'

    **I won't go into this further as testing Javascript is a topic on its own**

Unit testing the post tagging feature with Rspec
------------------------------------------------

1.  Create a **models** folder within the **spec** folder
1.  Create a **post\_spec.rb** file
    *  require 'rails_helper'
    *  describe 'Post'
        *  describe '#tag names='
            *  describe 'no tags'
                * it 'does nothing'
            *  describe 'one tag that does not exist'
                *  it 'adds a tag to the post'
            *  describe 'two tags that do not exist'
                *  it 'adds both tags to the post'
            *  describe 'multiple space separated tags'
                *  context 'without spaces'
                    *  it 'adds each tag to the post'
            *  describe 'tag already exists'
                * it 'reuses existing tag'

**You would then fill these out with typical RSpec helpers and expectations, stubs and doubles as was learnt in Week 2...**

Stubbing out the saving of images to Amazon Web Services S3
-----------------------------------------------------------

1. Within **rails\_helper.rb**, paste in the following code:
          
        require 'aws'
        AWS.stub!
        AWS.config(:access_key_id => "TESTKEY", :secret_access_key => "TESTSECRET")

Typical helpers used in Capybara
--------------------------------

*  visit '/...'
*  fill_in '...', with: '...'
*  click_button '...'
*  click_link '...'
*  attach_file 'Image', Rails.root.join('spec/images/kittycats.jpg')

    **NOTE: Image here refers to the name that you gave the attach image field in your form helper**
    
    **NOTE: You need to have saved this kittycats.jpg image in the self-created images folder**

**NOTE: If there is a case of conflicting field names on your page, use the following (where *.new\_user* is the class you are targetting within your page/test:**

        within '.new_user' do
          ...fill_in...etc.
          ...click_button...etc.
        end
        expect ...

Typical expectations used in Capybara
-------------------------------------

*  expect(page).to have\_content "..."
*  expect(page).to have\_link "..."
*  expect(page).to have\_field "..."
*  expect(current_path).to eq "/..."
*  expect(page).not\_to have_css "img.uploaded-pic" (where 'uploaded-pic' is a class)
*  expect(page).to have\_css 'h1', 'Posts tagged with #yolo'

Suggestions on why your Capybara tests may not work
---------------------------------------------------

* Have you required **rails\_helper.rb**?
* Have you created a fake user?
* Have you created a fake post (and linked it to the fake user)?
* Have you logged in as the fake user using **login_as** before testing logging in features?
* Have you created your fake objects within the right scope? If you want to create
say, a User for all of your following tests, consider using **let!** and **before do end** statements, e.g.

        let!(:user) { User.create(name: 'Sally', email: 'test@test.com', password: '12345678', password_confirmation: '12345678') }
    
        before do
          login_as user
        end

