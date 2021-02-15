### Angular Workspace & Build Configurations
#### Aspects
- Setting up separate environments for different purposes (testing in this case).
- How Angular does file replacements and injects different data from files on compile and runtime for these different configs.
#### Sources
- Angular Docs
  - [Workspace Config](https://angular.io/guide/workspace-config)
  - [Build Config](https://angular.io/guide/build)

### Protractor (E2E Testing for Angular)
#### Aspects
- Potractor process and tech stack, specifically how it is built ontop of WebDriverJS (Javascript Implementation of Selenium) which then uses tools like Jasmine and Browser Drivers to spin up a local browser to simulate E2E testing.
- Configuring Angular to run a specific test configuration which provides Stubs for services that communicate with the backend such that E2E testing can be conducted in an isolated environment.
- Usage structure of Protractor, specifically how it directly interacts with HTML Elements based on a variety of Selectors through the browser to perform user actions during E2E testing.
### Sources
- Youtube
- [Potractor Docs](http://www.protractortest.org/#/)

### Github Actions
#### Aspects
- Setting up Virtual Environments to run E2E Testing as a part of CI when PRs are made to the main repository.
- Configuring Virtual Environments and performing manual driver configurations to add support for headless E2E tests for multiple browsers.