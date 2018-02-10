---
title: "Getting Nuts Sorting Code"
date: 2018-02-10T23:25:20+01:00
draft: false
---

>He maketh peace in thy borders, and filleth thee with the finest of the wheat.
>Ps 147:14

---

I don't get to coding a lot recently, which is something I pity and something I can hardly change for the time being. But anyways, tonight I got to sit with the code, still the Random Quote Machine.

Bear with me. I know I wrote earlier that it was basically done and I wanted to move on, but I have so many ideas that would not only add functionality but make me dive into different fields of Javascript, so basically, an opportunity to learn. What can I want more.

Like what I learned tonight. Man that took a while to even understand what the problem could be.

Let me start with a snippet of code:

```Javascript
$("document").ready(function(){
  getQuote(apiArray,quoteSingleton);
  $("#tweetbutton").on('click',function(){
                    window.open("https://twitter.com/intent/tweet?hashtags=quotes&related=freecodecamp"+encodeURIComponent(", author of quotes app challange")+quoteSingleton.twitter+encodeURIComponent(", author of API used")+",tonkllr"+encodeURIComponent(", author of Quotes Machine App")+"&text="+quoteSingleton.getText()+" - "+quoteSingleton.getAuthor())});
  $("#hubbutton").on('click', function(){
                    //get hub and channel name
                    var fullchannel=document.getElementById("channel").value.split("@");
                    var quote= "[quote="+quoteSingleton.getAuthor()+"]"+quoteSingleton.getText()+"[/quote]"+"<br/>[b]Credit: "+quoteSingleton.getCredit()+"[/b]";
                    window.open("https://"+fullchannel[1]+"/rpost?&body="+quote+"&title=News from the Quotes Machine&channel="+fullchannel[0]);
                  });
  $("#newquote").on('click',function(){
                    getQuote(apiArray,quoteSingleton);
  });
});
```
This used to be the part of the code that defines how to react to clicks on certain html button elements, addressed by id. We all know how to do this: jQuery .on() method with "click" as the first parameter and the full callback function as the second. Don't bother too much with the function's bodies, they don't matter too much for what we look at here.

So my idea was to clean up the code a bit, because I intended to add keyboard control of the Quotes Machine, just for some more excercise and because I know, professional software will always have keyboard control. It's faster and more conveniet, once you get used to it. On my private computer I have installed [awesomewm](https://awesomewm.org/) to get used to this myself, but I disgress...

I wanted to get the callback functions seperate and call them by name like so:

```Javascript
function tweet(quoteObject){
                  window.open("https://twitter.com/intent/tweet?hashtags=quotes&related=freecodecamp"+encodeURIComponent(", author of quotes app challange")+quoteObject.twitter+encodeURIComponent(", author of API used")+",tonkllr"+encodeURIComponent(", author of Quotes Machine App")+"&text="+quoteObject.getText()+" - "+quoteObject.getAuthor())
                };

function hubshare(quoteObject){
                  //get hub and channel name
                  var fullchannel=document.getElementById("channel").value.split("@");
                  var quote= "[quote="+quoteSingleton.getAuthor()+"]"+quoteSingleton.getText()+"[/quote]"+"<br/>[b]Credit: "+quoteSingleton.getCredit()+"[/b]";
                  window.open("https://"+fullchannel[1]+"/rpost?&body="+quote+"&title=News from the Quotes Machine&channel="+fullchannel[0]);
                };

$("document").ready(function(){
  getQuote(apiArray,quoteSingleton);
  $("#tweetbutton").on('click', tweet(quoteSingleton));
  $("#hubbutton").on('click', hubshare(quoteSingleton));
  $("#newquote").on('click', getQuote(apiArray,quoteSingleton));
});
```

The idea was to being able to call these functions from other event handler as well. You name it: When adding keyboard event handlers. I'd have the code for a certain reactionat one place, and whenever a change needs to be I can change it at once for click and keypressed...

Anyway, rearranging my code like this broke the whole thing. The functions just fired without waiting for a click, just on loading the page. And afterwards, clicking the buttons would do nothing. Some base stuff must have been broken.

The problem is, this is not how you call the callback functions. I tried moving the functions from before to below the document.ready part, but this didn't help. Reading the jQuery documentation did help, though I needed a bit to find the right section on the right page. Look [here](https://api.jquery.com/on/) in the chapter "Passing data to the handle", or read on and see how my code looks like now:

```Javascript
function hubshare(quoteObject){
                  //get hub and channel name
                  var fullchannel=document.getElementById("channel").value.split("@");
                  var quote= "[quote="+quoteObject.data.getAuthor()+"]"+quoteObject.data.getText()+"[/quote]"+"<br/>[b]Credit: "+quoteObject.data.getCredit()+"[/b]";
                  window.open("https://"+fullchannel[1]+"/rpost?&body="+quote+"&title=News from the Quotes Machine&channel="+fullchannel[0]);
                };

function tweet(quoteObject){
                  window.open("https://twitter.com/intent/tweet?hashtags=quotes&related=freecodecamp"+encodeURIComponent(", author of quotes app challange")+quoteObject.data.getTwitter()+encodeURIComponent(", author of API used")+",tonkllr"+encodeURIComponent(", author of Quotes Machine App")+"&text="+quoteObject.data.getText()+" - "+quoteObject.data.getAuthor());
                };


$("document").ready(function(){
  getQuote({data:{apiList: apiArray, quoteObject: quoteSingleton}});
  $("#tweetbutton").on('click', quoteSingleton, tweet);
  $("#hubbutton").on('click', quoteSingleton, hubshare);
  $("#newquote").on('click', {apiList: apiArray, quoteObject: quoteSingleton}, getQuote);
});
```

You need to add the data passed to the function as an own parameter. So you have the function name and its parameters as seperate parameters of the .on() method. In my code I pass the variable "quoteSingleton", which is really an object, to the functions "tweet" and "hubshare". And suddenly all went well.

I only had to take into account, that I couldn't just reach the Object data from within the functions like I used to, I had to put ".data" in the middle, what is given to the function is an object with the property "data" that holds everything you passed.

I had some more struggles with the getQuote function, due to it originally being passed two parameters, the quoteSingleton Object which it fills with a new quote and metadata such as author and source, and apiArray, which holds a number of APIs the app retrieves quotes from (That's right, we do not rely on one API alone).

So I had to wrap the two parameters in an object to pass it to the function. And on the first call for the initial quote, outside of event handlers, I had to wrap this up in another object with the sole property "data". I guess this can be done easier, but for now it works.

So the actual keyboard controls are not added yet, but this should be an easy thing to start with next time I get to coding. And there's even more in the making.
