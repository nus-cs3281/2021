# Github Actions 

## Aspects

1. CI / CD

### Aspect 1: Setting up of pipelines for various Operating Systems 

#### Summary 

With the introduction of Github Actions, it has provided developers a **free**
service in order to build and test their applications. This is important 
especially with regards to CATcher, where we need to ensure that the builds
 for Unix, Windows and MaC systems works for usage. 

#### Knowledge Gained 

1. Setting up of pipelines to build and test on Unix, Windows and Mac 
Systems **concurrently**

### Aspect 2: Deployment to Staging Site via Github Actions 

#### Summary 

By using Github Actions, we can enable a separate **staging repository** to
fetch the current `master` branch of our `CATcher-org/CATcher` repository and 
run its own deployment **separate** from the main CATcher repository

#### Knowledge Gained 

1. Enabling conditional statements to block workflows from being run
2. Using Github Actions to allow reposioties to fetch from one 
another despite having no common upstream.

## Resources 

The Resources below are to familiarize yourself with how Github Actions works
and examples of jobs that you can run. 

1. [Github Actions Documentation](https://docs.github.com/en/actions)
2. [Checkout@v2](https://github.com/marketplace/actions/checkout)

The Following PRs can serve as a starting point to how Github Actions are 
used for CI / CD on open-source projects

1. [Adding Github Actions config file for builds and tests #509](https://github.com/CATcher-org/CATcher/pull/509)
2. [Adding Staging site deployment for CATcher #546](https://github.com/CATcher-org/CATcher/pull/546)

# Angular Test Bed Utility 

## Aspects 

1. Component Testing 

### Aspect 1: Setting up of component tests over unit tests 

#### Summary 

With the aid of the `TestBed` Utility given by angular, we can set up component 
tests such that we can test the usability of the compoentns used in the 
CATcher application. Not only that, it would also give us great benefit 
by being able to detect the relavant HTML changes when needed. 

#### Knowledge Gained 

1. Learning how to use the `TestBed` configuration to set up the desired 
compoent for testing using custom injections. 
2. Learning how the components interact with the DOm and using state 
to change the result of the required HTML Elements. 

## Resources 

The resources below are to familiarize yourself with how you can use 
Angular's `TestBed` API for Component DOM Testing 

1. [Basics to Component DOM Testing](https://angular.io/guide/testing-components-basics#component-dom-testing)

The Following PRs can serve as a starting point to how to use the 
`TestBed` Utility in your Angular application. 

1. [Assignee Component: Add Tests](https://github.com/CATcher-org/CATcher/pull/555)