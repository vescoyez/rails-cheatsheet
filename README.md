# Rails Cheatshit

## Table of Contents

- [New App](#new-app)
- [Model](#model)
- [Routes](#routes)
- [Controller](#controller)
- [DB Tasks](#db-tasks)
- [Authentication (with Devise)](#authentication-with-devise)

## New App

```
$ rails new -T --database postgresql YOUR_APP_NAME
```

- `-T` : remove tests
- `--database postgresql` : use postgresql database
- `YOUR_APP_NAME` : replace it with your app name

## Model

Create a model
```
$ rails g model *object_name* *attribute*:*type* *attribute*:*type*
```
- `*object_name*` : replace it by your model name (Capital letter & singular)
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
$ rails destroy model *object_name*
```

Add attributes to model
```
$ rails g migration Add*Attribute*To*ObjectName*s
```
- `*Attribute*` : CamelCase & singular
- `*ObjectName*`: Model Name plurial

Remove attributes from model
```
$ rails g migration Remove*Attribute*From*ObjectName*s
```
- `*Attribute*` : CamelCase & singular
- `*ObjectName*`: Model Name, CamelCase & plurial

## Routes

Add to config/routes.rb
```ruby
Rails.application.routes.draw do
  resources :*object_name*s
end
```

If you don't need all of them
```ruby
Rails.application.routes.draw do
  resources :*object_name*s, only: [:create, :index, :destroy]
end
```
or
```ruby
Rails.application.routes.draw do
  resources :*object_name*s, except: [:create, :index, :destroy]
end
```

## Controller

Create a controller
```
$ rails g controller *object_name*
```

7 Instance methods
```ruby
# app/controller/*object_name*s_controller.rb
class *ObjectName*sController < ApplicationController
  def index         # GET /*object_name*s
  end

  def show          # GET /*object_name*s/:id
  end

  def new           # GET /*object_name*s/new
  end

  def create        # POST /*object_name*s
  end

  def edit          # GET /*object_name*s/:id/edit
  end

  def update        # PATCH /*object_name*s/:id
  end

  def destroy       # DELETE /*object_name*s/:id
  end
end
```

Get all object instances
```ruby
 def index
  @*object_name* = *ObjectName*.all
 end
```

Get 1 object instance
```ruby
 def show
  @*object_name* = *ObjectName*.find(params[:id])
 end
```

Create & update
```ruby
class *ObjectName*sController < ApplicationController
 def create
  @*object_name* = *ObjectName*.new(*object_name*_params)
  @*object_name*.save
  
  redirect_to *object_name*_path(@*object_name*)
 end

 def update
  @*object_name* = *ObjectName*.find(params[:id])
  @*object_name*.update(*object_name*_params)
  
  redirect_to *object_name*_path(@*object_name*)
 end

 private

 def *object_name*_params
  params.require(:*object_name*).permit(:*attribute*, :*other_attribute*)
 end
end
```

Delete

```ruby
class *ObjectName*sController < ApplicationController
  def destroy
    @*object_name* = *ObjectName*.find(params[:id])
    @*object_name*.destroy

    redirect_to *object_name*s_path
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
