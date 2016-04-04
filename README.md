# Rails Cheatshit

## New App

`$ rails new -T --database postgresql YOUR_APP_NAME`

- `-T` : remove tests
- `--database postgresql` : use postgresql database
- `YOUR_APP_NAME` : replace it with your app name
 
## Authentication (with Devise)

Add to Gemfile
```
gem 'devise'
```

Run in terminal
```
$ bundle install
$ rails generate devise:install
```

Add to config/environments/development.rb
```
config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
```

Add in your views (app/views/layouts/application.html.erb for example)
```html
<p class="notice"><%= notice %></p>
<p class="alert"><%= alert %></p>

```


Add to config/initializers/devise.rb
```
config.mailer_sender = 'please-change-me-at-config-initializers-devise@example.com'
```

Create User Model
```
$ rails generate devise User
```

Migrate DB
```
$ rake db:migrate
```

Helpers
```ruby
user_signed_in?
# => true / false

current_user
# => User instance / nil
```

Custom devise views
```
$ rails g devise:views
```

### Protect your routes

Add to app/controllers/application_controller.rb
```ruby
class ApplicationController < ActionController::Base
  before_action :authenticate_user!
end
```

To skip login for some page
```ruby
class PagesController < ApplicationController
  skip_before_action :authenticate_user!, only: :home

  def home
  end
end
```
