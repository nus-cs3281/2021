# Github Actions 

## Aspects

1. CI / CD

### Aspect 1: Setting up of pipelines for various Operating Systems 

#### Summary 

With the introduction of Github Actions, it has provided developers a **free** service in order 
to build and test their applications. This is important especially with regards to CATcher, 
where we need to ensure that the builds for Unix, Windows and MaC systems works for usage. 

#### Knowledge Gained 

1. Setting up of pipelines to build and test on Unix, Windows and Mac Systems **concurrently**

### Aspect 2: Deployment to Staging Site via Github Actions 

#### Summary 

By using Github Actions, we can enable a separate **staging repository** to fetch the current `master` 
branch of our `CATcher-org/CATcher` repository and run its own deployment **separate** from the 
main CATcher repository

#### Knowledge Gained 

1. Enabling conditional statements to block workflows from being run
2. Using Github Actions to allow reposioties to fetch from one another despite having no common upstream.

## Resources 

The Resources below are to familiarize yourself with how Github Actions works and examples of jobs that
you can run. 

1. [Github Actions Documentation](https://docs.github.com/en/actions)
2. [Checkout@v2](https://github.com/marketplace/actions/checkout)

The Following PRs can serve as a starting point to how Github Actions are used for CI / CD on 
open-source projects

1. [Adding Github Actions config file for builds and tests](https://github.com/CATcher-org/CATcher/pull/509)
2. [Adding Staging site deployment for CATcher](https://github.com/CATcher-org/CATcher/pull/546)