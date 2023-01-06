
# Intercom

Please follow these integration steps:
1. Customize the following script as needed to fit your requirements.
2. Load intercom in your page.
3. Import this script immediately after that.


```javascript
// Hide Intercom when needed. 
function hideIntercom() {
    Intercom('hide');
    Intercom('update', {"hide_default_launcher": true});
}

// Show Intercom when needed. 
function showIntercom() {
    Intercom('update', {"hide_default_launcher": false});
}

window.GLR = {
    appId: '<APP-ID>'
};
(function (w, d, s) {
    var j = document.createElement(s); j.async = 1; j.type = 'text/javascript'; j.src = 'https://widget.eu.karehq.com/latest.js';
    w.GLR = w.GLR || {};
    d.getElementsByTagName('head')[0].appendChild(j);
    
    const interval = setInterval(() => {
        kare.onLoad(function(callbackEvent) {
            console.log('my custom onLoad callback');
        });
        kare.onClose(function (callbackEvent) {
            showIntercom();
        });
        kare.onEscalate(function (callbackEvent, messageBody) {
            kare.close();
            kare.hideLauncher();
            showIntercom();
        });
        kare.onOpen(function (callbackEvent) {
            // If Intercom coexist with DSS we can close it when DSS
            // is opened. 
            // hideIntercom();
        });
        kare.onBeforeSocketOpen(function (callbackEvent) {
            console.log('my custom onBeforeSocketOpen callback');
        });

        // Hide Intercom by default.
        // Alternatively it can be hidden when DSS opens.
        hideIntercom();
    }, 1000);
})(window, document, 'script');
```

