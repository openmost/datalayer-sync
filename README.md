# DataLayer Sync

This code listen for any changes in ```dataLayer``` object and push changes into ```_mtm``` object.

```html
<script>
let syncDataLayer = function(array, callback) {
    array.push = function(e) {
        Array.prototype.push.call(array, e);
        callback(array);
    };
};

syncDataLayer(window.dataLayer, function(e) {
    window._mtm.push(dataLayer.at(-1))
});
</script>
```