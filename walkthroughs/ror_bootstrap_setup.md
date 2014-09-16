##Ruby On Rails 
#Bootstrap Setup

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


###Bootstrap Setup

Add the following gems to `Gemfile.rb`:

```ruby
# https://github.com/twbs/bootstrap-sass
gem 'bootstrap-sass', '~> 3.2.0'

# https://github.com/ai/autoprefixer-rails
gem "autoprefixer-rails"

# https://github.com/sstephenson/sprockets
gem 'sprockets'
```


Save the file and in terminal run:

```bash
$> bundle install
```


Add the following to `config/application.rb` inside the `module/class`:

```ruby
config.assets.precompile += %w(*.png *.jpg *.jpeg *.gif)
```


Add “.scss” suffix to the following file:

`app/assets/stylesheets/application.css`

So that the result is this:

`app/assets/stylesheets/application.css.scss`


Add the following lines to the top of `app/assets/stylesheets/custom.css.scss` 
after `*= require_self`: 

```css
@import "bootstrap-sprockets";
@import "bootstrap";
@import "bootstrap/variables";
@import "bootstrap/mixins";
```


In `app/assets/javascripts/application.js` add the following line just 
before this line `//= require_tree .`:

```js
//= require bootstrap-sprockets
```


Re-start the Rails server:

```bash
$> bin/rails server
```


Add custom styling, for example:

```ruby
<%= button_tag "bottun", class: 'btn btn-danger' %> 
```


For more info on bootstrap component customization:

```
http://getbootstrap.com/customize/#less-variables

http://getbootstrap.com/components/#navbar
```

