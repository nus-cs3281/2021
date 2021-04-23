## Angular

CATcher is an Angular web application.

### Aspects learnt
#### Angular Structure
Components, services, corresponding html and css files work together to form a cohesive application. While a component is a direct representation of visible parts of an application, a service is more subtle in the sense that it runs in the background to provide services to components where needed. By defining the service in the constructor of a component or another service, the component or service is able to access the methods defined in the service freely. The separation of components and services increases modularity and reusability, as through dependency injection (DI), the service class can provide services to different parts of the application.

RxJS stands for Reactive Extensions for Javascript. It supports reactive programming for Javascript, which allows changes in data to be propagated through the application instantly. Angular makes use of the RxJS library to support asynchronous programming and improve reactivity of an Angular application.

RxJS supports `Observables` and `Observers`, allowing `Observers` to receive updates on changes to the `Observable` it subscribes to. This implementation is similar to `Observables` and `Observers` in other programming langugages such as Java.

Pipes from the RxJS library are used very frequently in CATcher reduce clutter and improve readability of our codebase. It strings together operators in a sequence such that the operators can be applied to the given value in order.

Example of custom operators using pipes in CATcher:
```
export function assertSessionDataIntegrity() {
   return pipe(
     throwIfFalse(sessionData => sessionData !== undefined,
       () => new Error('Session Data Unavailable')),
     throwIfFalse(isSessionDataCorrectlyDefined,
       () => new Error('Session Data is Incorrectly Defined')),
     throwIfFalse(hasOpenPhases,
       () => new Error('There are no accessible phases.')));
 }
```

##### Knowledge Gained:
1. Enforcing separation of concerns in modules by splitting functions and logic into different components and services
2. Using observables to enable asynchronous operations in our application
3. Using pipes to efficiently transform data used in our application

### Resources Used & Summary

1. [Angular Guide](https://angular.io/guide/architecture) : Official Angular developer guide and introduction to basic Angular topics
2. [RxJS Guide](https://rxjs-dev.firebaseapp.com/guide/observable) : Official RxJS guide on Observables, Observers, Operators, Subscription, etc.
3. [Angular Guide on Navigation of Component Tree](https://angular.io/guide/dependency-injection-navtree) : Guide on how to navigate the component tree with Dependency Injection

#### Angular TestBed Utility - Component DOM Testing
Angular's TestBed Utility allows us to set up component tests for different components used in CATcher.

As opposed to our existing tests for testing individual functions within comoponents and services, these component tests allow us to inspect changes in the view of the component (which are caused by HTML changes). Hence, it gives us a way to look into UI-related problems users might face, something that our existing tests have not been able to achieve.

Steps to set up component tests:
1. Configure the testing module through `TestBed` function with the corresponding component's settings
2. Use `TestBed` function to create the component (fixture) to be tested
3. Observe HTML changes in the fixture during testing of functions by querying HTML elements of the fixture

##### Knowledge Gained:
1. Setting up component tests using Angular TestBed Utility
2. Inspecting HTML changes using Angular TestBed Utility

### Resources Used & Summary
1. [Angular Guide - Basics of testing components](https://angular.io/guide/testing-components-basics) : Official Angular developer guide for the basics of component tests
2. [Angular Guide - Component Fixture](https://angular.io/api/core/testing/ComponentFixture) : Official Angular developer guide on `ComponentFixture`
3. [Introduction to Unit Testing in Angular](https://www.digitalocean.com/community/tutorials/angular-introduction-unit-testing#:~:text=Fixtures%20have%20access%20to%20a,Angular%20to%20run%20change%20detection.) : Useful article on how to test component fixtures

## Jasmine (Javascript Testing)

CATcher follows the Jasmine Style for writing tests.

### Aspects learnt
#### Test Suite
The very basics of Jasmine testing involves the test suite, which is started by calling the global Jasmine function `describe`. The test suite may consist of several tests(specs) within itself, which is done by calling `it`. Coming from Java background, one thing I learnt about Javascript testing was that it is descriptive (as the name `describe` suggests), which makes it easier to understand the tests.

#### Spy
One distinctive aspect of Jasmine testing (or other Javascript testing frameworks such as Jest) is the `Spy` object. It allows users to stub functions and keep track of useful information such as the number of calls to the function, parameters it has been called with, etc., which is very useful for testing different parts of a function under test thoroughly. For example, if function B is called within function A which is under test, the user is able to find out detailed information regarding how function B is called within A by creating a spy object of B. This allows testers to verify that B has indeed been called with the correct arguments correct number of times.

##### Knowledge Gained:
1. Writing unit tests for different components and functions in Typescript
2. Using Spy objects in tests to stub functions and improve quality and efficiency of tests

### Resources Used & Summary

1. [Official Jasmine documentation](https://jasmine.github.io/api/3.6/global) : This is the official Jasmine documentation for Jasmine 3.6
2. [Introduction to Jasmine 2.0](https://jasmine.github.io/2.0/introduction.html) : This is a good summary / introduction of Jasmine test features

## Electron

CATcher uses Electron for its Desktop application. Electron is a framework for building cross-platform desktop applications. It has allowed us to build a desktop app off our Angular web application codebase.

### Aspects learnt

Electron uses the module `ipcMain` to communicate to renderer processes from the main process. As an Event Emimtter, it is able to send out and listen for events, allowing inter-process communication within the Electron application (as the name suggests).

Because it is a desktop application, it is important that we account for different operating systems in our code, since different operating systems would behave differently to changes in our application.

Example of adapting application logic to Linux O.S.
```
  if (isLinuxOs()) {
    // app icon needs to be set manually on Linux platforms
    windowOptions['icon'] = ICON_PATH;
  }
```

##### Knowledge Gained:
1. Understanding behaviours and characterisitics of different operating systems
2. Adapting application logic to different operating systems

### Resources Used & Summary

1. [Official Electron Guide](https://www.electronjs.org/docs/tutorial) : This is the official Electron documentation


## Github Authentication
CATcher uses OAuth 2.0 Protocol to authenticate users. Users are required to log into their Github accounts before they can start using CATcher.

### Aspects learnt

#### Web Application Authorization Flow

The basic flow is as follows:

1. CATcher opens a separate window that navigates to GitHub for login.
2. Once Github verifies the user's identity, the user is redirected to CATcher with a temporary authorization code.
3. CATcher exchanges the code for the access token, which allows CATcher to access Github API within CATcher.

The above authorization process works perfectly fine when it comes to verifying a user's identity and accessing Github API, however there is a security flaw.

This authentication process is vulnerable to cross-site request forgery (CSRF) attacks, which compromises users' privacy and security.

CSRF attack as a broad term refers to an attack that tricks a victim into doing things on a website in which they are currently authenticated by causing the victim to send a malicious request to the website server.

In the case of CATcher specifically, during the authentication process of a user, an attacker can send their own authentication session ID to CATcher before CATcher can receive the actual response for the user from Github. This tricks CATcher into thinking that the user has been authenticated, and allows the user into CATcher, using the attacker's Github account. This means that whatever information the user uploads onto Github through CATcher would instead be uploaded to the attacker's Github account, instead of the user's.

What we can do to prevent this attack is to add the `state` parameter in our authentication process. In our first step, before navigating to Github, CATcher can generate a random string called `state` (high entropy needs to be ensured for security) and send it to Github together with other details. Upon authenticating the user, Github will then send back the `state` parameter to CATcher, and this allows CATcher to check the returned `state` parameter against the sent `state`. If the `state` parameters do not match, it might point at a potential CSRF attack, as Github will definitely return us the correct state. CATcher will then ignore the response and wait until the correct `state` is received.

##### Knowledge Gained:
1. Understanding the authentication process for OAuth 2.0
2. Improving security of an authentication process by adding `state` parameter

### Resources Used & Summary

1. [Github Docs](https://docs.github.com/en/developers/apps/authorizing-oauth-apps) : Github Documentation on authorizing OAuth applications
2. [OAuth CSRF & the 'state' parameter](https://www.youtube.com/watch?v=_xrhWLqX1j0) : A short video on how CSRF attacks can diminish security of an application and how state paramters can prevent CSRF attacks
3. [Cross-site request forgery (CSRF)](https://owasp.org/www-community/attacks/csrf) : An article on how CSRF attacks occur
