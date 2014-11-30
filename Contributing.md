We are happy to accept contributions to the project in the form of pull requests and issue reporting.  Please feel free to create an issue ticket using the [GitHub Issues]() tool.  

For any issues please include as much detail as possible in your tickets including your operating system, Java version, StreamFlow version, and any other environmental details you think necessary to recreate the issue.  Even better, if you would like to contribute a fix for your issue or new feature please submit a pull request for review by the development team.

Before submitting any new code via pull requests, please consult the following documentation which describes the overall branching model and source code formatting guidelines.

## Branching Model
StreamFlow uses the [GitFlow](http://nvie.com/posts/a-successful-git-branching-model/) branching model to create releases and merge new features into the codebase.  We highly recommend reading over the GitFlow blog post if you have never used the GitFlow branching model before.

In compliance with the GitFlow model, we have a master branch, a develop branch, and various feature, hotfix, and release branches.  The `master` branch is the target for any new software releases and represents the places branch to pull from for production ready code.  

The `master` branch should **NEVER** be committed to directly and should only receive updates during the release process.  The `master` branch is expected to be stable and contains the newest features that have been merged in.  

The `develop` branch is assumed to be stable and contains all of the newest features which have been merged back into develp.  The `develop` branch should **NEVER** be committed to directly and should only receive updates from merged feature branches.

When you are ready to submit some code fixes or features, please create a `feature` branch which should be forked from `develop`.  The instructions in the following section will walk through the process to create a new feature branch using the [jgitflow]() Maven plugin.

## Creating a Feature Branch

## Source Formatting