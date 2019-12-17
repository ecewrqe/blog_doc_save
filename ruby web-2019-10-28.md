[document](https://devdocs.io/rails~5.0/ "document")

## ruby rail

#### init
```
ruby --version (2.5.3)
gem install rails
rails --version (5.2.3)
```
1, install yarn
2, gem install bundler
- create project
```
rail new project
```

```
rails generate controller Name [action...]
```

controller:
app->controller->app_controller.rb
```
class OtherController < ApplicationController
	def index
	end
	def help
	end
end
```

route:
config->route.rb
```

```

templates:
app->views->name->action.html.erb
app->views->layouts->application.html.erb
```
<%= stylesheet_link_tag 'stylesheet' %>
<%= javascript_pack_tag 'javascript' %>
<%= csrf_meta_tags %>
```
config->initializers->assets.rb
```
Rails.application.config.assets.precompile += %w( ss.css other.js ... )
```



models:
```
rails generate model <model name> col01:String:unique col02:integer:xxx
rails destroy model <model name>

```

app->models->
db->migrate

start:
```
rails server
-p:port, -b:bind ip, -d: deamon,
```


#### Http

#### template

```
<%= expression %>

<%= @this_variable %>
<%= func p1, p2, p3, p4:value %>

<% for i in xx %>
<% end %>

<%  %>
```

#### static
- stylesheets
app->assets->stylesheets->.scss

- images
app->assets->images->xxx

- javascripts
app->javascript->channels->action's js
app->javascript->packs->application.js

```
require("@rails/ujs").start()
require("turbolink").start()
require("@rails/activestorage").start()
require("channels").start() // action js

```


#### route


```
# 明るく
get '/welcome/index'
post '/welcome/index'

# parameters
get '/welcome/index/:id', id=>/\d+/
params:=> :controller, :action
controller: params

get '/welcome/index/:id' => "welcome#index"
第一引数はurlのパターン、第二引数はコントローラーの実体
get, post, matchそれらはすべて適用


match ':controller/:action/:arg', via: [:get, :post], as: "alias name"

controller: redirect_to xxx_url
###############

controller :blog do
	match "index/:id" => "blog#index", constrains: {
		id: /xxx/
	}
end
```

- resource

```
resource welcome do
	match "login" via: [:get, :post]
end
# /welcome/login
```

#### authentication
http_basic_authenticate_with name, password, except





