# Angular Workspace & Build Configurations

---

## Summary

Angular supports the implementation of separate workspaces and build configurations to support a variety of tasks. Typically in a default project, Angular has pre-defined development and production environments, but, with our use case in CATcher, there is a need to differentiate the build instructions for a test environment that we can run E2E and other forms of testing on.

## Aspects

- Setting up separate environments for different purposes (testing in this case).
- Learning about how Angular does file replacements and injects different data from files on compile and runtime for these different configs.
- Developing Mock-Services to simulate backend API (Github API) responses and bypass certain actions such as Authentication to allow for an isolated test environment that is injected to the application on runtime.

## Sources
- Angular Docs
  - [Workspace Config](https://angular.io/guide/workspace-config)
  - [Build Config](https://angular.io/guide/build)

# Protractor (E2E Testing for Angular)

---

## Summary

Protractor is the go-to E2E testing framework for Angular applications. It is essentially a wrapper around Selenium (a Javascript testing framework that assists automating browser actions by interacting with individual browser drivers). With Protractor, in the context of CATcher, we are now able to simulate the application experience and interface on a variety of web-browsers to better catch potential issues that may arise when users utilize our production application.

## Aspects

- Learned about the Potractor automation process and tech stack, specifically how it is built ontop of Selenium which then uses tools like Jasmine and Browser Drivers to spin up a local browser to simulate E2E testing.
- Configuring Angular to run a specific test configuration which provides Stubs for services that communicate with the backend such that E2E testing can be conducted in an isolated environment.
- Understanding the usage structure of Protractor, specifically how it directly interacts with HTML Elements based on a variety of Selectors through the browser to perform user actions during E2E testing.
- Experience modifying Web-Elements such that they are compatible with multiple browser drivers to provide a stable testing framework

### Sources
- Youtube
- [Potractor Docs](http://www.protractortest.org/#/)

# Github Actions

---

## Summary

My contributions to the CI/CD pipeline was the implementation of E2E testing carried out on Pull Requests to ensure Regression. This is especially necessary as we move to formalize the web-platform as a default and begin code-quality improvements and feature implementations. Adding E2E testing to our pipeline was also the more efficient method as opposed to having it added to the pre-push hooks that we currently use for linting. This is because not only do we save time on each push, as E2E tests are typically long due to the need to spin up the necessary drivers and browsers, but also ensures that tests are constantly verified on standardized operating systems with the latest drivers for compatibility.

## Aspects

- Learning of the various tools, virtual environments and configurations provided by the Github Actions Team.
- Setting up Virtual Environments to run E2E Testing as a part of CI when PRs are made to the main repository.
- Configuring Virtual Environments and performing manual driver configurations to add support for headless E2E tests for multiple browsers.
- Learning to parallelize E2E testing interface to reduce time taken during our CI/CD tests prior to PR approval.

## Sources
- StackOverflow (Primarily)
- [actions/virtual-environments Repo](https://github.com/actions/virtual-environments)