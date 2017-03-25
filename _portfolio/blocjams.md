---
layout: post
title: Bloc Jams
feature-img: "img/sample_feature_img.png"
thumbnail-path: "img/bloc-jams.png"
short-description: Bloc Jams is a dynamic music player built using AngularJS

---

#### SUMMARY

Bloc Jams is a digital music player similar to Spotify. Interactivity in the application was initially implemented using native JavaScript. To make it more efficient the code was refactored with jQuery and AngularJS.  

#### EXPLANATION

After learning the fundamentals of HTML, CSS and native JavaScript building this dynamic music player app was my first project as a Bloc student. Early on in the project I was given the necessary code to start linking the various components of this app. At that stage my primary objective was to understand the code and how it affects the interactivity of the app. With time I was able to develop a better understanding of how CSS transitions can be used to animate the appearance of an application and how I can use DOM selectors to perform actions on elements in a web app. 
Experience with DOM scripting is essential for all web developers but in addition to that it is also important to learn how to use helper libraries such as jQuery and frameworks such as AngularJS. 

I used jQuery to refactor the native JavaScript I had implemented thus far. Refactoring made the code simple, clean and efficient by allowing me to replace many lines of JavaScript with a single line of jQuery code. 

I also refactored the native JavaScript using AngularJS framework which helps build a dynamic single-page-application (SPA). I learned that I can combine AngularJS specific elements and attributes in HTML templates with JavaScript code to render a dynamic view that a user can interact with in a browser. Much like jQuery, AngularJS also allows us to cut back on the many lines of JavaScript therefore making it more efficient to develop a web application. 
 
#### PROBLEM

Since this application is part of the Bloc curriculum building it was a guided process. However, once the application was up and running I independently added two features:

1) auto-play next song in the play list

2) mute a song

#### SOLUTION

In order to auto-play the next song:

{% highlight javascript %}
$rootScope.$apply(function() {
    //use Buzz's  getTime() method to get the current time (in seconds) of the song
    SongPlayer.currentTime = currentBuzzObject.getTime();
    var duration = currentBuzzObject.getDuration();
    // to automatically play next song when current song ends.
    if(SongPlayer.currentTime === duration) {
        SongPlayer.next();
    }
{% endhighlight %}

In order to mute a song:

{% highlight javascript %}
SongPlayer.mute = function() {
    // if song isn't muted and user clicks on volume icon
    if(!SongPlayer.muted) {
        // Save the volume of the song when it was muted. So we can return back to it when unmuted. 
        SongPlayer.volBeforeMute = SongPlayer.volume;
        SongPlayer.muted = true;
        SongPlayer.setVolume(0);
    } // to unmute
    else {
        SongPlayer.muted = false; SongPlayer.setVolume(SongPlayer.volBeforeMute);
    } 
}; 
{% endhighlight %}

#### CONCLUSION

For me the main advantage of using a large library like jQuery is that it enables us to perform tons of functions and because it has simple syntax we write much less code to achieve certain functionality in the app. While jQuery and AngularJS are both powerful web development tools, I prefer using AngularJS because it allows us to break the code into smaller pieces that interact with each other. For a student it makes things a bit easier to understand. With AngularJS we can use two-way binding between models and views by using specific declarative AngularJS element attributes in the HTML templates. Therefore, requiring less code.  

