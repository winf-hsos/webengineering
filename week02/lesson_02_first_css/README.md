# Table of Contents
- [First Steps with CSS (`01_html_with_first_css.html`)](#first-steps-with-css-01_html_with_first_csshtml)
  - [HTML sets the structure, CSS the design](#html-sets-the-structure-css-the-design)
  - [Colors](#colors)
  - [CSS Selectors](#css-selectors)

# First Steps with CSS

The following explanations reference the source file [`01_html_with_first_css.html`](https://github.com/winf-hsos/webengineering/blob/master/week02/01_html_with_first_css.html).

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <title>Haster Bier Shop</title>

    <!-- This is where the CSS sytling goes -->
    <style>
        h1 {
            color: #009EE3; /* Make all <h1> HS-blue */
            font-size: 28px; /* Set the size of all <h1> */
        }
        
        /* Make all paragraphs, list items, and headings 
           show in a nice font */
        p, li, h1 {
            font-family: 'Open Sans';
        }
    </style>
</head>

<body>
    <h1>Willkommen in unserem Online-Shop</h1>
    <p>Bestellen Sie unsere Haster Biere bequem online:</p>
    <ul>
        <li>Haster Helles</li>
        <li>Haster Dunkles</li>
    </ul>
</body>

</html>

```

## HTML sets the structure, CSS the design

The HTML from [lesson 01](https://github.com/winf-hsos/webengineering/tree/master/week02/lesson_01_first_html) shows in a browser, but the style of it can be improved. Fortunately, we have a great way to style HTML elements: **Cascading Style Sheets** or short **CSS**.

We have learned before that, within the `<body>` tag, you can describe *what* you want to display on the website and how the general structure of the different elements should be. With CSS, you can describe for each element *how* it should be displayed in the browser.

CSS works very simple: We define a scope for our style using a **CSS selector**, and then we use **CSS properties** to describe how everything within that scope should look like. The general structure for a CSS rule looks like this:

```css
css-selector {
    property-a: value;
    property-b: value;
    ...
}
```

Let's look at this a bit closer.

## CSS Selectors

CSS selectors allow to define a scope, i.e. the set of elements to which a certain style should be applied. There are different types of CSS selectors, let's start with the easiest and most inutuitive one: Calling the elements by their names:

```css
h1 {
    ...
}
```
The above CSS code defines a scope that contains all `<h1>` elements. So for all top level headings, whatever we define within the curly brackets will be applied. For example:

```css
h1 {
    color: #009EE3;
}
```

This makes all top level headings show in a blue color. Colors are often expressed in hexadecimal code, and the code above encodes the light blue our school applies in its corporate design. (If you want to pick your own color you can get the hex-code [here](https://www.w3schools.com/colors/colors_picker.asp)).

What if we wanted to sytle `<h2>` and `<h3>` headings the same way? Well, we simply list them separated by commas:

```css
h1, h2, h3 {
    color: #009EE3;
}
```

The scope now contains three elements, and the CSS rule is applied to all of them. The same could have been achieved by writing three separate rules, but that makes the code longer and harder to read:

```css
h1 {
    color: #009EE3;
}

h2 {
    color: #009EE3;
}

h3 {
    color: #009EE3;
}
```

As mentioned, naming the HTML element we want to style is the simplest way. But it is only one of many. There are other ways to define a scope, such as:

- Using CSS classes
- Using ID attributes
- Combinations of element name, CSS classes and ID attributes

We'll skip them for now, but we'll come back to them later. First we need to understand the second part of a CSS rule, the CSS properties.

## CSS Properties

With CSS selectors we define the set of elements we want to style. With CSS properties, we actually describe their style. A property has a name and a value, and both are separated by a colon. 

With the `color:` property from the example above, we have already introduced the first and a very important CSS property. It sets the color of an HTML element's text content. We can therefore apply the `color:` property to any HTML element that contains text, for example:

```css
p {
    color: red;
}

li {
    color: #000000;
}
```

We told the browser to make all text in paragraphs red. Note that there are [built in keywords for colors](https://www.w3schools.com/colors/colors_names.asp) that you can simply use in CSS. Moreover, we set all text in list items to black (`#00000` is the color code for black, `#ffffff` is the code for white).

There are many more CSS properties to style text. [Here](https://www.w3schools.com/css/css_text.asp) you find a good overview. For example:

```css
p {
    color: #c0c0c0;
    text-align: center;
}
```

This makes all text in paragraphs show in a light grey and let it appear centered. This could be a good formatting for quoutes, for example.

Let's leave with that. We'll learn more about CSS in the following lessons.

## Summary

What we have learned in this lesson:

- HTML describes the *what*, CSS the *how*
- CSS rules have a CSS selector and a set of CSS properties with values
- A CSS selector defines the scope for a style, that is a set of elements to apply the style to
- A CSS selector can use HTML elements to define the scope. Other options are CSS classes, ID attributes oder a combination
- Depending on the HTML element, certain CSS properties can be applied or not. The `color:` property, for example, only applies to HTML elements with text