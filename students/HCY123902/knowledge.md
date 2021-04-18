### Tool/Technology 1

#### Vue.Js

Documentation:

https://vuejs.org/v2/guide/index.html

Introduction tutorial:

https://www.vuemastery.com/courses/intro-to-vue-js/

--------------------------------------------------------------------------------------------------------------------------

##### Vuex

Description of the tool: State management tool for Vue applications

Aspect: Introduction to Vuex

Documentation:

https://vuex.vuejs.org/guide/

Introduction tutorial:

https://www.youtube.com/watch?v=5lVQgZzLMHc

--------------------------------------------------------------------------------------------------------------------------

##### Vue LifeCycle Management

Description of the tool: Vue component life cycle hook

Aspect: Methods that can be used to create hook between Vue component and the template, enabling the pug file template to load information before rendering, and the Vue component to access information in the pug file template before and after its initial rendering.

`created` is a useful method for loading the data and retrieving the window hashes that are needed for the vue component after the component is created.

`beforeMount` is often used to access and modify the information of the component before the rendering.

`mounted` is called after the rendering and it is also used to access or modify the information of the component.

It is important to distinguish between `create` and `beforeMount` and `mounted`.

Related article:

https://medium.com/@akgarg007/vuejs-created-vs-mounted-life-cycle-hooks-74c522b9ceee

Documentation:

https://vuejs.org/v2/guide/instance.html#Instance-Lifecycle-Hooks

--------------------------------------------------------------------------------------------------------------------------

##### Vue Computed Properties and Watchers

Description of the tool: Respective usage of computed properties and watchers in Vue component

Aspect: There is difference between the computed properties and watchers in Vue component. Computed property is often used to compute value based on declared variable, while watcher is often used to detect change in watched object and make the response respectively.

Documentation:

https://vuejs.org/v2/guide/computed.html

### Tool/Technology 2

#### SCSS and CSS

Description of the tool: style sheet used by Vue User Interface Component

Aspect: Difference between the usage of class selector in css and scss style sheet

Documentation:

https://www.w3schools.com/cssref/css_selectors.asp

Related post:

https://stackoverflow.com/questions/11084757/sass-scss-nesting-and-multiple-classes

https://stackoverflow.com/questions/30505225/how-to-reference-nested-classes-with-css

https://stackoverflow.com/questions/13051721/inheriting-from-another-css-class

Related article about the naming convention in css style sheet:

https://www.freecodecamp.org/news/css-naming-conventions-that-will-save-you-hours-of-debugging-35cea737d849/

### Tool/Technology 3

#### Pug

Description of the tool: template used to render the Vue component

Aspect: Difference and similarities between Pug and Html

Documentation:

https://pugjs.org/api/getting-started.html

Introduction tutorial:

https://www.youtube.com/watch?v=kt3cEjjkCZA

### Tool/Technology 4

#### Figma 

Description of the tool: Grapgical User interface design tool that allows automatic generation of css style sheet. This can save time for writing css according to the User Interface Design

Aspect: Introduction to the usage of Figma

Introduction tutorial:

https://www.youtube.com/watch?v=3q3FV65ZrUs

### Tool/Technology 5

#### JavaScript Syntax

Description of the tool: JavaScript Syntax related to the retrival of map keys, conversion from map to array, and iteration of map

Aspect: Some instance methods of retriving key and entry in the Map class, such as `map.keys` and `map.entries`, do not seem to work in RepoSense frontend script. The corresponding class methods in the Object class, such as `Object.entries` and `Object.keys`, can be used as an alternative option. 

Related post:

https://stackoverflow.com/questions/35341696/how-to-convert-map-keys-to-array

https://stackoverflow.com/questions/16507866/iterate-through-a-map-in-javascript/54550693

https://stackoverflow.com/questions/42196853/convert-a-map-object-to-array-of-objects-in-java-script

Documentation:

https://devdocs.io/javascript-map/

https://devdocs.io/javascript-object/

### Tool/Technology 6

#### Java URL decoder

Description of the tool: The Java URL decoder to decode string

Aspect: The Java URL decode can convert hexadecimal value in the string to the corresponding character, which is helpful in path string conversion.

Documentation:

https://docs.oracle.com/javase/8/docs/api/java/net/URLDecoder.html

### Tool/Technology 7

#### Git command for squashing commit

Description of the tool: The git command to squash multiple commits

Aspect: It is necessary to squash multiple commits together when working on pull request, which can make the commit history look concise and clear to the reviewer.

Related post:

https://stackoverflow.com/questions/5189560/squash-my-last-x-commits-together-using-git

Documentation:

http://git-scm.com/docs/git-rebase#_interactive_mode

http://git-scm.com/docs/git-reset#Documentation/git-reset.txt-emgitresetemltmodegtltcommitgt

### Tool/Technology 8

#### Cloning of Github repositories

Description of the tool: Cloning of GitHub repositories with SSH

Aspect: The steps to take to connect to GitHub with SSH and clone repository with SSH protocol URL. It is a potential direction to explore for RepoSense to allow the usage of SSH protocol URL when specifying the repository to clone.

Related post

https://stackoverflow.com/questions/6167905/git-clone-through-ssh/16134428

Steps to take to connect to Github using SSH:

https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh

Cloning of repository with SSH:

https://docs.github.com/en/github/getting-started-with-github/about-remote-repositories#cloning-with-ssh-urls
