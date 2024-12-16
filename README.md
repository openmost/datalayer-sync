# Synchronize Google Tag Manager and Matomo data layers (```dataLayer``` and ```_mtm```)


> [!IMPORTANT]
> This code is now part of Matomo 5.2.0 and above ! 🥳
>
> If you want to enable this function in your container, edit your container and activate "Actively synchronise from the Google Tag Manager data layer".

## The problem (only for Matomo < 5.2.0)
Many websites have both Google Tag Manager and Matomo Tag Manager implemented on their source code.
To get similar events on Google Analytics and Matomo, the two Tag Managers have to share a common ```dataLayer```.

Matomo is compatible with GTM data layer only with data present in ```dataLayer``` array before page load.
Every event or data pushed to the dataLayer after the page load **will not** be detected by Matomo Tag Manager.


## The solution
To keep Matomo up to date with the dataLayer content, we have to synchronize GTM ```dataLayer``` with Matomo ```_mtm```.
This code listen for every ```push()``` in Google dataLayer, pick the latest item pushed in ```dataLayer```, and push this item to ```_mtm``` data layer.

**You can copy this code and past it in a custom HTML tag triggered on every page view in Matomo Tag Manager.**

ENJOY !

```html
<script>
window.dataLayer = window.dataLayer || [];
window._mtm = window._mtm || [];
let syncDataLayer = function(array, callback) {
    array.push = function(e) {
        Array.prototype.push.call(array, e);
        callback(array);
    };
};

syncDataLayer(window.dataLayer, function(e) {
    window._mtm.push(window.dataLayer[window.dataLayer.length-1]);
});
</script>
```

A demo is available in ```index.html``` file on this project, you just have to look on the console for empty ```_mtm```, click the button to push event and data, and see the magic !

<!--
## Optional : Completely remove Google Tag Manager
In some cases, you want to keep the dataLayer but completely remove the Google Tag Manager tracking code. You can remove the code, but you must keep the initialization of ```dataLayer```.

Replace the Google Tag Manager code with this simple initialization code.

```html
<script>
 // Initialize GTM dataLayer to keep dataLayer working 
 window.dataLayer = window.dataLayer || [];
</script>
```
-->
