# Week 2

## The HTML Structure (`00_html_scaffold.html`)
This file demonstrates the simple structure of an HTML document. Within the `<body>`- tags, 
you also find some important HTML-tags:

- `<h1>` - Is short for *heading of level 1*
- `<p>` - Defines a paragraph of text
- `<ul>`- Is short for *unordered list* and defines the start of a such
- `<li>`- Is short for *list item*

The first line `<!DOCTYPE html>` is mandatory and tells the browser that this is an HTML document and must be handled as such.

An HTML-tag is enclosed by the symbols `<` and `>`, and the name of the tag stands inbetween. Many HTML-tags come in two versions, for example:

- `<html>`and `</html>`

See the difference? The second has an additional slash before the tag's name. This denotes it a *closing tag*. So why the start- and closing tags? Because an HTML document is hierachical and forms a tree, and everything within a specific start- and closing is a branch of this tree.

For example, the `<html>`-element is the root of tree, i.e. the mother of all branches. That makes sense, because everything else is contained in the start- and closing-tag of the `<html>`-element.

The two direct children of the `<html>`-element are the `<head>`- and the `<body>`-elements. So the tree looks like this (actually it is drawn upside down, but that is how we usually do it in IT):

```
           <html>
             │
        ┌────┴────┐
        |         |
      <head>     <body>
```

If we consider the child-elements of the `<head>`- and `<body>`-elements, we get this tree:

```
              <html>
                │
        ┌───────┴───────┐
        |               |
      <head>          <body>
        |               |
   ┌────┴──┐       ┌────┼───┐
<meta>  <title>   <h1> <p> <ul>
                            |
                         ┌──┴──┐
                        <li>  <li>
```

There are elements that only have an opening-tag, for example the `<img>`-tag to display images in an HTML document. These elements can't have other elements as children; they are therefore leafs of the tree.

The table sums up the HTML tags we have learned about so far.

| Tag | Description |
| --- | --- |
| `<html>` | Root element for every HTML document |
| `<head>` | Contains meta data, style information (CSS), links and other things that are part of the document itself |
| `<meta>` | Defines a meta datum for the document |
| `<title>` | Sets the title that is shown in the top bar of the browser |
| `<body>` | Contains the visible content of the HTML document |
| `<h1>` | Defines a heading of the highest level |
| `<p>` | Defines a paragraph of text |
| `<ul>` | Introduces an unordered list |
| `<li>` | Defines one item in a list (unordered or ordered) |

# First Steps with CSS (`01_html_with_first_css.html`)

The HTML above shows in a browser, but the style of it can be improved. Fortunately, we have a great way to style HTML-elements: CSS (Cascading Style Sheets).