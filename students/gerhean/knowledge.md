### Vue

Creating a new Vue project
- https://cli.vuejs.org/guide/creating-a-project.html#vue-create

Things I learned about Vue:
- It does not watch nested properties (of arrays, objects) in the state. 

Adding Pug to new Vue project:
- https://dev.to/reiallenramos/vue-pug-and-scss-27pl

Ensure Vue works in sub-paths by configuring publicPath:
- https://forum.vuejs.org/t/publicpath-in-production/79698

### Vue Good Practices

- Use async await instead of promises.

- The proper way to change a reactive deeply nested state object [link](https://stackoverflow.com/questions/46985067/vue-change-object-in-array-and-trigger-reactivity)

- Passing a function to pass data to parents is an anti-pattern, it is better to use an [`$emit` event](https://vuejs.org/v2/guide/components.html#Listening-to-Child-Components-Events) to pass data.

- Using [store pattern](https://vuejs.org/v2/guide/state-management.html#Simple-State-Management-from-Scratch) for global variables.

### Javascript Observations
- Difference between `@change` and `@input` [link](https://stackoverflow.com/questions/17047497/difference-between-change-and-input-event-for-an-input-element)
- Using `Buffer` to encode string to Base64 [link](https://stackoverflow.com/questions/6182315/how-to-do-base64-encoding-in-node-js)

### Node Libraries for Vue

BootstrapVue
- Useful for UI/UX design.
- https://bootstrap-vue.org/docs

dotenv
- Useful for secrets.
- https://www.robertcooper.me/front-end-javascript-environment-variables



### Github

Nicer commit messages are beneficial even to myself, as it helps me to remember what I did previously.

Using [octokit/core](https://www.npmjs.com/package/@octokit/core) for GitHub's REST & GraphQL APIs
- How to fork a repository [link](https://docs.github.com/en/rest/reference/repos#forks)
- Adding secrets (need to get public key first) [link](https://docs.github.com/en/rest/reference/actions#create-or-update-a-repository-secret)
- Create or update file contents [link](https://docs.github.com/en/rest/reference/repos#create-or-update-file-contents)

Setting up oauth for github authentication
- https://docs.github.com/en/developers/apps/authorizing-oauth-apps
- Parameters are simply query string
- First redirect users to https://github.com/login/oauth/authorize
- Users will then be redirected back to your site with a `code` query string parameter
- Getting the access token requires a client_secret which should not be stored anywhere in the frontend, hence we use [gatekeeper](https://github.com/prose/gatekeeper) as a backend to store the client_secret.
- The token recieved from gatekeeper can be used to authenticate octokit.
