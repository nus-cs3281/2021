### `htmlparser2`: DOM Traversal, Manipulation, and Things to Watch Out

MarkBind uses a combination of [`htmlparser2`](https://github.com/fb55/htmlparser2) and [`cheerio`](https://cheerio.js.org/)
for operations with the HTML representation before finalizing the page to be served, the former is for parsing HTML strings
into pseudo-DOM nodes, and the latter is for manipulating those nodes, adding/removing new nodes, and selecting specific nodes,
akin to [`jQuery`](https://jquery.com/).

However, as `cheerio` does a lot of work behind the scenes (especially in `cheerio.load`), the library can be deemed as expensive to use in a
performance-sensitive environment. Thus, `cheerio` is used sparingly, and when the situation calls for it. With this in mind,
I had to be creative in manipulating the DOM as best as I can without `cheerio`.

After some research and manual look into the DOM nodes in MarkBind, I get some understanding of the DOM nodes are structured. \
A DOM node provided by `htmlparser2` is generally structured like this:

```js
{
    type: "tag",
    name: "span",
    attribs: { class: "no-lang" }
    children: [
        {
            type: "text",
            data: "This is a span",
            children: [],
            parent: ..., // Circular reference to the "tag" node
            prev: null,
            next: null
        }
    ],
    parent: null,
    prev: null,
    next: null
}
```

Where each properties are:
- `type`: The type of the node, it can be `"tag"` for an element with HTML-tags, `"text"` for just a plain-text, and more.
(Note that `"text"` nodes cannot exist alone, it must be a child of another node)
- `name`: In the case of `"tag"` nodes, this is the name of the tag, such as `"span"`, `"div"`, etc.
- `attribs`: In the case of `"tag"` nodes, this is an object to store attribute data of the tag, such as classes, ids, etc.
- `data`: In the case of `"text"` nodes, this is the actual text content.
- `children`: An array of DOM nodes which describes the node's children.
- `parent`: The DOM node for the node's parent.
- `prev`: The DOM node preceding this node.
- `next`: The DOM node right after this node.

Once I understood this structure, I can start to play around with the DOM nodes.

First thing is to try to do a traversal of the node. Generally, we can do a depth-first traversal by
recursing to its `children`. I found it very useful if you want to process continuous data such as texts with markups inside it.

Then manipulation is as simple as modifying the properties of the DOM nodes.
- If you want to add new classes, you can just append the `attribs.class` property with the new class.
- If you want to reorder the children, you can directly change the `children` array.
- In a similar vein, if you want to add or remove child nodes, you can directly add or remove it on the `children` array.

However, when you play around with the actual structure of the node (like the last two above), we need to watch out several
things on the references.
- Set the `prev` value to be the node that is directly before this.
- Set the `next` value to be the node that is directly after this.
- After the two is finished, update the node's `parent` to the node's actual parent.

This should address the referencing pretty well for the node to be rendered properly. However, I have to note that the
references are better to be handled with `cheerio` whenever possible, and direct manipulation of references are to be done
when under a constraint.

### `highlight.js`: Tokens and HTML-Preserving Quirks

In researching ways to partially highlight text in a syntax-highlighted code block, I ended up understanding part of the way
[`highlight.js`](https://highlightjs.org/) applies syntax highlighting to a text.

`highlight.js` breaks down the text to *tokens*, where each token will be wrapped as a `<span>` and has a specific styling
prepared in the applied theme's CSS. These tokens may actually contain other tokens, in which it will be displayed as nested
elements. The token names are useful if you want to target specific styling to specific types of tokens, such as adding stronger
color for variables, muting down the color for comments, and so on. 

Then there is a quirk to `highlight.js` highlighting method that it somehow preserves HTML in a code block when the text is
being broken down to tokens. That is, the HTML code does not get parsed and are not considered a part of the code block. That
means, one can add a custom styling HTML element wrapping a part of the text and the styling will still be displayed even though
the element was parsed and styled by `highlight.js`.

This was a good candidate for partial text highlighting. But unfortunately, this does not apply when the language being parsed
is HTML and other variants (e.g. XML). So, partial text highlighting cannot be reliably done in this way across all languages.

References:
- https://github.com/highlightjs/highlight.js/issues/1561