---
layout: post
title: Open Source Project: Ruby Gem Stockex
---

As a student at Bloc I had to create an open source project and so I decided to write a Ruby gem. That was the easy part, I had to then figure out what I want my gem to do, and what goals do I want to achieve with this project.

#### The Goals:

  - To test my knowledge of some of the things I had learned so far in Ruby.
  - To create a simple gem that other students can understand and if they'd like use as a model for their project.
  - To use the gem for some task I do in my personal life.

#### What should the gem do?

This was though to figure out, if you visit [RubyGems.org](https://rubygems.org/) you will see that there are several well written gems available for practically every purpose. So it's okay if your idea isn't completely unique as long as it's useful to you and a handful of other people. If you build a gem for yourself then it makes writing methods easier. For instance, every year during the tax season I have to do certain calculations for my stocks and for that I need specific values. So a gem that gets stock market values for me is perfect. And with time I can always add more functionality.

If you would like to follow along here is the source code on github: [Stockex](https://github.com/architapatelis/stockex)

## Getting Started

If you are going to build a gem that relies on an API do your research: is the API free or do you have to pay for it? What is the API call frequency? What is the format(JSON/CSV/XML) of the response? What are the values in the response, and how can you use them? I decided to use [Alpha Vantage](https://www.alphavantage.co/) their API is simple, easy to use and gives all the necessary values.

### Creating the Gem Layout:

If you haven't already installed the bundler gem, go ahead and do so:

```ruby
gem install bundler
```

Now, in your console navigate to the directory where you would like to create the `your_gem_name` directory and use the following command:

```ruby
bundle gem your_gem_name

```

This command will:

  - creates the directory and initial structure of the gem
  - initializes a git repository for the gem
  - writes an initial gemspec

### Make the appropriate changes to the your_gem_name.gemspec file

Here is an example of my file - [stockex.gemspec](https://github.com/architapatelis/stockex/blob/master/stockex.gemspec)

Make sure you add the appropriate development and runtime dependencies as well all the relevant information about your gem.

Note that `spec.files` are the files that will be included in your gem when it is built. These are the files that hold the code that needs to be executed when someone uses your gem. For instance in Stockex I used:

```ruby
spec.files         = Dir['lib/stockex.rb', 'lib/**/*.rb']

#for now you can do something like this:

spec.files         = Dir['lib/your_gem_name.rb', 'lib/**/*.rb']

```
_I will discuss this further later on._

Then install the dependencies specified in your Gemfile, using:

```ruby
bundle install

```

You are now ready to make your _initial commit_ and push the files to your remote repository.

Note: As you work on your project it's best to create new branches for each group of tasks.

## Creating a file structure

Now that you have a basic layout it's time to create some files and decide where the code should be placed.

With in `lib/stockex/equity.rb` I set up a `module Stockex` that has a `class Equity`. Therefore when a user wants to call a method in the Stockex gem, they would use: `Stockex::Equity.some_method_in_stockex_gem()`.

If you look at the `class Equity` [here](https://github.com/architapatelis/stockex/blob/master/lib/stockex/equity.rb) you will notice that it _includes_ several other modules. Each module represents a group of methods that perform certain tasks. For instance `Module Quotes` holds methods that get data related to time series(daily, weekly, monthly etc.) whereas `Module Indicators` holds methods that get data for certain stock indicators(obv, adx, aroon etc.).

This next part can cause some confusion so bare with me, have a look at the following [module Quote](https://github.com/architapatelis/stockex/blob/master/lib/stockex/indicators.rb)

This little bit of code might confuse you:

```ruby
module Quotes
    def self.included(stocks)
      stocks.extend(ClassMethods)
    end

    module ClassMethods
        #methods to get time series quotes go here
    end

```

`self.included` is called whenever this module is included (like it is in `class Equity`). When this happens, extend adds the ClassMethods methods to Quotes.
If there are any methods that are not within ClassMethods, then they are instance methods. For example:

```ruby
module Quotes
    def self.included(stocks)
      stocks.extend(ClassMethods)
    end

    def i_m_some_instance_method
        #some code goes here
    end

    module ClassMethods
        #methods to get time series quotes go here
    end

```

But why bother writing `self.include` and `module ClassMethods`? And in `class Equity` why not just use `extend Quotes` instead of `include Quotes`? Well, of course you could and your gem would work just fine. I chose to do it this way because what if down the line I wanted to add instance methods to `Module Quotes`? In that case, I can simply come and write the methods and not change any of the previous code, and everything would work just fine. I will leave the decision making up to you.

The other thing I want to mention are the `require` file paths, make sure if methods within one file rely on those within a different file you use `require` correctly.

In `class Equity` you will notice that there is a section of _private_ methods, these methods are called from within methods that belong to several different modules. Since, we want to keep our code dry it's best to place them here instead of placing them in every module.

### One final thing

In `stockex.rb` I have some code that is used to configure a users API Key, by initializing a new instance of `class Configuration`.

This brings me back to, the `stockex.gemspec` file:

```ruby
spec.files         = Dir['lib/stockex.rb', 'lib/**/*.rb']

```

If I didn't place `lib/stockex.rb` in there, then the gem wouldn't be able to configure the API key. If you are not placing any code within this file then you don't have to include it in `spec.files`.

## Ready to Build Your Gem!

Now you have some code, so you can test it out locally or use it as a gem in a ruby app. Simply use `rake install` to build your gem.

Be aware that the installed version of the gem has been compiled. If we change the source then you will have to uninstall/reinstall the gem to see the change.

```

gem uninstall your_gem_name
rake install

```
