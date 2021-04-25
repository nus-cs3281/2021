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

# Hot-reload vs. Live-reload

This topic is something that has confused me for quite awhile when I started working on MarkBind. All my doubts were cleared when I read up on an [article](https://reactnative.dev/blog/2016/03/24/introducing-hot-reloading#hot-reloading) recommended by my mentor. I would like to just explain these concepts in the context of MarkBind. 

`hot-reload` is a concept that deals with development mode in MarkBind and is especially useful in helping to boost our productivity as developers. When we serve in development mode, `hot-reload` will be executed when changes are detected to any source files that are bundled by webpack. Since our source files in `core-web` and `vue-component` are bundled by webpack, if you were to make any changes to the source files in `core-web` or `vue-components`, `hot-reload` will be triggered. 

Essentially, `hot-reload` patches the changes that we have made in our source files during runtime to the files that are served in the browser (without reloading the browser). Thus, we do not lose any state which is really helpful when we are tweaking the UI.

Note that `hot-reload` is entirely enabled by webpack, specifically [`webpack-hot-middleware`](https://github.com/webpack-contrib/webpack-hot-middleware) and [`webpack-dev-middleware`](https://github.com/webpack/webpack-dev-middleware). As the name suggests, these are middlewares that are injected into our development server, `live-server`, so that the `hot-reload` capability is enabled for our source files.

`live-reload`, on the other hand, will **reload** the browser whenever changes to another set of source files are detected (e.g. `md`, `mbdf`). Here is how it works in MarkBind currently. [`chokidar`](https://github.com/paulmillr/chokidar), a file watcher, will watch the source files for any changes made by the author. Upon detection of addition / deletion / modification of the source files, `chokidar` will execute the callback functions passed into it. The callback functions that we pass into `chokidar` will trigger the rebuild of the source files and output the built files into the appropriate output folder. Once `live-server` detects updates in the output folder, it will trigger a browser reload to update the files on the browser.

You can see `hot-reload` as a feature that is meant for MarkBind developers (although the `live-reload` feature is also useful to us for testing purposes), and `live-reload` as a feature that is targeted at authors who use MarkBind to aid their write-up process.

# Server-Side Rendering (SSR) using Vue [draft]

Implementing SSR for MarkBind is definitely the highlight of my learning and contributions this semester. In the following sections, I will be giving a summary of what I have learnt and implemented for MarkBind with regards to SSR. Note that I will only be discussing the main ideas of SSR in MarkBind.

## What is SSR?

To understand what SSR entails, it is first important to understand what Client-Side Rendering (CSR) is. You probably would have seen such terminologies before if you read articles related to web development in general. But what are they really and why do we need them? I found that this article, [Rendering on the Web](https://developers.google.com/web/updates/2019/02/rendering-on-the-web), really gives a detailed explanation on the various rendering options, their motivations, and the difference between them.

## Why adopt SSR in MarkBind?

In MarkBind's case, our motivation to introduce SSR is to tackle the problem of [flash-of-unstyled-content (FOUC)](https://www.techrepublic.com/blog/web-designer/how-to-prevent-flash-of-unstyled-content-on-your-websites/). The reason as to why FOUC occurs in the first place is due to our use of CSR. To give an example, when we send our HTML page over to the client's browser, a portion of our HTML page can look something like this: 

```
<box>
	example	
</box>
```

Note that `<box>` is MarkBind's Vue component that needs to be compiled by Vue so that it can be turned into proper HTML. After compilation and rendering by Vue, it may look something like this:

```
<div data-v-7d17494f="" class="alert box-container alert-default">
	<div data-v-7d17494f="" class="box-body-wrapper">
		<div data-v-7d17494f="" class="contents">
			example	
		</div>	
	</div>
</div>
```

As you can see, there is a drastic difference in the HTML markup. Most importantly, the classes, which our CSS applies styling to, only appears after Vue compiles and renders the `<box>` markup. Notice that the inner content of the `<box>` markup, `example`, would still be rendered by the browser. This means that during the time Vue compiles and renders the markup `<box>` markup, there will be no styling applied to `example` — we face a problem of FOUC here.

One way to resolve this is to use SSR instead of CSR; by compiling and rendering the HTML markup on the server before we send the HTML page to the client.

## How to implement SSR? 

Implementing SSR is not trivial. Beyond just implementing SSR, most of the time you would have to tweak your implementation to fit the setup that you are working with. Thus, your implementation of SSR may differ in one way or another when compared to the SSR examples you may find online.

Here are some good resources to get started with SSR: 
- [Official Vue SSR Guide](https://ssr.vuejs.org/)
- [Vue & SSR: The best practices](https://www.youtube.com/watch?v=OF3A_arLJ8k&t=893s&ab_channel=VueMastery) — This is a talk conducted by Sebastien Chopin, where he brings you through the various stages of implementing SSR. The code for his implementation of SSR in various stages can be found [here](https://github.com/Atinux/vue-ssr-amsterdam). 
- [VuePress](https://github.com/vuejs/vuepress) — Created by the core development team of Vue, this is a static site generator that uses SSR behind the scenes. It provides a good reference for us to see how SSR can be implemented and how we can modify their implementation to fit our setup.
 
## Implementing SSR for MarkBind

### Pre-requisites

#### <ins>Understanding how Vue works</ins>

Before we start looking at how to implement SSR for MarkBind, it is important to first have a good understand of how Vue works. Some foundational questions that you may want to ask are:
- What is a Vue instance?
- What does it mean to compile Vue?
- What are render functions?
- Are there any differences between compiling Vue on client-side versus server-side?
- What is the difference between compiling and rendering?

I think that the [official Vue documentation](https://vuejs.org/v2/guide/) does a decent job in giving explanations to the questions above. However, on top of the official documentation, I would also **highly** recommend watching this talk by the founder of Vue, [Evan You - Inside Vue Components - Laracon EU 2017](https://www.youtube.com/watch?v=wZN_FtZRYC8&ab_channel=LaraconEU), where he explained the inner-workings of Vue.

#### <ins>Understanding MarkBind's Architecture</ins>

There are four packages in MarkBind's codebase:
1) `cli`
2) `core`
3) `core-web`
4) `vue-components`

It is important that we first understand how these packages interact with one another and the role each of them play in the entire MarkBind setup, this is because all of them will be affected to various extents if we were to implement SSR in MarkBind. 

To give a better context of how these packages work in tandem to SSR, I will provide a short explanation below. If you are interested, you may, however, read about the project structure [here](https://markbind.org/devdocs/devGuide/design/projectStructure.html).

`vue-components` is where we house all of our components like `<box>` and `<panel>` which users can use to enhance the appearance / behaviour of their pages. Essentially, `vue-components` is packaged as a [Vue plugin](https://vuejs.org/v2/guide/plugins.html) which we will install on Vue on the client-side, so that HTML tags like `<box>` can be transformed by Vue into proper HTML. 

`core-web` contains the client-side script assets that we will send to the user's browser so that javascript can be executed on the browser. This is also where Vue will install the `vue-components` plugin, create the Vue instance for each page, and then compile and render the page into proper HTML. The  

Note that all of the compilation and rendering happens on the client-side — what we are doing is client-side rendering. 

If you have watched this video, [Evan You - Inside Vue Components - Laracon EU 2017](https://www.youtube.com/watch?v=wZN_FtZRYC8&ab_channel=LaraconEU), that I have highlighted above and have some understanding of MarkBind in general, you should also understand the following:
- Each page in MarkBind represents a **Vue instance**
- Each page in MarkBind is essentially a **template** which Vue has to compile into a Javascript [`render`](https://vuejs.org/v2/guide/render-function.html) function (the actual process is in fact **template** —> **AST** —> `render` function, but we will skip the explanation for **AST** for brevity sake)
- The [`render`](https://vuejs.org/v2/guide/render-function.html) function returns the [**Virtual DOM**](https://blog.logrocket.com/how-the-virtual-dom-works-in-vue-js/) of the MarkBind page
- The [**Virtual DOM**](https://blog.logrocket.com/how-the-virtual-dom-works-in-vue-js/) helps to generate the **Actual DOM** (which is the HTML mark-up that is achieved after Vue is done compiling and rendering the page)

Note: By creating a Vue instance for the page on the client-side, the compilation of the page into `render` function and transformation of the HTML markup is done behind-the-scenes by Vue. However, we must split up the steps and have more control over the process when doing SSR.

### Integrating SSR into MarkBind's setup

After understanding how Vue works and how MarkBind's packages are related, we then look at our MarkBind setup to see how we can integrate SSR into the setup. In essence, what we want to achieve here is to "shift" the client-side rendering onto our server, which is in the `core` library.

To achieve that, we can break down the process into three steps: 1) compiling each page into Vue `render` function, 2) installing `vue-components` plugin on Vue, 3) instantiating a Vue instance with the `render` function to output the final HTML markup strings.

#### <ins>1) Compiling each page into Vue `render` function</ins>

In step one, we are looking to compile each page into the `render` function on the server, instead of on the browser. It is important to understand that this `render` function is representative of our final Vue application for that particular page. Thus, before we compile the page into Vue `render` function, it is imperative that we finalize all the necessary DOM modifications we need to make for the page. 

Compiling each page into a `render` function is straightforward. We simply use `vue-template-compiler` to help us do that. Below is an illustration of how it works. 

```
  // Compile Vue Page
  const compiledVuePage = VueCompiler.compileToFunctions(content);

  // Set up script content
  const outputContent = `
    var pageVueRenderFn = ${compiledVuePage.render}; // This is the render function!
    var pageVueStaticRenderFns = [${compiledVuePage.staticRenderFns}]; // This is the render function!
  `;
```

This then brings us to the concept of [**universal application**](https://ssr.vuejs.org/guide/universal.html#data-reactivity-on-the-server) in server-side rendering. Basically, in our context, this means that our application on the client-side and the server-side should be the **same**. Notice that what we are doing so far in step one helps us to achieve that exactly; as we use pass in the same `render` function, which represents our page, when instantiating the Vue instance on both client-side and server-side.

On top of passing in the same `render` function, we have to also remember to pass in the exact same state data when initializing Vue instance. This is because if we pass in different states into the Vue instance on client-side vs. server-side, this may cause the final HTML mark-up between the client-side and server-side to be different. And this is problematic as it causes **hydration issue**, which will be elaborated in a later section.

#### <ins>2) Installing `vue-components` plugin on Vue</ins> 

As mentioned, on the server-side, the `render` function that we have obtained in step one will have to be passed in when instantiating the Vue instance. However, before we instantiate the Vue instance, we have to first install the `vue-components` plugin on Vue, so that the Vue instance will be able to render our page Vue application into proper HTML strings (e.g. `<box>` component can be rendered into its appropriate content).

In our old CSR setup, we did not have a way to bring this `vue-components` plugin into the `core` package. Thus, we have to look at our **webpack** setup in `core-web` and modify it so that we are able to bring that plugin into our `core` package for SSR. 

Let me explain this process briefly.

In our webpack configuration, we have to define what is known as **client-entry** and **server-entry**. The former is the bundle that we will be sending to the client and the latter is the bundle which we will need to use in the server for SSR.

**client-entry** involves: 
- `core-web` client-side (browser) scripts
- `vue-components` plugin

**server-entry** involves:
- `vue-components` plugin

**client-entry** is essentially what we had in the old CSR setup (no changes). The only change here is adding a new `server-entry` and bundling just the `vue-components` plugin that is needed for SSR.

#### <ins>3) Instantiating a Vue instance with the `render` function to output the final HTML markup strings</ins>

In the third step, we want to finally execute SSR by using the `render` function to help us output the final HTML markup strings that we can send to the client.

As mentioned, the `render` function that we have obtained in step one will have to be passed into the Vue instance on both the client-side and server-side. In CSR, we would typically pass in the raw page template string as the argument instead of the `render` function. But this means that Vue will execute compilation on the page template string behind-the-scenes for us. Since we have already compiled our page into Vue `render` function, we will simply pass in the `render` function when instantiating Vue instance and avoid compiling our page again. 

Here is a simple illustration of step three.

```
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

To explain more about `appFactory` in the code snippet above is an interesting one, one crucial component in our SSR setup is `VueCommonAppFactory`. As mentioned earlier, both client-side and server-side will have to create their respective Vue instances. However, those two Vue instances have to represent the same Vue page application; meaning that `render` function, as well as the state data between them, have to be the same. This helps us to achieve a **universal application** so that we don't run into **hydration issues**, which will break our SSR implementation.

## Client-side Hydration in SSR

Hydration is one of the biggest challenges you can face in implementing SSR.

According to Vue, [Hydration](https://ssr.vuejs.org/guide/hydration.html) refers to the client-side process during which Vue takes over the static SSR HTML sent by the server and turns it into dynamic DOM that can react to client-side data changes. Fundamentally, hydration means letting Vue "accept" your SSR HTML output and making it reactive.

You may have noticed that I kept emphasising on ensuring proper **universal application** multiple times. Well, the reason is because if the application that we have on the client-side (the virtual DOM tree) differs from the SSR HTML mark-up that we send to the client, Vue will reject your SSR HTML output, bail client-side hydration, and execute full client-side rendering.

Theoretically, it isn't difficult to maintain a universal application per-se, as long as you ensure two things: 
1) the state data are the same between client-side and server-side, 
2) after compiling and rendering the Vue page application, the SSR HTML mark-up is not modified.

There are also other scenarios that can cause hydration issues, which will be explained in the following sections. Thus, it is important that we nail down the **universal application** aspect of SSR first, before we deal with other scenarios that cause hydration issues. 

### Penalties of Bailing Client-side Hydration

On top of the wasted time and effort in executing SSR, we will also incur the additional time penalty of executing client-side hydration (as compared to normal CSR).

Fortunately, even if we face hydration issues and execute full CSR, we will still avoid the FOUC problem. The reason for this is because the SSR HTML markup should resemble the CSR HTML markup to a large extent. Hydration issues typically occurs due to minor differences between client-side rendered virtual DOM tree and the server-rendered content, supposedly. Of course, this is assuming that we are adhering to the **universal application** concept as much as possible. 

### Types of Hydration Issues faced when implementing SSR in MarkBind

The hydration issues that I have faced so far can be separated into two categories:

1) Violation of HTML spec (e.g. having block-level elements within `<p>` tag) 
2) Modifying the SSR HTML mark-up after compiling and rendering the page

The second category is easy to resolve — we just have to avoid modifying the SSR HTML markup! 

The main challenge that I faced was with the first category. Typically, if you were to write raw HTML from scratch, you will be warned whenever you nest block-level element within a `<p>` tag. However, this isn't the case when we use Vue/Javascript to generate our DOM. The browser will not raise any errors nor complain about it. 

Thus, it is imperative that we ensure the HTML markup that we generate is compliant with the [HTML content models specification](https://www.w3.org/TR/2011/WD-html5-20110525/content-models.html) as far as possible (e.g. do not have `<div>` nested within `<span>`).

### Dealing with the Hydration Issues 

The first important step in resolving hydration issues is to ensure that you are using **development version** of Vue and not the production version. This is because only **development Vue** can report the exact mismatching nodes that are causing the hydration issues. Production Vue will only report a generic `Failed to execute 'appendChild' on 'Node': This node type does not support this method` error message that doesn't help you in finding out where/what the hydration issue is. It is nearly impossible to address hydration issues without the hydration error logs provided by development Vue. 

Furthermore, when I was dealing with hydration issues, I even noticed that there are cases where production Vue did not warn about hydration issues even when they occured (silent failures). Thus, remember to always use development Vue when trying to deal with hydration issues.


hydration issue short circuits (first mismatch found, will just report that, so a page may have many hydration issues)
requires manual debugging (use binary search method to isolate which source syntax is causing the hydration issues)
always look for hydration issues starting from the top of the source file, since the hydration goes from top to bottom of html file

impt to use dev Vue to debug hydration issues (sometimes production may not even have warnings, silent failure and it automatically does CSR, reference: https://github.com/vuejs/vue/issues/5907#issuecomment-439072836)


difficulties faced in resolving hydration issue #1548

## Other Challenges Faced when Implementing SSR

beyond implementing SSR, additional problems to solve:
SSR in development mode, requiring modules from strings
Ensuring CI still works - resolving CI issues (work locally but not on CI) cross-env, setting node env variable
webpack
ensuring that things work in all modes (normal serve, which uses built bundle / dev mode, which auto tracks bundle hot-reload / one page mode)

always remember to rebase (when dealing with CI bugs), another person added new test file

retaining unrendered HTML output in testing

## Final Thoughts 

Lots of research and learning has to be done, on Vue and MarkBind. Beyond that, there are also implementation challenges, you will also discover that there are existing bugs in the codebase that gives you a much harder time in implementing SSR. Furthermore, these bugs may throw you off and mislead you into thinking that there were some errors in your implementation, when the fact is that these are existing bugs that are messing with your implementation. Thus, there will be many moments where you will scratch your head as you try to understand what is going on. You will realise that being able to identify and diagnose problems accurately is a very important skill to have, and sometimes it only comes with experience. 

in implementing SSR, I also discovered that it ironed out other existing issues in MarkBind #1548. moving in the right direction in general.

In addition, you will find that the way you implement certain features / resolve certain problems often are not the best way and you will find it out in the hard way - when you have spent a lot of time to resolve the problem, only to find during code review that there is a much simpler and elegant way of resolving the problem. So much time could have been saved. 

At the end, it seems like cost-to-reward ratio isn't very good, it seems like so much has to be done in implementing SSR just to resolve the FOUC issue. However, the reward, in my opinion, really comes from how much I learnt about MarkBind's architecture and Vue in general.

List the aspects you learned, and the resources you used to learn them, and a brief summary of each resource.
...
