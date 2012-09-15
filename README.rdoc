1. Open db/schema.rb
2. Under
    t.string   "picture"
Add
    t.integer  "votes", :default => 0
    t.decimal  "score", :default => 0
3. Open app/controllers/ideas_controller.rb
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
