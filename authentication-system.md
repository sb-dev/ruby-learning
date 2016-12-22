# Authentication system

## Create model

* Generate User model

* Update User
```ruby
class User < ActiveRecord::Base
    has_secure_password
end
```

*gem 'bcrypt', '~> 3.1.7' required*

* Update migration files
```ruby
class CreateUsers < ActiveRecord::Migration
  def change
    create_table :users do |t|
      t.string :first_name
      t.string :last_name
      t.string :email
      t.string :password_digest
      t.timestamps
    end
  end
end
```

***has_secure_password*** *uses bcrypt algorithm to securely hash a user's password, which then gets saved in the password_digest column*


* Generate Users controller

## Add sign up feature

* Add routes
```ruby
Rails.application.routes.draw do
  root 'albums#index'
  get 'albums' => 'albums#index'
  get 'albums/new' => 'albums#new'
  get 'albums/:id' => 'albums#show', as: :album
  post 'albums' => 'albums#create'

  get 'signup'  => 'users#new'
  resources :users
end
```

* Update controller
```ruby
class UsersController < ApplicationController
    def new
        @user = User.new
    end
end
```

* Update view, edit app/views/users/new.html.erb
```html
<%= form_for(@user) do |f| %>
    <div class="field">
        <%= f.label :first_name %><br>
        <%= f.text_field :first_name %>
    </div>
    <div class="field">
        <%= f.label :last_name %><br>
        <%= f.text_field :last_name %>
    </div>
    <div class="field">
        <%= f.label :email %><br>
        <%= f.email_field :email %>
    </div>
    <div class="field">
        <%= f.label :password %><br>
        <%= f.password_field :password %>
    </div>
    <div class="actions">
        <%= f.submit "Sign up" %>
    </div>
<% end %>
```

* Update controller
```ruby
class UsersController < ApplicationController
    def new
        @user = User.new
    end
    def create
      @user = User.new(user_params)
      if @user.save
        session[:user_id] = @user.id
        redirect_to '/'
      else
        redirect_to '/signup'
      end
    end

    private
    def user_params
        params.require(:user).permit(:first_name, :last_name, :email, :password)
    end
end
```

*Sessions are stored as key/value pairs*
```ruby
session[:user_id] = @user.id
```

## Using Rails console

The Rails console is a useful tool to interact with Rails apps.

```bash
rails console
```

* Get all user
```bash
User.all
```

## Add Login feature

* Generate a Sessions controller

* Add route
```ruby
Rails.application.routes.draw do
  get 'login'  => 'sessions#new'
end
```

* Update controller
```ruby
class SessionsController < ApplicationController
  def new
  end
end
```

* Update view, edit app/views/sessions/new.html.erb
```html
<%= form_for(:session, url: login_path) do |f| %>
  <%= f.email_field :email, :placeholder => "Email" %>
  <%= f.password_field :password, :placeholder => "Password" %>
  <%= f.submit "Log in", class: "btn-submit" %>
<% end %>
```

* Add route
```ruby
Rails.application.routes.draw do
  get 'login'  => 'sessions#new'
  post 'login' => 'sessions#create'
end
```

* Update controller
```ruby
def create
  @user = User.find_by_email(params[:session][:email])
  if @user && @user.authenticate(params[:session][:password])
    session[:user_id] = @user.id
    redirect_to '/'
  else
    redirect_to 'login'
  end
end
```

## Add Logout feature

* Add route
```ruby
Rails.application.routes.draw do
    get 'login'  => 'sessions#new'
    post 'login' => 'sessions#create'
    delete 'logout' => 'sessions#destroy'
end
```

* Update controller
```ruby
class SessionsController < ApplicationController
  def new  
  end
  def create
    @user = User.find_by_email(params[:session][:email])
    if @user && @user.authenticate(params[:session][:password])
      session[:user_id] = @user.id
      redirect_to '/'
    else
      redirect_to 'login'
    end
  end
  def destroy
    session[:user_id] = nil
  	redirect_to '/'
  end
end
```

## Add restriction feature

* Update Application controller, edit app/controllers/application_controller.rb
```ruby
class ApplicationController < ActionController::Base
  # Prevent CSRF attacks by raising an exception.
  # For APIs, you may want to use :null_session instead.
  protect_from_forgery with: :exception

  helper_method :current_user
  def current_user
    @current_user ||= User.find(session[:user_id]) if session[:user_id]
  end

  def require_user
    redirect_to '/login' unless current_user
  end
end
```

* Update controllers which require authentication
```ruby
class AlbumsController < ApplicationController
  before_action :require_user, only: [:index, :show]

  def index
    @albums = Album.all
  end

  def show
    @album = Album.find(params[:id])
    @photos = @album.photos
  end
end
```

* Update view, edit app/views/layouts/application.html.erb
```html
<% if current_user %>
  <ul>
    <li><%= current_user.email %></li>
    <li><%= link_to "Log out", logout_path, method: "delete" %></li>
  </ul>
<% else %>
  <ul>
    <li><%= link_to "Login", 'login' %></a></li>
    <li><%= link_to "Signup", 'signup' %></a></li>
  </ul>
<% end %>
```

***current_user*** *method allow us to access the current user*

***require_user*** *redirects to the root of the app if there is no such user*

***before_action*** *acts as filters by calling methods before executing controller actions.*
