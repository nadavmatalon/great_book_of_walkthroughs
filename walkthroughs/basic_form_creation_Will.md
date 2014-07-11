Creating a form once the initial set up has been completed. <br>
<br>
<br>
**controllername** = the name of the relevent controller<br>
**pagename** = page where you want to view results<br>
**field** = Whatever field you want to use<br>
**type** = Examples are text, string, decimal etc<br>
**buttonname** = Whatever name you give the button<br>
**modelname** = Usually the name of the controller but singular<br>
**field_type** = Use relevent field type. Options are: check_box, color_field, date_field, datetime_field, datetime_local_field, email_field, file_field, hidden_field, month_field, number_field, password_field, phone_field, range_field, radio_button, search_field, telphone_field, text_area, text_field, time_field, url_field, week_field.<br>
<br>
Please note that when capitalized you should capitalize.<br>
<br>
1. create a test in **controllername**_feature_spec.rb:<br>
describe 'creating **controllername**' do<br>
	it 'adds the **modelname** to the **pagename**'<br>
		visit '/**controllername**/new'<br>
		fill_in '**field**', with: 'Example Text' (repeat for multiple fields)<br>
		click_button '**buttonname**'<br>
<br>
		expect(current_path).to eq **controllername**_path<br>
		expect(page).to have_content 'Example Text'<br>
	end<br>
end<br>
2. rspec<br>
3. in **controllername**_controller.rb define a method for new:<br>
def new<br>
	@**modelname** = **Modelname**.new <br>
end<br>
4. rspec<br>
5. create new.html.erb in app/views/**controllername**/<br>
6. rspec<br>
7. in views/**controllername**/new.html.erb add:<br>
<%= form_for @**modelname** do |f| %><br>
	<%= f.label :**field** %>         (repeat this and next line for each field)<br>
	<%= f.**field_type** :**field** %> <br> 
<br>
	<%= f.submit '**buttonname**' %><br>
<% end %><br>
8. rspec<br>
9. in **controllername**_controller.rb create create method:<br>
def create<br>
<br>
end<br>
10. rspec<br>
11. in create method add:<br>
@**modelname** = **Modelname**.new(params[:**modelname**].permit(:**field**))  (add all text fields separated by a comma)<br>
@**modelname**.save!<br>
redirect_to posts_path<br>
12. rspec (should be all clear)<br>
13. rails s (to see it in your browser)<br>
14. in browser go to localhost/3000/**controllername** to see a list of the items<br>
15. in browser go to localhost/3000/**controllername**/new to see the form
<br>
<br>
Adding additional fields no already in database<br>
Any fields that are being created must be in the database.  If you want to add an additional field not included when making the model do a db migration using: <br>
bin/rails g migration Add**Field**To**Controllername** **field**:**type**<br>
bin/rake db:migrate <br>
You can add more than one field at a time just by adding more **field**:**type** with a space between each one.<br>