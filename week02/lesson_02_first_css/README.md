# Table of Contents
- [First Steps with CSS (`01_html_with_first_css.html`)](#first-steps-with-css-01_html_with_first_csshtml)
  - [HTML sets the structure, CSS the design](#html-sets-the-structure-css-the-design)
  - [Colors](#colors)
  - [CSS Selectors](#css-selectors)

# First Steps with CSS (`01_html_with_first_css.html`)

The following explanation references the source file [`01_html_with_first_css.html`](https://github.com/winf-hsos/webengineering/blob/master/week02/01_html_with_first_css.html).

## HTML sets the structure, CSS the design

The HTML above shows in a browser, but the style of it can be improved. Fortunately, we have a great way to style HTML elements: **Cascading Style Sheets** or short **CSS**.

Within the `<body>` tag, you can describe *what* you want to display on the website and how the general structure of the different elements should be. With CSS, you can describe for each element *how* it should be displayed in the browser.

## Colors

An important aspect of every design is the choice of colors. By default, the browser shows text and headings in black on a white background. We can change that with CSS.

```css
h1 {
    color: #009EE3;
}
```

The above CSS code tells the browser to show all `<h1>` elements in the color `#009EE3`, which is the hexadecimal code for the light blue our school applies in its corporate design. (If you want to pick your own color you can get the hex-code [here](https://www.w3schools.com/colors/colors_picker.asp)).

## CSS Selectors