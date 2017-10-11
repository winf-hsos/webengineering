# Table of Contents
- [HTML elements for images, hyperlinks, and text formatting](#html-elements-for-images--hyperlinks--and-text-formatting)
  * [Images](#images)
  * [Hyperlinks](#hyperlinks)
  * [Text Formatting](#text-formatting)
- [Links](#links)

# HTML elements for images, hyperlinks, and text formatting

The following explanations reference the source file [`index.html`](https://github.com/winf-hsos/webengineering/blob/master/week02/lesson_04_images_links_formatting/index.html) and [`lieferbedingungen.html`](https://github.com/winf-hsos/webengineering/blob/master/week02/lesson_04_images_links_formatting/lieferbedingungen.html).

## Images

Images are an important part of every website. Whether you want to display your products or your staff, or you want user to upload profile images. In HTML, theres is only one tag to place images:

```html
<!-- Places an image on the website -->
<img alt="Woman drinking beer" style="float: right" height="150px" src="images/woman_beer.jpg">
```

To display an image, we need to tell the browser where to find the image with the `src` attrribute. In this case the image lies in a folder `images` relativ to the current page.

We can also define an images display size, either with the `width` or `height` attribute. In this case, we want the image to have an exact height of 150 pixels, no matter the original size. 

In case an image cannot be displayed (either it is not found or images were disabled), the browser instead shows the text that is defined by the `alt` attribute.

In the example above, we additionally add a `style` attribute to tell the browser to let the image float on the right. This way, it appears next to the text.

## Hyperlinks

The whole idea of the world wide web was to link websites together and the let the user navigate through the web. Links are therefore an integral part of any website. They can be defined with the `<a>` tag.

```html
<a href="lieferbedingungen.html">Lieferung</a>
```

The `<a>` tag requires one attribute `href` that tells the browser where to link. In this case, we want the browser to open a website with the name `lieferbedingungen.html` that is stored in the same folder as the current page. We can also link to any other folder on the webserver, or we can link to a totally different URL such as `https://www.hs-osnabrueck.de`.

By default, links open in the same browser window or tab. The user can usually open a link in a new tab by holding `Ctrl` while clicking a link. However, in same cases we want the user to stay on our website, no matter if she holds `Ctrl`, and therefore open a link in a new window or tab no matter what. We can use the `target` attribute to accomplish this:

```html
<a href="https://www.hs-osnabrueck.de" target="_blank">HS Osnabrueck Website</a>
```

The value `_blank` tells the browser to open a new tab when a user clicks the link.

## Text Formatting

Although we can do any formatting with CSS, there are some predefined tags that help format tags. It is recommended to stick to this tags - and if necessary style them via CSS as required - instead of defining everything with pure CSS. That's because using these tags tells search engines like Google that a certain text is emphasized and it can weight it accordingly.

There are two general tags for emphasizing text in HTML:

```html
<strong>This text is by default shown in bold letters</strong>

<em>This text is by default shown in italic letters</em>
```

The two tag are *semantic* tags for emphasizing text passages. If you are only interested in showing letter in bold, italic oder underlined, you can use the following:

```html
<b>This text is bold</b>

<i>This text is italic</i>

<u>This text is underlined</u>
```

If you want to change the color of text, you need to use CSS. For example, if you want to do it inline, you can use the `<span>` tag:

```html
<span style="color: red"><b>This text is bold and red</b></span>
```

There are some more tags for formatting text, which you can read about [here](https://www.w3schools.com/html/html_formatting.asp)

# Links

- [HTML Images at W3Schools.com](https://www.w3schools.com/html/html_images.asp)
- [HTML Links at W3Schools.com](https://www.w3schools.com/html/html_links.asp)
- [HTML Text Formatting at W3Schools.com](https://www.w3schools.com/html/html_formatting.asp)