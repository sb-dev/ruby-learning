# One to many relationship

## Create model

* Generate a model named Tag.

* Generate a model named Destination.

* Update Tag, edit app/models/tag.rb
```ruby
class Tag < ActiveRecord::Base
  has_many :destinations
end
```

*A single Tag can have multiple Destinations*

* Update Tag, edit app/models/destination.rb
```ruby
class Destination < ActiveRecord::Base
  belongs_to :tag
end
```

*Destination belongs to a single Tag*

* Update migration files, in db/migrate/
```ruby
class CreateTags < ActiveRecord::Migration
  def change
    create_table :tags do |t|
      t.string :title
      t.string :image
      t.timestamps
    end
  end
end
```

```ruby
class CreateDestinations < ActiveRecord::Migration
  def change
    create_table :destinations do |t|
      t.string :name
      t.string :image
      t.string :description
      t.references :tag
      t.timestamps
    end
  end
end
```

* Run migration to update the database
```bash
rake db:migrate
```

* Seed the database (travel_app_seeds.rb)
```bash
rake db:seed
```

## Create list view

* Generate controller Tags

```bash
rails generate controller Tags
```

```ruby
class TagsController < ApplicationController
    def index
      @tags = Tag.all
    end
end
```

* Add route
```ruby
Rails.application.routes.draw do
  get 'tags' => 'tags#index'
end
```

* Update view, edit app/views/tags/index.html.erb
```html
<% @tags.each do |tag| %>
<div class="card col-xs-4">
    <%= image_tag tag.image  %>
    <h2> <%= tag.title %> </h2>
</div>
<% end %>
```

## Create Tag details view

* Add route
```ruby
Rails.application.routes.draw do
  get 'tags' => 'tags#index'
  get '/tags/:id' => 'tags#show', as: :tag
end
```

***as:*** *used to name the route*

* Update controller
```ruby
def show
  @tag = Tag.find(params[:id])
  @destinations = @tag.destinations
end
```

* Update view, edit app/views/tags/show.html.erb
```html
<h2 class="tag-title"><%= @tag.title %></h2>
<div class="cards row">
<% @destinations.each do |dest| %>
  <div class="card col-xs-4">
    <%= image_tag dest.image  %>
    <h2> <%= dest.name %> </h2>
    <p> <%= dest.description %> </p>
  </div>
<% end %>
</div>
```

* Update view, edit app/views/tags/index.html.erb
```html
<div class="card col-xs-4">
    <%= image_tag tag.image  %>
    <h2> <%= tag.title %> </h2>
    <%= link_to "Learn more", tag_path(tag) %>
</div>
<% end %>
```

***tag_path(t)*** *used to generate the URL to a specific tag's path, for example /tag/1*

## Create Destination details view

* Add route
```ruby
Rails.application.routes.draw do
  get '/destinations/:id' => 'destinations#show', as: :destination
end
```

* Update controller
```ruby
class DestinationsController < ApplicationController
    def show
      @destination = Destination.find(params[:id])
    end
end
```

* Update view, edit app/views/destinations/show.html.erb
```html
<%= image_tag dest.image %>
<h2> <%= dest.name %> </h2>
<p> <%= dest.description %> </p>
```

* Update view, edit app/views/tags/show.html.erb
```html
<h2 class="tag-title"><%= @tag.title %></h2>
<div class="cards row">
<% @destinations.each do |destination| %>
  <div class="card col-xs-4">
    <%= image_tag dest.image %>
    <h2> <%= destination.name %> </h2>
    <p> <%= destination.description %> </p>
    <p><%= link_to "See more", destination_path(destination) %></p>
  </div>
<% end %>
</div>
```

## Create Destination edit view

* Add route
```ruby
Rails.application.routes.draw do
    get '/destinations/:id/edit' => 'destinations#edit', as: :edit_destination
    patch '/destinations/:id' => 'destinations#update'
end
```

* Update controller
```ruby
class DestinationsController < ApplicationController
    def edit
        @destination = Destination.find(params[:id])
    end

    def update
      @destination = Destination.find(params[:id])
      if @destination.update_attributes(destination_params)
        redirect_to(:action => 'show', :id => @destination.id)
      else
        render 'edit'
      end
    end

    private
    def destination_params
        params.require(:destination).permit(:name, :description)
    end
end
```

* Update view, edit app/views/destinations/edit.html.erb
```html
<%= image_tag @destination.image %>
<%= form_for(@destination) do |f| %>
<div class="field">
    <%= f.label :name %><br>
    <%= f.text_field :name %>
</div>
<div class="field">
    <%= f.label :description %><br>
    <%= f.text_field :description %>
</div>
<div class="actions">
    <%= f.submit "Update" %>
</div>
<% end %>
```

* Update view, edit app/views/destinations/show.html.erb
```html
<%= image_tag @destination.image %>
<h2><%= @destination.name %></h2>
<p><%= @destination.description %></p>
<p><%= link_to "Edit", edit_destination_path(@destination) %></p>
```
