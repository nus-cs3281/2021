## Front-End Knowledge
 
### Angular

#### Basics
Components are the building blocks of the web application. 
The component class is where data is temporarily stored and loaded
into the HTML pages. Components handle user interactions e.g. what happens
when a user clicks a button. Components form the bridge to other parts
of the application e.g. backend, via services e.g `HttpRequestService`, `AccountService`.  

**Aspects Learned**
* Understanding the component lifecycle 
* Understanding the underlying web technologies (HTML, CSS)
* Injecting data into webpages 
* Understanding event emitters

**Resources:**
[Intro to Angular Components](https://angular.io/guide/component-overview),
[Component Lifecycle](https://angular.io/guide/lifecycle-hooks)

#### Angular CLI
The Angular CLI allows the developer to quickly create components, modules and tests. Learning to use the CLI helped me to improve my workflow.

**Resources:** 
[Angular CLI tools](https://cli.angular.io/)

#### Routing
**Aspects Learned**
* Defining new routes
* Configuring the routing module

**Resources:**
[Angular Routing Guide](https://angular.io/guide/router#defining-a-basic-route)

#### Front-End Testing
Front-end testing is done using Jest/Jasmine which allows us to perform
snapshot-testing. Snapshot testing is a technique that ensures that
the UI does not change unexpectedly.

**Aspects Learned**
* Snapshot Testing
* Mock functions (`spyOn`, `callFake`)

**Resources:**
[Snapshot testing](https://jestjs.io/docs/en/snapshot-testing),
[Jest Object](https://jestjs.io/docs/en/jest-object),
[Jasmine callFake](https://medium.com/@cinish/jasmine-spying-using-callfake-23625310bacf)

#### RXJS
**Aspects Learned**
* Perform async operations via `Observable`
* Understanding RxJS operators

**Resources:** 
[RxJS Observables](https://angular.io/guide/rx-library), 
[Subscription](https://rxjs-dev.firebaseapp.com/guide/subscription),
[RxJS Operators](https://rxjs-dev.firebaseapp.com/guide/operators)

---

## Back-End Knowledge

### Objectify
**Aspects Learned**
* Defining and registering an Entity (corresponding to a single POJO class)
* Basic operations (e.g. `save()`, `delete()`, `save()`)
* Learnt about key-only queries and its benefits
* Understanding the key format and how it has changed from Objectify v5 to v6
* Understand the usage of `key.toUrlSafe()` and `key.toLegacyUrlSafe()`
* Understanding Query cursors

**Resources:**
[Objectify Concepts](https://github.com/objectify/objectify/wiki/Concepts),
[Entities](https://github.com/objectify/objectify/wiki/Entities),
[Objectify v6](https://github.com/objectify/objectify/wiki/UpgradeVersion5ToVersion6),
[Query Cursor](https://cloud.google.com/appengine/docs/standard/java/datastore/query-cursors)

---

### Google Cloud Datastore
**Aspects Learned**
* Running a Google Cloud Datastore Emulator
* Exporting and importing data to emulate production Datastore environment 

**Resources:**
[Running the Emulator](https://cloud.google.com/datastore/docs/tools/datastore-emulator),
[Export and Import Data](https://cloud.google.com/datastore/docs/tools/emulator-export-import)
