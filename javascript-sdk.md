# Dialpad Widget

This documentation explains how to setup and present Dialpad widget on your website.

It can be triggered with a button launcher (default), or launching via calling the javascript API.

## Installation

Before you can use the widget you will need to add the following snippet to the website you wish to track. This will enabled the widget with button launcher by default. See [Advanced Configuration](#advanced-configuration) to change this behaviour.

**Please use the snippet from the settings page in your console. The appId here is just an example. Also, the environment of the widget source is location dependent. This example uses the `us` environment but yours may differ.**


```html
<script>
window.GLR = {
  appId: 'APPID-FROM-CONSOLE'
};

(function(w, d, s){
  var j = document.createElement(s); j.async = 1; j.type = 'text/javascript'; j.src = 'https://widget.eu.karehq.com/latest.js';
  w.GLR = w.GLR || {};
  d.getElementsByTagName('head')[0].appendChild(j);
})(window, document, 'script');
</script>
```

# Advanced configuration

Configuration options to pass to `window.GLR` before widget setup.

|  Property    |   Type    | Description                                                                                                                                                                                                                                                 |
|--------------|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| appId        |   string  | Your client key. Required.                                                                                                                                                                                                                                  |
| showLauncher |   bool    | Show or hide the button launcher. Defaults to `true`. Useful if you want to use your own custom button to launch the widget. Optional.                                                                                                                      |
| isFullscreen | bool      | Loads the widget in fullscreen. This removes navigation so the end-user cannot close the widget. This is ideal for mobile app webviews.                                                                                                                     |
| userId          |   string  | Set the user id in the config. This will get sent down on initialization can will appear in the console. Optional.                                                                                                                                          |
| customHeaders   |   Object  | Send down custom headers to the backend so add metadata to your customers’ conversations. Only custom headers that start with “dialpad-meta-” will be accepted. Any headers that do not follow this pattern will be ignored and will not be sent. Optional. |
| query | string | Starting query the widget will send when opened                                                                                                                                                                                                             |
| welcomeMessageId | string | Id of the response that the widget will welcome the user when its opened and connected                                                                                                                                                                      |
| showPopoverAfter | int | Number of seconds for the popup to appear. Defaul 3 seconds                                                                                                                                                                                                 |

If query or welcomeMessageId are configured at the level of `window.GLR` object, the widget will send the query or welcome with that response every time the user opens a new session. In case you prefer to configure this behavior dynamically in function of the actions of the user you could use the method [`open` from the API](#API)

## Fullscreen

In order to make the widget fullscreen on a particular page you can either add the `isFullscreen` property to the `GLR` object. For example:

```
window.GLR = {
  appId: 'APPID-FROM-CONSOLE',
  isFullscreen: true,
  userId: 'my-unique-user-id',
  customHeaders: {
    'dialpad-meta-my-custom-header': 'my-custom-value',
    'dialpad-meta-custom-header-two': 123
  }
};
```

or you can add the `isFullscreen` querystring to the parent URL. For example:

```
https://www.page-where-widget-is-loaded.com?isFullscreen
```

# API

The Javascript SDK allows web developers to control the widget programmatically. The API can be accessed via the `window.kare`.

| Property             |  Type   | Value            |
|----------------------|---------|------------------|
| open({query = undefined, welcomeMessageId=undefined})  | method  | Opens the widget bottom right and starts a session. If query or welcomeMessageId are specified, it will open the widget sending that query or welcoming the user with that response.  |  
| close()  | method  | Closes the widget window completely |
| showLauncher()  | method  | Show the widget launcher |
| hideLauncher()  | method  | Hide the widget launcher |
| onLoad(callback)  | method  | Calls the callback that has been passed as an argument when the widget has finished loading. |
| onClose(callback)  | method  | Calls the callback that has been passed as an argument when the widget is closed. We send the conversation ID as a parameter to the callback method. Example: `kare.onClose(function(event){console.log('my custom close callback')})`. This will log ‘my custom close callback’ |
| onOpen(callback)  | method  | Open event, triggered with a callbackEvent when the widget is opened by a user. |
| onEscalate(callback, message)  | method  | Escalate event, triggered is the user clicks on any escalation button. |
| addBeacon(beaconConfigurationObject)  | method  | Register a beacon to the SDK. [More information](./beacons.md) |
| setUserProfileTags(tags, forceRefresh) | method | Overrides the user profile tags. First parameter expects an array of strings, second (optional) is a boolean to trigger a forced refresh. |
| host_locale | Override user locale with the current website locale. |

## Example usage of setUserProfileTags
```javascript
window.kare.setUserProfileTags(['first-tag', 'second-tag']) // This won't trigger a refresh, so the current conversation will still use the previous tags
window.kare.setUserProfileTags(['first-tag', 'second-tag'], true) // This will set the tags and also will trigger a forced refresh, opening a new conversation

```

## Launching the widget programmatically with Javascript.

To use the widget programmatically on initialization make sure to embed these calls inside the `onLoad` callback, like so:
```javascript
//E.g.
window.kare.onLoad(function() {
    window.kare.open();
});
```


```javascript
window.kare.open();
```
```javascript
window.kare.open({query:'How can I add a payment method?'})
```

```javascript
window.kare.close();
```

```javascript
window.kare.showLauncher();
```

```javascript
window.kare.hideLauncher();
```

## Setting event handlers programmatically with Javascript.

```javascript
window.addEventListener('load', ()=>{
     window.kare.onLoad(function(event){
      console.log('Widget was initialized.');
    });
})
```


```javascript
window.addEventListener('load', ()=>{
     window.kare.onClose(function(event){
      console.log('Widget was closed.', event.conversationId, 
        event.createdAt)
    });
})
```

```javascript
window.addEventListener('load', ()=>{
     window.kare.onOpen(function(event){
      console.log('Widget was opened.', event.conversationId, 
        event.createdAt)
    });
})

```

```javascript
window.addEventListener('load', ()=>{
    window.kare.onEscalate((event, message) => {
      console.log(`open zoppin with conversation id 
        ${event.conversationId}`);
    });
})
```

Example of an onEscalate using the message aspect to perform different actions.


```javascript
window.addEventListener('load', ()=>{
    window.kare.onEscalate((event, message) => {
      if (message.aspect === 'resolution/missing') {
        // perform action
      } else {
        // perform different action
      }
        const zopim = window.$zopim
      
    });
})
```

### Callbacks

All callbacks methods will be invoked with a single event parameters which looks like the following.

```javascript
callbackEvent {
    “conversationId”: “<id>”, // the conversation ID assigned by the server
    “createdAt”: “<time_stamp>” // event timestamp (assigned by the Javascript SDK)
}
```

## Inside a web page

The API can also be used inside a webpage on html element events.

```
<button onclick="window.kare.open();">Open at side of page</button>
<button onclick="window.kare.close();">Close the widget</button>
```

## Showing and hiding the widget depending on the current page

There may be some instances that you will want to show or hide the widget and/or launcher based on the page that the user is on. For instance, you may only want to show the widget on the contact us page. (E.g https://yourdomain.com/contact-us).

How you do this depends on how your website is structured. Do you have a single page application or a multiple page application? Do you use a common header and footer for every page? We'll go through some common scenarios and hopefully they well help. If you still can't manage to display the widget in the way you would like please don't hesitate to get in touch with us.

### Important

If the widget is loaded on a page it is always there, whether you see it or not. Using the API described above you can open and close the widget and you can show and hide the launcher button. If you decide to hide the launcher button, then the user will not be able to open the widget unless you specifically supply an action that the user can perform to open the widget (using our API).

### Scenarios

#### Multiple page application

Usually you'll know if you have a multiple page application because the web pages will completely refresh when you change page.

**You need to know if you have a common header and/or footer for each page, or if each page is completely different.**

##### Each page is different

If each page is completely different, then all you need to do to show the widget on one page is put the widget script on that page.

##### Common header/footer

If you have a common header and/or footer then you can put the Dialpad Self Service widget script in the header or footer of your website with the `showLauncher` flag set to `false` and then call the Dialpad API `kare.showLauncher()` to make it visible on the page that you want to show it on. For example:

```html
  <script>
    window.GLR = {
      appId: 'APPID-FROM-CONSOLE',
      showLauncher: false
    };
    (function(w, d, s){
      var j = document.createElement(s); j.async = 1; j.type = 'text/javascript'; j.src = 'https://widget.eu.karehq.com/latest.js';
      w.GLR = w.GLR || {};
      d.getElementsByTagName('head')[0].appendChild(j);
    })(window, document, 'script');
  </script>
```

Then, in your client-side code you could show the launcher programmatically:

```javascript
  if (page === 'contact-us') {
    kare.showLauncher();
  }
```

##### Loading the widget conditionally server-side 

If you are using a server-side technology you could use this to load the widget on just one page.

For example with a php website you could do something like this:

```html
<?php if ($page === 'contact-us'){ ?>

  <script>
    window.GLR = {
      appId: 'APPID-FROM-CONSOLE'
    };
    (function(w, d, s){
      var j = document.createElement(s); j.async = 1; j.type = 'text/javascript'; j.src = 'https://widget.eu.karehq.com/latest.js';
      w.GLR = w.GLR || {};
      d.getElementsByTagName('head')[0].appendChild(j);
    })(window, document, 'script');
  </script>

<?php } ?>
```

### Single page application

If your website is a single page application then you'll likely load our widget once. If you want to show the widget on just one page (or specific pages) then best thing to do would be to load it with the launcher hidden by default:

```html
  <script>
    window.GLR = {
      appId: 'APPID-FROM-CONSOLE',
      showLauncher: false
    };
    (function(w, d, s){
      var j = document.createElement(s); j.async = 1; j.type = 'text/javascript'; j.src = 'https://widget.eu.karehq.com/latest.js';
      w.GLR = w.GLR || {};
      d.getElementsByTagName('head')[0].appendChild(j);
    })(window, document, 'script');
  </script>
```

Then in your page load handler you'll need to do something like this (Please note this is pseudocode and the actual implementation will depend on your application and programming language):

```javascript
  //Wait for page to load
  onPageLoad(page => {
      //Wait for widget to load
      kare.onLoad(function(event) {
          if (page === 'contact-us') {
              // If the current page is contact us, then show the launcher.
              kare.showLauncher();
          } else {
              // Otherwise, hide the launcher and close the widget.
              kare.hideLauncher();
              kare.close();
          }
      });    
  });
```
