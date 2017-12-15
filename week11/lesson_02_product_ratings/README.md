## Implementation of a Product Rating system

In this quick tutorial, we'll run through the code of the [index.html](index.html) to see how we can extend our previous products list examples to implement the functionality of a simple rating system: 

- Users can thumb-up or thumb-down a product (beer in this case)
- A percentage of thumbs-up in relation to total votes is calculated and displayed

## Main ideas and concepts

The following ideas and concepts are used within this recipe:

- User authentication
- Read from the firebase real-time database once
- Write to the firebase real-time database
- Dynamically create `div` elements with information from the database
    - Product information
    - Rating information
- The use of a so called `Promise` as an alternative to `callback` functions

The rating system for this example works as follows:

- A user must be signed-in to rate a product
- A user can only rate a specific once
- A rating can be either thumb-up or thumb-down (1 or 0)
- We keep the user's rating in the database under the node `userratings/$uid`, where `$uid` is the placeholder variable for the currently signed-in user
- We keep the product ratings from all users under the node `productratings/$id`, where `$id` is the placeholder variable for the product's unique id (increasing integer numbers)
- A product rating entry consists of the two fields `thumbsup` and `votes`. We count how often users have rated the product positively, and how often the product got voted for in total. From this, we can calculate a percentage of positive votes, which we display in the product's information.


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

## Adding the thumb-up and down icons

In this example we use [font awesome](https://fontawesome.com/) to quickly integrate scalable icons such as the thumb-up and thumb-down for our simple rating system.

Integrating font awesome is as easy as:


```javascript
<script defer src="https://use.fontawesome.com/releases/v5.0.1/js/all.js"></script>
```

From then on, we can integrate icons using the `<i>` tag, as shown below.

```html
<i class="fas fa-thumbs-up fa-2x"></i>
```

This will show a thumbs-up icon with twice the original size. We'll use this icon when we dynamically create the products `div` elements.


## Record the rating when a signed-in user clicks a thumb

First, lets add the necessary functionality to actually rate a product by clicking on either one of the thumbs. When you look at the code, you see that we assign an action via the `onclick` attribute to both icons:

```html
<i onclick="thumb(this, 1)" class="fas fa-thumbs-up fa-2x"></i>
<i onclick="thumb(this, 0)" class="fas fa-thumbs-down fa-2x"></i>
```

The function `thumb()` takes two arguments: 

1. `this` refers to the current node in the document that triggered the function call. That means, the parameter will reference the thumb icon that was clicked when a user submitted his rating.
2. The second argument tells the `thumb()` function if a thumb-up (1) or thumb-down (0) was clicked. Depending on what was clicked, the database must be updated accordingly.


## The `thumb()` function

The function takes care of new and updated ratings. First, the function checks if a user is signed in. If not, the function won't do anything, as in this example only signed-in users are allowed to rate products.

```javascript
 var user = firebase.auth().currentUser;
 
 if(user) {
     ...
 }
```

If a user is signed in, the function proceeds as as follows:

### Step 1. Check if the user has previously rated the product he just clicked on

```javascript
var userRatingRef = firebase.database().ref('userratings/' + user.uid + '/' + productId);

userRatingRef.once('value', function(userRating) {
    var previousRating = userRating.val();

    // If the user has already voted for this product
    if (previousRating !== null) {
    ...
    }
```


### Step 2: Get the product's rating from the database and update based on wheter the user rated this product before or not

In any case, the next step is always to get the product's rating from the database:

```javascript
// Get a reference to the product's rating (not user specific)
var productRatingRef = firebase.database().ref('productratings/' + productId);
...
productRatingRef.once('value', function(productRating) {
    ...
}
```

The only thing that differs based on whether the user has rated this product before or not is how we change the product's rating. If the user has already voted for this product, we can't simply add another rating. This would allow a user to rate a product as often as he likes. We must retract his previous rating first:

```javascript
var ratingObj = { thumbsup: prodRating.thumbsup - previousRating + rating, votes: prodRating.votes };

productRatingRef.set(ratingObj);
userRatingRef.set(rating);
```

By subtracting the previous rating from `prodRating.thumbsup`, we make sure that the thumbsup value stays correct (if the user has changed his rating from up to down, the previous up is retracted. If vice versa, there is nothing to retract, as we only count the thumbs-up and total votes). The total votes stay the same, as the user has retracted his old vote, but at the same time added a new vote.

If the user has not voted for this product yet, the rating is simply added to the product's rating (no retraction necessary):

```javascript
var ratingObj = { thumbsup: prodRating.thumbsup + rating, votes: prodRating.votes + 1 };
 
productRatingRef.set(ratingObj);
userRatingRef.set(rating);
```

The last step after we updated the user's and the product's ratings it to update the display in the product's information. We do this by calling the function:

```javascript
refreshProdcutList();
```

## Update the product list after a vote was added / changed

We define one function to update the product's information, including the ratings. This allows us to call this function when the page is loaded as well as when a user submits a vote. This is how the function looks like:

```javascript
function refreshProdcutList() {
    productsRef.once('value').then(value => {

        var products = value.val();
        var promises = [];

        products.forEach((product, index) => {
            promises.push(setupProductDiv(product, index));
        })

        Promise.all(promises).then((productDivs) => {

            // Get the div element that contains the products
            var listContainer = document.querySelector("#productList");

            while (listContainer.firstChild) {
                listContainer.removeChild(listContainer.firstChild);
            }

            productDivs.forEach(productDiv => { listContainer.appendChild(productDiv); });
        });
    });
}
```

As you can see, we us a new construct called a `Promise` within this function. A promise is similar to a callback function, and allows us to define that something should happen only when a previous step is finished. This is important, if steps are interdependent, i.e. if the second step relies on the result of the first step.

So where do we use the promise exactly?

```javascript
var promises = [];

products.forEach((product, index) => {
    promises.push(setupProductDiv(product, index));
})

Promise.all(promises).then((productDivs) => {
    ...
}
```

Let's go through this step by step. First we define an empty array called `promises`.

```javascript
var promises = [];
```

We then loop through all products in our database (which we have read before) using `forEach`, in this case we have 4 products.

```javascript
products.forEach((product, index) => {
    promises.push(setupProductDiv(product, index));
})
```

And what do we do for each product? We call a function `setupProductDiv()` and push the result of this function into our array. So what does this function do? As we'll see, it will dynamically create the `div` element with all the product's information, including the ratings, and return it. But since this operation requires to query the firebase database, it takes some time to complete. We therefore return a *promise* first, and that promise will later be replaced by the finished `div` element when all necessary steps are done. 

So we now have an array full of promises for new and shiny `div` elements that we want to append to our products list. But we only want to do that when *each one of them* is finished. Since we have put all those promises into an array, we can simply pass this array to the system function `Promise.all()`. This ensures that whatever we define in the `then()` part is executed only when *all promises* have been fullfilled, i.e. all `div` elements are ready!

We now only have to append the `div` elements for each product to our list with the id `#productList`:

```javascript
// Get the div element that contains the products
var listContainer = document.querySelector("#productList");

// Remove all old div elements (this is faster than innerHTML = "")
while (listContainer.firstChild) {
    listContainer.removeChild(listContainer.firstChild);
}

// Loop through all productDivs and append them to the list
productDivs.forEach(productDiv => { listContainer.appendChild(productDiv); });
```

## Setup the product `div` elements

We need to understand one more function: `setupProductDiv()`. As always, it helps breaking the function down into its main steps:

```javascript
 function setupProductDiv(productObj, productId) {

    // Create a new div that we can fill with the product info and return later
    var newDiv = document.createElement('div');
    ...

    // Get the rating for this product (using a promise again)
    return getRatingForProduct(productId).then(ratingForProduct => {

        ...
        
        // Get the current user's rating for this product (to color the thumbs)
        return getUserRatings().then(userRating => {

            ...
        
            // Setup the div with all infos
            newDiv.innerHTML = ' ... ';

            // Return the div (resolve the promise)
            return newDiv;
        })
    });
}
```

First, the function creates a new empty `div` element. This element is returned at the end of the function to resolve (or replace) the promise. Inbetween, we add the product and rating information and format it using HTML and CSS.

In the next step, the function needs the product rating from the database. For simplicity we encapsulated this in another function called `getRatingForProduct()`. This function returns a promise, and we can only continue when that promise is resolved, i.e. when we get the data back from the database.

When we have the product rating, we now need the rating of the currently signed-in user. We need this information to color the thumb-up and thumb-down icons green or red (or nothing at all, if the user hasn't rated a product yet). Again, we use a promise and a function `getUserRatings()` for that. Only when we also have this information, we can continue.

Now that we have everything (product info, product rating, user rating), we can setup the `div` element and return it.

That's it! We have successfully implemented a simple rating system for our products. 

## Make sure you secure your data

We haven't covered security aspects of a shopping cart implementation. If you implement a rating system cart for your web application, make sure you also apply database security rules to let only authorized users submit ratings.