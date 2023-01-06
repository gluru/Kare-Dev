# Dialpad Webcrawler Bot

Dialpad uses a webcrawler to import webpages from third party solutions which have not been integrated using the APIs. 
Once added to the list of resources to be imported the target website will receive requests from out bot.


Webmasters can identify the Dialpad Webcrawler bot by looking for the following signature: 

```KareBot/0.2 (https://developer.karehq.com/bot)```


# Robots.txt guidelines

The Dialpad webcrawler bot does not need to index you whole website. In fact, it would be better to just exclude all pages which do not
contain knowledge that should be indexed.


Knowledge managers can configure the behaviour of the Dialpad webcrawler bot from the control panel of the DX Console. 


Webmaster can apply robots rules by targeting the `KareBot` agent. For instance with the following example a webmaster could only allow articles and faqs while blocking everything else. 

```
User-agent: KareBot
Disallow: /
Allow: /articles
Allow: /faq
```
