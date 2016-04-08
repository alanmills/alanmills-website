Using Atom editor shortcuts to format Markdown named anchors links
==================================================================
**Author**: Alan Mills  
**Date**: [07 April 2016 16:47](/blog/history/2016-04.md)
**Tags**: [Atom](/blog/categories/atom.md), [Markdown](/blog/categories/markdown.md)

I write all of my blog posts with Atom in the Markdown format and often I will have a summary including anchor links to other parts of the page.  I was doing a lot of extra keyboard work until I discovered some Atom keyboardfu.

The steps to creating a Markdown linked table of contents:
* [Creating table of contents](creating-table-of-contents)
* [Create a heading](create-a-heading)
* [Adding links to the table of contents](adding-links-to-the-table-of-contents)
* [Using Atom editor shortcuts to format the name anchor links](using-atom-editor-shortcuts-to-format-the-name-anchor-links)

Creating table of contents
--------------------------
I use a bulleted list for my table of contents using standard Markdown
``` Markdown
Contents:
* This is the first item
* This is the second item
```

Create a heading
----------------
With the list of items from the bulleted list, I convert these to headings
```Markdown
Contents
* This is the first item
* This is the second item

This is the first item
----------------------

This is the second item
-----------------------

```

Adding links to the table of contents
-------------------------------------
The next step is to use the Markdown syntax for links to mark each item in the table contents.  This is done by surrounding the text with square brackets, followed by parentheses [First Item]\()  
```Markdown
Contents
* [This is the first item]()
```

Using Atom editor shortcuts to format the name anchor links
-----------------------------------------------------------
Now for each item in the table of contents
* copy the heading text - place the cursor in the square parentheses and press `^⌘M`, `⌘C`,
* paste the text in to the parentheses `⌘V`
* Select the text in the parentheses `^⌘M`
* While the text is still selected, convert to lower case `⌘K ⌘L`

```Markdown
* [This is the first item](this is the first item)
```

* Now select the first space within the parentheses
* Press `⌘D` to select the next space in the file
* Repeat pressing `⌘D` until you have selected all the spaces within the parentheses
* Now type the character -
* You're done 😀

```Markdown
* [This is the first item](this-is-the-first-item)
```
