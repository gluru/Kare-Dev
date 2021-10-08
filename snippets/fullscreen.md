# Opening the widget fullscreen

The web widget can be configured to be opened in full screen. 

In order to do so first add `isFullscreen: true` to the GLR object then open the widget automatically on load. 

```
<script>
  window.GLR = {
    appId: 'APP-ID',
    isFullscreen: true,
  };
  (function(w, d, s){
    var j = document.createElement(s); j.async = 1; j.type = 'text/javascript'; j.src = 'https://widget.eu.karehq.com/latest.js';
    w.GLR = w.GLR || {};
    d.getElementsByTagName("head")[0].appendChild(j);
    w.addEventListener('load', ()=>{
        kare.onLoad(function(event){
        kare.open();
      });
    })
  })(window, document, "script");
</script>
```
