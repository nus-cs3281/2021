## Angular

### Aspects learnt
CATcher is an Angular web application.

#### Angular Structure
Components, services, corresponding html and css files work together to form a cohesive application. While a component is a direct representation of visible parts of an application, a service is more subtle in the sense that it runs in the background to provide services to components where needed. By defining the service in the constructor of a component or another service, the component or service is able to access the methods defined in the service freely. The separation of components and services increases modularity and reusability, as through dependency injection (DI), the service class can provide services to different parts of the application.

RxJS stands for Reactive Extensions for Javascript. It supports reactive programming for Javascript, which allows changes in data to be propagated through the application instantly. Angular makes use of the RxJS library to support asynchronous programming and improve reactivity of an Angular application.

RxJS supports `Observables` and `Observers`, allowing `Observers` to receive updates on changes to the `Observable` it subscribes to. This implementation is similar to `Observables` and `Observers` in other programming langugages such as Java.

Pipes from the RxJS library are used very frequently in CATcher reduce clutter and improve readability of our codebase. It strings together operators in a sequence such that the operators can be applied to the given value in order.

### Resources Used & Summary

1. [Angular Guide](https://angular.io/guide/architecture) : Official Angular developer guide and introduction to basic Angular topics
2. [RxJS Guide](https://rxjs-dev.firebaseapp.com/guide/observable) : Official RxJS guide on Observables, Observers, Operators, Subscription, etc.
3. [Angular Guide on Navigation of Component Tree](https://angular.io/guide/dependency-injection-navtree) : Guide on how to navigate the component tree with Dependency Injection

## Jasmine (Javascript Testing)

CATcher follows the Jasmine Style for writing tests.

### Aspects learnt

#### Test Suite
The very basics of Jasmine testing involves the test suite, which is started by calling the global Jasmine function `describe`. The test suite may consist of several tests(specs) within itself, which is done by calling `it`. Coming from Java background, one thing I learnt about Javascript testing was that it is descriptive (as the name `describe` suggests), which makes it easier to understand the tests.

#### Spy
One distinctive aspect of Jasmine testing (or other Javascript testing frameworks such as Jest) is the `Spy` object. It allows users to stub functions and keep track of useful information such as the number of calls to the function, parameters it has been called with, etc., which is very useful for testing different parts of a function under test thoroughly. For example, if function B is called within function A which is under test, the user is able to find out detailed information regarding how function B is called within A by creating a spy object of B. This allows testers to verify that B has indeed been called with the correct arguments correct number of times.

### Resources Used & Summary

1. [Official Jasmine documentation](https://jasmine.github.io/api/3.6/global) : This is the official Jasmine documentation for Jasmine 3.6
2. [Introduction to Jasmine 2.0](https://jasmine.github.io/2.0/introduction.html) : This is a good summary / introduction of Jasmine test features

## Electron

CATcher uses Electron for its Desktop application. Electron is a framework for building cross-platform desktop applications. It has allowed us to build a desktop app off our Angular web application codebase.

### Aspects learnt

Electron uses the module `ipcMain` to communicate to renderer processes from the main process. As an Event Emimtter, it is able to send out and listen for events, allowing inter-process communication within the Electron application (as the name suggests).

Because it is a desktop application, it is important that we account for different operating systems in our code, since different operating systems would behave differently to changes in our application.

### Resources Used & Summary

1. [Official Electron Guide](https://www.electronjs.org/docs/tutorial) : This is the official Electron documentation