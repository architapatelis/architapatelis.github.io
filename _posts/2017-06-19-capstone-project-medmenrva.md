---
layout: post
title: Capstone Project - Medmenrva
---

My approach for this project was simple, I wanted to test myself. As a student at Bloc I've learned JavaScript and Ruby on Rails, but those were not the languages I wanted to use for my project. Not because I don't like them, but because I wanted to see if I can independently learn a new programming language. Not just understand it, but learn enough to actually build something with it. It was important for me to know that because, when I search for jobs or internships I always come across companies that are doing some interesting work. But, they aren't necessarily looking for JS and Ruby programmers. Creating the [Medmenrva](https://github.com/architapatelis/medmenrva) app gave me the much needed confidence that, while I might not become an expert at any one language in the short time that I have been programming, I can certainly build upon the basic principles of programming and learn all sorts of new things. The [Medmenrva](https://github.com/architapatelis/medmenrva) app is simple, it isn't ground breaking by any means but, independently achieving even the smallest tasks made this a fun project to work on. If there is anything that I have learned about coding, it's that if you aren't having fun along the way you will never be good at programming. Because then it's just about - great how do I solve this problem or I am so tired, why can't I fix this bug.

[Medmenrva](https://github.com/architapatelis/medmenrva) is actually a very useful app, often times people have to take several medications in a given day. They have to take these medicines at different times, most often with different dosages and there might be specific directions associated with each medicine. I wanted to create a reminder app, that uses text messages to alert users when it's time to take their medicine. I decided to use Python for this project. It has some similarities to Ruby but also differs in a number of ways, giving me the opportunity to learn new things. More importantly however, Python is a regarded as one of the more popular languages.

I used [Twilio](https://www.twilio.com/) for the text messaging service. The nice thing about using Twilio, is that they have great [documentation](https://www.twilio.com/docs/api/rest/sending-messages) on how to use their API.

Before starting this project I hadn't worked with any cloud computing platforms, so that was also something new and challenging for me to learn. I ended up using Google App Engine to host my application in Google managed data centers. The nice thing about using Google App Engine is the fact that just about everyone these days has a Google account. Users visiting the Medmenrva website have access to the home page, beyond that they must be registered Medmenrva members. But, before they signup for Medmenrva they need to login to their Google account, this step takes care of user authentication for us. You can read more about user authentication and creating the login and logout urls - [here](https://cloud.google.com/appengine/docs/standard/python/users/loginurls).

The Cloud Datastore uses a structured data model, and each data object is stored as an [entity](https://cloud.google.com/appengine/docs/standard/python/datastore/entities). Here is an example of the Medicine model, it's used to create Medicine entities that have several properties (name, directions, dosage etc.).

```python

from google.appengine.ext import ndb
from datetime import datetime

class Medicine(ndb.Model):
    name = ndb.StringProperty()
    directions = ndb.StringProperty()
    dosage = ndb.IntegerProperty()
    interval = ndb.IntegerProperty()
    quantity = ndb.IntegerProperty()
    date_and_time = ndb.DateTimeProperty()
    timezone = ndb.StringProperty()
    member = ndb.KeyProperty()


```

To save an entity in the Datastore you would use the ```put()``` method. That's great, now you know how to create an entity and store it, but how do you retrieve that stored information? You can read about query for retrieving entities from the App Engine Datastore - [here](https://cloud.google.com/appengine/docs/standard/python/datastore/queryclass). I will give you an example of how I retrieved data in Medmenrva. What if I want to get the list of all the medicines for a particular member? Here's how I did that:

```python

def get_medicines_by_member_key(member_key):
        return Medicine.query(Medicine.member==member_key).fetch()


```
The ```fetch()``` method is used to obtain a list of all matching entities.

On the other hand, if I want a specific medicine, I would use the ```get()``` method to obtain the first single matching entity found in the Datastore. Like so:

```python

def get_medicine_by_key_id(key_id):
        med = None
        try:
            med = ndb.Key(urlsafe=key_id).get()
        except:
            med = None
        return med


```

Personally for me the toughest part of creating this app was figuring out how Google App Engine works. How to create entities, what a Key is and what are the available methods. Once you figure out how to use the App Datastore to save and retrieve information the rest of the code becomes rather simple. Working on this project was a great learning experience. As a student my knowledge of all the possibilities in the programming world is still some what limited. Up until I did this independent project, I always followed a guideline, which makes things easier. But, when you have to go about reading documentation on your own and put things together from scratch, you don't realize the complexity of even the most basic app. It's when the fun begins(or torture, however you want to look at it).
