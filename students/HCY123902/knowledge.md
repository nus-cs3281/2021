### Tool/Technology 1

Vuex

Description of the tool: State management tool for Vue applications

Aspect: Introduction to Vuex

https://www.youtube.com/watch?v=5lVQgZzLMHc

### Tool/Technology 2

Figma 

Description of the tool: Grapgical User interface design tool that allows automatic generation of css style sheet. This can save time for writing css according to the User Interface Design.

Aspect: Introduction to the usage of Figma

https://www.youtube.com/watch?v=3q3FV65ZrUs

### Tool/Technology 3

Scss and Css

Description of the tool: style sheet used by Vue User Interface Component.

Aspect: Difference between the usage of class selector in css and scss style sheet.

https://www.w3schools.com/cssref/css_selectors.asp

https://stackoverflow.com/questions/11084757/sass-scss-nesting-and-multiple-classes

https://stackoverflow.com/questions/30505225/how-to-reference-nested-classes-with-css

https://stackoverflow.com/questions/13051721/inheriting-from-another-css-class

The naming convention in css style sheet

https://www.freecodecamp.org/news/css-naming-conventions-that-will-save-you-hours-of-debugging-35cea737d849/

### Tool/Technology 4

Vue LifeCycle Management

Description of the tool: Vue component life cycle hook

Aspect: Methods that can be used to create hook between Vue component and the template, enabling the pug file template to load information before rendering, and the Vue component to access information in the pug file template before and after its initial rendering.

`created` is a useful method for loading the data and retrieving the window hashes that are needed for the vue component after the component is created.

`beforeMount` is often used to access and modify the information of the component before the rendering.

`mounted` is called after the rendering and it is also used to access or modify the information of the component.

It is important to distinguish between `create` and `beforeMount` and `mounted`

https://medium.com/@akgarg007/vuejs-created-vs-mounted-life-cycle-hooks-74c522b9ceee

https://vuejs.org/v2/guide/instance.html#Instance-Lifecycle-Hooks

### Tool/Technology 5

Javascript Syntax for map

Description of the tool: Javascript Syntax related to the retrival of map keys, conversion from map to array, and iteration of map.

Aspect: Some instance methods of retriving key and entry in the Map class, such as `map.keys` and `map.entries`, do not seem to work in RepoSense frontend script. The corresponding class methods in the Object class, such as `Object.entries` and `Object.keys`, can be used as an alternative option. 

https://stackoverflow.com/questions/35341696/how-to-convert-map-keys-to-array

https://stackoverflow.com/questions/16507866/iterate-through-a-map-in-javascript/54550693

https://stackoverflow.com/questions/42196853/convert-a-map-object-to-array-of-objects-in-java-script

https://devdocs.io/javascript-map/

https://devdocs.io/javascript-object/

### Tool/Technology 6

Vue Computed Properties and Watchers

Description of the tool: Respective usage of computed properties and watchers in Vue component

Aspect: There is difference between the computed properties and watchers in Vue component. Computed property is often used to compute value based on declared variable, while watcher is often used to detect change in watched object and make the response respectively.

https://vuejs.org/v2/guide/computed.html

