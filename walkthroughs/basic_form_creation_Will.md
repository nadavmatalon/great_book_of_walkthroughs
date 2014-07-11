
Creating a form once the initial set up has been completed.

[controller_name] = the name of the relevent controller
[page_name] = page where you want to view results
[field] = Whatever field you want to use
[type] = Examples are text, string, decimal etc
[button_name] = Whatever name you give the button
[model_name] = Usually the name of the controller but singular
[field_type] = Use relevent field type. Options are: check_box, color_field, date_field, datetime_field, datetime_local_field, email_field, file_field, hidden_field, month_field, number_field, password_field, phone_field, range_field, radio_button, search_field, telphone_field, text_area, text_field, time_field, url_field, week_field.

Please note that when capitalized you should capitalize.

1. create a test in [controller_name]_feature_spec.rb:
describe 'creating [controllername]' do
	it 'adds the [model_name] to the [page_name]'
		visit '/[controller_name]/new'
		fill_in '[field]', with: 'Example Text' (repeat for multiple fields)
		click_button '[button_name]'

		expect(current_path).to eq [controller_name]_path
		expect(page).to have_content 'Example Text'
	end
end
2. rspec
3. in [controller_name]_controller.rb define a method for new:
def new
	@[model_name] = [Model_name].new
end
4. rspec
5. create new.html.erb in "app/views/[controller_name]"
6. rspec
7. in "views/[controller name]/new.html.erb" add:
<%= form_for @[model_name] do |f| %>
	<%= f.label :[field] %>         (repeat this and next line for each field)
	<%= f.[field_type] :[field] %> 

	<%= f.submit '[button_name]' %>
<% end %>
8. rspec
9. in "[controller_name]_controller.rb" create create method:
def create

end
10. rspec
11. in create method add:
@[model_name] = [Model_name].new(params[:[modelname]].permit(:[field]))  (add all text fields separated by a comma)
@[model_name].save!
redirect_to posts_path
12. rspec (should be all clear)
13. rails s (to see it in your browser)
14. in browser go to localhost/3000/[controller_name] to see a list of the items
15. in browser go to localhost/3000/[controller_name]/new to see the form


Adding additional fields no already in database
Any fields that are being created must be in the database.  If you want to add an additional field not included when making the model do a db migration using:
<!-- bin/rails g migration Add[Field]To[Controller_name] [field]:[type] -->
bin/rake db:migrate 
You can add more than one field at a time just by adding more [field]:[type] with a space between each one.



