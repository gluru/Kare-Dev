# LiveChatWidget

1. Deploy LiveChatWidget in a page.
2. In the DX console enable the `custom` escalation point.
3. Customize the Dialpad Self Service's script as follows.


For more information about how to customize your `LiveChatWidget` integration please check the [LiveChatWidget's API](https://developers.livechat.com/docs/extending-chat-widget/javascript-api/).


```javascript
function hideLiveChatWidget() {
    if (!LiveChatWidget) return;
    LiveChatWidget.call('hide');
}
function loadLiveChatWidget(open) {
    if (!LiveChatWidget) return;
    LiveChatWidget.call('maximize');
    LiveChatWidget.on('visibility_changed', function(data) {
        if (data.visibility === 'minimized') {
            hideLiveChatWidget();
            kare.showLauncher();
        }
    });
}
window.GLR = {
    appId: '<YOUR-APP-ID>'
};
(function(w, d, s) {
    var j = document.createElement(s);
    j.async = 1;
    j.type = 'text/javascript';
    j.src = 'https://widget.eu.karehq.com/latest.js';
    w.GLR = w.GLR || {};
    d.getElementsByTagName('head')[0].appendChild(j);
    j.addEventListener('load', () => {
        kare.onLoad(function(callbackEvent) {
            console.log('my custom onLoad callback');
        });
        kare.onClose(function(callbackEvent) {
            console.log('my custom onClose callback');
        });
        kare.onEscalate(function(callbackEvent, messageBody) {
            console.log('my custom onEscalate callback');
            kare.close();
            kare.hideLauncher();
            loadLiveChatWidget();
        });
        kare.onOpen(function(callbackEvent) {
            console.log('my custom onOpen callback');
        });
        kare.onBeforeSocketOpen(function(callbackEvent) {
            console.log('my custom onBeforeSocketOpen callback');
        });
        hideLiveChatWidget();
    });
})(window, document, 'script');
```
