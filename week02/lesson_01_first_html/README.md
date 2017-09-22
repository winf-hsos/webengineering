# Table of Contents
- [The HTML Structure](#the-html-structure)
  * [The Document is A Tree](#the-document-is-a-tree)
  * [First HTML Tags](#first-html-tags)
  * [Summary](#summary)
  * [Remarks](#remarks)

# The HTML Structure

The following explanations reference the source file [`00_html_scaffold.html`](https://github.com/winf-hsos/webengineering/blob/master/week02/lesson_01_first_html/01_html_scaffold.html).

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <title>Haster Bier Online-Shop</title>
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

## The Document is A Tree
This file demonstrates the structure of an HTML document. An HTML document has a **hierachical structure**, that means we can draw it as a tree, as shown below. 

The nodes of the HTML tree are called HTML **elements**, or simply elements. Elements are denoted with HTML **tags**. An HTML tag is enclosed by the symbols `<` and `>`, with the name of the tag written inbetween. 

Most tags come in two flavors: An opening and a closing tag. The difference is a single slash `/` that is added to the closing tag, such as `</html>`. Anything between those tags constitutes a subtree or branch.

The stem of an HTML tree is called **root element**. This is always the `<html>` element. The `<html>` element has exactly two children: `<head>` and  `<body>`.Therefore, the tree up to the seconds level looks like this:

```
           <html>
             │
        ┌────┴────┐
        |         |
      <head>     <body>
```
## First HTML Tags

The above is a fairly small tree. To make things more interesting, each of the two subtrees can have children of its own, and they can again have children, and so forth. There is no limit to the depth of the tree. 

In the example in [`00_html_scaffold.html`](https://github.com/winf-hsos/webengineering/blob/master/week02/00_html_scaffold.html), the full tree looks like this (actually it is drawn upside down, but that is how we usually do it in IT for better readability):

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

As you can see, the `<head>` tag has two children, `<meta>` and `<title>`. They both contain information about the HTML document (or website), and they are not displayed on the page. They are useful to describe the content, for example for search engines such as Google. In contrast, the `<body>` tag's children make up the visible part of the HTML document.

So what is the visible part in this case? Well, within the `<body>` tags, we find the following:

- An `<h1>` tag, which is short for a *heading of level 1*
- A `<p>` tag, which declares a *paragraph* of text
- An `<ul>` tag, which is short for *unordered list* and defines the start of such
- Two `<li>` tags, which each denote a *list item* of the unordered list


It is important to note that there are elements that only have an opening-tag. For example, the `<img>` tag to display images in a document. Such elements can't have other elements as children; they are therefore leafs of the tree.

It is crucial that you understand the basic structure of an HTML document, as it is going to be with us during the rest of the semester.

## Summary

The key takeaways are:

- An HTML document is a tree
- HTML tags are the nodes of the tree, and everything inbetween a pair of tags is a subtree
- Meta information goes in the `<head>` element, while visible stuff lives in the `<body>` element

The table sums up the HTML tags we have learned about so far. We'll come back to them in the following lessons.

| Tag | Description |
| --- | --- |
| `<html>` | Root element for every HTML document |
| `<head>` | Contains mostly meta data and style information (CSS) |
| `<meta>` | Defines a meta datum for the document (e.g. title, keywords, description) |
| `<title>` | Sets the title that is shown in the top bar of the browser |
| `<body>` | Contains the visible content of the HTML document |
| `<h1>` | Defines a heading of the highest level |
| `<p>` | Defines a paragraph of text |
| `<ul>` | Introduces an unordered list |
| `<li>` | Defines one item in a list (unordered or ordered) |

# Remarks

**NOTE 1:** The first line `<!DOCTYPE html>` tell the browser that this is an HTML document and that it should be display accordingly.