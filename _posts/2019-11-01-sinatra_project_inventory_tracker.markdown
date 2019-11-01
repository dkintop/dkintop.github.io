---
layout: post
title:      "Sinatra Project: Inventory Tracker"
date:       2019-11-01 15:52:43 +0000
permalink:  sinatra_project_inventory_tracker
---


github repo: [https://github.com/dkintop/sinatra-inventory-tracker](http://)

For my second portfolio project we werre assigned to develop an application using Sinatra as our web framework and ActiveRecord as the ORM tool for interacting with our SQLite databse. This application uses a MVC structure that makes editing current features and adding additional features fairly easy. I decided to create a basic inventory tracker.

How it works:

This application has three object models that include a user, a store, and an item class. The relationships are set up so that a user *belongs to* a store and an item *belongs to* a store, therfore a store *has many* users and *has many* items.
The reason I chose to structure my relationships in this way is because I wanted a store to be able to have multiple users who can then edit that stores inventory, however I did not want a user who is associated with store A to be able to edit inventory information of store B. Upon registration, the user must input an ID that correlates to an existing store in the database or register a new store. Through the use of a foreign key on the user model and the item model, the user gets assigned a store based on the store ID that they registered with. The item index page is setup in such a way that users can only view and edit items that have the same store ID as the current user through the following logic:
```
get '/items' do 
    @items = Item.all.select{|item| item.store == current_user.store} 
    @store = current_user.store
    erb :'items/index'
end
```
This route then renders the Item index page and displays only the items  of the store that the current user is assigned to (the current_user method is a method defined in the application controller that returns the user object of the user who is currently logged in through checking session[:user_id] and finding the user with that same id). @items will then return an array of all items that belong to the store that the user belongs to.

item index page: 
```
<table>
   <caption> Inventory </caption>
    <tr>
        <th>Item</th>
        <th>Price</th>
        <th>Count</th>
    </tr>
    <% @items.each_with_index do |item, index| %>
        
        <tr>
            <td><%=index +1%>. <a href='/items/<%=item.id%>'><%= item.name.capitalize %></a></td>
            <td>$<%=item.price%></td>
            <td><%=item.count%></td>
        </tr>
    <% end %>
    
</table>

<a href='/items/new'><p class= 'link'> Add New Item </p></a>
```

On the item index page we are able to take the @items variable and dynamically create table rows for each item in @items through the use of erb tags. Through creating the table rows dynamically, we ensure that all rows will be populated and there will always be enough rows for our data to fit into. From here the user can then add a new item to this stores inventory list, edit an existing item, or log out whcih will redirect the user to the registration page. 

Overall this project really helped understand how form data is processed by a controller action and then can be used in all sorts of ways to create an application that is dynamimc and useful in solving real world problems. 
