---
title: "Tired After Days"
date: 2018-01-21T01:33:17+01:00
draft: false
---

>I will worship toward thy holy temple, and praise thy name for thy lovingkindness and for thy truth: for thou hast magnified thy word above all thy name.
>
>Ps 138:2

---

It's late. Again. And I still didn't move forward with the Quotes Machine. But I've come to a resolution:

# I won't keep trying calling the Hubzilla API through a Javascript HTTP request.

Problem is: Same-Origin-Policy. As far as I understand it, you can overcome this through enabling CORS server-side, but Hubzilla is a decentralised network where you cannot rely on all servers having the proper headers set. My own instance of Hubzilla runs on uberspace, where I only have webspace, but no control over the server.

There are other ways circumventing cross-origin errors. One being JSON-P, which also needs to be supported server-side, and I did not find any hint that the Hubzilla API would support this.

And then I read about hidden forms and iframes attatched to the server and what not. So one would end up with some kind of hack, that would do hardly anything else than what window.open does.

By accessing the input form Hubzilla offers under /rpost/ through https, I can preset values if the message and get all info through without bigger problems.

I have been told on the Free Code Camp forums to not use window.open() as an ajax call, as this could lead to the app breaking due to popup-blockers and it generally is a bad way of coding, a hack, which could cost you a job interview when seen by prospect bosses.

While I understand all this, I still think, after quite a while of reasoning, that it is the best approach with my given knowledge of the issue. I read about popup-blockers not blocking when the new window comes up as a result of a user input like a click. Well, the idea is to click on the "Share on Hubzilla" Button, which would lead to window.open() being called.

I am still looking forward to being able to send my first working ajax POST request. But this will be for another time.

Strange as it is, I got the message through once by means of a html form alone, without Javascript. Wonder why this did work, but creating hidden forms, autofilling them and firing the submit in secret seems like a hack to me as well, especially as I cannot retrieve the return JSON and lead the user to the newly created message within the network then.

Tomorrow - no, rather today, that is, when the sun rises - is sunday. This shall be a day of rest (I've heard that before somewhere). Starting on monday I'll see what I can do with ajaxing to twitter and GNU.social. Well, GNU.social is similar to Hubzilla, decentralised and all, so the solution might be analoguous.

Can't wait to get this project done and move on.
