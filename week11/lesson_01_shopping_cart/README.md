## Implementation of a Shopping Cart

In this quick tutorial, we'll run through the code to see how we need expand our previous products list examples to implement the functionality of a shopping cart. In the following, we'll see how to store a shopping cart for a *signed-in* user. We don't cover the following:

- Allow users to start a shopping cart when they are not signed in (this is possible, but would create more complexity)
- Clearing the shopping cart (you should be able to add such functionality with what you already know)
- Removing items from the shopping cart (you should be able to add such functionality with what you already know)


## Example Products Database

In this lesson, I provide you with an example [`json`](database_export.json) file you can import into your database. 

```
[ {
  "description" : "Ein Pilsener von Welt.",
  "name" : "Krombacher Test",
  "price" : 12.99
}, {
  "description" : "Herrliches Herforder",
  "name" : "Herforder",
  "price" : 10.99
}, {
  "description" : "Heute ein Veltins!",
  "name" : "Veltins",
  "price" : 12.99
}, {
  "description" : "Kann man auch trinken!",
  "name" : "Radebacher Pils",
  "price" : 12.49
} ]
```

## Add a "Buy now" button

This example extends the product list example from week 6. Here, we have already added simple buttons with no functionality to each product in the list. Let's now create that functionality.

In the example, we created the button dynamically for each product. We still do it this way, but we slightly changed how the button (back then it was a `<a>` link element):

`<button onclick="addToCart(this)" class="btn btn-primary addCartBtn" disabled>Buy now</button>`

So what changed?

1. `onclick="addToCart(this)"` - This tells the button to call a function when it is clicked. Adding the parameter `this` allows us to track the source of the click, i.e. which specific product was clicked.
2. `class="... addCartBtn"` - We'll use this CSS class to select all "Buy now" buttons and enable or disable them based on the login state.
3. `disabled` - Let's assume the button is disabled at first. If someone logs in, we enable the buttons using the CSS class from 2.

## Add information to each product with the `data`-attributes.

By adding the parameter `this` to the call of `addToCart` we'll be able to find the source of the button click. But how can we then get information about the product? One way is to use so called `data`-attributes (see also [here](https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Use_data_attributes)).

You already know how to set attributes for an HTML element, for example to hide it. In the same way, we can add attributes that start with `data-*` which are treated in a special way. Let's do it for our parent `div` element of each product:

```javascript
newDiv.setAttribute("data-id", index);
newDiv.setAttribute("data-name", product.name);
newDiv.setAttribute("data-price", product.price);
```

Setting theses attributes allows us to retrieve them in the `addToCart` function, as shown below.

## Implement `addToCart`

Notice the signature of the function:

```javascript
function addToCart(event) {
 ...
}
```

The parameter `event` contains the so called click-event that we passed earlier as `this`. Via this object (or event), we have access to the `data`-attributes we defined above in the following way:

```javascript
var id  = event.parentElement.dataset.id,
var name = event.parentElement.dataset.name,
var price = event.parentElement.dataset.price,

```

The `event` object has a property named `parentElement`. This is the enclosing `div` of the product information. The button is the source of the event, so the `parentElement` is the parent of the button. Since we defined the 3 `data`-attributes for that parent `div`-element, we can use the property `dataset` to access the value for each of them.

Next, we get a database reference to the user's shopping cart and the very product he just tried to add:

```javascript
var cartRef = firebase.database().ref('shoppingcarts/' + user.uid + "/" + productObj.id);
```

Remember that `user.uid` contains the current user's id (if logged in - that's why we need a signed in user for this example). By adding the product id to the path, we can check if that product was already in the cart:

```javascript
// Get the current product from the cart (if is already there)
cartRef.once('value', function(value) {

    var productInCart = value.val();

    // If the product is already in the cart, add one to the quantity
    if (productInCart) {
        productObj.quantity = productInCart.quantity + 1;
    }

    // Update (or add) the product in the cart
    cartRef.set(productObj);
});

```

If the product was already in the cart, we take the currrent quantity and add 1 to it. If it wasn't, we set the quantity to 1 (not shown in the snippet above).

## Listen for changes to the cart

In the `loginChanged` function, which is called when a user signs in or out, we add a listener to the user's shopping cart in the database. This listener calls the `cartChanged` function:

```javascript
function cartChanged(newCart) {
    var cart = newCart.val();

    var itemsList = document.getElementById("itemsList");
    itemsList.innerHTML = "";

    var total = 0;

    cart.forEach(
        function(item, index) {
            var listItemText = item.name + ": " + item.price + " x " + item.quantity + " = " + item.price * item.quantity + " EUR";
            var newItem = document.createElement("li");
            newItem.innerHTML = listItemText;
            itemsList.appendChild(newItem);
            total += item.price * item.quantity;
        });

    document.getElementById("total").innerText = total + " EUR";
}
```

This should look familiar, as it is basically the same as for the products list. We iterate through all items in the cart and create a new `li`-element and append it to a previously defined `ul`-element. Finally, we calculate the total for the cart and output that as well.

## Enable and disable all "Buy now" buttons

We said we wanted to disable all "Buy now" buttons when there is no signed in user. To get all buttons, we gave them the same CSS class `addCartBtn`. We can use this CSS class as a selector in `getElementsByClassName` and disable/enable the buttons as follows:

```javascript
var user = firebase.auth().currentUser;
if (user) {
    var addToCartButtons = document.getElementsByClassName("addCartBtn");
    [].forEach.call(addToCartButtons, btn => { btn.removeAttribute("disabled"); });
}
else {
    var addToCartButtons = document.getElementsByClassName("addCartBtn");
    [].forEach.call(addToCartButtons, btn => { btn.setAttribute("disabled", ""); });
}
```

## Make sure you secure your data

We haven't covered security aspects of a shopping cart implementation. If you implement a shopping cart for your web application, make sure you use database security rules to let only authorized users to read a shopping cart.