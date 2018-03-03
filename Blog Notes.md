# Blog Notes

## Starting Blog

`$ rails new tech_talent_blog`

`$ cd tech_talent_blog`

## Aside:

When we run *rails g scaffold*, we tend to take for granted what it's giving us.

The scaffold generate command is essentially a combination of two different generate commands:

```
$ rails g model Resource attribute:datatype attribute:datatype

$ rails g controller Resource index new show edit create update destroy
```

(though the latter would give us actions & views for "create", "update" and "destroy", while scaffold only gives actions)

In addition, *scaffold* sets up connections between these pages, which we would have to create manually otherwise.

Let's try making those connections on our own...

## Color Query Strings/Params

# Practice Controller

We at least need a controller and views to try this out, so let's build a practice controller:

```
$ rails g controller Practice index about
```

We're going to use these two pages to pass information back and forth,

using *Query Strings*

## What are Query Strings?

You've seen query strings before,

whether you realize it or not.

Here's a URL from a search on airbnb.com:

![img](https://s3.amazonaws.com/media-p.slid.es/uploads/264630/images/3446317/Screen_Shot_2017-01-30_at_12.41.52_PM.jpg)

![img](https://s3.amazonaws.com/media-p.slid.es/uploads/264630/images/1454117/red_arrow_up.png)

See that question mark in the URL? That's the beginning of a query string.

A query string holds additional information, stored in variables, which the URL may not address.

In this example, the URL handles the location,

while the Query String handles all other data (dates, # of guests, etc.)

<Image Source: <https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcT0OBKqXoeECQRImxl9MbE1-eJqFCgg531qT9RGfwbmTzyHeqcj-A> >

# Query Strings

![img](https://s3.amazonaws.com/media-p.slid.es/uploads/264630/images/3446317/Screen_Shot_2017-01-30_at_12.41.52_PM.jpg)

![img](https://s3.amazonaws.com/media-p.slid.es/uploads/264630/images/1454117/red_arrow_up.png)

The ampersands in the Query String separate different variable/value pairs.

![img](https://s3.amazonaws.com/media-p.slid.es/uploads/264630/images/1454117/red_arrow_up.png)

![img](https://s3.amazonaws.com/media-p.slid.es/uploads/264630/images/1454117/red_arrow_up.png)

So how can we attach query strings to our URLs?

It's all in the *links*!

<Image Source: <https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcT0OBKqXoeECQRImxl9MbE1-eJqFCgg531qT9RGfwbmTzyHeqcj-A>>

# config/routes.rb

```
get 'index' => 'practice#index'

get 'about' => 'practice#about'
```

### Index html links

In apps/views/practice/index.html.erb:

```html
<!-- index.html.erb -->

<h1>Choose a Color!</h1>

<p>
    <%= link_to "Red", about_path(color: "red") %>
</p>

<p>
    <%= link_to "Blue", about_path(color: "blue") %>
</p>

<p>
    <%= link_to "Green", about_path(color: "green") %>
</p>

<p>
    <%= link_to "Yellow", about_path(color: "yellow") %>
</p>
```

On link #5: 	link_to is rails way to do hyperlinks, "red" is the text that will be linked, about_path says I should go wherever routes said "about" should go, (color: "red") is a query string being passed through to give info on the other end.

# ACTUALLY STARTING BLOG

### Create Post Resource

It's time to use g scaffold!

Accuracy is important! Check your spelling:

Rails does not recognize the "sting" data type

Remember: what do we need to do after we scaffold?

```
$ rails db:migrate
```

Now **"Post"** is a *real* database table!

## SIDE QUEST: Added some content to play with.

# IMPORTANT: PULLING BOOTSTRAP

### Bootstrap is a major part of building a website and will need to be applied.

Let's use Bootstrap to make this app at least more readable.

But this time, let's do it the Rails ways: let's use Gems!

```rails
source 'https://rubygems.org'

git_source(:github) do |repo_name|
  repo_name = "#{repo_name}/#{repo_name}" unless repo_name.include?("/")
  "https://github.com/#{repo_name}.git"
end


# Bundle edge Rails instead: gem 'rails', github: 'rails/rails'
gem 'rails', '~> 5.0.1'
# Use sqlite3 as the database for Active Record
gem 'sqlite3'
# Use Puma as the app server
gem 'puma', '~> 3.0'
# Use SCSS for stylesheets
gem 'sass-rails', '~> 5.0'
gem 'bootstrap-sass'
gem 'jquery-rails'

#and more below

```



We already have the **sass-rails gem**,

we now need** **

**gem ****\*bootstrap-sass***

**\* **AND*

**\*gem jquery-rails***

And don't forgot to run *'\**bundle install**'*

## Don't forget to rename: in app/assets/stylesheets application.css to application.scss

#### this makes it SASS(bootstrap-sass) compatible.

then add:

```ruby
@import 'boostrap-sprockets';
@import 'boostrap';
```

### Add gems to 'application' in javascript:

```
//= require jquery
//= require jquery_ujs
//= require bootstrap-sprockets
//= require turbolinks
//= require_tree .
```



# LETS SET UP COMMENTS

in rails cmd we're going to create a scaffold:

```cmd
$ rails g scaffold Comment author:string comment_entry:text

$ rails db:migrate
```

### But...

So we can now create Comments...

but they're just out there in the ether, unattached to any blog post.

```cmd
$ rails g migration AddValueToComments post_id:integer

$ rails db:migrate
```

### Remember! in controller we have to add:

```
# Never trust parameters from the scary internet, 
# only allow the white list through.
def comment_params
    params.require(:comment).permit(:author, 
    :comment_entry, :post_id)
end
```

we only need to add :post_id to the end of this. The rest should be there.

### :post_id lets use create association's between the two resources.

#### Assiociations

##### there are many like it, but we are using One-to-Many. 

## What does that mean? 

one instance of a resource "has many" of the other, and those instances of the other resource only "belong(s) to" that instance of the first.

Still a parent-child relationship, but with several children. ex:

```
$ rails g scaffold State name:string

$ rails g scaffold City name:string state_id:integer
```

which does:

```
class State < ApplicationRecord

    has_many :cities

end
```

and the child being:

```
class City < ApplicationRecord

    belongs_to :state

end
```

### this will be put in app/models/comment.rb



# MOVING ON

### in _form we will add a post_id without the author knowing.

this looks like:

```

  <div class="field">
    <%= form.hidden_field :post_id, value: @post.id %>
  </div>
```

in the big bad code, it looks like this:

```
  <div class="field">
    <%= form.text_field :author, placeholder: "Written By..." %>
  </div>

  <div class="field">
    <%= form.text_area :comment_entry, placeholder: "Tell Me Something..."  %>
  </div>

  <div class="field">
    <%= form.hidden_field :post_id, value: @post.id %>
  </div>

  <div class="actions">
    <%= form.submit %>
  </div>
```



