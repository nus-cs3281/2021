# Intersection Observer API
---

This web API was brought to my attention by my mentor when I was implementing the Disqus plugin for MarkBind.

Disqus comments are typically positioned at the bottom of the page. Since it can be expensive to load the comments, they should only be loaded when necessary — that is when users want to read them. Therefore, it is appropriate to introduce lazy-loading here, so that comments are only loaded when the user scrolls to the comment section.

However, implementing a solution to detect whether a user has scrolled to a particular section can be difficult and "messy". This is where the **Intesection Observer API** comes in handy. One of the main motivations for this API was to tackle the lazy-loading problem; by providing a way to detect when a target element comes into the viewport.

## Sources

Mozilla usually have very comprehensive and well-written documentation for web APIs. This is the case for [Intersection Observer API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API) as well.

Another good resource that I would recommend to learn about this API is an [introductory video by Kevin Powell](https://www.youtube.com/watch?v=T8EYosX4NOo&ab_channel=KevinPowell).

# Markdown Parsers
---

Markdown is one of the core components of MarkBind. As I was working on the markdown feature to enable `++` for larger text, `--` for smaller text, and `$$` for underlining, I decided to research more about the markdown parsers in general.

As a start, I was curious as to why we chose [**markdown-it**](https://github.com/markdown-it/markdown-it) over other alternatives. This led to the question: what could be some of the features that we need from a markdown parser? I felt that this [short article](https://css-tricks.com/choosing-right-markdown-parser) gave a pretty good explanation of the various features of a markdown parser and why we may we want to choose on library over another, based on the features we need. On top of that, this [summary and quick comparison](https://npmcompare.com/compare/markdown-it,marked,remarkable,showdown#:~:text=markdown%2Dit%20has%20more%20frequent,on%20Github%20and%20more%20forks) of the well-known markdown parsers available gave me a glimpse as to why we decided on **markdown-it**.

Even though **marked** is considerably more popular than **markdown-it** based on download count, **markdown-it** actually offers more features to users such as a better curation of community-written plugins. More importantly, **markdown-it** has good adherence to the [CommonMark specification](https://spec.commonmark.org/), which gives us a standard, unambiguous syntax specification for Markdown. Here is a [specific instance](https://github.com/MarkBind/markbind/pull/1483#issuecomment-782983050) where adherence to CommonMark specification has benefitted us in making syntax decisions for MarkBind. Furthermore, the API documentation for **markdown-it** seems to be more thorough and well-written than **marked**. This leads me to believe that perhaps **marked** is a better choice for lightweight usage while **markdown-it** is more suitable for users who require more complex features (which can be found in the form of plugins).

What I have written so far is based on my brief insight into the available markdown parsers. There are a lot more details and nuances that I have yet to look into like — markdown parsers are actually quite complex! I will continue to update this space when I find out more about markdown parsers. 

# Why you should **not** use `setTimeout` to resolve asynchronous bugs (even when it *seems* to work)
---

Whenever I suspect that I am looking at an asynchronous bug, I would usually use `setTimeout` to help me ascertain whether the bug is asynchronous in nature. Not knowing better, I used to employ it as a way to execute a function after a certain asset is loaded. However, after hearing [Philip Roberts' explanation on Javascript's event loop](https://www.youtube.com/watch?v=8aGhZQkoFbQ&ab_channel=JSConf) and learning more about it, I know to never do it again. If Javascript's event loop sounds foreign to you, I highly recommend that you watch the video! It is arguably one of the most popular videos that explains the concept in a manner that is easily understandable with really good visual examples.

I learnt that `setTimeout(functionToBeExecuted, 0)` doesn't actually execute `functionToBeExecuted` after 0 millisecond — it only tries to execute the function after 0 millisecond, provided the call stack is empty. If there are other frames on the call stack, then it would have to wait until the frames are resolved before the callback function, `functionToBeExecuted`, is pushed on the stack for execution.

Here's the main problem: `setTimeout` is non-deterministic. There isn't any guarantee on when the callback function would execute. Thus, it's not a good idea to rely on this non-deterministic scheduling to execute your function at a *specific* time. 

Suppose you wish to use `setTimeout` to schedule a callback after a certain asset is loaded, you would have to "estimate" when this asset would be loaded. And this estimation is no more than a gamble when we have to take the speed of the user's network and/or device into consideration. Thus, this is never a reliable way to execute your callback function at a specific time. 

Instead, a better solution, perhaps, would be to look for an appropriate [event hook](https://developer.mozilla.org/en-US/docs/Web/Events) such as [DOMContentLoaded](https://developer.mozilla.org/en-US/docs/Web/API/Window/DOMContentLoaded_event) to ensure that your function executes at the specific time that you want. 

# Hot-reload vs. Live-reload
---

This topic confused me for quite awhile when I started working on MarkBind. However, all my doubts were cleared when I read up on an [article](https://reactnative.dev/blog/2016/03/24/introducing-hot-reloading#hot-reloading) recommended by my mentor. Let me explain these concepts in the context of MarkBind. 

## Hot-reload

`hot-reload` is a feature that is enabled when MarkBind is served in development mode. And it is especially useful in helping to boost our productivity as developers. When served in development mode, `hot-reload` will be triggered when there are any changes to the source files that are bundled by webpack. Since our source files in `core-web` and `vue-component` are bundled by webpack, `hot-reload` will be triggered if we were to make any changes to the source files in `core-web` or `vue-components`.

During runtime, `hot-reload` patches the changes that we have made in our source files to the files that are served in the browser, without having to reload the browser. Thus, we do not lose any state which is really helpful when we are tweaking the UI.

Note that `hot-reload` is entirely enabled by webpack, specifically [`webpack-hot-middleware`](https://github.com/webpack-contrib/webpack-hot-middleware) and [`webpack-dev-middleware`](https://github.com/webpack/webpack-dev-middleware). As the names suggest, these are middlewares that are injected into our development server, `live-server`, so that the `hot-reload` capability is enabled for our source files.

## Live-reload

`live-reload`, on the other hand, will **reload** the browser whenever changes to the source files are detected. In `live-reload`, we are looking at a different set of source files (e.g. `md`, `mbdf`) when compared to `hot-reload`. Let me go through briefly how `live-reload` works in MarkBind currently. 

We make use of [`chokidar`](https://github.com/paulmillr/chokidar), a file watcher, to watch the source files for any changes made by the author. Upon detection of addition / deletion / modification of the source files, `chokidar` will execute the callback functions passed into it. The callback functions that we pass into `chokidar` will trigger the rebuild of the source files and output the built files into the designated output folder. Once `live-server` detects updates in the output folder, it will trigger a browser reload to update the files on the browser.

## Use-cases

Essentially, you can see `hot-reload` as a feature that is meant for MarkBind developers (although the `live-reload` feature is also useful to us for testing purposes), and `live-reload` as a feature that is targeted at authors who use MarkBind to aid their write-up process.

# Server-Side Rendering (SSR) using Vue
---

Implementing SSR for MarkBind is definitely the highlight of my learning and contributions this semester. In the following sections, I will be giving a summary of what I have learnt and implemented for MarkBind with regards to SSR. Note that I will only be discussing the main ideas of SSR. 

## What is SSR?

To understand what SSR entails, it is first important to understand what Client-Side Rendering (CSR) is. You probably would have seen such terminologies before if learnt about web development in general. But what are they really? And why do we need them? Let me kindly refer you to this article, [Rendering on the Web](https://developers.google.com/web/updates/2019/02/rendering-on-the-web). This article has a really good write-up on the various rendering options, their motivations, and the difference between them.

## Why should MarkBind adopt SSR? 

Our motivation to introduce SSR in MarkBind is to tackle the problem of [flash-of-unstyled-content (FOUC)](https://www.techrepublic.com/blog/web-designer/how-to-prevent-flash-of-unstyled-content-on-your-websites/). The reason as to why FOUC occurs in the first place is due to MarkBind's usage of CSR. 

Let me give an example of how CSR causes FOUC. When we send our HTML page over to the client's browser, a portion of our HTML page can look something like this: 

``` html
<box>
	example	
</box>
```

Note that `<box>` is a Vue component of MarkBind that needs to be compiled and rendered by Vue so that it can be transformed into proper HTML. After compilation and rendering by Vue, it may look something like this:

``` html
<div data-v-7d17494f="" class="alert box-container alert-default">
	<div data-v-7d17494f="" class="box-body-wrapper">
		<div data-v-7d17494f="" class="contents">
			example	
		</div>	
	</div>
</div>
```

As you can see, there is a drastic difference in the HTML markup. Most importantly, the classes, which our CSS applies styling to, only appears after Vue compiles and renders the `<box>` markup. 

Notice that the inner content of the `<box>` markup, `example`, would still be rendered by the browser both scenarios. This means that during the time Vue compiles and renders the `<box>` markup, there will be no styling applied to `example`. Thus, we face a problem of FOUC here.

A straightforward and proper way to resolve this is to use SSR instead of CSR; by compiling and rendering the HTML markup on the server before we send the HTML page to the client.

## How can SSR be implemented? 

Implementing SSR is not trivial. Beyond just implementing SSR, most of the time you would have to tweak your implementation to fit the setup that you are working with. Therefore, your implementation of SSR may differ in one way or another when compared to the SSR examples that you may find online.

Here are some good resources to get started with SSR: 
- [Official Vue SSR Guide](https://ssr.vuejs.org/)
- [Vue & SSR: The best practices](https://www.youtube.com/watch?v=OF3A_arLJ8k&t=893s&ab_channel=VueMastery) — This is a talk conducted by Sebastien Chopin, where he brings you through the various stages of implementing SSR. The code for his implementation of SSR in various stages can be found [here](https://github.com/Atinux/vue-ssr-amsterdam). 
- [VuePress](https://github.com/vuejs/vuepress) — Created by the core development team of Vue, this is a static site generator that uses SSR behind the scenes. It provides a good reference for us to see how SSR can be implemented and how we can modify their implementation to fit our setup.
- [Vue Hackernews](https://github.com/vuejs/vue-hackernews-2.0)

I believe that by going through the resources above, you will have a good idea of how SSR is generally implemented in "classical" setups.
 
## Implementing SSR for MarkBind

After gaining a level of understanding of how SSR can be implemented in "classical" setups, we will now look at how SSR can be implemented in MarkBind.

### Pre-requisites

#### <ins>Understanding how Vue works</ins>

Before we start looking at how to implement SSR for MarkBind, it is important to first have a sound understanding of how Vue works. Some foundational questions that you may want to ask are:
- What is a Vue instance?
- What does it mean to compile Vue?
- What are render functions?
- Are there any differences between compiling Vue on client-side versus server-side?
- What is the difference between compiling and rendering?

Generally, I think that the [official Vue documentation](https://vuejs.org/v2/guide/) gives a decent explanation to the questions above. However, on top of studying the official documentation, I would also **highly** recommend watching this talk by the founder of Vue, [Evan You - Inside Vue Components - Laracon EU 2017](https://www.youtube.com/watch?v=wZN_FtZRYC8&ab_channel=LaraconEU), where he explained the inner-workings of Vue.

#### <ins>Understanding MarkBind's Architecture</ins>

It is important that we first understand how packages MarkBind interact with one another and the role each of them play in the entire MarkBind setup, this is because all of them will be affected to various extents if we were to implement SSR in MarkBind; in my opinion, SSR is considered an architectural feature, after all.

There are four packages in MarkBind's codebase:
1) `cli`
2) `core`
3) `core-web`
4) `vue-components`

To give a better context of how these packages work in tandem to SSR, I will provide a short explanation below. However, if you are interested, you may read about the project structure [here](https://markbind.org/devdocs/devGuide/design/projectStructure.html).

`vue-components` is where we house all of our [component assets](https://markbind.org/userGuide/usingComponents.html) like `<box>` and `<panel>` which users can use to enhance the appearance / behaviour of their pages. Essentially, `vue-components` is packaged as a [Vue plugin](https://vuejs.org/v2/guide/plugins.html) which we will install on Vue on the client-side, so that HTML tags like `<box>` can be transformed by Vue into proper HTML. 

`core-web` contains the client-side script assets that we will send to the user's browser so that Javascript can be executed on the HTML markup. This package is also where Vue will install the `vue-components` plugin, create the Vue instance for each page, and then compile and render the page into proper HTML.

Note that all of the compilation and rendering so far happens on the client-side — what we are doing is client-side rendering. 

If you have watched this video, [Evan You - Inside Vue Components - Laracon EU 2017](https://www.youtube.com/watch?v=wZN_FtZRYC8&ab_channel=LaraconEU), that I have highlighted above, and have some understanding of MarkBind's codebase in general, you should also understand the following:
- Each page in MarkBind represents a **Vue instance / application**
- Each page in MarkBind is essentially a **template** which Vue has to compile into a Javascript [`render`](https://vuejs.org/v2/guide/render-function.html) function (the actual process is actually [**template** —> **AST** —> `render` function], but we will skip the explanation for **AST** for brevity sake)
- The [`render`](https://vuejs.org/v2/guide/render-function.html) function returns the [**Virtual DOM**](https://blog.logrocket.com/how-the-virtual-dom-works-in-vue-js/) of the MarkBind page
- The [**Virtual DOM**](https://blog.logrocket.com/how-the-virtual-dom-works-in-vue-js/) helps to generate the **Actual DOM** (which is the HTML mark-up that is achieved after Vue is done compiling and rendering the page)

**Note**: By creating a Vue instance for the page on the client-side (without passing in a `render` function as argument), the compilation of the page into `render` function and transformation of the HTML markup is done behind-the-scenes by Vue. When implementing SSR, however, we must split up the steps of compilation and rendering so that we have more control over the process.

### Integrating SSR into MarkBind's setup

After understanding how Vue works and how MarkBind's packages are related, we then look at our MarkBind setup to see how we can integrate SSR into the setup. In essence, what we want to achieve here is to "shift" the client-side rendering onto our server, which is in the `core` library.

To achieve that, we can break down the process into three steps: 1) compiling each page into Vue `render` function, 2) installing `vue-components` plugin on Vue, 3) instantiating a Vue instance with the `render` function to output the final HTML markup strings.

#### <ins>1) Compiling each page into Vue `render` function</ins>

In step one, we are looking to compile each page into the `render` function on the server, instead of on the browser. It is important to understand that this `render` function is representative of our final Vue application for a particular page. Thus, before we compile the page into Vue `render` function, it is imperative that we finalize all the necessary DOM modifications we need to make for the page. There should be no DOM modifications going forward. 

Compiling each page into a `render` function is straightforward. We simply use `vue-template-compiler` to help us do that. Below is an illustration of how it works. 

``` js
  // Compile Vue Page
  const compiledVuePage = VueCompiler.compileToFunctions(content);

  // Set up script content
  const outputContent = `
    var pageVueRenderFn = ${compiledVuePage.render}; // This is the render function!
    var pageVueStaticRenderFns = [${compiledVuePage.staticRenderFns}]; // This is the render function!
  `;
```

This then brings us to the concept of [**universal application**](https://ssr.vuejs.org/guide/universal.html#data-reactivity-on-the-server) in SSR. Basically, in our context, this means that our Vue page application on the client-side and the server-side must be the **same**. Notice that what we are doing so far in step one helps us to achieve that exactly; as we pass in the same `render` function, which represents our page, when instantiating the Vue instance on both client-side and server-side.

On top of passing in the same `render` function, we have to also remember to pass in the **exact same state data** when initializing Vue instance. This is because if we pass in different states into the Vue instance on client-side vs. server-side, this may cause the final HTML mark-up between the client-side and server-side to be different, causing us to have "different" applications on client-side and server-side. This is problematic as it causes what is known as [**hydration issue**](https://ssr.vuejs.org/guide/hydration.html), which will be elaborated in a later section.

#### <ins>2) Installing `vue-components` plugin on Vue</ins> 

As mentioned, on the server-side, the `render` function that we have obtained in step one will have to be passed in when instantiating the Vue instance. However, before we instantiate the Vue instance, we have to first install the `vue-components` plugin on Vue, so that the Vue instance will be able to render our page Vue application into proper HTML strings (e.g. `<box>` component can be rendered into the appropriate content that we have wrote for it in `vue-components` package).

In our old CSR setup, we did not have a way to bring this `vue-components` plugin into the `core` package. Thus, we have to look at our **webpack configuration** in `core-web` and modify it so that we are able to bring that plugin into our `core` package, to be able to conduct SSR. 

Let me explain the general changes we have to make to the webpack configuration.

In our webpack configuration, we have to define what is known as **client-entry** and **server-entry**. The former is the bundle that we will be sending to the client and the latter is the bundle which we will need to use in the server for SSR.

**client-entry** involves: 
- `core-web` client-side (browser) scripts
- `vue-components` plugin

**server-entry** involves:
- `vue-components` plugin

**client-entry** is essentially what we had in the old CSR setup (no changes). The only change here is adding a new `server-entry`, which bundles just the `vue-components` package that is needed for SSR.

The code structure above is inspired by [Vue's official documentation for SSR](https://ssr.vuejs.org/guide/structure.html#code-structure-with-webpack).

#### <ins>3) Instantiating a Vue instance with the `render` function to output the final HTML markup strings</ins>

In the third step, we want to finally execute SSR by using the `render` function to help us output the final HTML markup strings that we want to send to the client.

As mentioned, the `render` function that we have obtained in step one will have to be passed into the Vue instance on both the client-side and server-side. 

In CSR, we would typically pass in the raw page template string as the argument instead of the `render` function. But this means that Vue will execute compilation on the page template string behind-the-scenes for us. Since we have already compiled our page into Vue `render` function in the first place, we will simply pass in the `render` function when instantiating Vue instance and avoid compiling our page again. 

Here is a simple illustration of step three.

``` js
  const { MarkBindVue, appFactory } = bundle;

  const LocalVue = createLocalVue();
  LocalVue.use(MarkBindVue); // Install vue-components plugin! 

  // Create Vue instance!
  const VueAppPage = new LocalVue({
    render(createElement) {
      return compiledVuePage.render.call(this, createElement); // This is the render function!
    },
    staticRenderFns: compiledVuePage.staticRenderFns, // This is the render function!
    ...appFactory(), // Pass in state data!
  });

  let renderedContent = await renderToString(VueAppPage); // This is the SSR output!
```

Let me explain more about `appFactory` in the code snippet above. `appFactory` actually comes from `VueCommonAppFactory`, which is one of the crucial components in our SSR setup. As mentioned earlier, both client-side and server-side will have to create their respective Vue instances. However, those two Vue instances have to represent the same Vue page application; meaning that `render` function, as well as the state data between them, have to be the same. This helps us to achieve a **universal application** so that we don't run into **hydration issues**, which will break our SSR implementation.

## Client-side Hydration in SSR

Hydration is one of the biggest challenges you can face in implementing SSR for any setup.

According to Vue, [Hydration](https://ssr.vuejs.org/guide/hydration.html) refers to the client-side process during which Vue takes over the static SSR HTML sent by the server and turns it into dynamic DOM that can react to client-side data changes. 

During the hydration process, Vue essentially `diffs` your SSR HTML markup against the virtual DOM generated by the render function on the client-side. If any difference is found, meaning that the application that we have on the client-side (the virtual DOM tree) differs from the SSR HTML mark-up that we send to the client, Vue will reject the SSR HTML output, bail client-side hydration, and execute full client-side rendering. 

This means that our effort in implementing SSR will be in vain; which is why I kept emphasizing on ensuring a proper **universal application** in the previous sections. Theoretically, it isn't difficult to maintain a universal application per-se, as long as you ensure two things: 
1) the state data are the same between client-side and server-side, 
2) after compiling and rendering the Vue page application, the SSR HTML mark-up is not modified.

However, there are also other scenarios that can cause hydration issues, which will be explained in the following sections. In view of that, it is important that we nail down the **universal application** aspect of SSR first, before we deal with the other scenarios that can cause hydration issues. 

### Penalties of Bailing Client-side Hydration

When hydration fails, on top of the wasted time and effort in executing SSR, we will also incur the additional time penalty of executing client-side hydration (where CSR will follow afterwards). 

Fortunately, even if we face hydration issues and execute full CSR, the FOUC problem will still be resolved nonetheless. The reason for this is because the SSR HTML markup should resemble the CSR HTML markup to a large extent. 

Supposedly, hydration issues typically occurs due to minor differences between client-side rendered virtual DOM tree and the server-rendered content. Of course, this is assuming that we are adhering to the **universal application** concept as much as possible.

### Types of Hydration Issues

The hydration issues that I have encountered when implementing SSR for MarkBind so far can be separated into two categories:

1) Violation of HTML spec (e.g. having block-level elements within `<p>` tag) 
2) Modifying the SSR HTML mark-up after compiling and rendering the page

The second category is easy to resolve — we just have to avoid modifying the SSR HTML markup! 

The first category was challenging, however. Typically, if you were to write raw HTML from scratch, you will be warned whenever you nest block-level element within a `<p>` tag. However, this isn't the case when we use Vue/Javascript to generate our DOM. The browser will not raise any errors nor complain about it. 

Thus, it is imperative that we ensure the HTML markup that we generate is compliant with the [HTML content models specification](https://www.w3.org/TR/2011/WD-html5-20110525/content-models.html) as much as possible (e.g. do not have `<div>` nested within `<span>`).

### Dealing with the Hydration Issues 

In this section, I will give some advice as to how you can resolve hydration issues in MarkBind. Remember to always refer to the developer console of the browser that you are using as that is where the hydration error messages will be logged.

#### <ins>Using Development Vue</ins>

The first important step in resolving hydration issues is to ensure that you are using **development version** of Vue and not the production version. 

This is because only **development Vue** can report the exact mismatching nodes that are causing the hydration issues. Production Vue will only log a generic `"Failed to execute 'appendChild' on 'Node': This node type does not support this method"` error message that doesn't help you in finding out where/what the hydration issue is. Thus, it is nearly impossible to resolve hydration issues without the hydration error logs provided by development Vue. 

Furthermore, when I was dealing with hydration issues, I even noticed that there are cases where [production Vue did not warn about hydration issues](https://github.com/vuejs/vue/issues/5907#issuecomment-439072836) even when they occured (silent failures). Thus, remember to always use development Vue when trying to deal with hydration issues.

#### <ins>Short-Circuiting Nature of Hydration Process</ins>

Note that the hydration process is "short-circuiting" in nature. The assertion process of hydration will go in a top-down manner. Once it encounters a mismatch of nodes, it will immediately bail hydration and execute client-side rendering. This also means that there can be potentially more hydration issues in the later part of the document. 

#### <ins>Understanding the Hydration Error Log</ins>

Here is an example of how the hydration error log may look like in development Vue:

```
! Parent: >div

! Mismatching childNodes vs VNodes:
  NodeList(62) [...]
  VNodes(58) [...]

! [Vue Warn]: The client-side rendered virtual DOM tree is not matching server-rendered content. This is likely caused by incorrect HTML markup, for example nesting block-level elements inside <p>, or missing <tbody>. Bailing hydration and performing full client-side render.
```

When you first look at the numbers, it may come across as alarming that there are 62 nodes that are mismatching. However, that is not exactly the case. It simply means that in the parent node `<div>`, the SSR HTML markup has 62 child nodes and the in the virtual DOM of the client-side, there are 58 VNode children.

From my experience, most, if not all, of the hydration issues I faced in MarkBind are similar to this particular error log, where the problem is due to `nesting block-level elements inside <p>`. And this problem stems from an existing bug [#958](https://github.com/MarkBind/markbind/issues/958#issuecomment-821011663).  

Going forward, there should not be such hydration issues anymore. But if there is ever a case, you may want to take a look at [#958](https://github.com/MarkBind/markbind/issues/958#issuecomment-821011663) to see if the hydration issue is somehow related to it.

#### <ins>Debugging Hydration Issues</ins>

Trying to pinpoint the cause of the hydration issues in your source file can be a very tedious and manual process. Taking into account the top-down assertion process of hydration, it may be a good idea to start from the top of the source file and try to isolate which part of the source file is causing hydration issues. The "binary search" approach can also a great way to isolate the source syntax that is causing the hydration issue. 

Most importantly, regardless of which debugging approach you undertake, I think we should always seek to isolate the part of the source file which we suspect is causing hydration issue. When dealing with a large document, it is always a good idea to take suspicious part of the document out and test it separately, to see if there is any hydration issues.

Debugging hydration issues is not easy but here are some resources that really helped me in rectifying the hydration issues when implementing SSR on MarkBind:
- [Understand and solve hydration errors in Vue.js](https://www.sumcumo.com/en/understand-and-solve-hydration-errors-in-vue-js)
- [What to do when Vue hydration fails](https://blog.lichter.io/posts/vue-hydration-error/)

## Other Challenges Faced when Implementing SSR for MarkBind

Implementing SSR for MarkBind also introduced quite a few challenges along the way. I will not delve too much into the details of these challenges but here are some of them:
- Learning more about how Vue works
- Learning about webpack and how it is currently used in MarkBind
- Understanding MarkBind's architecture and analyzing how SSR can be integrated into the setup 
- Setting up SSR for MarkBind in development mode
- Ensuring that the SSR implementation works in all modes
- Deciding how the automated tests would be affected with SSR in place (we decided to retain the HTML output that is not server-rendered)
- Resolving CI issues where the tests run fine locally but not on certain CI platforms (e.g. setting Node environment variables; [`cross-env`](https://www.npmjs.com/package/cross-env) really helped a lot here)
- Resolving hydration issues due to existing bugs in MarkBind (#1548)[https://github.com/MarkBind/markbind/pull/1548]

In my opinion, there aren't many resources out there for Vue SSR as compared to React SSR. Thus, if the resources for Vue SSR are not sufficient, you may want to reference some of the React SSR resources; the principles for both should be somewhat similar.

## Final Thoughts 

Working on SSR required a lot of research and learning across the latter half of the semester. Beyond that, there were also quite a few implementation challenges along the way; where existing bugs were discovered in the codebase that gave me a harder time in implementing SSR. Some of these bugs even threw me off and misled me into thinking that there were some errors in my implementation. 

There were quite a few moments where such problems got me scratching my head to figure out what was going on. This got me to realise the importance of having the ability to identify and diagnose problems *accurately* and that sometimes this only comes with experience (a.k.a learning it the hard way).

In addition, code reviews with peers and senior contributors have never failed to surprise me. It is during these code reviews that I always found out the hard way that there were much easier and more elegant ways to solve the problem than the approach I came up with. Despite thinking that I could have saved so much time and finished the task earlier, I soon realized that it was not the point. The learning from the review process, in my opinion, really was the main takeaway from the module.  

At the end of the day, even though the cost-to-reward ratio for implementing SSR doesn't seem to be the best (where so much work is done just to resolve the FOUC issue), we also have to consider that in working on this PR, we also managed to iron out other existing important issues like [#1548](https://github.com/MarkBind/markbind/pull/1548). Furthermore, it was a good opportunity that allowed me to learn about MarkBind's architecture and the associated technologies in greater detail. All in all, I think that the time and effort spent on this was well worth it.
