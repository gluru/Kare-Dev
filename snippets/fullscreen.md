# Opening the widget fullscreen

The web widget can be configured to be opened in full screen. 

In order to do so first add `isFullscreen: true` to the GLR object then open the widget automatically on load. 

```javascript
<script>
  window.GLR = {
    appId: '<APP-ID>',
    isFullscreen: true,
  };
  (function(w, d, s){
    var j = document.createElement(s); 
        j.async = 1; j.type = 'text/javascript'; 
        j.src = 'https://widget.<ENV>.karehq.com/latest.js';
    w.GLR = w.GLR || {};
    d.getElementsByTagName("head")[0].appendChild(j);
    const interval = setInterval(() => {
        if (kare) {
            kare.open();
            clearInterval(interval);
        }
    }, 1000);
  })(window, document, "script");
</script>
```

Please don't forget to configure the `APP-ID` and the `ENV` of your instance. 
