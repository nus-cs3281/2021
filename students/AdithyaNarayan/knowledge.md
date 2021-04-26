
### Angular

- Typing in TypeScript, Components, Modules and File Architecture, Data Binding, Services, Dependency Injection
- An Angular app typically has service classes to handle communication with backend server and local storage. RxJs is usually used to create async services
- Angular Modules: to aid dependency injection among components
- Angular Models: Used for automatic data binding which changes the view when the model changes
- Structural Derivatives: `ngIf` and `ngFor` for templating 
  Source: [Angular Documentation](https://angular.io/docs), [Tour of Heroes Tutorial](https://angular.io/tutorial)

### Jest

- Unit testing, component testing, snapshot testing, mocking services and API calls
  - Common matchers: `exepct()`, `toBe()`, `not, toEqual()`
  - Truthiness: `toBeTruthy()`, `toBeFalsy()`, etc
  - Number comparision: `toBeGreaterThan()`, etc
  - String comparition: `toMatch()`
  - Arrays: `toContain()`
  - Exceptions: `expect()` + `toThrow()`
  - Mocking services: `spyOn()` + `returnValue()`

Source: [Jest Documentation](https://jestjs.io/docs/en/getting-started), [TEAMMATES Codebase](https://github.com/teammates/teammates)

### Objectify

- API for effective querying of entities such as filtering and member projection. Used to group and eliminate fields.
  - Defining entities: annotate a class with `@Entity` (other tags like `@Cache` can also be used if caching is needed)
  - Base operations: `save()`, `load()`, etc

Source: [Objectify Documentation](https://github.com/objectify/objectify/wiki)

### Google App Engine

- Indexing properties of an entity for searching, filtering and projecting in the queries
  - Use `@Index` to annotate the attribute used as index
  - GAE enforces usage of efficient queries and makes it almost impossible to write slow queries

Source: [Google App Engine Java Documentation](https://cloud.google.com/appengine/docs/standard/java)

### RxJS

- Parallelize dependent API calls using `map`, `zip` and `mergeAll`
- `map`: takes in a parameter project to apply to each value emitted by the source Observable
- `zip`: combine multiple observables into 1 observable
- `mergeAll`: convert a high-order observable into a first-order observable that delivers all values that inner observables emitted
  Source: [RxJS Developer Guide](https://rxjs-dev.firebaseapp.com/guide/overview)

### Google Cloud Logging API

- Filtering of logs using labels after listing the entries
  - Use GCP client library `google.cloud.logging`
 - Mocked a local logs service using an in-memory model 


Source: [Google Cloud Logging Java Documentation](https://cloud.google.com/logging/docs/api)

### Selenium E2E

- Identify web components using css selector, classname and id
  - Use annotator `@FindBy` and method `findElement`
- Simulate user events like click and scroll using web driver
  - Use method click with the previously found web elements as parameters
- E2E stubbing
  - Create a stub service to receive user events and log data to overcome GCP limitation on the development server

Source: [TEAMMATES Codebase](https://github.com/teammates/teammates)
