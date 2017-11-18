---
layout: post
title:      "Sinatra Portfolio Project"
date:       2017-11-18 15:05:58 -0500
permalink:  sinatra_portfolio_project
---

##Read the Docs
     Flipping (metaphorically) through the Ruby documentation was something done out of necessity. It is a reference text more akin to a dictionary than a manual. Months ago I toyed with the idea of making flashcards for the proper implementation and syntax for various methods because I've always felt flashcards are an extremely effective method for rote memorization. I decided not to, arguing that I would naturally absorb more information navigating the docs as needed than attempting to anticipate what might be most useful from a place of relative ignorance. I may revisit that idea in the near future but in the present I find myself consulting the documentation more during the refactoring process, to try and make my code more readable and/or concise. 
		 I tried to use the Active Record documentation in the same manor while building my Sinatra portfolio project. I found that the material available couldn't be more different from the Ruby Docs. The Active Record guide was worthy of a complete read through in a way I had previously not encountered. It was on my second or third reading while trouble shooting my application that I realized the raw power of validations and callbacks for active record models. Shaping and customizing models beyond simple associations is the literal foundation the MVC architecture. 
		 
		 The problem with my Sinatra app, like the majority of the problems I encounter writing code, was in the most obvious of details. The completed, working code for my object model shown below utilizes a callback to a for a method in a separate module. 
		 
```
class Bird < ActiveRecord::Base
  extend Wiki::ClassMethods
  include Wiki::InstanceMethods

  validates_presence_of :species, allow_blank: false
  validates :species, uniqueness: { case_sensitive: false }
  before_save :set_wiki_info 
  has_many :sightings
  has_many :users, through: :birds

end # End of Class
```

I used the wikipedia client gem in lieu of building my own scraper because I thought it would be good to try source and learn how to implement a gem without being taught, as I was with Nokogiri. My Wiki module includes methods to obtain information from wikipedia and combines those methods into a single methods executed by a callback to add information to the object prior to being saved. Some of the methods shown below were written for use by features that were not included. 

```
module Wiki
  module InstanceMethods
    def set_wiki_info
      if !self.species.nil?
        self.summary = summary
        self.image_url =  main_image_url 
      end
    end  

    def wiki_scrape
      Wikipedia.find( self.species ) 
    end
      
    def main_image_url
      wiki_scrape.main_image_url
    end

    def summary
      wiki_scrape.summary
    end

    def summary_parse
      sum_array = wiki_scrape.summary.split(". ")
      sum_array[0..4].join(". ") + "..."
    end

    def page_url
      wiki_scrape.fullurl
    end
  end 

end  #  End of Module
```

I originally included an 'update' in my set_wiki_info method, which when combined with any callback, created an infinite loop executing the callback then the method over and over. Once I realized where the problem existed it was a simple fix but in the process I was forced to examine and try and implement all of the relevant callbacks. Ultimately when done right, the model will take care of itself. 
