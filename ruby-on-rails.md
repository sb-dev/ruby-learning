## Ruby On Rails

## Basics

* Generate Ruby On Rails app
```bash
rails new MessengerApp
```

* Download dependencies
```bash
bundle install
```

* Start app
```bash
rails server
```

## Create model

* Generate model
```bash
rails generate model Message
```

* Make changes to DB, edit db/migrate/
```ruby
class CreateMessages < ActiveRecord::Migration
  def change
    create_table :messages do |t|
	  t.text :content
      t.timestamps #include created_at and updated_at
    end
  end
end
```

* Update the database with the new messages data model
```bash
rake db:migrate
```

* Seed the database with sample data from db/seeds.rb
```ruby
m1 = Message.create(content: "We're at the beach so you should meet us here! I make a mean sandcastle. :)")

m2 = Message.create(content: "Let's meet there!")
```

```bash
rake db:seed
```

* Generate controller
```bash
generate controller Messages
```

## Add view action

* Create new route and map it to a controller's index action, edit config/routes.rb
```ruby
Rails.application.routes.draw do
  get 'messages' => 'messages#index'
end
```

*Map default routes (index, show, new, create, edit, update and destroy)*
```ruby
resources :messages
```

*Restrict default routes*
```ruby
resources :messages, only: [:index, :show]
```

* Add action to Messages controller in app/controllers
```ruby
class MessagesController < ApplicationController
    def index
      @messages = Message.all
    end
end
```

* Update view, edit app/views/messages/index.html.erb
```html
<% @messages.each do |message| %>
<div class="message">
    <p class="content"><%= message.content %></p>
    <p class="time"><%= message.created_at %></p>
</div>
<% end %>
```

## Add create actions

* Add route, edit config/routes.rb
```ruby
Rails.application.routes.draw do
  get 'messages' => 'messages#index'
  get 'messages/new' => 'messages#new'
  post 'messages' => 'messages#create'
end
```

* Add action, edit app/controllers/messages_controller.rb
```ruby
class MessagesController < ApplicationController
    def index
      @messages = Message.all
    end

    def new
      @message = Message.new
    end

    def create
      @message = Message.new(message_params)
      if @message.save
        redirect_to '/messages'
      else
        render 'new'
      end
    end

    private
    def message_params
        params.require(:message).permit(:content)
    end
end
```

* Add view, edit app/views/messages/new.html.erb
```html
<%= form_for(@message) do |f| %>  
  <div class="field">
    <%= f.label :message %><br>
    <%= f.text_area :content %>
  </div>
  <div class="actions">
    <%= f.submit "Create" %>
  </div>
<% end %>
```

* Add link, edit app/views/messages/index.html.erb
```html
<%= link_to 'New Message', "messages/new" %>
<% end %>
```
