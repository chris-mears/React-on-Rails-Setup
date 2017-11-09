# React-on-Rails-Setup
1. Set up new rails
```
rails new //app_name --api -T -d postgresql
cd into root directory
rails db:create
rails s //to test app is running
```

2. set up controllers, models and routes
```
rails g model //Model fied name
```
```
rails g controller //PluralModel
```
```Bash
rails db:migrate //only after verything is good to go
```
3. setup routes
```Ruby
namespace :api do
resources :resource_name
end
```

4. Set up controller methods
...CRUD
...white list params
...example
```Ruby
class Api::CreaturesController < ApplicationController
  def index
    @creatutes = Creature.all
    render json: @creatures
  end

  def show
    creature_id = params[:id]
    @creature = Creature.find_by_id(creature_id)
    render json: @creature
  end

  def create
    @creature = Creature.new(creature_params)
    if @creature.save
      render json: @creature
    end
  end

  def update
    creature_id = params[:id]
    @creature = Creature.find_by_id(creature_id)
    @creature.update_attributes(creature_params)
    render json: @creature
  end

  def destroy
    creature_id = params[:id]
    @creature = Creature.destroy(creature_id)
    render json: {
      msg: "Delete Successful"
    }
  end

  private

  def creature_params
    # whitelist params return whitelisted version
    params.require(:creature).permit(:name, :description)
  end
end
```






