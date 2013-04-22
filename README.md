## Style your links

##### Add CSS class to your link element

Open /app/views/ideas/index.html.erb

Replace

```ruby
<td><%= link_to 'Show', idea %></td>
```

With

```ruby
<td><%= link_to 'Show', idea, class: 'test' %></td>
```

##### Add CSS to design this link element

Open /app/assets/stylesheets/application.css

Add

```css
a:visited.test { font-weight: bold;  color: red;}
a.test { font-weight: bold; color: red; text-decoration:none;}
```

##### To make your table look nicer, add padding 

```css
td { padding: 10px;}
```

## Shorten the description on index
Open /app/controllers/ideas_controller.rb

After

```ruby
@ideas = Idea.all
```

Add

```ruby
chars_to_display = 50
@ideas.each do |x|
  if x.description.length > chars_to_display
    x.description = x.description[0, 50] + '...'
  end
end
```

## Add voting feature to your app

Open your terminal and run the command `rails g migration AddFields`

Go to db/migrate

Find a file with a similar name: 20130418195753_add_fields.rb

Open it and replace 

```ruby
  def up
  end

  def down
  end
end
```

With

```ruby
  def up
    add_column :ideas, :votes, :integer, :default => 0
    add_column :ideas, :score, :decimal, :default => 0
  end

  def down
    remove_column :ideas, :votes
    remove_column :ideas, :score
  end
end
```

Run the command `rake db:migrate`


Open app/controllers/ideas_controller.rb

Before the very last

```ruby
end
```

Add

```ruby
def vote
  idea = Idea.find(params[:id])
  idea.votes += 1
  idea.score = idea.score + params[:idea][:score].to_d
  idea.save!
  redirect_to '/'
end
```

Open /config/routes.rb

Replace

```ruby
resources :ideas
```

With

```ruby
resources :ideas do
  member do
    post 'vote'
  end
end
```

Open /app/views/ideas/index.html.erb

After

```ruby
<td><%= link_to 'Destroy', idea, method: :delete, data: { confirm: 'Are you sure?' } %></td>
```

Add

```ruby
<td><span id="score"><%= idea.score.to_d / idea.votes %></span></td>

<td>
  <%= form_for(idea, url: vote_idea_path(idea), method: 'post') do |f| %>
    1 <input type='radio' name='idea[score]' value='1' />
    2 <input type='radio' name='idea[score]' value='2' />
    3 <input type='radio' name='idea[score]' value='3' />
    4 <input type='radio' name='idea[score]' value='4' />
    5 <input type='radio' name='idea[score]' value='5' />
    <button>Vote!</button>
  <% end %>
</td>
```

After

```html
<th></th>
```
Add

```html
<th>Score</th>
<th width="20%">Vote</th>
```
