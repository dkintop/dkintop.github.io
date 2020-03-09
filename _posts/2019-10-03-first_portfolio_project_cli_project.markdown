---
layout: post
title:      "First Portfolio Project: CLI project"
date:       2019-10-03 18:08:31 -0400
permalink:  first_portfolio_project_cli_project
---



For a demonstration of how my CLI application functions, go to  [https://www.youtube.com/watch?v=os0v-eajw5s](http://)


For my CLI project I created a CLI application that could scrape data from [https://www.beeradvocate.com/beer/top-rated/](http://) and use that data to create new instances of a Beer class that I would then use to display the information of a specific beer from the top 50 ranked beers on the previously mentioned site. Let's start with the Beer class to know what kind of information we will be pulling from the website.  


**The Beer class**
```
class Beer  
  attr_accessor :name, :brewery, :type, :abv
  @@all = []
  
 def initialize(name, brewery, type, abv)
    @name = name
    @brewery = brewery 
    @type = type
    @abv = abv
    @@all << self
  end 
  
 def self.all 
    @@all
  end 
end
```


When defining our beer class we use an attr_accessor method to define our object attributes so that we can both set and get these attributes by calling them as instance methods on an instance of the Beer class. At time of initialization of a new beer instance, it will be initialized with all required information to set the values of those attributes.Notice the @@all class variable set to an empty array and that our initialize method uses a shovel operator to add each new Beer instance to the @@all array, we will be using that later. Next, I'll show you how I retrieved the data and used that data to instantiate our new Beer objects.

**The Scraper class**

The Scraper class is the "meat and potatoes" of this application. All of the code in this class allows us to scape our data using the 'nokogiri' and 'open-uri' gems that I installed in the gemfile, as well as instantiate a new Beer object for each beer in the top 50 ranked beers from BeerAdvocate.com. 

```
class Scraper 
  
  
  def self.scrape
    doc = Nokogiri::HTML(open('https://www.beeradvocate.com/beer/top-rated/'))
    all_beers = doc.search(".hr_bottom_light[@align='left']") 
    i = 1
    all_beers.each do|beer| 
      if beer.css('a b').text != "" && i <=50
        name = beer.css('a b').text
        brewery = beer.css('span.muted a').first.text
        type = beer.css('span.muted a')[1].text
        abv = beer.css('span').text.split("|")[1].strip
        i += 1
       Beer.new(name, brewery, type, abv)
      end
    end
  end 
end 
```

This class contains only one class method to be called on the Scraper class itself (I'll get to how and when we do that).
It starts by opening the website we are scraping out data from with the first line ` doc = Nokogiri::HTML(open('https://www.beeradvocate.com/beer/top-rated/'))`. This returns all of the xml data on the entire page, so we have to narrow it down. The next line set to the "all_beers" variable uses the nokogiri method `.search` to return an array of all xml elements that contain the css elements provided as an argument. Now at this point, we are getting closer, but I was still returning too much information that was making it difficult to iterate over using the `.each` method. To fix this issue I used an if statement that would only select the elements that also contained a specific css selector, as I found that all of the elements I was attempting to select contained this particular selector and the ones I did not want did not contain it. I also used another conditional statment using a counter variable since all elements passed the 50th element began throwing errors due to alterations in the css selectors. Once I narrowed it down to this point I was able to pull out specific data and use some problem solving skills to parse the data into the format that I wanted and set the name, brewery, type, and abv variables. I then was able to use these variables as arguments to initialize a new Beer object for each beer represented in the top 50 with the line that reade ` Beer.new(name, brewery, type, abv)`. Cool! All the heavy lifting was done in one method, and since our initialize method in our Beer class adds each instance created to the @@all array, we now have a collection of all 50 beers just by calling our class method 'scrape' on our 'Scraper class'.

**The Interface class**

This class is where we set up our user interface to allow the user to interact with the application. I will spare you the details, since it is mostly a lot of control flow using if statements and while loops, but I do want to draw attention to a few methods that I used here.

```
 def run
    create_beer_objects
    script
  end 
  
  def create_beer_objects
  Scraper.scrape 
  end
```

Our 'create_beer_objects' method calls on our class method 'scraper' to instantiate all of our Beer objects and we call that first in our run method so that the rest of our program will know about them and be able to run properly when we call our class methods on our beer objects to obtain the individual attribute values. I'll provide the rest of interface methods below in case you are curious how they all flow together. I had a lot of fun with this project and really solidified my understanding of object relationships and the improtance of understanding your return values, especially important when dealing with scraping.





```
  def age_verifier
    puts "Top Beer Finder"
    puts "Welcome, before beginning please enter your age."
    age = gets.chomp.to_i 
    if age < 21 
    puts "Sorry, You must be at least 21 years old to use Top Beer Finder"
    puts "Program will self destruct in 5 seconds"
    sleep(5)
    end
    age 
  end 
  
  
  def list_beers
    Beer.all.each_with_index do |beer_object, index|
      puts "#{index +1}. #{beer_object.name}"
    end
  end 
  
  def choice_messages
    ["Good choice!", "Nice pick!", "That's a good one!", "Great!, here you go!", "One of the best!"]
  end 
  
  
  def select_beer
    puts "---------------------------------------------------------------------"
    puts "Please enter the number of the beer you would like to know more about"
    input = gets.chomp.to_i
    if input >= 1 && input <= 50
      puts "---------------------------------------------------------------------"
      puts choice_messages.sample
      puts "Rank: #{input}"
      puts "Beer name: #{Beer.all[input - 1].name}"
      puts "Made by: #{Beer.all[input - 1].brewery}"
      puts "Type: #{Beer.all[input - 1].type}"
      puts "ABV: #{Beer.all[input - 1].abv}"
      puts "---------------------------------------------------------------------"
    else 
      puts "Please select a valid option"
    end
  end 
  
  
  def script   
    if age_verifier >= 21
      input = nil
      while input != "exit"
        puts "---------------------------------------------------------------------"
        puts "Top 50 craft beers"
        puts "---------------------------------------------------------------------"
        list_beers
        select_beer
        puts "To know more about a different beer on our list press enter, to exit type 'exit' and press enter."
        input = gets.chomp 
      end 
      puts "---------------------------------------------------------------------"
      puts "Bye!"
      puts "All product information displayed by this application provided by:" 
      puts "https://www.beeradvocate.com/beer/top-rated/"
    end
  end 
end
```




