# NAVBAR



```ruby
      <ul class="nav navbar-nav">

      </ul>
      <form class="navbar-form navbar-left">
 
      </form>
      <ul class="nav navbar-nav navbar-right">
      <li><%= link_to "Edit Profile", edit_user_registration_path %></li>
      <li><%= link_to "Make a New Post", new_post_path %></li>
      <li><%= link_to "Sign Out", destroy_user_session_path, method: :delete %></li>
      

      </ul>
```

to:

```ruby
<div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
      <ul class="nav navbar-nav">
        <% if user_signed_in? %>
        <li><%= link_to "Make a New Post", new_post_path %></li>
        <% end %>

      </ul>
      <form class="navbar-form navbar-left">
 
      </form>
      <ul class="nav navbar-nav navbar-right">
      <% if user_signed_in? %>
        <li><%= link_to "Edit Profile", edit_user_registration_path %></li>
        
        <li><%= link_to "Sign Out", destroy_user_session_path, method: :delete %></li>
      <% end %>

      </ul>
```

