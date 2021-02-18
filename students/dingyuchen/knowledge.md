### Linter and Formatters

Linters are tools that provide static analysis for your code while formatters help to check the style of the code.

There are certain overlaps in the functionality and rule-sets with both linters and formatters, but in general, it will be good to have both as development dependencies in a project.

In the CATcher project (at the current point of writing), we use `tslint` to enforce certain styles. 
This is integrated with continuous integration (CI) in order to ensure a certain quality of pull requests.

However, `tslint` does not fix simple errors that are being caught.
In fact, there are certain rules that are not caught at all, even though it is specified in the rule-set. (e.g. indent-level : 2)
This is where `prettier`, a formatter, should come in to enforce consistency of style across the code base.

Furthermore, `prettier` being an _opinionated_ formatter, means that there isn't much set up to be done. 

`prettier` can be integrated with `tslint` in a few ways.
In general, most projects use a plugin to disable all formatting rules so that they can be handled solely by `prettier`.
`prettier` can also be added as a linting rule and which defers to the linter to fix the formatting errors. 
However, there is usually a lot of squiggly-underlines that cause a lot of distraction and speed of formatting is a little slower.

In this project, I decided to apply `prettier` as a pre-commit hook so that any new changes are automatically formatted. 
This also somewhat decouples the formatter from the linter. 
The formatting rules are still checked by the linter, but the formatter will fix them, as `prettier` doesn't throw errors.

### GitHub Actions

GitHub Actions are a way to add automation to the repository. 
Most repositories make use of it to implement continuous integration (CI) and continous deployment (CD).

One of the interesting things that I noticed about GitHub actions is that all commands are akin to some form of package, 
available through the GitHub Marketplace.

Running a GitHub Action simply requires the action name and a version to be specified in the `actions.yml` file.
