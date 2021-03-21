### Intersection Observer API 

This web API was brought to my attention by my mentor when I was implementing the Disqus plugin for MarkBind. 

Disqus comments are typically positioned at the bottom of the page. It can be quite expensive to load the comments and we should only do so when it is necessary -- that is when users actually want to read them. Thus, it is appropriate to introduce lazy-loading here, where we only load the comments when user scrolls to the comment section. 

However, implementing a solution to detect whether a user has scrolled to a particular section can be difficult and "messy". This is where the **Intesection Observer API* comes in handy. One of the motivations for this API was to resolve the lazy-loading problem, by providing a way to detect when a target elements comes into the viewport. 

https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API

Mozilla usually have very comprehensive and well-written documentation for web APIs and this is no exception. 

https://www.youtube.com/watch?v=T8EYosX4NOo&ab_channel=KevinPowell

Another resource that I would recommend to learn about this API is an introductory video by Kevin Powell. 

### Markdown Parsers

As I was implementing the markdown feature to enable `++` for larger text, `--` for smaller text, and `$$` for underlining, I decided to learn more about the parser that we were using, **markdown-it**, and the other choices available out there, because markdown is one of the core component of MarkBind.

During my research, I found out that there are a few established competitors available for JavaScript, with **markdown-it** being one of them. 

https://npmcompare.com/compare/markdown-it,marked,remarkable,showdown#:~:text=markdown%2Dit%20has%20more%20frequent,on%20Github%20and%20more%20forks

The above provides a summary of the major markdown parsers available and a quick comparison between them.

https://css-tricks.com/choosing-right-markdown-parser/

The above provides the various features of a markdown parser and helps you to decide why you may want to choose one library over another, based on the features you may need. 

Even though **marked** is a more popular parser than **markdown-it** based on downloads, **markdown-it** offers more features to users such as better curation of community-written plugins and the adherence to the CommonMark spec. Furthermore, at a quick glance, the API documentation for **markdown-it** seems to be more thorough and well-written. This leads me to believe that perhaps **marked** is a better choice for lightweight users, and **markdown-it** is more suitable for users who require more complex features (which can be found in the form of plugins).

What I have written so far is based on my brief insight into the available markdown parsers. There are a lot more details and nuances that I have yet to look into like (markdown parsers are actually quite complex!). I will continue to update this space when I find out more about markdown parsers. 

### Why you should not use setTimeout to resolve async bug (even when it seems to work)

Whenever I suspect an asynchronous issue, I usually would use setTimeout to help me ascertain that I am really facing an asynchronous bug. I used to employ it as a resolution to execute a function after a certain asset is loaded; but never again after learning more about JavaScript's event loop.

https://www.youtube.com/watch?v=8aGhZQkoFbQ&ab_channel=JSConf

I highly recommend watching Philip Roberts explaining what event loop is about. He explains it in a manner that is easily understandable with really good visual examples. 

This was where I learnt that setTimeout(example, 0) isn't quite what it is -- that it doesn't execute example function after 0 millisecond. Although it would try to execute example function after 0 millisecond, it also depends on whether the call stack is clear. If there are other frames on the call stack, then it would have to wait until the frames are resolved before the setTimeout callback is pushed on the stack for execution.

The main problem is that this is non-deterministic, so there isn't any guarantee on when it would execute. And it's not a good idea to rely on this non-deterministic scheduling to execute your function. 

Furthermore, suppose you wish to use setTimeout to schedule a function call after a certain asset is loaded, you would have to "estimate" when this asset would be loaded. And this estimation is always a gamble because we have to consider the speed of the user's network and device. Thus, this is never a reliable way. 

Instead, a better solution would be to look for an event hook like "DOMContentLoaded" to ensure your function executes at the specific time that you want.  

List the aspects you learned, and the resources you used to learn them, and a brief summary of each resource.
...

https://developers.google.com/web/updates/2019/02/rendering-on-the-web

https://snipcart.com/blog/vue-render-functions

https://www.youtube.com/watch?v=wZN_FtZRYC8&ab_channel=LaraconEU

// 'Templates should only be responsible for mapping the state to the UI.
// Avoid placing tags with side-effects in your templates, such as script, as they will not be parsed.
