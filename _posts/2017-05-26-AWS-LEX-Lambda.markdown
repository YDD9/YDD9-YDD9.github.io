---
layout: post
title:  "AWS Lex and Lambda for Facebook Messenger chat bot"
date:   2017-05-26 22:49:39 +0100
categories: AWS Lex Lambda
comments: true
---




*AWS Lex* is a Amazon Cloud chat AI service. It gets user inputs via text(or voice), then form a json containing info user provided, now comes the Lambda service(Python2.7 case) which can process the data as json according to your coded business logic. i.e. Pizza order, flower order, take appointments...

Just follow the Lex get started document to get a basic understand of what the service does, the possible ways to work with other services i.e. Polly(AI text to speach service), and the deployment of Lex Bot.

The get started example - the Order* Bot is relatively simple but complete and it is a very good example to start with:
1. Lex Bot text interface
1. The json data and schema Lex collects and passes to Lambda service 
2. Lambda service entry point/function(handler configuration)
3. Entry function of Lambda service load the json data(as event) that Lex Bot provides and returns result json data to Lex Bot, Lex Bot will then answer user via text(or voice)
3. Entry function has another AWSLexBot object context which has predefined function by AWS
4. CloudWatch service logs all for Lambda service, this function is very usefull when debugging

After finishing configuration of OrderFlowers, you should then do the deployment of a customer Bot example, pay attention to above  listed points and you will find each step make more sense.

Before deployment AWS Lex, you can only interact with Bot via text which is much less interesting for me, because the cool thing should be talk and answer live conversation.

Actually AWS espects that you make Lex Bot available in a mobile APP which can take voice or create a Facebook page and add AWS Lex Bot into messenger of the page. When you browse the Facebook page you can then talk to the Bot.

## Mobile APP Deployment case:
You will need mobilehub service and knowledge of IOS SDK or Android SDK or Web server Node.js(no example).

## Facebook page and messenger deployment case:
To begin the steps here, you must make sure your AWS Lex Bot is working and pass already the test in AWS. If yes, now you should follow the steps of [AWS deployment](http://docs.aws.amazon.com/lex/latest/dg/fb-bot-association.html) which involves Facebook page and messenger. 

First login to [facebook developer website](https://developers.facebook.com) and fowllow the [Quick Start](https://developers.facebook.com/docs/messenger-platform/guides/quick-start) 

PS: To create Facebook APP, your Facebook account must be verified which means associating your phone number to your Facebook account, otherwise you can't create any APPs and a misleading error message is kind of "Sorry, we're having issues."

Create a new page on Facebook and generate page token for it:
EAAcFJjuHwgYBAIucxJflcZBtohp7ZBZA6gJe2K4YIhdmfRsCX8OlMfJluUxReDBbLp1ZBlFZBM6xrZALpIrSlZAojUabpjBcNEKZAqmqW0wzPKQyKhiZBjXt20DfN2KjQfNZCuqz0AKN03I7y  
![FB_PAGE_TOKEN_GEN]({{ site.url }}/images/FB_PAGE_TOKEN_GEN.jpg)

Get familiar with the Facebook developer Dashboard, you will find your APP SECRET.

Now go back to AWS Lex - section channel - Facebook, specify above info and set a verify token then generate the callback uri.

Switch again to Facebook developper - section products - Messenger settings - webhooks, add the callback uri from AWS and give the same verify token. At last, subscribe this webhookthe to your created Facebook page.  
![FB_PAGE_WEBHOOK]({{ site.url }}/images/FB_PAGE_WEBHOOK.jpg)

You notice a new products WEBHOOK also appears on the left, try click and edit it. Double verify if it's working with verify token. At the beginning I didn't do this and the messenger responded nothing when I sent message.

Facebook API kit could also helpful, I didn't use much, but guess it's a must when debug.
[graph API Facebook](https://developers.facebook.com/tools/explorer/1975986602623494?method=GET&path=%7Bmessage-id%7D&version=v2.9)


When you're ready and pass test, you need to submit your app to Facebook review and then it will go public, before that it's admin, tester, developer access only. Install Facebook messenger on your Iphone or Android, login it and search your Bot, let's talk.  
![FP_CHAT]({{ site.url }}/images/FP_CHAT.jpg)

Normally smart phone can convert speach to text, but the Bot can't answer in voice, the real talking communication needs the text to speach which means an AWS Polly service on top, how this is done in lamba or what ???


to be continued...



Similar deployment on different platform:

[Google chat](https://www.raizlabs.com/dev/2017/01/build-ai-assistant-api-ai-amazon-lambda/)

[slack chat](https://nicholasjackson.io/2017/04/25/slack-bot-aws-lambda/)


[Twilio chat](https://www.twilio.com/blog/2015/09/build-your-own-ivr-with-aws-lambda-amazon-api-gateway-and-twilio.html)





<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/

var disqus_config = function () {
this.page.url = {{page.url}};  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = {{page.id}}; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};

(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://https-ydd9-github-io.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>

<script id="dsq-count-scr" src="//https-ydd9-github-io.disqus.com/count.js" async></script>