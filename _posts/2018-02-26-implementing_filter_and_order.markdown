---
layout: post
title:      "Implementing Filter and Order "
date:       2018-02-26 18:19:36 +0000
permalink:  implementing_filter_and_order
---

In order to fullfill the requirements for my Rails project I needed to better familiarize myself with the use of scope methods. Scope methods appear identical to class methods when called. They are primarily used to locate records in a database. Unlike a class method, a scope method will always return an ActiveRecord relation, as opposed to 'nil', which can cause errors and require additional code to handle these cases. Though I did not utilize this functionaliy in my project, another major benefit to using scope methods is that they are extendible and can be provided additional methods for use on an object only when its parent scope method is called. For my purposes the most useful characteristic of scope methods is their ability to be used in conjunction with one another in a method chain. 

Thr first scope methods I implemented displayed the full collection of recipes from by the date created or by prep time (:oldest, :newest, :longest, :shortest). First, to display these methods as options in a select input, I created a helper method that simply returned a hash with a placeholder and symbol which would be passed in the parameters when selected.  A conditional statement in the index action, where the filter call was sent, redirects to a url mapped to a similarly named action calls the scope method and displays the ordered results by rendering the index view. 


Unfortunately this created a problem when combined with the recipe filtering methods. 

The filter parameters on my Recipe application allow a user to view recipes by the category and/or a single ingredient as well as be combined with . The parameters passed to the scope methods here are submitted via select input on the index page. 		 
	
		class Recipe < ApplicationRecord

			scope :by_ingredient,  ->(ingredient_id) {
				joins(:recipe_ingredients)
				.where("recipe_ingredients.ingredient_id = ?", ingredient_id)
		}
			scope :by_category, ->(category_id) {
				joins(:recipe_categories)
				.where("recipe_categories.category_id = ?", category_id)
		}	
		scope :by_user, ->(user_id){where(:user_id => user_id)}
	 	end
		
While implementing this feature I ran into an issue. If an order option is present and the browser is redirected to the feature url, and the required parameters( ingredient_id, category_id, and _user_id) are not passed on to the new request. To correct this, before every call to the index action those parameters are stored in the session hash with the key :filter_params. 

At this point in my implementation I have the ability to reorder the complete recipe collection or the recipes belonging to a single user, as well as the filter parameters required to query the database are stored in the session.  

The class method below creates a new hash of the filter_params stored in the session with a non-nil value and confirms that at least one filter parameter is present before calling any scope methods. 

    def self.filter_options(filter_params)
		 filter_keys = filter_params.select{ |k, v| v.present?}   
		 if filter_keys.present?
			 filter_keys.inject(self){|k, v| k.send(*v)}
		 else
			 Recipe.all
		 end
    end 

Passing self (Recipe) to inject I'm able to append each scope method call with its required argument, and create a single query.  The parameters of this query are determined by the non-nil values  stored in the session hash. 
                 `Recipe.by_category(category_id).by_ingredient(ingredient_id).by_user(user_id) `
  


