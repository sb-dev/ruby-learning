# Many to many relationship

## Create model

* Generate models named Movie, Actor and Part.

```bash
rails generate model <model name>
```

* Update Movie, edit app/models/movie.rb
```ruby
class Movie < ActiveRecord::Base
    has_many :parts
    has_many :actors, through: :parts
end
```

* Update Actor, edit app/models/actor.rb
```ruby
class Actor < ActiveRecord::Base
    has_many :parts
    has_many :movies, through: :parts
end
```

* Update Part, edit app/models/part.rb
```ruby
class Part < ActiveRecord::Base
    belongs_to :movie
    belongs_to :actor
end
```

***has_many :through*** *association used to connect the Movie model to the Actor model through the Part model.*

* Update migration files in db/migrate/
```ruby
class CreateMovies < ActiveRecord::Migration
  def change
    create_table :movies do |t|
	  t.string :title
      t.string :image
      t.string :release_year
      t.string :plot
      t.timestamps
    end
  end
end
```

```ruby
class CreateActors < ActiveRecord::Migration
  def change
    create_table :actors do |t|
	  t.string :first_name
      t.string :last_name
      t.string :image
      t.string :bio
      t.timestamps
    end
  end
end
```

```ruby
class CreateParts < ActiveRecord::Migration
  def change
    create_table :parts do |t|
	  t.belongs_to :movie, index: true
	  t.belongs_to :actor, index: true
      t.timestamps
    end
  end
end
```

*Add foreign keys pointing to Movie and Actor tables*

* Generate controller Movies
```bash
rails generate controller Movies
```

## Create Movie list view

* Add route
```ruby
Rails.application.routes.draw do
     get "movies" => "movies#index"
end
```

* Update controller
```ruby
class MoviesController < ApplicationController
  def index
    @movies = Movie.all
  end
end
```

* Update view, edit app/views/movies/index.html.erb
```html
<% @movies.each do |movie| %>
    <div class="movie">
        <%= image_tag movie.image %>
        <h3> <%= movie.title %> </h3>
        <p> <%= movie.release_year %></p>
     </div>
<% end %>
```

## Create Movie details view

* Add route
```ruby
Rails.application.routes.draw do
     get '/movies/:id' => 'movies#show', as: :movie
end
```

* Update controller
```ruby
class MoviesController < ApplicationController
    def show
      @movie = Movie.find(params[:id])
      @actors = @movie.actors
    end
end
```

* Update view, edit app/view/movies/show.html.erb
```html
<div class="movie">
    <div class="info">
        <%= image_tag @movie.image %>
        <h3 class="movie-title"><%= @movie.title %></h3>
        <p class="movie-release-year"><%= @movie.release_year %></p>
        <p class="movie-plot"><%= @movie.plot %></p>
    </div>
</div>

<h2>Cast</h2>
<% @actors.each do |actor| %>
    <div class="actor">
          <%= image_tag actor.image %>
          <h3 class="actor-name"><%= actor.first_name %> <%= actor.last_name %></h3>
          <p class="actor-bio"><%= actor.bio %></p>
    </div>
<% end %>
</div>
```

* Update view, edit app/views/movies/index.html.erb
```html
<% @movies.each do |movie| %>
	<div class="movie">
        <%= image_tag movie.image %>
        <h3> <%= movie.title %> </h3>
        <p> <%= movie.release_year %></p>
        <%= link_to "Show",movie_path(movie) %>
  </div>
<% end %>
```

## Create Actor list view

* Generate Actors controller

* Add route
```ruby
    get "actors" => "actors#index"
```

* Update controller
```ruby
class ActorsController < ApplicationController
    def index
        @actors = Actor.all
    end
end
```

* Update view, edit app/views/actors/index.html.erb
```html
<% @actors.each do |actor| %>
<div class="actor col-xs-2">
    <%= image_tag actor.image %>
    <h3> <%= actor.first_name %> <%= actor.last_name %></h3>
    <p> <%= actor.bio %> </p>
</div>
<% end %>
```

* Add route
```ruby
    get "/actors/:id" => "actors#show", as: :actor
```

* Update controller
```ruby
class ActorsController < ApplicationController
    def show
        @actor = Actor.find(params[:id])
        @movies = @actor.movies
    end
end
```

* Update view, app/views/actors/show.html.erb
```html
<div class="actor">
  <%= image_tag @actor.image %>
  <div class="info">
    <h3 class="actor-name"><%= @actor.first_name %> <%= @actor.last_name %></h3>
    <p class="actor-bio">  </p>
  </div>
</div>

<h2>Movies</h2>
<% @movies.each do |movie| %>
<div class="movie">
  <%= image_tag movie.image %>
  <h3 class="movie-title"><%= movie.title %></h3>
  <p class="movie-release-year"><%= movie.release_year %></p>
  <p class="movie-plot"><%= movie.plot %></p>
</div>
<% end %>
```

* Update view, app/views/actors/index.html.erb
```html
<% @actors.each do |actor| %>
  <div class="actor col-xs-2">
    <%= image_tag actor.image %>
    <h3> <%= actor.first_name %> <%= actor.last_name %></h3>
    <p> <%= actor.bio %> </p>
    <% link_to "Show", actor_path(actor) %>
  </div>
<% end %>
```
