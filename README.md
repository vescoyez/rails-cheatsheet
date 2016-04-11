# Rails Cheatshit

## Table of Contents

- [New App](#new-app)
- [Model](#model)
- [DB Tasks](#db-tasks)
- [Authentication (with Devise)](#authentication-with-devise)

## New App

`$ rails new -T --database postgresql YOUR_APP_NAME`

- `-T` : remove tests
- `--database postgresql` : use postgresql database
- `YOUR_APP_NAME` : replace it with your app name

## Model

Create a model
```
$ rails g model *model_name* *attribute*:*type* *attribute*:*type*
```
- `*model_name*` : replace it by your model name (Capital letter & singular)
- `*attribute*` : replace it by the model attribute
- `*type*` : replace it by the attribute type : 
  * `string`
  * `text`
  * `integer`
  * `float`
  * `date`
  * `datetime`
  * `array`

Migrate DB
```
$ rake db:migrate
```

Destroy a model
```
$ rails destroy model *model_name*
```

Add attributes to model
```
$ rails g migration Add*Attribute*To*Model*s
```
- `*Attribute*` : CamelCase & singular
- `*Model*`: Model Name plurial

Remove attributes from model
```
$ rails g migration Remove*Attribute*From*Model*s
```
- `*Attribute*` : CamelCase & singular
- `*Model*`: Model Name, CamelCase & plurial

### Create the 7 CRUD routes

Add to config/routes.rb
```ruby
Rails.application.routes.draw do
  resources :*model*s
end
```

If you don't need all of the
```ruby
Rails.application.routes.draw do
  resources :*model*s, only: [:create, :index, :destroy]
end
```
or
```ruby
Rails.application.routes.draw do
  resources :*model*s, except: [:create, :index, :destroy]
end
```

7 Instance methods
```ruby
# app/controller/models_controller.rb
class *Model*sController < ApplicationController
  def index         # GET /*model*s
  end

  def show          # GET /*model*s/:id
  end

  def new           # GET /*model*s/new
  end

  def create        # POST /*model*s
  end

  def edit          # GET /*model*s/:id/edit
  end

  def update        # PATCH /*model*s/:id
  end

  def destroy       # DELETE /*model*s/:id
  end
end
```

Get all object instances
```ruby
 def index
  @*model* = *Model*.all
 end
```

Get 1 object instance
```ruby
 def show
  @*model* = *Model*.find(params[:id])
 end
```

Create & update
```ruby
class *Model*sController < ApplicationController
 def create
  @*model* = *Model*.new(*model*_params)
  @*model*.save
  
  redirect_to *model*_path(@*model*)
 end

 def update
  @*model* = *Model*.find(params[:id])
  @*model*.update(*model*_params)
  
  redirect_to *model*_path(@*model*)
 end

 private

 def *model*_params
  params.require(:*model*).permit(:*attribute*, :*other_attribute*)
 end
end
```

Delete

```ruby
class *Model*sController < ApplicationController
  def destroy
    @*model* = *Model*.find(params[:id])
    @*model*.destroy

    redirect_to *model*s_path
  end
end
```

## DB tasks

Drop the database (lose all your data!)
```
$ rake db:drop
```

Create the database with an empty schema
```
$ rake db:create
```

Run pending migrations on the database schema
```
$ rake db:migrate
```

Revert the last migration
```
$ rake db:rollback
```

Drop database + replay all migration
```
$ rake db:reset
```

## Authentication (with Devise)

Add to Gemfile
```ruby
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
