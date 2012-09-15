## Style your links
Add css class to your link element
Open /app/views/ideas/index.html.erb

Replace

```ruby
<td><%= link_to 'Show', idea %></td>
```

With

    <td><%= link_to 'Show', idea, class: 'test' %></td>

Add CSS to design this link element

Open /app/assets/stylesheets/application.css

Add

    a:visited.test { font-weight: bold;  color: red;}
    a.test { font-weight: bold; color: red;}

## Shorten the description on index
Open /app/controllers/ideas_controller.rb

After

    @ideas = Idea.all

Add


    chars_to_display = 5
    @ideas.each do |x|
      if x.description.length > chars_to_display
        x.description = x.description[0, 5] + '...'
      end
    end

## Add voting feature to your app

Open db/schema.rb

Under

    t.string   "picture"

Add

    t.integer  "votes", :default => 0
    t.decimal  "score", :default => 0

run `rake db:setup`

Open app/controllers/ideas_controller.rb

Before

    end

Add

    def vote
      idea = Idea.find(params[:id])
      idea.votes += 1
      idea.score = idea.score + params[:idea][:score].to_d
      idea.save!
      redirect_to '/'
    end

Open /app/views/ideas/index.html.erb

After

    <td><%= link_to 'Destroy', idea, method: :delete, data: { confirm: 'Are you sure?' } %></td>

Add

    <td>Score: <span id="score"><%= idea.score / idea.votes %></span></td>
    <td>
      <%= form_for(idea, url: vote_idea_path(idea), method: 'post') do |f| %>
        1<input type='radio' name='idea[score]' value='1' />
        2<input type='radio' name='idea[score]' value='2' />
        3<input type='radio' name='idea[score]' value='3' />
        4<input type='radio' name='idea[score]' value='4' />
        5<input type='radio' name='idea[score]' value='5' />
        <button>Vote!</button>
      <% end %>
    </td>

After

    <th></th>

Add

    <th></th>
    <th></th>

Open /config/routes.rb

Replace

    resources :ideas

With

    resources :ideas do
      member do
        post 'vote'
      end
    end
