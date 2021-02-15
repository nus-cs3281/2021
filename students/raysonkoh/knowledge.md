### Github-Pages

#### Aspect 1: Useful CI Environmental Variables
One can leverage on environmental variables to detect a range of information such as:
- Which CI platform is the code being run on
- Getting the repo slug in the form `owner_name/repo_name`

Some useful resources:
- [Snippet of Codecov's bash script which leverages heavily on CI environmental variables](https://github.com/codecov/codecov-bash/blob/master/codecov#L499-L951) - Credit to HSU ZHONG JUN for suggesting the resource in [Support more CI platforms for markbind deploy #1432](https://github.com/MarkBind/markbind/issues/1432).

#### Aspect 2: Usage of Github Tokens
While working on expanding `markbind deploy` to work on various CI platforms, there may be some custom setup needed to be able to have permissions to be able to push to the remote repo. This can get quite complicated. Github offers a solution in the form of Personal Access Token, where the user can specify specific and granular permissions for the usage of the token. This greatly increases portability as the same code that uses the github token would work on different CI platforms.

For Github Actions, there is a default `GITHUB_TOKEN` secret that the user can use to authenticate in a workflow run. The token expires after the job is finished.

Some useful resources:
- [Github - Creating a personal access token](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token)
- [Github - HTTP-based Authentication](https://docs.github.com/en/developers/apps/authenticating-with-github-apps#http-based-git-access-by-an-installation)
- [Github Actions - Authentication in a workflow](https://docs.github.com/en/actions/reference/authentication-in-a-workflow)
#### Aspect 3: Testing CI scripts during development
I have found that there are not a lot of resources online regarding the testing CI scripts during development. This is especially so since Markbind is using a [Lerna mono-repo structure](https://lerna.js.org/) and it needs to be installed prior to deploying to Github-Pages. After many trials and errors, I have found a way that works. 

The following is a general guideline for a CI build script that allows you to test your code changes:
1. Navigate out of the current directory and [git clone your forked repo](https://stackoverflow.com/questions/1911109/how-do-i-clone-a-specific-git-branch)
2. Navigate into the your cloned repo and run the setup instructions in your repo
3. Navigate into the original directory - most CI platforms provide an environment variable for this (eg. Travis-CI provides `TRAVIS_BUILD_DIR`)
4. Run the deploy steps

Some alternatives that I have considered:
- [Github Packages](https://docs.github.com/en/packages/learn-github-packages/about-github-packages)
- [How to install NPM package from Github directly](https://stackoverflow.com/questions/17509669/how-to-install-an-npm-package-from-github-directly)

#### Aspect 4: Skip CI directive
The `skip ci` directive is useful if you do not want a specific commit that was pushed to trigger a CI build. This was used in the commit message to the `gh-pages` branch using `markbind deploy --ci`, as that commit may unintentionally trigger a CI build.

Some useful resources:
- [Circle CI - skip ci](https://circleci.com/docs/2.0/skip-build/#skipping-a-build)
- [Travis CI - skip ci](https://docs.travis-ci.com/user/customizing-the-build/#skipping-a-build)
- [Appveyor CI - skip ci](https://www.appveyor.com/docs/how-to/filtering-commits/#skip-directive-in-commit-message)
- [Github Actions - skip ci](https://github.blog/changelog/2021-02-08-github-actions-skip-pull-request-and-push-workflows-with-skip-ci/)

### Tool/Technology 2

List the aspects you learned, and the resources you used to learn them, and a brief summary of each resource.

...