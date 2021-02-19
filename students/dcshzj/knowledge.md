### GitHub Actions

GitHub Actions is a new platform hosted on GitHub for Continuous Integration and Continuous Deployments (CI/CD) purposes. Previously, RepoSense was using TravisCI for running the test suite, but due to [changes in their pricing model](https://blog.travis-ci.com/2020-11-02-travis-ci-new-billing), there was an urgent need to switch to GitHub Actions that provided unlimited minutes for open source projects.

The [workflow syntax](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions) is the main starting point in designing a new CI job in GitHub Actions (known as a workflow). There are [various types of events that trigger a workflow run](https://docs.github.com/en/actions/reference/events-that-trigger-workflows), although a `pull_request` event is the most common.

However, as code from a pull request is run based on the code submitted by the PR owner, [it can result in pwn requests](https://securitylab.github.com/research/github-actions-preventing-pwn-requests) and **cause a compromise in the secrets** stored in the repository. Hence, GitHub made a design decision of not sharing any repository secrets to workflow runs that are triggered by the `pull_request` event. If a workflow needs to run and access secrets, they will have to be triggered using the `pull_request_target` event.

RepoSense was vulnerable for a period of time due to this. Hence, [work was done to mitigate the issue](https://github.com/reposense/RepoSense/pull/1411) through the use of [workflow artifacts](https://docs.github.com/en/actions/guides/storing-workflow-data-as-artifacts).

Thus, a lesson learnt is to always write code with security in mind. The original design of the workflow was aimed at working around the limitations imposed by GitHub. However, it turns out **it is still possible to compromise security** with a carefully designed PR. Therefore, understanding the security implications of writing code is vital in ensuring that we write secure code.

### JUnit

[JUnit](https://junit.org/junit4/) is a framework for writing tests for Java code. It is also a framework used by RepoSense in writing test cases quickly and easily.

When conducting research [to reduce the testing time](https://github.com/reposense/RepoSense/issues/1349), I had to learn more about how the test suite was designed, especially with the annotations used. Thankfully, there was [a tutorial on JUnit annotations](https://www.softwaretestinghelp.com/junit-annotations-tutorial/) which explained the sequence of functions that are run based on the annotations provided. A simplified sequence is as follows:

<box>
@BeforeClass → @Before → @Test (1) → @After → @Before → @Test (2) → @After → @AfterClass
</box>

However, as many of the test functions are spread across multiple test classes, there was a need to run certain code after all test classes were run. That is, the code needs to run after every class's @AfterClass. A [StackOverflow question](https://stackoverflow.com/questions/9903341/cleanup-after-all-junit-tests) suggested to use JUnit's RunListener, which is a hook that can run before any test runs and after all tests have finished.

That felt like it was sufficient, but it turns out, **it is possible that the tests were prematurely terminated**, resulting in the code for RunListener not being able to run.

Instead, a [blog post suggested](http://stiemannkj1.gitlab.io/use-Runtime-addShutdownHook-instead-of-RunListener-testRunFinished/):

> Don’t Clean Up After Tests with `RunListener.testRunFinished()`, Add a Shutdown Hook Instead

Even so, it is still possible for the Java Virtual Machine (JVM) to be terminated externally, such that it is not even possible to run code from the shutdown hook. Nonetheless, it does show that despite your best efforts, **there is always a way for the user to cause havoc**, which is really a demonstration of Murphy's Law.
