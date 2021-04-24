# Intersection Observer API

This web API was brought to my attention by my mentor when I was implementing the Disqus plugin for MarkBind.

Disqus comments are typically positioned at the bottom of the page. Since it can be expensive to load the comments, they should only be loaded when necessary — that is when users want to read them. Therefore, it is appropriate to introduce lazy-loading here, so that comments are only loaded when the user scrolls to the comment section.

However, implementing a solution to detect whether a user has scrolled to a particular section can be difficult and "messy". This is where the **Intesection Observer API** comes in handy. One of the main motivations for this API was to tackle the lazy-loading problem; by providing a way to detect when a target element comes into the viewport.

## Sources

Mozilla usually have very comprehensive and well-written documentation for web APIs. This is the case for [Intersection Observer API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API) as well.

Another good resource that I would recommend to learn about this API is an [introductory video by Kevin Powell](https://www.youtube.com/watch?v=T8EYosX4NOo&ab_channel=KevinPowell).

# Markdown Parsers

Markdown is one of the core components of MarkBind. As I was working on the markdown feature to enable `++` for larger text, `--` for smaller text, and `$$` for underlining, I decided to research more about the markdown parsers in general.

As a start, I was curious as to why we chose [**markdown-it**](https://github.com/markdown-it/markdown-it) over other alternatives. This led to the question: what could be some of the features that we need from a markdown parser? I felt that this [short article](https://css-tricks.com/choosing-right-markdown-parser) gave a pretty good explanation of the various features of a markdown parser and why we may we want to choose on library over another, based on the features we need. On top of that, this [summary and quick comparison](https://npmcompare.com/compare/markdown-it,marked,remarkable,showdown#:~:text=markdown%2Dit%20has%20more%20frequent,on%20Github%20and%20more%20forks) of the well-known markdown parsers available gave me a glimpse as to why we decided on **markdown-it**.

Even though **marked** is considerably more popular than **markdown-it** based on download count, **markdown-it** actually offers more features to users such as a better curation of community-written plugins. More importantly, **markdown-it** has good adherence to the [CommonMark specification](https://spec.commonmark.org/), which gives us a standard, unambiguous syntax specification for Markdown. Here is a [specific instance](https://github.com/MarkBind/markbind/pull/1483#issuecomment-782983050) where adherence to CommonMark specification has benefitted us in making syntax decisions for MarkBind. Furthermore, the API documentation for **markdown-it** seems to be more thorough and well-written than **marked**. This leads me to believe that perhaps **marked** is a better choice for lightweight usage while **markdown-it** is more suitable for users who require more complex features (which can be found in the form of plugins).

What I have written so far is based on my brief insight into the available markdown parsers. There are a lot more details and nuances that I have yet to look into like — markdown parsers are actually quite complex! I will continue to update this space when I find out more about markdown parsers. 

# Why you should **not** use `setTimeout` to resolve asynchronous bugs (even when it *seems* to work)

Whenever I suspect that I am looking at an asynchronous bug, I would usually use `setTimeout` to help me ascertain whether the bug is asynchronous in nature. Not knowing better, I used to employ it as a way to execute a function after a certain asset is loaded. However, after hearing [Philip Roberts' explanation on Javascript's event loop](https://www.youtube.com/watch?v=8aGhZQkoFbQ&ab_channel=JSConf) and learning more about it, I know to never do it again. If Javascript's event loop sounds foreign to you, I highly recommend that you watch the video! It is arguably one of the most popular videos that explains the concept in a manner that is easily understandable with really good visual examples.

I learnt that `setTimeout(functionToBeExecuted, 0)` doesn't actually execute `functionToBeExecuted` after 0 millisecond — it only tries to execute the function after 0 millisecond, provided the call stack is empty. If there are other frames on the call stack, then it would have to wait until the frames are resolved before the callback function, `functionToBeExecuted`, is pushed on the stack for execution.

Here's the main problem: `setTimeout` is non-deterministic. There isn't any guarantee on when the callback function would execute. Thus, it's not a good idea to rely on this non-deterministic scheduling to execute your function at a *specific* time. 

Suppose you wish to use `setTimeout` to schedule a callback after a certain asset is loaded, you would have to "estimate" when this asset would be loaded. And this estimation is no more than a gamble when we have to take the speed of the user's network and/or device into consideration. Thus, this is never a reliable way to execute your callback function at a specific time. 

Instead, a better solution, perhaps, would be to look for an appropriate [event hook](https://developer.mozilla.org/en-US/docs/Web/Events) such as [DOMContentLoaded](https://developer.mozilla.org/en-US/docs/Web/API/Window/DOMContentLoaded_event) to ensure that your function executes at the specific time that you want. 

# Server-Side Rendering (SSR) using Vue [draft]

This is the highlight of my learning and contributions to MarkBind this semester.

There are so many terminologies thrown around in web development: client-side rendering (CSR), server-side rendering (SSR), pre-rendering, etc. But what are they really and why do we need them? I found that this [article](https://developers.google.com/web/updates/2019/02/rendering-on-the-web) really helped to give a good summary of what these terminologies stand for and the motivation behind them.

In MarkBind's case, the main motivation to introduce SSR is to resolve the flash-of-unstyled-content (FOUC) problem. SSR is certainly not trivial to implement and most of the time you would implement it slightly differently from the explanations given in the [official guide](https://ssr.vuejs.org/) and some model samples such as [VuePress](https://github.com/vuejs/vuepress) and [...](https://github.com/Atinux/vue-ssr-amsterdam).

However, before we even start to learn about SSR, it is important to thoroughly understand how Vue works - what are render functions? what does it mean to compile Vue? What is the difference between compiling on client-side versus server-side? What is the difference between compiling and rendering? What is a Vue application? 

After understanding the Vue-related questions, we then need to look at our MarkBind setup, what are we currently doing? Pre-render? How can we integrate SSR into our setup (SSR on its own is troublesome enough, integrating it into our setup introduces more difficulty e.g. development mode, webpack)?

Lots of research and learning has to be done, on Vue and MarkBind. Beyond that, there are also implementation challenges, you will also discover that there are existing bugs in the codebase that gives you a much harder time in implementing SSR. Furthermore, these bugs may throw you off and mislead you into thinking that there were some errors in your implementation, when the fact is that these are existing bugs that are messing with your implementation. Thus, there will be many moments where you will scratch your head as you try to understand what is going on. You will realise that being able to identify and diagnose problems accurately is a very important skill to have, and sometimes it only comes with experience. 

In addition, you will find that the way you implement certain features / resolve certain problems often are not the best way and you will find it out in the hard way - when you have spent a lot of time to resolve the problem, only to find during code review that there is a much simpler and elegant way of resolving the problem. So much time could have been saved. 

At the end, it seems like cost-to-reward ratio isn't very good, it seems like so much has to be done in implementing SSR just to resolve the FOUC issue. However, the reward, in my opinion, really comes from how much I learnt about MarkBind's architecture and Vue in general.

List the aspects you learned, and the resources you used to learn them, and a brief summary of each resource.
...

https://developers.google.com/web/updates/2019/02/rendering-on-the-web

https://snipcart.com/blog/vue-render-functions

https://www.youtube.com/watch?v=wZN_FtZRYC8&ab_channel=LaraconEU

// 'Templates should only be responsible for mapping the state to the UI.
// Avoid placing tags with side-effects in your templates, such as script, as they will not be parsed.

using dev vue vs prod vue

hydration gotchas (pros and cons of ssr) -- ensuring universal

setting up ssr in dev mode

requiring modules from string

webpack

setting node env variable

resolving CI issues (work locally but not on CI) cross-env

why SSR?

ensuring that things work in all modes (normal serve, which uses built bundle / dev mode, which auto tracks bundle hot-reload / one page mode)

hot-reload vs live-reload

SSR really exposed me to how the different packages of MarkBind interact and work with each other

always remember to rebase (when dealing with CI bugs), another person added new test file

hydration issue short circuits (first mismatch found, will just report that, so a page may have many hydration issues)
requires manual debugging (use binary search method to isolate which source syntax is causing the hydration issues)
always look for hydration issues starting from the top of the source file, since the hydration goes from top to bottom of html file

impt to use dev Vue to debug hydration issues (sometimes production may not even have warnings, silent failure and it automatically does CSR, reference: https://github.com/vuejs/vue/issues/5907#issuecomment-439072836)
