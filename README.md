1. Open db/schema.rb

Under

    t.string   "picture"

Add

    t.integer  "votes", :default => 0
    t.decimal  "score", :default => 0

3. run ´rake db:setup´

4. Open app/controllers/ideas_controller.rb

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

5. Open /app/views/ideas/index.html.erb

After

    <td><%= link_to 'Destroy', idea, method: :delete, data: { confirm: 'Are you sure?' } %></td>

Add

    <td>Score: <span id="score"><%= idea.score / idea.votes %></span></td>
    <td>
      <%= form_for(idea, url: vote_idea_path(idea), method: 'put') do |f| %>
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

6. Open /config/routes.rb

Replace

    resources :ideas

With

    resources :ideas do
      member do
        post 'vote'
      end
    end
