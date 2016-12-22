# Authorization system

## Create model

* Update migration file, add role
```ruby
class CreateUsers < ActiveRecord::Migration
  def change
    create_table :users do |t|
      t.string :first_name
      t.string :last_name
      t.string :email
      t.string :password_digest

	  t.string :role

      t.timestamps null: false
    end
  end
end
```

* Run the migration, add editor method

* Update user class
```ruby
class User < ActiveRecord::Base
  has_secure_password

  def editor?
    self.role == 'editor'
  end
end
```

* Check model
```bash
rails console
> mateo = User.find_by(email: 'mateo@email.com')
> mateo.editor?
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

  def require_editor
    redirect_to '/' unless current_user.editor?
  end

end
```

* Update controllers which require authentication, add another *before_action*
```ruby
class RecipesController < ApplicationController

  before_action :require_user, only: [:show, :edit, :update, :destroy]
  before_action :require_editor, only: [:show, :edit]

  def show
    @recipe = Recipe.find(params[:id])
  end

  def edit
    @recipe = Recipe.find(params[:id])
  end

  def update
    @recipe = Recipe.find(params[:id])
      if @recipe.update(recipe_params)
        redirect_to @recipe
      else
        render 'edit'
      end
  end

  def destroy
    @recipe = Recipe.find(params[:id])
    @recipe.destroy
    redirect_to root_url
  end

  private
    def recipe_params
      params.require(:recipe).permit(:name, :ingredients, :instructions)
    end

end
```

* 
