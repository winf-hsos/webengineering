## Using Data Attributes

We can use data attributes to assign values that we care about to any HTML element. We can then read these data attributes using JavaScript. The syntax for data attributes is always `data-*`, where `*` is a placeholder for a name we must define.

```html
<div id="myDiv" data-name="Krombacher Pils" data-price="12.99">
```

We can now access the `data` attributes from JavaScript as follows:

```js
var divEl = document.getElementById("myDiv");
var productName = divEl.dataset.name;
var productPrice = divEl.dataset.price;
```

This is an important concept when we for example generate a list of products using data from a database. For each product, we add a button to add the product to a shopping cart (or to rate it for that matter). When that button is clicked, we need to know the source of the button click, i.e. which specific product that button belonged to. When we assign `data` attributes at creation time and pass the element to the `onclick`-function, we can easily access the information about the product:

```js
<div data-name="Krombacher Pils" data-price="12.99">
    ...
    <buttton onclick="addToCart(this)">Buy now</button>
</div>
...

<script>

function addToCart(el) {
    // The data attributes are defined on the parent of the button
    var productName = el.parentElement.dataset.name;
    var productPrice = el.parentElement.dataset.price;
}

</script>
```


