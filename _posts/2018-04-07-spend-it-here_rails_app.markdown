---
layout: post
title:      "spend-it-here-rails-app"
date:       2018-04-07 20:40:30 -0400
permalink:  spend-it-here_rails_app
---


Up until now I've built a CLI gem that scrapes CoinMarketCap.com and a logging app in Sinatra designed to track cryptocurrency trades. For my Rails app, I wanted to follow this theme of cryptocurrency but focus on my favorite aspect of the scene: adoption.

I get very excited envisioning a future in which there exists a self-sustaining ecosystem of businesses, individuals, and communities that exchange the value of their goods and services entirely in cryptocurrency. Seeing as how adoption is stil in its infancy, as are many of the projects in the cryptocurrency space, I wanted to develop an easy way for users to find out where they can spend their coins/tokens.

There  are undeniable deterrents for consumers to spend in cryptocurrency. For example, the U.S. government views crypto as an asset, thus, leaving consumers liable to a capital gains tax calculated from acquisition to time of transaction/sale. Additionally, crypto, currently, is wildly volatile, which can make it impractical for both businesses to accept it and for users to spend it. The value the assets once had on one day may carry 70% less of its purchasing power by week's end.

Yet, there are the brave few who do it anway!

So, this app is for y'all.

### Breaking it down:

I used devise for my User model. Users can create an account by signing up.
![sign up](https://i.imgur.com/0UjUNxK.png)

I'm eatmytacos@gmail.com for illustrative purposes.
![log in](https://i.imgur.com/I7TTvYV.png)

If I'm unhappy with my account, I can cancel it. Or I can change my password.
![edit account](https://i.imgur.com/23JNHaA.png)

Here's my barebones navbar, void of any styling. I wanted to get everything working before using bootstrap to style my code:
![navbar](https://i.imgur.com/0jB6HDF.png)

Now that I'm signed in, I can add a cool business I stumbled upon, or know of, that takes crypto payments:
![new business](https://i.imgur.com/mC5Jl17.png)

I'm not entirely satisfied with the massive list of checkboxes, but drop down menus for businesses that take upwards of 10 currencies seemed more inconvenient than checking off boxes of the top 100 scraped coins all in one go.

Alternatively, I can leave a review and create a business in an existing or a new location, simultaneously. This is made possible through aggressively nesting forms. It took a long time for me to figure out the syntax of my strong params in the `ReviewsController`, but it was worth it, and I definitely feel I have a stronger grasp on nested forms than how I felt while I was going through the units in the Learn curriculum.
![add a review](https://i.imgur.com/EDoV7tN.png)

Both the businesses and reviews are available to be edited. Businesses may be updated by all users, as this app is intended to be community managed--wikipedia style.

In the future, I want to create roles for my users and add moderators to keep things tidy and wholesome. I also want to find a more elegant solution for filters, as I know filtering is possible through scope methods in my models. I've created a class that acts as a join table for three of my models--Business, Crypto, and Location.

Through this join table, I want to be able to query my database in order to find businesses that accept "Bitcoin" or "Ethereum." Alternatively, I can search for a location, "Brooklyn," to see which businesses I can spend what coins at. I wanted to make a join table with three foreign keys because I recognize that some businesses are franchises and can exist in multiple locations, though they may possess the same name. I'm still open to re-working my models and databases in the future, and I haven't delved deeply into every single possibility of scope methods for the Spendable model and controller because I'd like to explore whether or not there's a way to filter my data without refreshing the page. (Hello Javascript!)

Here's how I scope for `:offer_discounts` in my Business model:

```
class Business < ApplicationRecord
  belongs_to :location
  belongs_to :category
  has_many :spendables
  has_many :cryptos, through: :spendables
  has_many :items
  has_many :reviews
  has_many :users, through: :reviews

  validates :name, presence: true
  validates :description, presence: true
	
	def location_attributes=(location_attributes)
    self.location = Location.find_or_create_by(location_attributes) if !location_attributes.values.include?("") && !self.location_id
    save
  end

    def crypto_attributes=(crypto_ids)
      crypto_ids.values.first.reject { |value| value.to_s.empty? }.each do |id|
        spendable = Spendable.find_or_create_by(location_id: self.location.id, crypto: Crypto.find_by(id: id), business_id: self.id)
        self.spendables << spendable if !self.spendables.include?(spendable)
      end
			
      self.save
    end

  scope :offer_discounts, -> (offer_discounts) { where(discount_offered: true) }

```

My url looks like this: ![offer discounts](https://i.imgur.com/w4aAhRH.png)

which will render this page in the browser:
![offer discounts2](https://i.imgur.com/eamhfv8.png)

Since businesses belong to a category, I've created a categories index to make it easier to search for consumer needs:
![categories index](https://i.imgur.com/t9BK6hx.png)

Last but not least, it's possible to route to `/reviews`, which will lead to an index of all reviews ever created, or a user can simply go to their reviews via `/users/:id`:
![user show page](https://i.imgur.com/vsXmXIj.png)

There's some silly data in there, but that unhappy customer's CVS review is legit. Got that off of yelp.

I found it hilarious that it just so happened to be a review about botched EOS chapstick. Shoutout to EOS haters/lovers. I see you.

### Conclusion:

I'm really excited to continue working on this app. I still have to work out the kinks for Google OAuth2, which I plan on working out tomorrow. For now, Spend It Here is using GitHub's omniauth, but I look forward to familiarizing myself with more authentication protocols in the future. All in all, I can see this app developing into a fun little passion project. I recently discovered Heroku, so maybe I'll go ahead an deploy it after I learn how to apply a more presentable front-end. Hehe.

[Here's the link to the repo!](https://github.com/rh24/spend-it-here-rails-app)
