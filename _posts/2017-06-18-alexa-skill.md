---
layout: post
title: Creating a Alexa Trivia Skill
---

As many of you know Amazon's Echo and Dot devices use the Alexa Voice Service(AVS) to allow users to play music, get the latest news updates, book vacations, order food, set alarms, make a to-do list, and more. As a student I was very interested in the concept but, aside from watching the Alexa commercial and seeing some Alexa related Youtube videos I had no real knowledge of how it all works. In order to create the Voice User Interface(VUI), it's important to understand how AVS works, how do we interact with it, what do the terms _intent_ and _utterances_ mean. Luckily Amazon has several sample projects on GitHub to help developers get started. To understand Alexa Skill development at a rudimentary level, I created two simple skills, [General Knowledge Trivia](https://github.com/architapatelis/alexa-trivia-skill-general-knowledge) (based on Amazon's [Sample Trivia Skill](https://github.com/alexa/skill-sample-nodejs-trivia)) and [Bee Facts](https://github.com/architapatelis/alexa-fact-skill) (based on Amazon's [Sample Facts Skill](https://github.com/alexa/skill-sample-nodejs-fact)). These sample projects are all based on a copy/paste approach, they walk you through Alexa skill setup in Amazon Developer Portal and how to create and link your Lambda Function to your newly created skill in the Developer Portal.

After going through some of the basic code I realized that an _intent_ is the action that Alexa performs, and this action is determined by what the user is requesting. For example, if the user asks for Help the _Help intent_ will handle that user request. So what's an _utterances_? These are the phrases that the user says in order to communicate with Alexa. It is important to note that in any spoken language there are many different ways to convey the same thing, we have to take that into account while providing a list of Sample Utterances. Once you make your changes to the sample _index.js_ you upload all of your files into a Amazon Web Services(AWS) Lambda Function, that will both run your code and scale your application. The important part here is 'run your code', in order for AWS lambda to run the Node.js code it has to be written using a particular syntax. I can read and write JavaScript, but integrating that with Alexa Skills specific syntax was the challenging part to understand.

### CUSTOM SKILL - [BOOK FINDER](https://github.com/architapatelis/alexa-skill-book-finder)

I am one of those people that always wants to read a book but, I never know what to read. I go to the book store look around and start googling to find best sellers in a specific genre. The book store never has good reception; so I get bored and I leave. Of course I could just google when I am at home, or I can just have Alexa tell me. Basically, my skill is created for me. The Book Finder skill is simple, users ask Alexa to recommend books in a particular genre and using the iDreamBooks API Alexa will give the user a list of bestsellers. Users can also pick a book number if they want to hear a snippet of the review.

As great as AVS is, you have to be aware of it's limitation. Sometimes Alexa won't understand what you are trying to say so it's important to test all the intent slot values. In the Book Finder skill, I created a custom list of all the valid genres and I had to test each one of them to make sure that Alexa was giving the right slot value to my lambda function. For instance sometime I would say "westerns" and Alexa would hear "restaurants", or I would say "health" and Alexa would hear "help".

#### Figuring out what's wrong with your lambda function:

It's great that once you upload your AWS Lambda Function, you can see all of your logging statements(read more about it [here](http://docs.aws.amazon.com/lambda/latest/dg/nodejs-prog-model-logging.html)). But, the other neat thing is using speech output, let Alexa tell you where you are in the function. For instance, if one intent function is supposed to call another function have Alexa tell you "I am in Get Review Intent".

```javascript

'GetReviewIntent': function (){
    alexa.emit(":tell", "I am in the Get Review Intent");
}

```

#### Using API:

I modeled my API function on Amazon's [City Guide Sample](https://github.com/alexa/skill-sample-nodejs-city-guide).

```javascript

function getListOfBooksJSON(genre, callback) {
    var APIKey = ""; //iDremBooks API Key
    var options = {
        //https://idreambooks.com/api/publications/recent_recos.json?key=APIKey&slug=fiction
        host: 'idreambooks.com',
        path: '/api/publications/recent_recos.json?key=' + APIKey + '&slug=' + genre,
        method: 'GET'
    };

    var req = http.request(options, (res) => {

        var body = '';

        res.on('data', (d) => {
            body += d;
        });

        res.on('end', function () {
            callback(body);
        });

    });
    req.end();

    req.on('error', (e) => {
        console.error(e);
    });
};

```

#### Using Google Analytics with Your Alexa Skill:

You can integrate the following [code](https://cloud.google.com/appengine/docs/flexible/nodejs/integrating-with-analytics) into your Lambda function to post event tracking data to Google Analytics. It's good to know the statistics for your skill, which intents do users use the most? Understanding the way a user interacts with your skill will help you make improvements. It's a little bit tricky, at least it was for me to call the tracking function from within each intent. Here's how I did that:

```javascript

trackEvent(
    'No Intent',
    'User said No',
    'User heard help message',
    '8', // Event value must be numeric.
    function(err, response){
        alexa.emit(':ask', HelpMessage, HelpMessage);
        if(err) {
            next(err);
            return;
        }
    });

```

I called the trackEvent function from all of the important intents, you can see all the details in the [GitHub](https://github.com/architapatelis/alexa-skill-book-finder) repository. In your Google Analytics account you can view these events fire in real time. In the property that you created in Google Analytics go to Real-Time and then Events. You should see the events register there when ever a user uses a particular intent.
