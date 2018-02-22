---
layout: post
title:      "tradetracker-sinatra-app"
date:       2018-02-21 21:11:04 -0500
permalink:  tradetracker-sinatra-app
---

##### It feels like I've come such a long way from the Object Oriented Ruby units of the curriculum. Since then, we've covered ORMs, ActiveRecord, Rack, and Sinatra, which brings me here to talk about my Sinatra Portfolio project.

##### I felt that the labs and Fwitter project leading up to this assignment adequately prepared me to build my own web app from scratch. Similar to my CLI data gem project, it helped motivation wise to pick a project that I'd find personally useful, so I built a simple trade tracking app that allows users to log trades they've made throughout the year/years past.

##### Additionally, I wanted to create the option of making publicly visible profiles along with publicly and privately visible trades.

##### As an aside, I'm really feeling the use of my github repo names as the title of my blog posts on the projects. I did this by creating a column in my `create_trades` migration called `viewable` that would take in a boolean value, with its default value being false.

```
class CreateTrades < ActiveRecord::Migration[5.1]
  def change
    create_table :trades do |t|
      t.boolean :viewable, default: false
      t.string :fiat_symbol
      t.string :coin
      t.integer :quantity
      t.integer :buy_value_fiat
      t.integer :sell_value_fiat
      t.string :date
      t.text :notes
      t.integer :user_id
    end
  end
end

```

##### For the sake of illustrating my next discussion topic, I'll go ahead and paste the rest of my migrations:

```

class CreateYears < ActiveRecord::Migration[5.1]
  def change
    create_table :years do |t|
      t.integer :year
    end
  end
end

class CreateUsers < ActiveRecord::Migration[5.1]
  def change
    create_table :users do |t|
      t.string :username
      t.string :email
      t.string :password_digest
    end
  end
end

```

##### The above migrations allowed me to relationally map these table to my `Year` class and `User` class, respectively.

##### Here, is the form with which a user would log a trade:
![](https://imgur.com/CGLa6vX)

##### Even though my `Trade` class validates input via `validates_presence_of`, and although this is useful when testing out objects and object relationships in the console, if a user simply leaves a field blank, the `params` hash will still create a value of an empty string `""` for each indicated key via `name=` inputs in a form. Extra sanitation is absolutely necessary! I accomplished this by creating an array of `params` values that I did not want to be an empty string. The only field I wanted to be left optionally 'blank' was the Notes: section.

``` 
  post '/trades' do
    trade = current_user.trades.build(params)
		
    if trade.save
      trade_year = Year.find_or_create_by(year: params[:date][0..4].to_i)
      useryear = UserYear.find_or_create_by(user_id: current_user.id, year_id: trade_year.id)
      redirect to '/trades'
    else
      flash[:message] = "Please, fill out fields with valid inputs."
      redirect to '/trades/new'
    end
		
    need_valid_input = [params[:coin], params[:quantity], params[:fiat_symbol], params[:buy_value_fiat], params[:sell_value_fiat], params[:viewable], params[:date]]
		
    if need_valid_input.include?("")
      flash[:message] = "Please, fill out fields with valid inputs."
      redirect to '/trades/new'
    end
  end
	
	```



##### Ultimately, I ended up with a product that renders a User Profile page like so:
![](https://imgur.com/a/uulqJ)

##### One of the more challening problems I stewed over for this project was how I'd update my `UserYear` table to reflect a change in a user's trade log. As an example, the image above showing the User Page displays that I made this trade in 2018. I wanted to know what would happen if I edited my trade's date to a day in 2017 instead of 2018. Would my Years Active: still show that I traded in 2018, even if I didn't have any other logs in that year?

##### Testing this question, showed that yes, it would. I needed to update my UserYear table to reflect the patch request for my trade.


```
patch '/trades/:id' do
    trade = Trade.find(params[:id])
    trade.update(coin: params[:coin], quantity: params[:quantity], buy_value_fiat: params[:buy_value_fiat], sell_value_fiat: params[:sell_value_fiat], date: params[:date], viewable: params[:viewable], notes: params[:notes])
		
    # Below lines will recreate a UserYear row reflect the new params[:date] input
		# while deleting the old date directly in trades/edit.erb
    # in order to update `Years Active: ` in users/private_show.erb
		
    trade_year = Year.find_or_create_by(year: params[:date][0..4].to_i)
    useryear = UserYear.find_or_create_by(user_id: current_user.id, year_id: trade_year.id)

    if trade.save
      redirect "/trades/#{trade.id}"
    else
      flash[:message] = "Please, fill out fields with valid inputs."
      redirect "/trades/#{trade.id}/edit"
    end
  end
	```

##### My first step was to delete the `UserYear` row that indicated a trade made in 2018. I did this with the following code directly in my `trades/edit.erb` form.

```
<% UserYear.where(year_id: Year.where(year: @trade.date[0..4].to_i)).destroy_all %>
```

##### Now, if I wanted to change my trade to a date in 2017:
![](https://imgur.com/a/kLOxz)

##### This edit will show up in my User Profile page, in addition to updating the Years Active: section.
![](https://imgur.com/a/EelU8)

##### I'm looking forward to building more and sharpening my understanding of CRUD apps and Rack requests. If you want a closer look at Trade Tracker, check out the respository [here](https://github.com/rh24/tradetracker-sinatra-app)!

