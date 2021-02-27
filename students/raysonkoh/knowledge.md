### Tool 1: Github Actions

#### Aspect 1: Exposing secrets to workflows triggered from a forked repo
Due to security concerns, Github Actions does not expose repo secrets to workflows which are triggered from a forked repo. However, there may be actual use-cases where certain automated workflows, which require certain repo secrets, may be useful such as PR previews, automated labelling of PRs, generate code coverage report, etc.

Github Actions provide the `pull_request_target` and `workflow_run` to cater for such use-cases. To summarize,  `pull_request_target` runs the trusted workflow code in **your** actual repo (not the forked repo), and as such, it is able to expose the repo secrets during the workflow run. On the other hand, `workflow_run` can be triggered after a prior workflow run has completed and it has access to the repo secrets. The ideal use-case that Github recommends for `workflow_run` is for the prior workflow to generate some <tooltip content="A file or collection of files produced during a workflow run">artifacts</tooltip>, and for the privileged workflow to take the generated artifacts, do some analysis and post some comments on the pull request.

Some useful resources:
- [Github Actions Docs - secrets not passed to workflows triggered from forked repo ](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#pull-request-events-for-forked-repositories-1)
- [Github Actions Docs - about storing workflow data as Artifacts](https://docs.github.com/en/actions/guides/storing-workflow-data-as-artifacts#about-workflow-artifacts)
- [Github's research and recommendations on securing workflows from forked repo](https://securitylab.github.com/research/github-actions-preventing-pwn-requests)
- [Github blog introducing `pull_request_target` and `workflow_run`](https://github.blog/2020-08-03-github-actions-improvements-for-fork-and-pull-request-workflows/)

#### Aspect 2: Github Tokens
For Github Actions, there is a default `GITHUB_TOKEN` secret that the user can use to authenticate in a workflow run. The token expires after the job is finished. With this, one doesn't need to explicitly generate Personal Access Token for workflows which require **write** repo permissions.

Some useful resources:
- [Github Actions Docs - About `GITHUB_TOKEN` secret](https://docs.github.com/en/actions/reference/authentication-in-a-workflow#about-the-github_token-secret)
- [Github Docs - Personal Access Token](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token)

### Tool 2: Markbind

#### Aspect 1: Testing CI scripts during development
There are not a lot of resources online regarding the testing CI scripts from a forked repo during development, especially for a mono-repo such as Markbind. The following is a general guideline for a CI build script that allows you to test your code changes:

1. Navigate out of the current directory and [git clone your forked repo](https://stackoverflow.com/questions/1911109/how-do-i-clone-a-specific-git-branch)
2. Navigate into the your cloned repo and run the setup instructions in your repo
3. Navigate into the original directory - most CI platforms provide an environment variable for this (eg. Travis-CI provides `TRAVIS_BUILD_DIR`)
4. Run the deploy steps

Some useful resources:
- [Atlassian Tutorial - What is a monorepo?](https://www.atlassian.com/git/tutorials/monorepos)
- [stackoverflow - How to install NPM package from Github directly](https://stackoverflow.com/questions/17509669/how-to-install-an-npm-package-from-github-directly) - A far simpler method that works for npm package repos which are not a monorepo

#### Aspect 2: Useful CI environmental variables
Implementation of `markbind deploy --ci` uses some useful CI environmental envariables to extract information such as:
1. Which CI platform is the code being run on
1. Getting the repo slug in the form `owner_name/repo_name`

Some useful resources:
- [Snippet of Codecov's bash script which leverages heavily on CI environmental variables](https://github.com/codecov/codecov-bash/blob/master/codecov#L499-L951) - Credit to HSU ZHONG JUN for suggesting the resource in [Support more CI platforms for markbind deploy #1432](https://github.com/MarkBind/markbind/issues/1432).
