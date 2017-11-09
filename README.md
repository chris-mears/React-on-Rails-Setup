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
rails db:migrate //only after everything is good to go
```
3. setup routes
```Ruby
namespace :api do
resources :resource_name
end
```

4. Set up controller methods

CRUD, white list params

<details>
  <summary> Example Code</summary>
  <p>
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
5. Set up React
```Javascript
create-react-app client
cd client
npm i axios styled-components react-router-dom
```
  </p>
</details>


5. Create a package.json file at the root level and add this JSON
```Javascript
{
  "name": "YOUR PROJECT NAME",
  "engines": {
    "node": "8.9.0"
  },
  "scripts": {
    "build": "cd client && npm install && npm run build && cd ..",
    "deploy": "cp -a client/build/. public/",
    "postinstall": "npm run build && npm run deploy && echo 'Client built!'"
  }
}
```
Note: "YOUR PROJECT NAME" should be the name of your root directory

6. Set up a proxy for our dev server within the client level `package.json
```Javascript
"proxy": "http://localhost:3001",
```
7. Install forman if not installed already
```
gem install foreman
```

8. Create a Procfile.dev
```
web: sh -c 'cd client && PORT=3000 npm start'
api: rails s -p 3001
```

9. Create a Procfile
```
web: rails s
```

10. Test app in development
```
foreman start -f Procfile.dev
```

11. Setup Heroku
```Ruby
heroku create
# Tell Heroku that you want both the Ruby and Node environments to build your project in.
heroku buildpacks:add --index 1 heroku/ruby 
heroku buildpacks:add --index 2 heroku/nodejs

git push heroku master

# Set up your production database
# After updating seed file
heroku run rails db:migrate db:seed 

#If you need to create db first
heroku addons:create heroku-postgresql:hobby-dev
```





