### DOM Traversal, Manipulation, and Things to Watch Out

MarkBind uses a combination of [`htmlparser2`]() and [`cheerio`]() for operations with the HTML representation before finalizing
the page to be served, the former is for parsing HTML strings into pseudo-DOM nodes, and the latter is for manipulating those nodes, adding/removing new nodes, and selecting specific nodes, akin to [`jQuery`]().

However, as cheerio does a lot of work behind the scenes, the library can be deemed as expensive to use in a
performance-sensitive environment. Thus, `cheerio` is used sparingly, and when the situation calls for it. With this in mind,
I had to be creative in manipulating the DOM as best as I can without `cheerio`.

After some research and manual look into the ones in MarkBind, I get some understanding of the DOM nodes are structured. \
A DOM node provided by `htmlparser2` is generally structured like so:

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
